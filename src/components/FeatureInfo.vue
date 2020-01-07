<template lang="pug">
div(v-if="p")
    h1 {{ p.name }}
    p.gray {{ p.helpCategory }}

    img.image(v-if="imageUrl" :src="imageUrl")
    table#FeatureInfo(v-if="feature").bg-white.b--gray.ba.helvetica.ma1
        tr
            th.pa2 Location
            td.pa2 {{ p.location }}
        tr
            th.pa2 Phone
            td.pa2 {{ p.contactNumber }}
        tr
            th.pa2 Address
            td.pa2(v-html="address")
        tr
            th.pa2 Type
            td.pa2 {{ p.assistanceType }}
</template>

<script>
import { EventBus } from './EventBus';
export default {
    name: "FeatureInfo",
    data: () => ({
        feature: undefined,
        ignoreProps: ['id','Longitude','Latitude', 'image_url']
    }),
    computed: {
        imageUrl() {
            return this.feature && this.feature.properties.image_url
        },
        address() {
            return this.p && this.p.contactAddress.replace(/, /, '<br/>') 
        },
        p() {
            return this.feature && this.feature.properties;
        }
    },
    created() {
        window.FeatureInfo = this;
    }
}
</script>

<style scoped>
#FeatureInfo th {
    text-align:  right;
}

.image {
    width: 100%;
}

</style>