<script setup>
// Utilizaremos composition API
  import {ref, reactive, onMounted} from 'vue'
  import{db} from './data/guitarras'
  import Guitarra from './components/Guitarra.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'

  // Ref componente Guitarra
  const guitarras = ref([])
  const carrito = ref([])

  onMounted (() =>{
    guitarras.value = db;
  })
  console.log(guitarras.value)

  const numero = ref(0)

  const agregarCarrito = (guitarra)=>{
    guitarra.cantidad = 1; 
    carrito.value.push(guitarra);
  }

    // Ref componente Header

  
</script>

<template>
<Header 
    :carrito="carrito"
/>

<main class="container-xl mt-5">
    <h2 class="text-center">Nuestra Colección</h2>

    <div class="row mt-5">
        
        <Guitarra
            v-for="guitarra in guitarras"
            :key="guitarra.id"
            :guitarra="guitarra" 
            @agregar-carrito="agregarCarrito"
            />

    </div>
</main>

<Footer />

  
</template>

