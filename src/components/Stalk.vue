<script setup lang="ts">
    import {ref, computed, Ref, onMounted} from "vue"
    import L from 'leaflet'
    import 'leaflet/dist/leaflet.css'
    

    const userName: Ref<string> = ref("")
    const Myposition: Ref<{lat: number, long: number}> = ref({lat:0, long:0})

    //POUR LEAFLET
    let map: L.Map
    function latLngToTile(lat, lng, zoom) {
        const x = Math.floor((lng + 180) / 360 * Math.pow(2, zoom));
        const y = Math.floor(
            (1 - Math.log(Math.tan(lat * Math.PI / 180) + 1 / Math.cos(lat * Math.PI / 180)) / Math.PI) / 2 * Math.pow(2, zoom)
        );
        return { x, y };
    }


    let local = () => {
        if(navigator.geolocation){
            navigator.geolocation.getCurrentPosition(success =>{
                Myposition.value = {
                    lat: success.coords.latitude,
                    long: success.coords.longitude
                }
            return true
            }, err=>{
                return false
            })
        }
        else return console.log("Erreur de geolocalisation")
    }
    const startTracking = () => {
        local()
        L.marker([Myposition.value.lat, Myposition.value.long]).addTo(map)
                .bindPopup(userName.value)
                .openPopup();
    }
    onMounted(() => {
    map = L.map('map').setView([-21.45518812597302, 7.096639284080204], 16)

    L.tileLayer('/Fian_xyz/{z}/{x}/{y}.png', {
        minZoom: 2,
        maxZoom: 16,
        attribution: 'Tuiles locales Fianarantsoa'
    }).addTo(map)
})

</script>

<template>
    <form @submit.prevent="startTracking">
        <div class="champ">
            <input v-model="userName" class="input-champ" placeholder="Entrez votre nom" required />
            <button type="submit">Commencer</button>
        </div>
    </form>
    <div class="composant">
        <div id="map"></div>
    </div>
</template>

<style scoped >
    #map{
        width: 80%;
        height: 40vh;
    }
    .composant {
        display: flex;
        align-items: center;
        flex-direction: column;
    }
</style>