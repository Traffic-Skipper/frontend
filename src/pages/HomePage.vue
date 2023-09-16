<template>
    <view class="container">
        <top-bar class="bar"/>
        <map-view v-if="state"
                  class="map"
                  :region="area"
                  :shows-user-location="true"
                  :shows-my-location-button="false"/>
    </view>
</template>

<script>
import TopBar from "../components/TopBar.vue";
import MapView from "react-native-maps";
import * as Location from "expo-location";
import * as Permissions from "expo-permissions";

export default {
    data: function() {
        return {
            state: false,
            area: {
                latitude: 0.0,
                longitude: 0.0,
                latitudeDelta: 0.0922,
                longitudeDelta: 0.0421
            }
        };
    },
    components: {
        TopBar,
        MapView,
    },
    mounted() {
        Permissions.askAsync(Permissions.LOCATION_FOREGROUND)
            .then(status => {
                if (status.granted) {
                    Location.getCurrentPositionAsync({}).then(location => {
                        this.area.latitude = location.coords.latitude
                        this.area.longitude = location.coords.longitude
                        this.state = true
                    });
                }
            })
            .catch(err => {
                console.log(err);
            });
    }
};
</script>

<style>
.container,
.map {
    flex: 1;
    position: relative;
    align-content: center;
    justify-content: center;
}

.bar {
    position: relative;
}
</style>
