<template>        
    <div id="map" />
</template>

<script>
    import mapboxgl from "mapbox-gl";
    import "mapbox-gl/dist/mapbox-gl.css";
    import { onMounted } from "vue";

    export default {
        name: "Map",
        props: ['matches'],
        
        setup(props) {
            onMounted(() => {
                var features = [];
                if (props.matches != undefined) {
                    props.matches.map(function (d) {
                        features.push(d);
                    })
                    console.log(features)
                }
                //NOT SECURE: for the sake of this project, I will encode the Mapbox token into the code for now. -JAB
                mapboxgl.accessToken = "pk.eyJ1IjoiamJ1aTk1IiwiYSI6ImNrbXRpd2g5NTBzZHgydW80dHJpc212cmsifQ.pNmYBGcBxaAw-f4B3YOBKQ";
                const map = new mapboxgl.Map({
                    container: "map",
                    style: "mapbox://styles/mapbox/streets-v11",
                    center: [-95.5, 40],
                    zoom: 4
                });
                if (features) {
                    features.forEach(function(marker) {

                    // create a HTML element for each feature
                    var el = document.createElement('div');
                    el.className = 'marker';

                    // make a marker for each feature and add to the map
                    new mapboxgl.Marker(el)
                        .setLngLat(marker.geometry.coordinates)
                        .setPopup(
                            new mapboxgl.Popup({ offset: 25 }) // add popups
                                .setHTML(
                                    '<h3>' +
                                    marker.properties.name +
                                    '</h3><p>' +
                                    marker.properties.address_1 + " " + marker.properties.address_2 +
                                    '</p><p>' +
                                    marker.properties.city + ", " + marker.properties.state + ", " + marker.properties.zipcode + 
                                    '</p>'
                                )
                        )
                        .addTo(map);
                    });
                }
                map.addControl(new mapboxgl.NavigationControl(), 'top-right'); 
            });
        },
    };
</script>

<style>
    #map {
        height: 70vh;
        width: 65vw;
    }
    .marker {
        background-image: url('../assets/marker-icon.svg');
        background-size: cover;
        width: 50px;
        height: 50px;
        border-radius: 50%;
        cursor: pointer;
    }
    @media only screen and (max-width: 600px) {
        #map {
            width: 100%;
        }
    }
</style>