<script setup lang="ts">
import { ref, onMounted } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

const userName = ref("")
// const madagascar = ref({
//     lat:-18.907136,
//     long:47.5168768
// })

let maps: L.Map

function startTracking() {
    if (!userName.value) {
        alert("Nom utilisateur manquant")
        return
    }
        if (navigator.geolocation) {
            navigator.geolocation.watchPosition(
                (position) => {
                    const data = {
                        Nom: userName.value,
                        lat: position.coords.latitude,
                        long: position.coords.longitude
                    }
                    L.marker([data.lat, data.long]).addTo(maps)
                        .bindPopup(data.Nom)
                        .openPopup();
                },
                (err) => {
                    alert(`Erreur de Localisation: ${err}`)
                },
                {
                    enableHighAccuracy: true,
                    timeout: 1000,
                    maximumAge: 0
                }
            )
        } else {
            alert("Navigateur incompatible avec la géolocalisation")
        }
    
       
}

onMounted(() => {
    maps = L.map('map').setView([0, 0], 2)
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap contributors'
    }).addTo(maps)
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

<style scoped>
#map {
    width: 80%;
    height: 60vh;
}
.composant {
    display: flex;
    align-items: center;
    flex-direction: column;
}
</style>
