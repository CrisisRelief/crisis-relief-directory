<template lang="pug">
#map.absolute.absolute--fill
</template>

<script>
import mapboxgl from 'mapbox-gl';
import U from 'mapbox-gl-utils';
import { sheets2geojson } from 'sheets2geojson';
import boundingBox from 'geojson-bounding-box';
import MapboxGeocoder from '@mapbox/mapbox-gl-geocoder';
import { EventBus } from './EventBus';

const d3 = require('d3-fetch');

function pointsToGeoJSON(points) {
    return  {
        type: 'FeatureCollection',
        features: points.filter(p => p.latitude && p.longitude)
        .map(p => ({
            type: 'Feature',
            geometry: {
                type: 'Point',
                coordinates: [+p.longitude, +p.latitude],
            },
            properties: {
                ...p
            }
        }))
    };
}

mapboxgl.accessToken = 'pk.eyJ1Ijoic3RldmFnZSIsImEiOiJjazNmNGV5enAwMTF1M2tuejhtc2twcXo5In0.mLPrYIYJ2FiFZ3KMqVIj6w';
export default {
    async mounted() {
        // replace this Mapbox access token with your own
        const map = new mapboxgl.Map({
            container: 'map',
            center: [150, -35],
            zoom: 10,
            style: 'mapbox://styles/stevage/ck53afte706y91cqqk4rcv8k8/draft',
        });
        U.init(map, mapboxgl);
        window.map = map;
        window.MapVue = this;



    // const sheetNo = 1;
        // I don't know in which context json1 works vs json2. 
        // https://medium.com/dali-lab/google-sheets-and-json-easy-backend-e29e9ef3df2
        // const json1 = `https://spreadsheets.google.com/feeds/cells/${sheetsId}/${sheetNo}/public/full?alt=json`
        // const json2 = `https://spreadsheets.google.com/feeds/cells/${sheetsId}/${sheetNo}/od6/public/values?alt=json`
        // const sheetID = (window.location.hash.match(/sheet=([a-zA-Z0-9_-]+)/) || [])[1] || '2PACX-1vR9qYMju-qqH_IL8e2ksN0wpVXfHBUEKF079WX1eSAgPFRG5z0RAmpjVwS8sVrZSC0fVrNpSMjaB5Cu';
        // const points = await sheets2geojson(sheetID);

        const cors = 'https://cors-anywhere.herokuapp.com/';
        const points = await d3.json(cors + 'https://k0as85pt27.execute-api.ap-southeast-2.amazonaws.com/dev/items')
        window.firemap.data.points = points;
        const pointsGeoJSON = pointsToGeoJSON(points);
        window.firemap.data.pointsGeoJSON = pointsGeoJSON;

        // console.log(pointsGeoJSON);
        map.U.onLoad(() => {
            EventBus.$emit('sites-loaded', pointsGeoJSON);
            EventBus.$emit('map-init', map)


            map.U.addGeoJSON('points', pointsGeoJSON);
            // map.U.addCircle('points-circles', 'points', {
            //     circleColor: 'hsl(330,100%,40%)',
            //     circleRadius: { stops: [[10,3], [12, 10]] }
            // });
            // map.U.hoverPointer('points-circles');
            map.U.addSymbol('points-icons', 'points', {
                textFont: ['Font Awesome 5 Pro Light'],
                textColor: 'blue',
                textField: nameToSymbolExpression(),
                textSize: 30,
                textAllowOverlap: true
            });
            map.U.hoverPointer('points-circles');
            map.on('click', 'points-circles', e => {
                console.log(e);
                window.FeatureInfo.feature = e.features[0];
            });
            map.fitBounds(boundingBox(pointsGeoJSON));
        });
        
    }
}

function nameToSymbolExpression() {
    const faIcons = require('./fa-unicode.json');
    const unicode  = icon => String.fromCharCode(Number(`0x${faIcons[icon]}`));
    console.log(faIcons);

    
    // const icons = {
    //     'Evacuation Centre': 'bed'
    // }
    const icons = {
        'General assistance/info': 'question-circle',
        'Money/other donations': 'usd-circle',
        'Accommodation/evac centres': 'home-heart',
        'Pets/animals':'paw-alt',
        'Supplies':'box-heart',
        'Local community groups':'people-carry',
        // 'All Australian Hospitals':
    }
    
    // const expr = ['match', ['get', 'helpCategory']];
    const expr = ['match', ['get', 'assistanceType']];

    Object.keys(icons).forEach(i => expr.push(i, unicode(icons[i])));
    console.log(expr);
    return [...expr, ''];
    
}


import 'mapbox-gl/dist/mapbox-gl.css';
import '@mapbox/mapbox-gl-geocoder/dist/mapbox-gl-geocoder.css';


</script>

<style scoped>
</style>