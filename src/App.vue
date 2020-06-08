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
      feed: "",
      odds: {
        yellow: (12*4),
        red: (1*4),
        green: (6*4),
        blue: (4*4),
        purple: (2*4)
      }
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
    numList: function(){
      if(this.feed.length < 1){
        return []
      }

      let result = this.feed.split(',');
      for(let i=0; i<result.length; i++){
        let entry = result[i];
        entry = parseInt(entry.trim());
        result[i] = entry
      }

      return result;
    },
    colList: function(){
      let result = {
        yellow: 0,
        red: 0,
        green: 0,
        blue: 0,
        purple: 0
      }
      for(let i=0; i<this.numList.length; i++){
        let entry = this.numList[i];
        result[ this.segments[entry] ]++
      }
      return result;
    },
    stats: function(){
      return {
        yellow: 100 * (this.colList.yellow / this.numList.length),
        red: 100 * (this.colList.red / this.numList.length),
        green: 100 * (this.colList.green / this.numList.length),
        blue: 100 * (this.colList.blue / this.numList.length),
        purple: 100 * (this.colList.purple / this.numList.length)
      }
    },
    averageSugg: function(){
      if(this.feed.length < 1){
        return "yellow"
      }

      let colours = ["yellow","green","blue","purple","red"]

      let highestColour = colours[0];
      let highest = this.odds[colours[0]] - this.stats[colours[0]];
      console.log(highest)

      for(let i = 1; i < colours.length; i++){
        if( this.odds[colours[i]] - this.stats[colours[i]] > highest){
          highestColour = colours[i];
          highest = this.odds[colours[i]] - this.stats[colours[i]];
        }
      }

      return highestColour;

      /*
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


      return lowest;*/
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
