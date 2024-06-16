<template>
  <n-button secondary strong round @click="goToHyermall">
    Go to HyerMall
  </n-button>
</template>

<script>
export default {
  methods: {
    goToHyermall() {
      this.$router.push('http://www.hyermall.com');
    }
  }
}
</script>
