<template>
  <div>
    <div id="progressBar">
      <div id="bar" class="innerbar"></div>
    </div>
  </div>
</template>

<script>
export default {
  data() {
    return {
      progressbarHandler: null,
      timeBegan: null,
      COLOR_RED: "red",
      COLOR_YELLOW: "yellow",
      currentColor: "none",
      MAX_TIME: 1000,
    };
  },
  computed: {},
  destroyed() {
    if (this.progressbarHandler != null) {
      clearInterval(this.progressbarHandler);
    }
  },
  methods: {
    start() {
      //console.log("start");
      this.timeBegan = new Date();
      let el = document.getElementById("bar");
      el.style.width = "0%";
      this.currentColor = "none";
      this.changeBarColor();

      if (this.progressbarHandler == null) {
        this.progressbarHandler = setInterval(this.updateProgressbar, 30);
      }
    },
    updateProgressbar() {
      var currentTime = new Date(),
        timeElapsed = new Date(this.countDownTime - (currentTime - this.timeBegan));
      let time = currentTime - this.timeBegan;
      let el = document.getElementById("bar");
      let width = (time / this.MAX_TIME) * 100 + "%";
      width = parseFloat(width).toFixed(2);
      if (width > 100) {
        width = 100;
        this.endGame();
      } else if (width > 87 && this.currentColor != this.COLOR_RED) {
        this.currentColor = this.COLOR_RED;
        this.changeBarColor();
      } else if (width > 60 && this.currentColor != this.COLOR_YELLOW && this.currentColor != this.COLOR_RED) {
        this.currentColor = this.COLOR_YELLOW;
        this.changeBarColor();
      }

      //console.log("width" + width);
      let widthStr = width + "%";
      el.style.width = widthStr;
    },
    changeBarColor() {
      let el = document.getElementById("bar");
      if (el == null) {
        return;
      }
      el.classList.remove("innerbar-red");
      el.classList.remove("innerbar-yellow");
      if (this.currentColor == this.COLOR_YELLOW) {
        el.classList.add("innerbar-yellow");
      } else if (this.currentColor == this.COLOR_RED) {
        el.classList.add("innerbar-red");
      }
    },
    endGame() {
      console.log("end game coming?");
      this.$emit("endGame");
      if (this.progressbarHandler != null) {
        clearInterval(this.progressbarHandler);
        this.progressbarHandler = null;
      }
    },
  },
};
</script>

<style scoped>
#progressBar {
  max-width: 330px;
  width: 90%;
  margin: 10px auto;
  height: 8px;

  border-radius: 3px;
  background: linear-gradient(#6fa6d66c, #7db1df54);
}

.innerbar {
  max-width: 330px;
  height: 100%;
  text-align: right;
  height: 8px; /* same as #progressBar height if we want text middle aligned */
  width: 30%;
  border-radius: 3px;
  background: linear-gradient(#5be6ba, #5ed1ad);
}
.innerbar-yellow {
  background: linear-gradient(#ffec3f, #e9dd71) !important;
}
.innerbar-red {
  background: linear-gradient(#f14545, #e65b5b) !important;
}
</style>
