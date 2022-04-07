<template>
  <MainScreen v-if="statusMatch === 'default'" @onStart="onHandleBeforeStart($event)"></MainScreen>
  <Interact v-if="statusMatch === 'match'" :cardConText="settings.cardConText" @onFinish="onGetResult"></Interact>
  <ResultScreen v-if="statusMatch === 'result'" :timer="timer" @onStartAgain="statusMatch = 'default'"/>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import Interact from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue"

import {shuffled} from "./utils/array"

export default {
  name: "App",
  components: {
    MainScreen,
    Interact,
    ResultScreen
  },
  data() {
    return {
      settings: {
        totalOfBlocks: 0,
        cardConText: [],
        startedAt: null,
      },
      statusMatch: "default",
      timer: 0,
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      console.log("running", config);
      this.settings.totalOfBlocks = config.totalOfBlocks;
      const firstCard = Array.from({ length: this.settings.totalOfBlocks / 2}, (_, i) => i + 1);

      const secondCard = [...firstCard]

      const cards = [...firstCard, ...secondCard];
      // console.log(cards)
      this.settings.cardConText = shuffled(shuffled(shuffled(shuffled(cards))))
      this.settings.startedAt = new Date().getTime();



      this.statusMatch = "match";
    },
    onGetResult() {
      this.timer = new Date().getTime() - this.settings.startedAt;
      this.statusMatch = "result"
    }
  }
};
</script>
