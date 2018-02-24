<template>
    <div id="capture">
        <input v-on:change="imageCaptured" type="file" accept="image/*" id="image-file" ref="fileInput" />
        <button id="load-button" @click="selectImg" ></button>
    </div>
</template>


<script>
export default {
    name: 'capture',
    methods: {
        imageCaptured: function (event) {
                if (event.target.files.length > 0) {
                    let file = event.target.files[0]
                    getBase64(file).then(
                        data => {
                            this.$emit('imageCaptured', {dataUri: data} );
                        }
                    )
                }
        },
        selectImg: function (){
            this.$refs.fileInput.click();
        }
    }
}

function getBase64 (file) {
  return new Promise((resolve, reject) => {
    const reader = new FileReader()
    reader.readAsDataURL(file)
    reader.onload = () => {

        if (file.size <= 1000000){
            resolve(reader.result); 
        }else {
            let scale = 1000000 / file.size;
            scaleImage(reader.result, file.type, scale).then( (dataUri) => resolve(dataUri) )
        }
        
    }
    reader.onerror = error => reject(error)
  })
}


function scaleImage(dataUri, type, scale){
    return new Promise( (resolve, reject) => {
            let img = document.createElement("img");
            img.src = dataUri;
            img.onload = () => {
                
                let canvas = document.createElement('canvas');
                canvas.width = img.width * scale;
                canvas.height = img.height * scale;
                canvas.getContext('2d').drawImage(img, 0, 0, canvas.width, canvas.height);

                resolve(canvas.toDataURL(type));                
            }
    });
}



</script>


<style scoped>
   #image-file {
       display: none;
   }

   #load-button {
    background: url('/static/img/icons/Camera-icon.png');   
    border: none;
    background-size: contain;
    width: 100px;
    height: 100px;
   }
   
</style>