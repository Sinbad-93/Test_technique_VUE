<script setup lang="ts">
import { ref } from 'vue';
import NumberInput from '@/components/NumberInput.vue'

// à modifier quand le nombre d'appel est depassé
const apikey = '5xIcIgWEbXL6rYucqg0GcD4FRDehbCGa'

var validator = ref(false);
var countryName = ref('Inconnu');

// appel fonction asynchrone API
function numberAPI(n : string){
  //console.log(n);
  fetchNumberApi(n)
        .then((data) => {
            //console.log(data);

            if(data.valid){
            console.log('valid');
            validator.value = true;
            countryName.value  = data.country_name; 
            }

            else {
              console.log('invalid');
            countryName.value  = "";
            validator.value  = false;
            return
            }
            
          if (!data) {
            alert("la requete a échoué");
          }
        })
        .catch((e) => console.log(e));
}

 //GET API externe NumVerify
 async function fetchNumberApi(number : string) {
        //chemin API externe NumVerify
      let path = `https://api.apilayer.com/number_verification/validate?number=`;

      let response = await fetch(path+ number, {
        method: "GET",
        headers: {
          //required auth key, 100 call month
          apikey: '5xIcIgWEbXL6rYucqg0GcD4FRDehbCGa',
        },
      });
    
      if (!response.ok) {
        // get error message from body or default to response status
        const error = response.status;
        //console.log('not response ok, error : ' + error);
        alert("une erreur innattendue s'est produite");
        return Promise.reject(error);
      }
      return await response.json();
    }
</script>

<template>
<div>
<h1>Test technique / SpinforEat</h1>

    <div>

      <NumberInput 
      :validator="validator"
      @phoneNumber="(n) => {numberAPI(n)}"></NumberInput>
      {{validator}}
</div>

<div>
    Validité du numéro : 
    <span v-if="validator" style="color : green">Valide</span>
    <span v-else style="color : red">Invalide</span>
  </div>

  <div>
        Pays du numéro : {{countryName }}
    </div>
  </div>
</template>

<style scoped>

</style>