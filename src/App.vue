<template>
  <main-screen @onStart="onHandleBeforeStart($event)" v-if="statusMatch === 'default'"></main-screen>
  <interact-screen v-if="statusMatch == 'match'" :cardsContext="settings.cardsContext" @onFinish="onGetResult">

  </interact-screen>
  <result-screen :timer="timer" v-if="statusMatch == 'result'" @startAgain="startAgain"></result-screen>
</template>

<script>
import MainScreen from "./components/MainScreen.vue";
import InteractScreen from "./components/InteractScreen.vue";
import { shuffled } from "./utils/array";
import ResultScreen from "./components/ResultScreen.vue";
// import { fireStoreCore } from "./config/firebase";


export default {
  // setup() {
  //   console.log(fireStoreCore.collection('transactions').get())
  // },
  name: "App",
  components: { MainScreen, InteractScreen, ResultScreen },
  data() {
    return {
      settings: {
        totalOfBlock: 0, // chế độ chơi 4x4 (16) 6x6 (36) ...
        cardsContext: [], // mảng chứa 2 mảng sau khi đã mix (secondCard và firstCard)
        startedAt: null, // Thời gian hoàn thành màn chơi
      },
      statusMatch: "default", // chuyển component
      timer: 0, // Thời gian hoàn thành màn chơi chơi
    };
  },
  methods: {
    onHandleBeforeStart(config) {
      this.settings.totalOfBlock = config.totalOfBlocks;
      const firstCards = Array.from(
        { length: this.settings.totalOfBlock / 2 },
        (_, i) => i + 1 //Mảng thứ 1; Chia đôi mảng tách ra thành 2 mảng
      );
      const secondCards = [...firstCards]; // Mảng thứ 2
      const cards = [...firstCards, ...secondCards]; // mix 2 mảng lại vưới nhau

      this.settings.cardsContext = shuffled(
        shuffled(shuffled(shuffled(cards)))
      ); // mix mảng ngẫu nhiên

      this.settings.startedAt = new Date().getTime();
      this.statusMatch = "match";
    },
    onGetResult() {
      // Get timer
      this.timer = new Date().getTime() - this.settings.startedAt

      // Chuyển sang component hoàn thành trò chơi (ResultScreen)
      this.statusMatch = "result";
    },
    startAgain() { // bắt đầu màn mới
      this.statusMatch = "default";
      console.log(2);
    }
  },
};
</script>
