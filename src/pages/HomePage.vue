<template>
    <view class="container">
        <top-bar class="bar"/>
        <map-view v-if="state"
                  class="map"
                  :region="area"
                  :shows-user-location="true"
                  :shows-my-location-button="false"
                  :provider="google">
            <heatmap :points="dataPoints"
                     :radius="50"/>
        </map-view>
    </view>
</template>

<script>
import storage from "../store";
import TopBar from "../components/TopBar.vue";
import MapView, {Heatmap, PROVIDER_GOOGLE} from "react-native-maps";
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
            },
            dataPoints: [],
            google: PROVIDER_GOOGLE,
            sensor: ''
        };
    },
    components: {
        TopBar,
        MapView,
        Heatmap
    },
    async mounted() {
        Permissions.askAsync(Permissions.LOCATION_FOREGROUND)
            .then(status => {
                if (status.granted) {
                    Location.getCurrentPositionAsync({}).then(async location => {
                        this.area.latitude = location.coords.latitude
                        this.area.longitude = location.coords.longitude
                        await this.getData()
                        this.state = true
                    });
                }
            })
            .catch(err => {
                console.log(err)
            });

        this.sensor = await storage.load({
            key: 'sensor'
        })
    },
    watch: {
        async sensor(newValue) {
            console.log(newValue)
            this.sensor = newValue
        }
    },
    methods: {
        async getData() {
            let response = await fetch('http://127.0.0.1:5000/historical-traffic-data/search?date=2023-06-22T07:34:00')
            let data = await response.json()

            this.dataPoints = data['results'].map((el) => {
                return {
                    latitude: el.latitude,
                    longitude: el.longitude,
                    weight: el['car_flow'] || el['lorry_flow'] || el['any_flow'] || '0.0'
                }
            })

            this.state = true
        }
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
