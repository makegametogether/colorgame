<template>
  <div class="outer-div">
    <color-game class="pb-9" />
  </div>
</template>

<script>
import ColorGame from "./ColorGame.vue";
export default {
  name: "Home",
  components: { ColorGame },
  data: function () {
    return {};
  },
  mounted() {},
  methods: {
    test() {
      console.log("test");
    },
  },
};
</script>

<style>
.outer-div {
  width: 100%;
  height: 100%;
  background: #d0cbc7;
  position: absolute;
}
</style>
