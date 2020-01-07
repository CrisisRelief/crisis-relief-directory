<template lang="pug">
    #app.flex.flex-column.vh-100.avenir
        #top.bb.b--gray.bg-washed-yellow.h3
            h1 FireMap work in progress
        #middle.flex.flex-auto
            #sidebar.br.b--light-gray.overflow-auto.w3.w-third-ns
                FeatureInfo
                ResourceFilter
                Results
                //- Sites

            #map-container.relative.flex-auto
                Map
                #overlay.absolute
        #bottom.bt.b--light-gray.flex-none.h2
</template>

<script>
import Map from './components/Map.vue'
import FeatureInfo from './components/FeatureInfo.vue'
import ResourceFilter from './components/ResourceFilter.vue'
import Results from './components/Results.vue'
import { EventBus } from './components/EventBus';

window.firemap = {
    data: {}
}


export default {
    name: 'app',
    components: {
      Map,
      FeatureInfo,
      ResourceFilter,
      Results
    }
}

require('tachyons/css/tachyons.min.css');
</script>

<style>
html, body {
  height: 100vh;
  width: 100%;
  margin:0;
  padding:0;
}

</style>
