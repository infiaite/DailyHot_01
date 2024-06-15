<template>
  <n-card :bordered="false" class="header" content-style="padding: 0">
    <section>
      <div class="logo" @click="router.push('/')">
        <img src="/ico/favicon.png" alt="logo" />
        <div class="name">
          <n-text>今日热榜</n-text>
          <n-text :depth="3">汇聚全网热点，热门尽览无余</n-text>
        </div>
      </div>
      

    

<script setup>
import {
  SunOne,
  Moon,
  Refresh,
  SettingTwo,
  HamburgerButton,
} from "@icon-park/vue-next";
import { mainStore } from "@/store";
import { NText, NIcon } from "naive-ui";
import { useRouter } from "vue-router";

const router = useRouter();
const store = mainStore();
const timeInterval = ref(null);
const showRefresh = ref(false);






// 监听路由参数变化
watch(
  () => router.currentRoute.value,
  (val) => {
    const isHome = val.path === "/";
    showRefresh.value = isHome ? true : false;
  }
);

onMounted(() => {
  window.$timeInterval = timeInterval.value = setInterval(() => {
    store.timeData = getCurrentTime();
  }, 1000);
  showRefresh.value = router.currentRoute.value?.path === "/" ? true : false;
});

onBeforeUnmount(() => {
  clearInterval(timeInterval.value);
});
</script>

<style lang="scss" scoped>
.header {
  height: 118px;
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: center;
  padding: 24px 5vw;
  z-index: 2;
  top: 0;
  background-color: transparent;
  transition: all 0.3s;
  section {
    width: 100%;
    max-width: 1800px;
    margin: 0 auto;
    display: grid;
    grid-template-columns: repeat(3, 1fr);
    align-items: center;
    justify-content: space-between;
    .logo {
      display: flex;
      flex-direction: row;
      align-items: center;
      cursor: pointer;
      img {
        width: 50px;
        height: 50px;
        margin-right: 16px;
        transition: all 0.3s;
      }
      .name {
        display: flex;
        flex-direction: column;
        span {
          &:nth-of-type(1) {
            font-size: 20px;
            font-weight: bold;
            transition: all 0.3s;
          }
          &:nth-of-type(2) {
            font-size: 12px;
          }
        }
      }
    }
    .current-time {
      display: flex;
      flex-direction: column;
      align-items: center;
      .date {
        font-size: 12px;
      }
    }
    .mobile {
      display: none;
    }
    @media (max-width: 768px) {
      display: flex;
      .logo {
        img {
          width: 40px;
          height: 40px;
        }
        .name {
          span {
            &:nth-of-type(1) {
              font-size: 18px;
            }
          }
        }
      }
      .current-time,
      .controls {
        display: none;
      }
      .mobile {
        display: block;
      }
    }
  }
}
</style>
