<template lang="pug">
#ResourceFilter
    p Enter your suburb to find your nearest resources
    #geocoder

</template>

<script>
import { EventBus } from './EventBus';
import MapboxGeocoder from '@mapbox/mapbox-gl-geocoder';
import mapboxgl from 'mapbox-gl';
export default {
    name: "ResourceFilter",
    data: () => ({
        
    }),
    created() {
        window.ResourceFilter = this;
    },
    mounted() {
        EventBus.$on('map-init', map => {
            const geocoder = new MapboxGeocoder({
                accessToken: mapboxgl.accessToken,
                mapboxgl: mapboxgl,
                countries: 'au',
                types: 'region,postcode,district,place,locality,neighborhood,address'
            })
            document.getElementById('geocoder')
                .appendChild(geocoder.onAdd(map));
            geocoder.on('result', ({result}) => {
                console.log(result); 
                EventBus.$emit('filter-home', result.geometry.coordinates);
            }).on('clear', () => {
                EventBus.$emit('filter-home', null);

            });
        });
    }
}
</script>

<style scoped>

</style>