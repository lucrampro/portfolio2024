<template>
	<div>
	  <h1>Projet : {{ projet }}</h1>
    <Title />
	  <!-- Ajoutez ici le contenu spécifique au projet -->
	</div>
  </template>
  
  <script setup>
  import { useRoute } from 'vue-router';
  
  const route = useRoute();
  const projet = route.params.projet;
  </script>
  