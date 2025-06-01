<script setup lang="ts">
import { ref} from 'vue'
import type { Ref } from 'vue'

const isActive: Ref<boolean> =  ref(false)
const classCard = ()=>{
    isActive.value = !isActive.value
}

</script>

<template>
    <div class="card">
      <slot name="contenue"/>
        <button @click="classCard">Activation</button>
    </div>
</template>


<style scoped src="../../assets/style/VboxStyle/carte.css">
</style>


