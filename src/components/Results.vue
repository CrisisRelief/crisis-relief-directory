<template lang="pug">
#Results(v-show="home")
    h2 Results
    .result(v-for="site in sites")
        h4 {{ site.properties.name }}
        p.gray {{ Math.round(site.distance*10)/10 }} km
</template>

<script>
import cheapRuler from 'cheap-ruler';
import { EventBus } from './EventBus';
export default {
    name: "Results",
    data: () => ({
        sites: undefined,
        home: undefined
        
    }),

    created() {
        window.Results = this;
        EventBus.$on('filter-home', home => {
            this.home = home;
            const ruler = cheapRuler(home[1]);
            for (const site of this.sites) {
                // console.log(ruler.distance(this.home, site.geometry.coordinates));
                site.distance = ruler.distance(home, site.geometry.coordinates)
            }
            this.sites.sort((a, b) => a.distance - b.distance);
            console.log(this.sites);
            // window.Map.zoomToBounds(this.bbox);
        });
        EventBus.$on('sites-loaded', sites => {
            this.sites = sites.features;
            console.log(this.sites);
        });
    },
    computed: {
    },
    watch: {
        home() {
            

        }
    }
}
</script>

<style scoped>

</style>