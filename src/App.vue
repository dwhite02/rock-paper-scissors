<template>
  <div id="app" >
    <GameScreen v-if="!gameState" v-on:begin="startGame" class="h-screen"/>
    <GamePlay v-else class="h-screen"/>
  </div>
</template>

<script>
import HelloWorld from './components/HelloWorld.vue'
import GameScreen from './components/GameScreen.vue'
import GamePlay from './components/GamePlay.vue'


export default {
  name: 'App',
  data () {
    return {
      gameState: false
    }
  },
  components: {
    HelloWorld, GameScreen, GamePlay
  },
  methods: {
    startGame: function () {
      this.gameState = true;
    }
  }

}
</script>

<style lang="scss">
@import url('https://fonts.googleapis.com/css2?family=Roboto+Condensed:wght@100;300;400;500;700;900&family=Roboto:wght@100;300;400;500;700;900&display=swap');

@font-face {
    font-family: "American Captain";
    src:url('/fonts/American Captain.woff2') format('woff2'),
        url('/fonts/American Captain.woff') format('woff'),
        url('/fonts/American Captain.ttf') format('truetype');
}


#app {
  font-family: 'Roboto', Helvetica, Arial, sans-serif;
  font-weight: 300;
  font-size: 1em;
  
  h1, h2, h3, h4, h5, h6 {
    font-family:'American Captain', 'Roboto', Helvetica, Arial, sans-serif;
    font-weight: 900 !important;
    letter-spacing: 2px;
  }

  button:focus {
    
    outline: none;
}
}
</style>
