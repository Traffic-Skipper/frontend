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
    margin-top: 60px;
    flex-direction: row;
    align-content: center;
}

.arms {
    margin-top: 4px;
    margin-left: 20px;
    height: 40px;
    width: 35px;
    object-fit: fill;
}

.bar {
    flex: 1;
}
</style>
