<script setup>
import MenuLateral from "./components/menuLateral.vue";
import { ref, watch, onMounted } from 'vue';

const notas = ref([]);
const nota_activa = ref(null);
const titulo_input = ref('');
const contenido_input = ref('');

// Cargar datos desde LocalStorage al iniciar
onMounted(() => {
  const notasGuardadas = localStorage.getItem('notas');
  if (notasGuardadas) {
    notas.value = JSON.parse(notasGuardadas);
  }
});

// Guardar datos en LocalStorage cada vez que cambien las notas
watch(notas, (newNotas) => {
  localStorage.setItem('notas', JSON.stringify(newNotas));
}, { deep: true });

// Crear Notas
function crear_nota() {
  const id = Math.random().toString(36).substring(2, 9);
  notas.value.push({
    id,
    titulo: 'Título',
    contenido: 'Descripción'
  });
  colocar_nota_activa(id);
}

//Colocar nota activa
function colocar_nota_activa(id) {
  nota_activa.value = id;

  let nota = notas.value.find(nota => nota.id === id);

  titulo_input.value = nota.titulo;
  contenido_input.value = nota.contenido;

  setTimeout(() => {
    document.querySelector('input[type="text"]').focus();
  }, 0);
}

  //Eliminar Nota
  function eliminar_nota({id, evt}){
    evt.stopPropagation();
    let nota = notas.value.findIndex(nota => nota.id === id);
    notas.value.splice(nota, 1);

    nota_activa.value = null;
    titulo_input.value = '';
    contenido_input.value = '';
  }

  //Actualizar Nota
  function actualizar_nota(){
    let nota = notas.value.findIndex(nota => nota.id === nota_activa.value)

    notas.value[nota].titulo = titulo_input.value;
    notas.value[nota].contenido = contenido_input.value;
  }


</script>


<template>
  <div class="bg-gray-900 text-white min-h-screen flex items-stretch justify-start">
    <MenuLateral 
    :nota_activa="nota_activa" 
    :notas="notas" 
    @nueva-nota="crear_nota" 
    @colocar-nota-activa="colocar_nota_activa"
    @eliminar-nota="eliminar_nota"/>
    <main class="flex-1">
      <div v-if="nota_activa" class="px-4 py-8 flex flex-col h-full">
        <input v-model="titulo_input"
        @input="actualizar_nota" type="text" 
        class="block w-full text-5xl pb-2 font-bold border-b-2
      border-gray-500 focus:border-white outline-none transition-colors duration-200" maxlength="40">
      <textarea v-model="contenido_input" 
        @input="actualizar_nota"
        class="block w-full h-full mt-4 text-3xl outline-none flex-1" maxlength="40">
      </textarea>
    </div>
    </main>
  </div>

</template>
