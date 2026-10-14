<script setup>
import { reactive, ref } from 'vue'

// 1. Importamos las imágenes para que Vite sepa encontrarlas
import cat1 from './resources/cat1.jpg'
import cat2 from './resources/cat2.jpg'
import cat3 from './resources/cat3.jpg'
import cat4 from './resources/cat4.jpg'
import cat5 from './resources/cat5.jpg'

// 2. Los datos de la aplicación (reactive = si cambian, la pantalla se actualiza)
const data = reactive({
  catActive: 0,
  gatos: [
    { name: 'Manuel',          image: cat1, nclicks: 0 },
    { name: 'Gato Marciano',   image: cat2, nclicks: 0 },
    { name: 'Gatendrik Lamer', image: cat3, nclicks: 0 },
    { name: 'Gato Soviético',  image: cat4, nclicks: 0 },
    { name: 'Gato Fresa',      image: cat5, nclicks: 0 }
  ]
})

// 3. ¿Se ve el área de admin? Empieza oculta
const visible = ref(false)

// 4. Copia temporal de los datos que se editan en el formulario
const form = reactive({ name: '', image: '', nclicks: 0 })

// Copia los datos del gato seleccionado dentro del formulario
function rellenarForm() {
  const gato = data.gatos[data.catActive]
  form.name = gato.name
  form.image = gato.image
  form.nclicks = gato.nclicks
}

// Click en un nombre de la lista
function gatSeleccionat(index) {
  data.catActive = index
  if (visible.value) rellenarForm()
}

// Click en la foto del gato
function catClick() {
  data.gatos[data.catActive].nclicks++
}

// Botón Admin: muestra el formulario ya relleno
function openAdmin() {
  rellenarForm()
  visible.value = true
}

// Botón Cancel: se cierra sin guardar nada
function cancelAdmin() {
  visible.value = false
}

// Botón Save: pasa los valores del formulario al gato y se cierra
function saveAdmin() {
  const gato = data.gatos[data.catActive]
  gato.name = form.name
  gato.image = form.image
  gato.nclicks = Number(form.nclicks)
  visible.value = false
}
</script>

<template>
  <h1>Cat Clicker Premium Pro</h1>

  <div class="layout">
    <!-- LISTA DE GATOS -->
    <ul class="lista">
      <li
        v-for="(actual, index) in data.gatos"
        :key="index"
        :class="{ activo: index === data.catActive }"
        @click="gatSeleccionat(index)"
      >
        {{ actual.name }}
      </li>
    </ul>

    <!-- ÁREA DEL GATO SELECCIONADO -->
    <div class="gato">
      <h2>{{ data.gatos[data.catActive].name }}</h2>
      <img
        :src="data.gatos[data.catActive].image"
        :alt="data.gatos[data.catActive].name"
        @click="catClick"
      />
      <p>Clicks: {{ data.gatos[data.catActive].nclicks }}</p>

      <button @click="openAdmin">Admin</button>

      <!-- ÁREA ADMIN (oculta por defecto) -->
      <div v-if="visible" class="admin">
        <label>
          Nombre
          <input v-model="form.name" type="text" />
        </label>
        <label>
          URL de la imagen
          <input v-model="form.image" type="text" />
        </label>
        <label>
          Clicks
          <input v-model="form.nclicks" type="number" min="0" />
        </label>
        <div class="botones">
          <button @click="cancelAdmin">Cancel</button>
          <button @click="saveAdmin">Save</button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.layout {
  display: flex;
  gap: 2rem;
  font-family: sans-serif;
}
.lista {
  list-style: none;
  padding: 0;
  min-width: 160px;
}
.lista li {
  padding: 0.5rem;
  cursor: pointer;
  border-radius: 6px;
}
.lista li:hover {
  background: #eee;
}
.lista li.activo {
  background: #333;
  color: white;
}
.gato img {
  width: 300px;
  cursor: pointer;
  border-radius: 8px;
}
.admin {
  margin-top: 1rem;
  padding: 1rem;
  border: 1px solid #ccc;
  border-radius: 8px;
  display: flex;
  flex-direction: column;
  gap: 0.5rem;
  max-width: 300px;
}
.admin label {
  display: flex;
  flex-direction: column;
}
.botones {
  display: flex;
  gap: 0.5rem;
}
</style>