<template>
  <div id="app">
    <header>
      <span>Battle</span>
    </header>
    <main>
    <div v-if="!apiKeyOK" id="key-container">
      <input type="text"  v-model="apiKey" placeholder="apikey" id="apiKey-field" @keyup.enter="saveKey" />
      <button type="button" @click="saveKey" id="save-button" v-if="apiKey.length > 10" >Save</button>
    </div>
    <div v-if="apiKeyOK">
      <capture v-on:imageCaptured="imageCaptured"></capture>
      <imageDisplay v-bind:src="dataUri" ></imageDisplay> 
      <analyze v-bind:dataUri="dataUri" v-bind:apiKey="apiKey" ></analyze>
      <watson-analyze v-bind:dataUri="dataUri" v-bind:apiKey="apiKey"  ></watson-analyze>
    </div>  
    </main>
  </div>
</template>

<script>

import Capture from './components/Capture'
import ImageDisplay from './components/ImageDisplay'
import Analyze from './components/Analyze'
import WatsonAnalyze from './components/WatsonAnalyze'

export default {
  name: 'app',
  components: {
    Capture,
    ImageDisplay,
    Analyze,
    WatsonAnalyze
  },
  data(){
    return {
      dataUri: '',
      apiKey: '',
      apiKeyOK: false
    }
  },
  created: function (){
  },
  methods: {
    imageCaptured: function (image) {
      this.dataUri = image.dataUri;

    },
    saveKey: function (event){
      this.apiKeyOK = true;
    }
  }

}
</script>

<style>
body {
  margin: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  color: #2c3e50;
}

main {
  text-align: center;
  margin-top: 40px;
}

header {
  margin: 0;
  height: 56px;
  padding: 0 16px 0 24px;
  background-color: #0760A6;/*#35495E;*/
  color: #ffffff;
  text-align: center;
}

header span {
  display: block;
  position: relative;
  font-size: 20px;
  line-height: 1;
  letter-spacing: .02em;
  font-weight: 400;
  box-sizing: border-box;
  padding-top: 16px;
}

#key-container {
  display: flex;
  flex-direction: column;
}

#apiKey-field{
  border: none;
  border-bottom: 1px solid #ccc;
  text-align: center;
  align-self: center;
}


#save-button {
  border: none;
  background: none;
  cursor: pointer;
  height: 50px;
  width: 50px;
  align-self: center;
}
</style>
