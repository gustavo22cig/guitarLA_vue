<script setup>
// Utilizaremos composition API
  import {ref, reactive, onMounted, watch} from 'vue'
  import{db} from './data/guitarras'
  import Guitarra from './components/Guitarra.vue'
  import Header from './components/Header.vue'
  import Footer from './components/Footer.vue'

  // Ref componente Guitarra
  const guitarras = ref([])
  const carrito = ref([])
  const guitarraPromo = ref({})

  watch(carrito, () => {
    // REvisa los cambios en el carrito
    guardarLocalStorage()
  },{
    // revisa cambio uno a uno los elementos
    deep: true
  })

  onMounted (() =>{
    guitarras.value = db;
    guitarraPromo.value = db[3]

    const carritoStorage = localStorage.getItem('carrito')
    if(carritoStorage){
        carrito.value = JSON.parse(carritoStorage)
    }
  })
//   console.log(guitarras.value)

  const numero = ref(0)

  const agregarCarrito = (guitarra)=>{
    const existeCarrito = carrito.value.findIndex(producto => producto.id === guitarra.id)
    if (existeCarrito >= 0) {
        carrito.value[existeCarrito].cantidad++;
    }else{
        guitarra.cantidad = 1; 
        carrito.value.push(guitarra);
    }
    guardarLocalStorage()
  }

  const incrementarCantidad = (id) =>{
    // retorna la poscion del carrito
    const index = carrito.value.findIndex(producto => producto.id === id)
    if(carrito.value[index].cantidad >= 5) return;
    carrito.value[index].cantidad++;
  }
  const decrementarCantidad = (id ) =>{
    const index = carrito.value.findIndex(producto => producto.id === id)
    if(carrito.value[index].cantidad <= 1) return;
    carrito.value[index].cantidad--
  }
  const eliminarProducto = (id) => {
    carrito.value = carrito.value.filter(producto => producto.id !== id)
  }
  const vaciarCarrito = () => {
    carrito.value = []
  }
  const guardarLocalStorage = () => {
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
  }
  
</script>

<template>
<Header 
    :carrito="carrito"
    :guitarraPromo = "guitarraPromo"
    @incrementar-cantidad = "incrementarCantidad"
    @decrementar-cantidad = "decrementarCantidad"
    @agregar-carrito = "agregarCarrito"
    @eliminar-producto = "eliminarProducto"
    @vaciar-carrito = "vaciarCarrito"
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

