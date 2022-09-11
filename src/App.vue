<script setup>
import { ref } from "vue";
const scroll = ref(false);
const hidden = ref(false);
const total = ref(5);
const time = 10;
const active = ref(0);
const preActive = ref(0);

const scrollHandler = () => {
  // scroll.value = window.scrollY > 0;
  if (window.scrollY > 0) {
    scroll.value = true;
    hidden.value = true;
  } else {
    scroll.value = false;
    hidden.value = false;
  }
};
const img = (n) => {
  return {
    backgroundImage: `url(https://picsum.photos/1920/1200?random=${n})`,
    animationDuration: `${time}s`,
    // animationDelay: `${((n - 1) * time) / 2}`,
    // color: `#00cc99`,
    // paddingTop: `${n}%`,
    // width: 100 + "%",
  };
};

/* 
  輪播範圍公式
  原本        0 1 2 3 4 
  近位+1      1 2 3 4 5  
  ----------------------
  +5 total   6 7 8 9 10
  ％５取餘數   1 2 3 4 0
  如果total = 5 就會取 0~4範圍
  如果total = 6 就會取 0~5範圍
*/
setInterval(function () {
  preActive.value = active.value;
  active.value = (active.value + 1 + total.value) % total.value;
  console.log(preActive.value, active.value);
}, (time / 2) * 1000); // 因為是秒*1000

window.addEventListener("scroll", scrollHandler);
</script>
<template>
  <div class="kv" :class="{ isActive: scroll }">
    <ul class="kvList">
      <div class="title" :class="{ isOpen: hidden }">
        <h1 class="">台北當代藝術展</h1>
        <p class="">01 JUN - 31 OCT</p>
        <div class="btnGroup">
          <a href="javascript:;" class="btn btn-primary">購買票券</a>
          <a href="javascript:;" class="btn btn-outline-primary">詳細資訊</a>
        </div>
      </div>
      <li
        v-for="n in total"
        :key="n"
        :style="img(n)"
        :class="{ animate: active === n - 1 || preActive === n - 1 }"
      ></li>
    </ul>
    <div class="text" :class="{ isOpen: !hidden }">
      <h2>Life on DESIGN</h2>
      <p>
        針對在地生活、文化及產業發展特色，運用設計思考詮釋新時代的意涵，並以展覽呈現在地設計。
      </p>
    </div>
  </div>
</template>

<style lang="scss">
/* @import "@/assets/base.css"; */
* {
  margin: 0;
  padding: 0;
  box-sizing: border-box;
  outline: 1px solid red;
}
ul {
  list-style: none;
}
body {
  height: 200vh;
}
.kv {
  position: relative;
  height: 100vh;
  background-color: #fff;
  color: #fff;
  @media screen and (min-width: 414px) {
    height: 70vh;
  }
  @media screen and (min-width: 768px) {
    height: 80vh;
  }
  @media screen and (min-width: 992px) {
    height: 100vh;
  }
}
.kvList {
  position: absolute;
  width: 100%;
  height: 100%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 2;
  transition: all 0.5s;
}
.kvList > li {
  width: 100%;
  height: 100%;
  position: absolute;
  background-position: center;
  background-repeat: no-repeat;
  opacity: 0;
}
.kvList > li.animate {
  animation: kvAnimation;
  animation-timing-function: linear;
  @media (orientation: portrait) {
    animation: kvAnimation-portrait;
  }
}
.isActive .kvList {
  max-width: 1200px;
  height: 360px;
}
/* title */
.title {
  position: absolute;
  width: 85%;
  top: 50%;
  left: 50%;
  transform: translate(-50%, -50%);
  z-index: 1;
}
.title h1 {
  font-size: 42px;
  @media screen and (min-width: 768px) {
    font-size: 60px;
  }
  @media screen and (min-width: 992px) {
    font-size: 100px;
  }
}
.title p {
  font-size: 24px;
  margin-bottom: 16px;
}
.btnGroup {
  display: flex;
}
.btnGroup .btn {
  padding: 10px 30px;
  text-decoration: none;
  letter-spacing: 2px;
  color: #fff;
}
@media screen and (min-width: 768px) {
  .btnGroup .btn {
    padding: 10px 50px;
  }
}
.btn-primary {
  background-color: rgb(145, 34, 81);
}
.btn-outline-primary {
  border: 2px solid rgb(145, 34, 81);
  margin-left: 20px;
}
/* text */
.text {
  width: 100%;
  position: absolute;
  padding: 15px;
  top: calc(50vh + 240px);
  left: 50%;
  transform: translateX(-50%);
  text-align: center;
  z-index: 1;
  display: block;
  color: #000;
  @media (orientation: portrait) {
    top: initial;
    bottom: 0;
  }
}
.isOpen {
  display: none;
}
.text h2 {
  font-size: 32px;
  @media screen and (min-width: 768px) {
    font-size: 60px;
  }
  @media screen and (min-width: 992px) {
    font-size: 100px;
  }
}
.text p {
  @media screen and (min-width: 768px) {
    font-size: 20px;
  }
}

@keyframes kvAnimation {
  0% {
    opacity: 0;
    background-size: 150% auto;
  }
  25% {
    opacity: 1;
  }
  50% {
    opacity: 1;
  }
  75% {
    opacity: 0;
  }
  100% {
    opacity: 0;
    background-size: 120% auto;
  }
}
@keyframes kvAnimation-portrait {
  0% {
    opacity: 0;
    background-size: auto 150%;
  }
  25% {
    opacity: 1;
  }
  50% {
    opacity: 1;
  }
  75% {
    opacity: 0;
  }
  100% {
    opacity: 0;
    background-size: auto 120%;
  }
}
</style>
