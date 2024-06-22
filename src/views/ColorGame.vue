<template>
  <div>
    <div v-show="state == STATE_GAME">
      <div class="level-text">Level : {{ level }}</div>
      <progress-bar ref="pb" v-on:endGame="endGame" />
    </div>

    <div v-show="state == STATE_NONE">
      <div class="start-div" @click="startGame">start</div>
      <div class="container-for-icon mt-5">
        <span class="item-for-icon" style="background: rgb(104, 89, 94)"></span>
        <span class="item-for-icon" style="background: rgb(163, 147, 147)"></span>
        <span class="item-for-icon" style="background: rgb(238, 214, 210)"></span>
        <span class="item-for-icon" style="background: rgb(232, 184, 180)"></span>
      </div>
    </div>
    <div v-show="state == STATE_RESULT">
      <div class="result-text mb-1">Record : Level {{ level }}</div>
      <div class="result-text-explain mb-2">{{ result_explain }}</div>
    </div>
    <div v-show="state != STATE_NONE" id="container" class="container mb-5"></div>

    <div v-show="state == STATE_GAME">
      <div class="level-text-explain" v-if="level == 1">Find the square with a different color</div>
    </div>

    <div v-show="state == STATE_RESULT">
      <div style="height: 30px"></div>
      <div class="mt-3 mb-3" style="text-align: center">
        <span class="result-btn-div result-btn-div-text ml-2 mr-2" @click="reGame">Play again</span>
      </div>
    </div>
  </div>
</template>

<script>
import ProgressBar from "./Progressbar.vue";

export default {
  name: "ColorGame",
  data: function () {
    return {
      STATE_NONE: "none",
      STATE_GAME: "game",
      STATE_RESULT: "result",
      state: "none",
      level: 0,
      result_explain: "",
    };
  },

  components: { ProgressBar },
  mounted() {
    this.state = this.STATE_NONE;
  },
  methods: {
    startGame() {
      this.state = this.STATE_GAME;
      this.level = 0;
      this.$refs.pb.start();
      this.nextProblem();
    },
    endGame() {
      this.state = this.STATE_RESULT;

      //정답 테두리 하기
      let el = document.getElementById("answer");
      el.classList.add("answer-border");

      //result 문구 넣기
      this.result_explain = this.getResultComment(this.level);
    },
    reGame() {
      //this.state = this.STATE_NONE;
      window.location.reload();
    },
    nextProblem() {
      this.level++;
      this.makeGrid(this.level);

      let items = document.getElementsByClassName("my-item");

      let r = this.rand(10, 245);
      let g = this.rand(10, 245);
      let b = this.rand(10, 245);

      let diffValue = this.getDiffValue(this.level);

      for (let i = 0; i < items.length; i++) {
        if (items[i].id == "answer") {
          items[i].style.background = this.getRGB(r + diffValue, g + diffValue, b + diffValue);
        } else {
          //items[i].style.background = "#00ff00";
          items[i].style.background = this.getRGB(r, g, b);
        }
      }
    },
    getRGB(r, g, b) {
      return "rgb(" + r + "," + g + "," + b + ")";
    },
    getDiffValue(level) {
      let value = 20;
      if (level < 2) {
        value = 27;
      } else if (level < 10) {
        value = 22 - level;
      } else {
        value = Math.floor(17 - level / 2);
        if (value < 7) {
          value = 7;
        }
      }

      return value;
    },
    makeGrid(level) {
      let container = document.getElementById("container");
      if (container != null) {
        container.innerHTML = "";
        container.classList.value = "";
        container.classList.add("container");
        let itemCount = 4;

        if (level == 1) {
          itemCount = 4;
          container.classList.add("container2by2");
        } else if (level == 2) {
          itemCount = 9;
          container.classList.add("container3by3");
        } else if (level < 5) {
          itemCount = 16;
          container.classList.add("container4by4");
        } else if (level < 8) {
          itemCount = 25;
          container.classList.add("container5by5");
        } else if (level < 12) {
          itemCount = 36;
          container.classList.add("container6by6");
        } else if (level < 17) {
          itemCount = 49;
          container.classList.add("container7by7");
        } else if (level < 23) {
          itemCount = 64;
          container.classList.add("container8by8");
        } else {
          itemCount = 64;
          container.classList.add("container8by8");
        }
        let answerNum = this.rand(0, itemCount);

        for (let i = 0; i < itemCount; i++) {
          let el = document.createElement("span");
          if (i == answerNum) {
            el.id = "answer";
            el.onclick = this.answer;
          }
          el.classList.add("my-item");

          container.appendChild(el);
        }
      }
    },
    answer() {
      if (this.state == this.STATE_GAME) {
        this.nextProblem();
      }
    },
    getResultComment(level) {
      let result_explain = "You're an expert.";

      return result_explain;
    },
    rand(min, max) {
      return Math.floor(Math.random() * (max - min)) + min;
    },
  },
};
</script>

