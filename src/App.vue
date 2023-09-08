<template>
  <app-main-screen v-if="statusMatch === 'default'" 
  @onStart="onHandleBeforeStart($event)" />

  <app-interact-screen v-if="statusMatch === 'match'" :cardContext="setting.cardContext" @onFinish="onGetResult()" />
  <app-result-screen v-if="statusMatch === 'result'" :timer="setting.timer" @onStartAgain="statusMatch = 'default' " />
  <app-copy-right/>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import CopyRight from "./components/CopyRight.vue";
import InteractScreen from "./components/InteractScreen.vue";
import ResultScreen from "./components/ResultScreen.vue";
import { shuffled } from "./utils/array";

export default {
  name: "App",
  data() {
    return {
      statusMatch: "default",
      setting: {
        totalOfBlocks: 0,
        cardContext: [],
        startedAt: null,
        timer:0,
      },
    };
  },
  components: {
    AppMainScreen: MainScreen,
    AppInteractScreen: InteractScreen,
    AppResultScreen: ResultScreen,
    AppCopyRight: CopyRight,
  },
  methods: {
    onHandleBeforeStart(config) {
      this.setting.totalOfBlocks = config.totalOfBlocks;
      const firstCards = Array.from(
        { length: this.setting.totalOfBlocks / 2 },
        (_, i) => i + 1
      );
      const secondCards = [...firstCards];
      const cards = [...firstCards, ...secondCards];
      this.setting.cardContext = shuffled(shuffled(shuffled(shuffled(cards))));

      this.setting.startedAt = new Date().getTime(); 
      this.statusMatch = "match";
    },
    onGetResult(){
      this.setting.timer = new Date().getTime() - this.setting.startedAt;

      this.statusMatch = "result";
    },  
  },
};
</script>
