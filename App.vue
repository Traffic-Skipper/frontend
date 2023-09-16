<template>
  <view class="container">
      <home-page/>
  </view>
</template>

<script>
import HomePage from "./src/pages/HomePage.vue";

export default {
    components: {
        HomePage,
    }
}
</script>

<style>
.container {
    flex: 1;
}
</style>
