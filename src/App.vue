<script setup lang="ts">
import { defineAsyncComponent, type Ref, ref } from 'vue';
import HelloWorld from './components/HelloWorld.vue';
import LoadingComponent from './components/LoadingComponent.vue';
import ErrorComponent from './components/ErrorComponent.vue';

const Home = defineAsyncComponent(() => import('./components/Home.vue'));
/*const Perfil = defineAsyncComponent({
  loader: () => import('./components/Perfil.vue'),
  loadingComponent: LoadingComponent,
  errorComponent: ErrorComponent,
});*/
// aqui se pude jugar un poco que pasa si tarda o si da error
const Perfil = defineAsyncComponent({
  loader: () => {
    return new Promise((resolve, reject) => {
      setTimeout(() => {
        resolve(import('./components/Perfil.vue'));
        reject('error');
      }, 900);
    });
  },
  loadingComponent: LoadingComponent,
  errorComponent: ErrorComponent,
});

const loading: Ref<number> = ref(0);

const increment = () => loading.value++;
</script>

<template>
  <HelloWorld msg="Vite + Vue" />
  <p>cargar componentes</p>
  <button @click="increment">cargar</button>
  <Home v-if="loading > 0" />
  <Perfil v-if="loading > 1" />
</template>

<style scoped></style>
