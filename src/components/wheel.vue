<template>
  <div class="container">
    <div v-for="(seg, num) in segments" :key="num" class="section"
      :style="'transform: translateX(-50%) rotate('+( 360 * (num/25) )+'deg)'"
    >
      <svg viewBox="0 0 100 100" style="height: 100%; width: 100%" preserveAspectRatio="none" >
        <polygon
          class="slice"
          points="0,0 50,100 100,0"
          style="fill: white;"
        />
        <polygon
          class="slice"
          points="0,0 50,100 100,0"
          :style="'fill: '+colours[seg]+';stroke: #4e5434;stroke-width: 3;'"
          @click="clicked(num)"
        />
      </svg>
      <p :style="'transform: translateX(-50%) rotate('+( -360 * (num/25) )+'deg)'" >
        {{num}}
      </p>
    </div>
    <div class="centerPlate"></div>
  </div>
</template>

<script>
export default {
  name: 'wheel',
  props: {
    segments: {}
  },
  data(){
    return{
      colours: {
        red: "#993e29",
        yellow: "#a8a032",
        green: "#4b9629",
        blue: "#294f96",
        purple: "#842996"
      }
    }
  },
  methods: {
    clicked: function(num){
      this.$emit('selected', num);
    }
  }
}
</script>

<!-- Add "scoped" attribute to limit CSS to this component only -->
<style scoped lang="scss">
.container{
  color: white;
  width: 500px;
  height: 500px;
  //background-color: black;
  position: relative;
}
.section{
  height: 45%;
  width: 11.5%;
  position: absolute;
  bottom: 50%;
  left: 50%;
  transform-origin: 50% 100%;
  p{
    top: 0;
    left: 50%;
    font-size: 1.6em;
    pointer-events: none;
    user-select: none;
    position: absolute;
  }
}
.slice:hover{
  fill-opacity: 0.6;
  cursor: pointer;
}
.centerPlate{
  width: 50%;
  height: 50%;
  position: absolute;
  left: 50%;
  top: 50%;
  transform: translate(-50%, -50%);
  border-radius: 50%;
  border: 6px solid #4e5434;
  background-color: #6a7345;
}

</style>
