<template>
<div>
<h2 v-if="dataUri" >Watson</h2>
<button v-on:click="doAnalyze" v-if="dataUri && state == 'notAnalyzed' && labels.length == 0" id="analyze-button" >WatsonAnalyze</button>
<img v-if="state=='loading'" className="image is-128x128" src="/static/img/icons/loading.svg" />
<ul id="label-list">
<li v-for="label in labels">
    {{ label.class }} : {{ label.score }}
</li>
 
</ul>
<div class="error">
{{error}}
</div>
</div>
</template>


<script>
export default {
  name: 'watson-analyze',
  props: ['dataUri', 'apiKey'],
  data () {
    return {
      labels: [],
      state: 'notAnalyzed',
      error: ''
    }
  },
  created () {
     
  },
   watch: {
        dataUri(val, oldval) { 
             this.labels = [];
             this.state = 'notAnalyzed';
             this.error = '';
        }
      },
  methods: {
    doAnalyze: function () {
      this.state = 'loading';


      let index = this.dataUri.indexOf('base64,');
      let image = this.dataUri.substring((index + 'base64,'.length));

   
      fetch('', {
        headers: {
          'x-Api-Key': this.apiKey,
          'Accept': 'application/json',
          'Content-Type': 'application/json',
        },
        method: 'POST',
        body: JSON.stringify({file: image})
      })
        .then((resp) => resp.json())
        .then((data) => {

          if (data && data.classes) {
            this.labels = data.classes;
          }
          this.state = 'analyzed';
        })
        .catch((err) => {
             this.error = JSON.stringify(err);
             this.state = 'analyzed';

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


.error {
  color: red;
}
</style>