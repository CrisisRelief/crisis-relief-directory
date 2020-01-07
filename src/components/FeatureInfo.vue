<template lang="pug">
div
    img.image(v-if="imageUrl" :src="imageUrl")
    table#FeatureInfo(v-if="feature").bg-white.b--gray.ba.helvetica.ma1
        tr(v-for="(value, prop) in feature.properties")
            template(v-if="ignoreProps.indexOf(prop) === -1")
                th.f6 {{ prop }}
                td.f6 {{ value }}
</template>

<script>
export default {
    name: "FeatureInfo",
    data: () => ({
        feature: undefined,
        ignoreProps: ['id','Longitude','Latitude', 'image_url']
    }),
    computed: {
        imageUrl() {
            return this.feature && this.feature.properties.image_url
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