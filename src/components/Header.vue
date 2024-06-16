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
      <div class="current-time" v-if="store.timeData">
        <n-text class="time">{{ store.timeData.time.text }}</n-text>
        <n-text class="date" :depth="3">
          {{
            store.timeData.lunar.GanZhiYear +
            "年 " +
            store.timeData.lunar.text +
            " " +
            store.timeData.time.weekday
          }}
        </n-text>
      </div>
      <div class="current-time" v-else>
        <n-text class="time">时间获取中</n-text>
      </div>
      <div class="controls">
        <n-space justify="end">
          <n-popover v-if="showRefresh">
            <template #trigger>
              <n-button secondary strong round @click="router.go(0)">
                <template #icon>
                  <n-icon :component="Refresh" />
                </template>
              </n-button>
            </template>
            刷新页面
          </n-popover>
          <n-popover>
            <template #trigger>
              <n-button
                secondary
                strong
                round
                @click="
                  store.setSiteTheme(
                    store.siteTheme === 'light' ? 'dark' : 'light'
                  )
                "
              >
                <template #icon>
                  <n-icon
                    :component="store.siteTheme === 'light' ? Moon : SunOne"
                  />
                </template>
              </n-button>
            </template>
            {{ store.siteTheme === "light" ? "深色模式" : "浅色模式" }}
          </n-popover>
          <n-popover>
            <template #trigger>
              <n-button secondary strong round @click="router.push('/setting')">
                <template #icon>
                  <n-icon :component="SettingTwo" />
                </template>
              </n-button>
            </template>
            全局设置
          </n-popover>
        
          <n-popover>
            <template #trigger>
              <n-button secondary strong round @click="router.push('http://www.hyermall.com')">
                <template #icon>
                  <n-icon :component="SettingTwo" />
                </template>
              </n-button>
            </template>
<a href="http://hyermall.com" target="_blank" class="btn-icon"><svg stroke="currentColor" fill="currentColor" stroke-width="0" viewBox="0 0 24 24" height="24" width="24" xmlns="http://www.w3.org/2000/svg"><path d="M12.001 2C6.47598 2 2.00098 6.475 2.00098 12C2.00098 16.425 4.86348 20.1625 8.83848 21.4875C9.33848 21.575 9.52598 21.275 9.52598 21.0125C9.52598 20.775 9.51348 19.9875 9.51348 19.15C7.00098 19.6125 6.35098 18.5375 6.15098 17.975C6.03848 17.6875 5.55098 16.8 5.12598 16.5625C4.77598 16.375 4.27598 15.9125 5.11348 15.9C5.90098 15.8875 6.46348 16.625 6.65098 16.925C7.55098 18.4375 8.98848 18.0125 9.56348 17.75C9.65098 17.1 9.91348 16.6625 10.201 16.4125C7.97598 16.1625 5.65098 15.3 5.65098 11.475C5.65098 10.3875 6.03848 9.4875 6.67598 8.7875C6.57598 8.5375 6.22598 7.5125 6.77598 6.1375C6.77598 6.1375 7.61348 5.875 9.52598 7.1625C10.326 6.9375 11.176 6.825 12.026 6.825C12.876 6.825 13.726 6.9375 14.526 7.1625C16.4385 5.8625 17.276 6.1375 17.276 6.1375C17.826 7.5125 17.476 8.5375 17.376 8.7875C18.0135 9.4875 18.401 10.375 18.401 11.475C18.401 15.3125 16.0635 16.1625 13.8385 16.4125C14.201 16.725 14.5135 17.325 14.5135 18.2625C14.5135 19.6 14.501 20.675 14.501 21.0125C14.501 21.275 14.6885 21.5875 15.1885 21.4875C19.259 20.1133 21.9999 16.2963 22.001 12C22.001 6.475 17.526 2 12.001 2Z"></path></svg></a>

            
            返回
          </n-popover>
          
          
          
       </n-space>
      
     
      
      
      
      </div>
      <div class="mobile">
        <n-dropdown
          :options="menuOptions"
          size="large"
          trigger="click"
          placement="bottom-end"
          @select="menuOptionsSelect"
        >
          <n-button secondary strong round>
            <template #icon>
              <n-icon :component="HamburgerButton" />
            </template>
          </n-button>
        </n-dropdown>
      </div>
    </section>
  </n-card>
</template>

<script setup>
import {
  SunOne,
  Moon,
  Refresh,
  SettingTwo,
  HamburgerButton,
} from "@icon-park/vue-next";
import { getCurrentTime } from "@/utils/getTime.js";
import { mainStore } from "@/store";
import { NText, NIcon } from "naive-ui";
import { useRouter } from "vue-router";

const router = useRouter();
const store = mainStore();
const timeInterval = ref(null);
const showRefresh = ref(false);

// 移动端时间模块
const timeRender = () => {
  return h(
    "div",
    {
      style: {
        display: "flex",
        flexDirection: "column",
        alignItems: "center",
        padding: "6px 18px",
      },
    },
    [
      h(NText, null, {
        default: () =>
          store.timeData ? store.timeData.time.text : "时间获取失败",
      }),
      h(
        NText,
        { depth: 3, style: "font-size: 12px" },
        {
          default: () =>
            store.timeData
              ? store.timeData.lunar.GanZhiYear +
                "年 " +
                store.timeData.lunar.text +
                " " +
                store.timeData.time.weekday
              : "日期获取失败",
        }
      ),
    ]
  );
};

// 移动端菜单
const menuOptions = [
  {
    key: "header",
    type: "render",
    render: timeRender,
  },
  {
    key: "header-divider",
    type: "divider",
  },
  {
    label: "刷新页面",
    key: "refresh",
    icon: () => {
      return h(NIcon, null, {
        default: () => h(Refresh),
      });
    },
  },
  {
    label: () => {
      return h(NText, null, {
        default: () => (store.siteTheme === "light" ? "深色模式" : "浅色模式"),
      });
    },
    key: "changeTheme",
    icon: () => {
      return h(NIcon, null, {
        default: () => (store.siteTheme === "light" ? h(Moon) : h(SunOne)),
      });
    },
  },
  {
    label: "全局设置",
    key: "setting",
    icon: () => {
      return h(NIcon, null, {
        default: () => h(SettingTwo),
      });
    },
  },
];

// 移动端下拉菜单点击事件
const menuOptionsSelect = (val) => {
  if (val === "refresh") {
    router.go(0);
  } else if (val === "changeTheme") {
    store.setSiteTheme(store.siteTheme === "light" ? "dark" : "light");
  } else if (val === "setting") {
    router.push("/setting");
  }
};

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
