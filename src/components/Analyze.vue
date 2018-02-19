<template>
<div>
<button v-on:click="doAnalyze" v-if="dataUri.length > 0 && state == 'notAnalyzed' && labels.length == 0" id="analyze-button" >Analyze</button>
<img v-if="state=='loading'" className="image is-128x128" src="/static/img/icons/loading.svg" />
<ul id="label-list">
<li v-for="label in labels">
    {{ label['Name'] }} : {{ label['Confidence'] }}
</li>
 
</ul>
</div>
</template>


<script>
export default {
  name: 'analyze',
  props: ['dataUri', 'apiKey'],
  data () {
    return {
      labels: [],
      state: 'notAnalyzed'
    }
  },
  created () {
     
  },
   watch: {
        dataUri(val, oldval) { 
             this.labels = [];
             this.state = 'notAnalyzed';
        }
      },
  methods: {
    doAnalyze: function () {
      this.state = 'loading';
      let index = this.dataUri.indexOf('base64,')
      let image = this.dataUri.substring((index + 'base64,'.length));
      
      fetch('', {
        headers: {
          'X-Api-Key': this.apiKey,
          'Accept': 'application/json',
          'Content-Type': 'application/json'
        },
        method: 'POST',
        body: JSON.stringify({
          analyze: image
        })
      })
        .then((resp) => resp.json())
        .then((data) => {
          if (data && data['Labels']) {
            this.labels = data['Labels']
          }
          this.state = 'analyzed';
        })
        .catch((err) => {
             console.error(err);
             this.state = 'notAnalyzed';

        });

    }
  }
}
</script>


<style scoped>

#analyze-button {
  border: none;
  background: none;
  cursor: pointer;
  height: 50px;
  width: 50px;
}

#label-list {
    list-style: none;
}
</style>