<style>
@import url("https://fonts.googleapis.com/css2?family=Sunflower:wght@300;500&display=swap");
@import url("https://fonts.googleapis.com/css?family=Comfortaa");
.container {
  margin: auto;
  display: grid;
  justify-content: center;
  align-content: center;
  background: #a8a8a8;
  width: 336px;
  border-radius: 6px;
}
.container2by2 {
  padding: 10px;
  gap: 8px;
  grid-template-columns: repeat(2, 155px);
  grid-template-rows: repeat(2, 155px);
}
.container3by3 {
  padding: 10px;
  gap: 8px;
  grid-template-columns: repeat(3, 101px);
  grid-template-rows: repeat(3, 101px);
}
.container4by4 {
  padding: 8px;
  gap: 6px;
  grid-template-columns: repeat(4, 76px);
  grid-template-rows: repeat(4, 76px);
}
.container5by5 {
  padding: 8px;
  gap: 6px;
  grid-template-columns: repeat(5, 60px);
  grid-template-rows: repeat(5, 60px);
}
.container6by6 {
  padding: 7px;
  gap: 5px;
  grid-template-columns: repeat(6, 50px);
  grid-template-rows: repeat(6, 50px);
}
.container7by7 {
  padding: 7px;
  gap: 5px;
  grid-template-columns: repeat(7, 42px);
  grid-template-rows: repeat(7, 42px);
}
.container8by8 {
  padding: 7px;
  gap: 4px;
  grid-template-columns: repeat(8, 37px);
  grid-template-rows: repeat(8, 37px);
}
.my-item {
  width: 100%;
  height: 100%;
  text-align: center;
  color: rgb(175, 57, 57);
  font-size: 30px;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 6px;
}
.level-text {
  margin-top: 60px;
  font-family: "Comfortaa", cursive;
  font-size: 2em;
  text-align: center;

  color: #444457;
}
.level-text-explain {
  margin-top: 20px;
  font-family: "Sunflower", sans-serif;
  font-weight: 300;
  font-size: 1.2em;
  text-align: center;

  color: #444457;
}
.reulst-div {
  text-align: center;
}
.result-text {
  margin-top: 45px;
  font-family: "Comfortaa", cursive;
  font-size: 1.6em;
  text-align: center;

  color: #1c5391;
}
.result-text-explain {
  font-family: "Sunflower", sans-serif;
  font-weight: 300;
  font-size: 1.3em;
  text-align: center;

  color: #444457;
}

.start-div {
  margin: auto;
  margin-top: 130px;
  margin-bottom: 20px;
  font-family: "Comfortaa", cursive;
  font-size: 2em;
  width: 150px;
  height: 45px;
  padding-top: 9px;
  text-align: center;
  border: 2px solid #444457;
  background-color: transparent;
  color: #444457;
}

.result-btn-div {
  width: 150px;
  margin-top: 100px;
  height: 70px;
  padding: 10px;
  text-align: center;
  border: 2px solid #444457;
  background-color: transparent;
}
.result-btn-div-text {
  font-family: "Sunflower", sans-serif;
  font-weight: 300;
  font-size: 1em;
  color: #444457;
}

.container-for-icon {
  margin: auto;
  display: grid;
  justify-content: center;
  align-content: center;
  background: #a8a8a8;
  width: 117px;
  border-radius: 5px;
  padding: 7px;
  gap: 5px;
  grid-template-columns: repeat(2, 50px);
  grid-template-rows: repeat(2, 50px);
}

.item-for-icon {
  width: 100%;
  height: 100%;
  text-align: center;
  color: rgb(175, 57, 57);

  font-size: 30px;
  font-weight: bold;
  display: flex;
  justify-content: center;
  align-items: center;
  border-radius: 5px;
}
.answer-border {
  border-style: outset;
  border-image: linear-gradient(to right, #f82459 0%, #f77d1a 50%, #f7c00d 100%);
  border-image-width: 2px;
  border-image-slice: 1;
}
</style>
