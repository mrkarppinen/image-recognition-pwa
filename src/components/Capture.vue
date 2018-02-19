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
                            this.$emit('imageCaptured', data);
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
    reader.onload = () => resolve(reader.result)
    reader.onerror = error => reject(error)
  })
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