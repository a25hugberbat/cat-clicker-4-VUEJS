<script setup>
import { reactive, ref } from 'vue'
let data = reactive({
    catActive: 0,
    ncats: 4,
    gatos: [
        { name: "Manuel",   image: "resources/cat1.jpg",  nclicks: 0 },
        { name: "Gato Marciano",   image: "resources/cat2.jpg",  nclicks: 0 },
        { name: "Gatendrik Lamer",   image: "resources/cat3.jpg",  nclicks: 0 },
        { name: "Gato Soviético",   image: "resources/cat4.jpg",  nclicks: 0 },
        { name: "Gato Fresa",   image: "resources/cat5.jpg",  nclicks: 0 }
    ]
});

function gatSeleccionat(index){
  console.log(index)
  data.catActive=index;
}

let visible=ref(false)

function toggleAdmin(){
  visible.value = !visible.value
}

function catClick(){
  data.gatos[data.catActive].nclicks++;
}

function renderCat(){
    const gato = data.gatos[data.catActive];
    textContent         = gato.name;
    img.src             = gato.image;
    img.alt             = gato.name;
    clicks.textContent  = gato.nclicks;
}

</script>

<template>
  <h1>Cat-clicker-4-VUEJS</h1>
    <ul>
     <li @click="gatSeleccionat(index)" v-for="actual,index in data.gatos">
      {{ actual.gatos }}
      </li>
  </ul>
  <button @click="toggleAdmin">on/off</button>
  <h1>{{ visible }}</h1>

  <p>
    Visit <a href="https://vuejs.org/" target="_blank" rel="noopener">vuejs.org</a> to read the
    documentation
  </p>
</template>
<style scoped></style>
