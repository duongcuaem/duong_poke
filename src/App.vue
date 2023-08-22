<template>
  <main-screen v-if="statusmatch==='default'" @onStart="onHandleBeforeStart($event)"/>
  <interact-screen 
  v-if="statusmatch==='match'" 
  @onFinish="onGetResult"
  :cardsContext="settings.cardsContext"/>
  <reasult-screen v-if="statusmatch==='result'" :timer="timer" @onStartAgain="statusmatch='default'"/>
  <coppyRightScreen />
</template>

<script>
  import mainScreen from './components/mainScreen.vue';
  import ReasultScreen from './components/ReasultScreen.vue';
  import coppyRightScreen from './components/coppyRightScreen.vue';
  import { shuffled } from "./utils/array";
  import InteractScreen from './components/InteractScreen.vue';

  export default {
      name: "App",
      components:{
        mainScreen,
        InteractScreen,
        coppyRightScreen,
        ReasultScreen,
      },
      data(){
        return {
          settings:{
            totalOfBlocks:0,
            cardsContext:[],
            startedAt : null,
          },
          statusmatch: "default",
          timer: 0
        }
      },
      methods:{
        onHandleBeforeStart(config){
          this.settings.totalOfBlocks = config.totalOfBlocks ;
          
          const firstCards = Array.from({length: this.settings.totalOfBlocks / 2}, (_,i) => i +1);
          const secondCards = [...firstCards];
          const cards = [...firstCards , ...secondCards];
          this.settings.cardsContext = shuffled(shuffled(shuffled(shuffled(cards))))
          // data ready
          this.settings.startedAt = new Date().getTime();
          this.statusmatch = "match";
        },
        onGetResult(){
          this.timer = new Date().getTime() - this.settings.startedAt
          this.statusmatch = "result";
        }
      }
  };
</script>

