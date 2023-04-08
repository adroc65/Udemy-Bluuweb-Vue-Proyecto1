<script setup>
  import { ref, computed } from "vue";
  const name = "Vue dinámico";
  const counter = ref(0);
  const arrayFavoritos = ref([]);

  const increment = () => {
    counter.value++;
  };
  const decrement = () => {
    counter.value--;
  };
  const reset = () => {
    counter.value = 0;
  };
  const add = () => {
    const date = Date.now()
    arrayFavoritos.value.push({ name: counter.value, fecha: date });
  };

  // Bloquear boton de ADD
  const bloquearBtnAdd = computed(() => {
    const numSearch = arrayFavoritos.value.find((num) => num === counter.value);
    if (numSearch === 0) return true;
    return numSearch ? true : false;
    // return numSearch || numSearch === 0
  });

  // Otra forma de bloquear el boton ADD
  const isRepeated1 = computed(() =>{
    return arrayFavoritos.value.includes(counter.value)
  });

  // Quitando la palabra return, hacer mas pequena la exprecion. Para arreglos unidemincionales.
  const isRepeated2 = computed(() => arrayFavoritos.value.includes(counter.value));
  
  // Se busca el numero de contador en el arreglo de objetos.
  const desabilitar = computed(()=> arrayFavoritos.value.find(item => item.name === counter.value));

  const classCounter = computed(() => {
    if (counter.value === 0) {
        return "zero";
    }
    if (counter.value > 0) {
        return "positive";
    }
    if (counter.value < 0) {
        return "negative";
    }
  });
</script>

<template>
  <div class="container text-center mt-3">
    <h1>Hola {{ name.toUpperCase() }}</h1>
    <h2 :class="classCounter">{{ counter }}</h2>
    <div class="btn-group">
      <button @click="increment" class="btn btn-success">Increment</button>
      <button @click="decrement" class="btn btn-danger">Decrement</button>
      <button @click="reset" class="btn btn-secondary">Reset</button>
      <button
        @click="add"
        :disabled="desabilitar"
        class="btn btn-primary"
      >
        Add
      </button>
    </div>
  </div>
  <div class="container">
    <ul class="list-group mt-4">
      <li
          class="list-group-item"
          v-for="(item, index) in arrayFavoritos"
          :key="index"
      >
        <div class="container text-center">
          <div class="row">
            <div class="col">
              {{ item.name }}
            </div>
            <div class="col">
              {{ item.fecha }}
            </div>
          </div>
        </div>
      </li>
    </ul>
  </div>
  
  <div class="container">
    <table class="table table-dark table-striped mt-4">
      <thead>
        <tr>
          <th scope="col">#</th>
          <th scope="col">Marcador</th>
          <th scope="col">Fecha</th>
        </tr>
      </thead>
      <tbody>
        <tr
          v-for="(item, index) in arrayFavoritos"
          :key="index"
        >
        <th scope="row">{{ index }}</th>
        <td>{{ item.name }}</td>
        <td>{{ item.fecha }}</td>
        </tr>
      </tbody>
    </table>
  </div>
  
</template>

<style scoped>
  .positive {
      color: rgb(29, 216, 29);
  }
  .negative {
      color: red;
  }
  .zero {
      color: peru;
  } 
</style>
