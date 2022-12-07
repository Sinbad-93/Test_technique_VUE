<script setup lang="ts">
import {ref,computed, onMounted} from 'vue'

var show = ref(false);

// utiliser mounted avec API composition
onMounted(() =>
{
  show.value = true
})

var number = "";
var btnValid = true;

// props du composant parent 
const props = defineProps({
  validator: Boolean
})

//(voir style) propriété dynamique de changement de couleur en fonction de la valeur d'une prop*
const backgroundColor = computed (() => props.validator ? 'greenyellow' : 'initial');

//déclarer l'émetteur
const emit = defineEmits<{
  (e: 'phoneNumber', str: string): void
}>()

function emitNumber(newNumber : string){
    //console.log(newNumber);

    //émettre l'évenement
    emit('phoneNumber',newNumber)
    
    //ajout de la logique du bouton
    btnValid = false;
    };

</script>

<template>
    <div>
    <br>
    <br>
    <Transition name="opa">
      <div v-if="show">
        <span>+</span>
        <input 
    class="design-input"
    type="text"
    placeholder="ex : 33745781256"
    v-model="number">

    <button :disabled="!btnValid" @click="emitNumber(number)">Valider</button>
    
    </div>
  </Transition>
    
  </div>
</template>

<style scoped>
/*propriété dynamique de changement de couleur en fonction de la valeur d'une prop*/ 
input {
  background-color: v-bind(backgroundColor);
}

/* animation opacité d'entrée & sortie pour l'input */
.opa-enter-active {
  animation: opa-in 2s;
}
.opa-leave-active {
  animation: opa-in 2s reverse;
}
@keyframes opa-in {
  0% {
    opacity : 0.1;
  }
  100% {
    opacity : 1;
  }
}
</style>