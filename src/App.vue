<template>
  <div id="app">
    <wheel :segments="segments"  @selected="addToFeed"/>
    <textarea class="feed" v-model="feed" />
    <div class="suggestionBox">
      <p style="color: black">suggestion: </p>
      <p :style="'text-shadow: 1px 1px #000; color: '+averageSugg">{{averageSugg}}</p>
    </div>
  </div>
</template>

<script>
import wheel from './components/wheel.vue'

export default {
  name: 'App',
  components: {
    wheel
  },
  data(){
    let segments = [];
    let colours = {
      red: "#993e29",
      yellow: "#a8a032",
      green: "#4b9629",
      blue: "#294f96",
      purple: "#842996"
    }
    return{
      segments: [
        "red", "yellow", "green", "yellow", "blue",
        "yellow", "green", "yellow", "purple", "yellow",
        "green", "yellow", "blue", "yellow", "blue",
        "green", "yellow", "purple", "yellow", "green",
        "yellow", "blue", "yellow", "green", "yellow"
      ],
      feed: ""
    }
  },
  methods: {
    addToFeed: function(e){
      if(this.feed.length > 0){
        this.feed += ", "
      }
      this.feed += e
    }
  },
  computed: {
    averageSugg: function(){
      if(this.feed.length < 1){
        return "yellow"
      }

      let cYellow = (12*4);
      let cRed = 4;
      let cGreen = (6*4);
      let cBlue = (4*4)
      let cPurple = (2*4)
      //console.log(cYellow + cRed + cGreen + cBlue + cPurple)

      let numList = this.feed.split(',');
      let colList = {
        yellow: 0,
        red: 0,
        green: 0,
        blue: 0,
        purple: 0
      }
      for(let i=0; i<numList.length; i++){
        let entry = numList[i];
        entry = parseInt(entry.trim());
        colList[ this.segments[entry] ]++
        numList[i] = entry
      }
      //console.log(numList)
      //console.log(colList)

      let pYellow = 100 * (colList.yellow / numList.length);
      let pRed = 100 * (colList.red / numList.length);
      let pGreen = 100 * (colList.green / numList.length);
      let pBlue = 100 * (colList.blue / numList.length);
      let pPurple = 100 * (colList.purple / numList.length);

      let lowestRat = pYellow/cYellow;
      let lowest = "yellow";
      if(pGreen/cGreen < lowestRat){
        lowestRat = pGreen/cGreen;
        lowest = "green"
      }else if(pBlue/cBlue < lowestRat){
        lowestRat = pBlue/cBlue;
        lowest = "blue"
      }else if(pPurple/cPurple < lowestRat){
        lowestRat = pPurple/cPurple;
        lowest = "purple"
      }else if(pRed/cRed < lowestRat){
        lowestRat = pRed/cRed;
        lowest = "red"
      }


      return lowest;
    }

  }
}
</script>

<style lang="scss">
body, html {
  margin: 0;
  padding: 0;
}
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  display: flex;
  flex-direction: column;
  align-items: center;
  min-height: 100vh;
  background-color: rgb(80,80,90);
}
.feed {
  width: 80%;
  max-width: 700px;
  min-height: 5em;
}
.suggestionBox{
  display: flex;
  >p:nth-of-type(1){
    margin-right: 0.5em;
  }
  background-color: rgba(255,255,255,0.5);
  border-radius: 1em;
  padding-left: 1em;
  padding-right: 1em;

}
</style>
