<template>
    <view class="container">
        <image class="arms" :source="require('../../assets/coat-of-arms.png')"/>
        <search-bar class="bar"/>
    </view>
</template>

<script>
import SearchBar from "./SearchBar.vue";

export default {
    name: "TopBar",
    components: {
        SearchBar
    }
}
</script>

<style>
.container {
    margin-top: 40px;
    flex-direction: row;
    align-content: center;
}

.arms {
    margin-top: 8px;
    margin-left: 20px;
    height: 35px;
    width: 30px;
    object-fit: fill;
}

.bar {
    flex: 1;
}
</style>
