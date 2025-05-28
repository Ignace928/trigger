<script setup lang="ts">
import { ref } from 'vue'
import LoginForm from './LoginForm.vue';

const count = ref(0)
</script>

<template>
  <div>
    <LoginForm></LoginForm>
    <div class="card">
      <button type="button" @click="count++">count is {{ count }}</button>
    </div>
  </div>

</template>

<style scoped>
.read-the-docs {
  color: #888;
}
</style>
