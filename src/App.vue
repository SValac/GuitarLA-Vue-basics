<script setup>
import { onMounted, ref, watch } from 'vue';
import Header from './components/Header.vue';
import Footer from './components/Footer.vue';
import GuitarraCard from './components/GuitarraCard.vue';
import {db} from './data/guitarras';

const guitarras = ref([])
const carrito = ref([]);
const guitarraPrincipal = ref({})

watch(carrito, () => {
    saveCartLocalStorage()
}, {
    deep: true
});

onMounted(() => {
  guitarras.value = db
  guitarraPrincipal.value = guitarras.value[3]
  const getCartLocalStorage = localStorage.getItem('carrito')

  if (getCartLocalStorage){
    carrito.value = JSON.parse(getCartLocalStorage)
  }
});

const saveCartLocalStorage = () =>{
    localStorage.setItem('carrito', JSON.stringify(carrito.value))
}

const agregarCarrito = (guitarra) => {

    const existeGuitarraCarrito = carrito.value.findIndex( existeGuitarra => existeGuitarra.id === guitarra.id)

    if (existeGuitarraCarrito >= 0) {
        guitarra.cantidad++
        return
    }
    guitarra.cantidad = 1
    carrito.value.push(guitarra)
};


const decrement = (guitarra) => {
    if (guitarra.cantidad > 1){
        guitarra.cantidad--    
    }
};
const increment = (guitarra) => {
    console.log('increment');
    guitarra.cantidad++
};

const removeFromCart = (guitarra) => {
    carrito.value = carrito.value.filter((item) => item.id !== guitarra.id)
};

const emptyCart = () => {
    carrito.value = []
}

</script>

<template> 
    <body>
        <Header 
            :carrito="carrito"
            :guitarraPrincipal="guitarraPrincipal"
            @increment="increment"
            @decrement="decrement"
            @agregar-carrito="agregarCarrito"
            @remove-from-cart="removeFromCart"
            @empty-cart="emptyCart"
        
        />  

        <main class="container-xl mt-5">
            <h2 class="text-center">Nuestra Colección</h2>

            <div class="row mt-5">

                <GuitarraCard 
                    v-for="guitarra in guitarras" 
                    :key="guitarras.id"
                    :guitarra="guitarra"
                    @agregar-carrito="agregarCarrito"
                />
                
            </div>
        </main>

        <Footer />
    </body>
</template>

<style>

</style>