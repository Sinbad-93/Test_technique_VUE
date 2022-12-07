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

<script>
export default {
data() {
    return {
        show : false,
        number : "",
        btnValid : false,
    }
},
mounted() {

  // permet la transition vue d'apparition
    this.show = true;
},
// surveiller dynamiquement le numéro
    watch: {
    number(newNumber, oldNumber) {
      if (newNumber) {
        // enlever la propagation direct car seulement 100 appel API
        /*console.log("new");
        this.emitNumber(newNumber);*/

        // ajout d'un bouton de validation
        this.btnValid = true;
      }
    }
  },
  methods: {
    emitNumber(newNumber){
      
      console.log(newNumber);
this.$emit('phoneNumber', newNumber);

//ajout de la logique du bouton
this.btnValid = false;
    }
}

}
</script>

<style>
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