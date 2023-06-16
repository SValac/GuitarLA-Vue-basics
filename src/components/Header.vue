<script setup>
import { computed } from 'vue';



const props = defineProps({
    carrito: {
        type: Array,
       required: true, 
    },
    guitarraPrincipal: {
        type: Object,
        required: true,
    }
});

defineEmits([
    'increment',
    'decrement',
    'agregar-carrito',
    'remove-from-cart',
    'empty-cart',
]);


const totalPagar = computed(() => {
     return props.carrito.reduce((total, guitarra) => total + (guitarra.precio * guitarra.cantidad), 0)
});

</script>

<template>
     <header class="py-5 header">
        <div class="container-xl">
            <div class="row justify-content-center justify-content-md-between">
                <div class="col-8 col-md-3">
                    <a href="index.html">
                        <img class="img-fluid" src="/public/img/logo.svg" alt="imagen logo">
                    </a>
                </div>
                <nav class="col-md-6 a mt-5 d-flex align-items-start justify-content-end">
                    <div 
                        class="carrito"
                    >
                        <img class="img-fluid" src="/public/img/carrito.png" alt="imagen carrito" />

                        <div id="carrito" class="bg-white">
                            <p 
                                v-if="carrito.length === 0"
                                class="text-center"
                                >El carrito esta vacio 
                            </p>
                            <div v-else>
                                <table 
                                    class="w-100 table">
                                    <thead>
                                        <tr>
                                            <th>Imagen</th>
                                            <th>Nombre</th>
                                            <th>Precio</th>
                                            <th>Cantidad</th>
                                            <th></th>
                                        </tr>
                                    </thead>
                                    <tbody>
                                        <tr v-for="guitarra in carrito" :key="guitarra.id">
                                            <td>
                                                <img class="img-fluid" :src="`/public/img/${guitarra.imagen}.jpg`" :alt="`imagen guitarra  {{ guitarra.nombre }}`">
                                            </td>
                                            <td>{{guitarra.nombre}}</td>
                                            <td class="fw-bold">
                                                    ${{guitarra.precio}}
                                            </td>
                                            <td class="flex align-items-start gap-4">
                                                <button
                                                    type="button"
                                                    class="btn btn-dark"
                                                    :disabled="guitarra.cantidad === 1"
                                                    @click="$emit('decrement', guitarra)"
                                                >
                                                    -
                                                </button>
                                                    {{ guitarra.cantidad }}
                                                <button
                                                    type="button"
                                                    class="btn btn-dark"
                                                    @click="$emit('increment', guitarra)"
                                                >
                                                    +
                                                </button>
                                            </td>
                                            <td>
                                                <button
                                                    class="btn btn-danger"
                                                    type="button"
                                                    @click="$emit('remove-from-cart', guitarra)"
                                                >
                                                    X
                                                </button>
                                            </td>
                                        </tr>
                                    </tbody>
                                </table>

                                <p class="text-end">Total pagar: <span class="fw-bold">${{totalPagar}}</span></p>
                                <button 
                                    class="btn btn-dark w-100 mt-3 p-2"
                                    @click="$emit('empty-cart')"
                                >Vaciar Carrito
                                </button>
                            </div>
                            
                        </div>
                    </div>
                </nav>
            </div><!--.row-->

            <div class="row mt-5">
                <div class="col-md-6 text-center text-md-start pt-5">
                    <h1 class="display-2 fw-bold">Modelo {{ guitarraPrincipal.nombre }}</h1>
                    <p class="mt-5 fs-5 text-white">{{guitarraPrincipal.descripcion}}</p>
                    <p class="text-primary fs-1 fw-black">${{ guitarraPrincipal.precio }}</p>
                    <button 
                        type="button"
                        class="btn fs-4 bg-primary text-white py-2 px-5"
                        @click="$emit('agregar-carrito', guitarraPrincipal)"
                    >Agregar al Carrito</button>
                </div>
            </div>
        </div>

        <img class="header-guitarra" src="/public/img/header_guitarra.png" alt="imagen header">
    </header>
</template>

<style>

</style>