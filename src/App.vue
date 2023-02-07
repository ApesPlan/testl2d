<script setup lang="ts">
import { onMounted, ref } from "vue";
import * as PIXI from "pixi.js";
import { Live2DModel } from "pixi-live2d-display";
// import HelloWorld from './components/HelloWorld.vue'
const pixiLive2dDisplayRef = ref();
const dir = "assets/l2d/";
const w = window as any;

onMounted(() => {
  init();
});

const init = async () => {
  const app = new PIXI.Application({
    width: w.innerWidth,
    height: w.innerHeight,
    view: pixiLive2dDisplayRef.value,
  });

  const model = await Live2DModel.from(dir + "c453_02/model.json");

  app.stage.addChild(model);

  // 变换
  model.x = 100;
  model.y = 100;
  model.rotation = Math.PI;
  model.skew.x = Math.PI;
  model.scale.set(2, 2);
  model.anchor.set(0.5, 0.5);

  // 交互
  model.on("hit", (hitAreas: any) => {
    if (hitAreas.includes("body")) {
      model.motion("tap_body");
    }
  });
};
</script>

<template>
  <div>
    <!-- <a href="https://vitejs.dev" target="_blank">
      <img src="/vite.svg" class="logo" alt="Vite logo" />
    </a>
    <a href="https://vuejs.org/" target="_blank">
      <img src="./assets/vue.svg" class="logo vue" alt="Vue logo" />
    </a> -->
    <canvas
        id="pixiLive2dDisplay"
        ref="pixiLive2dDisplayRef"
      ></canvas>
  </div>
  <!-- <HelloWorld msg="Vite + Vue" /> -->
</template>

<style scoped>
/* .logo {
  height: 6em;
  padding: 1.5em;
  will-change: filter;
}
.logo:hover {
  filter: drop-shadow(0 0 2em #646cffaa);
}
.logo.vue:hover {
  filter: drop-shadow(0 0 2em #42b883aa);
} */
</style>
