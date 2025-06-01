<script setup lang="ts">
import { ref, onMounted, onUnmounted } from 'vue'
import L from 'leaflet'
import 'leaflet/dist/leaflet.css'

const userName = ref("")
const domaine = "localhost:8020"
// const madagascar = ref({
//     lat:-18.907136,
//     long:47.5168768
// })

let maps: L.Map
const marque = new Map<string, L.Marker>()
let socket: WebSocket | null = null

function startTracking() {
    if (!userName.value) {
        console.log("Nom utilisateur manquant")
        return
    }

    socket = new WebSocket(`ws://${domaine}`)
    console.log("Nom utilisateur :", userName.value)

    socket.onopen = () => {
        console.log('Connecté WebSocket')

        if (navigator.geolocation) {
            navigator.geolocation.watchPosition(
                (position) => {
                    const data = {
                        Nom: userName.value,
                        lat: position.coords.latitude,
                        long: position.coords.longitude
                    }
                    socket?.send(JSON.stringify(data))
                    console.log('Position envoyée')
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

    socket.onmessage = (e) => {
        if (!maps){return;}
        console.log("Reçu de websoket:", e.data)
        const users = JSON.parse(e.data)

        users.forEach((user: { Nom: string, lat: number, long: number }) => {
            if (!marque.has(user.Nom)) {
                const marker = L.marker([user.lat, user.long])
                    .addTo(maps)
                    .bindPopup(user.Nom)
                marque.set(user.Nom, marker)
            } else {
                marque.get(user.Nom)?.setLatLng([user.lat, user.long])
            }
        })
    }
}

onMounted(() => {
    console.log("73: Monté")
    maps = L.map('map').setView([0, 0], 2)
    L.tileLayer('https://{s}.tile.openstreetmap.org/{z}/{x}/{y}.png', {
        attribution: '© OpenStreetMap contributors'
    }).addTo(maps)
})
onUnmounted(() => {
    console.log("Bey geo map")
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
