<script setup lang="ts">
import { ref } from 'vue'
import EmployeVue from './Employe.vue'
import LoginForm from './LoginForm.vue'
import Carte from './Vbox/Carte.vue'
import Superviseur from './Superviseur.vue'


const count = ref(0)
</script>

<template>

  <div>
    <Superviseur/>
  </div>

</template>

<style scoped>
</style>
