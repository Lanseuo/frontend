<template>
    <div class="home">
        <vue-headful title="Fridays For Future Regionalgruppen" />

        <l-map class="map" 
            ref="map" 
            :center="center" 
            :zoom=7 
            :options="mapOptions">
        <l-tile-layer :url="url" 
                    :attribution="attribution"/>
        <l-marker @click="clickMarker(demo)" 
                v-for="demo in demos" 
                :icon="demo.icon" 
                :lat-lng="demo.latLng" 
                :key="demo.id">
        <l-popup>
            <MarkerDetails :model="selectedMarker"/>
        </l-popup>
        </l-marker>
        </l-map>

    </div>
</template>

<script>

import L from 'leaflet';
import { LMap, LTileLayer, LMarker, LPopup } from 'vue2-leaflet';

import MarkerDetails from '@/components/MarkerDetails.vue';

export default {
    name: 'home',

    data() {
        return {
            mapOptions: {},
            url: 'https://{s}.tile.osm.org/{z}/{x}/{y}.png',
            attribution: 'Map data © <a href="https://openstreetmap.org">OpenStreetMap</a> contributors',
            center: new L.LatLng(50.9, 10.6),
            selectedMarker: null
        };
    },

    components: {
        LMap,
        LTileLayer,
        LMarker,
        LPopup,
        MarkerDetails
    },

    computed: {
        demos() {
            return this.$store.getters['demos/getItems'];
        }
    },

    methods: {
        clickMarker(demo) {
            this.selectedMarker = demo;
        },
    },

    beforeCreate() {
        this.$store.dispatch('demos/getList');
        this.$store.dispatch('localgroups/getList');
    }
}
</script>

<style>
.leaflet-popup-content {
    width: 250px;
}
</style>


<style scoped>
.home {
    height: calc(100vh - 70px);
    width: 100%;
}
</style>
