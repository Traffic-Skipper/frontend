<template>
    <view class="container">
        <image class="icon" :source="require('../../assets/search.png')"/>
        <text-input class="input"
                    enter-key-hint="search"
                    placeholder="E.g. A1 or Gossau"
                    v-model="text"
                    :on-submit-editing="moveRegion"/>
    </view>
</template>

<script>
import storage from "../store";

export default {
    name: "TopBar",
    data: () => {
        return {
            text: ''
        }
    },
    methods: {
        async moveRegion() {
            let response = await fetch(`http://127.0.0.1:5000/sensor/search?query=${this.text}`)

            await storage.save({
                key: 'sensor',
                data: await response.json()
            })
        }
    }
}
</script>

<style>
.container {
    flex-direction: row;
    align-items: center;
    justify-content: flex-start;

    height: 50px;

    margin-left: 20px;
    margin-right: 20px;
    margin-bottom: 20px;
    border-radius: 16px;
    border-style: solid;
    border-color: #e7e7e7;
    border-width: 1px;

    padding-left: 10px;
    padding-right: 10px;
}

.input {
    flex: 1;
    width: 100%;
}

.icon {
    height: 25px;
    width: 25px;
    margin-right: 10px;
}
</style>
