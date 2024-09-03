<template>
    <div id="app">
            
    </div>
</template>
  
<script setup>
import { useRouter } from 'vue-router';
import { onMounted } from 'vue';

const router = useRouter();

onMounted(() => {
  router.replace('/home');
});
</script>