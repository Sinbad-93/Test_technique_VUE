<template>
<div>
      <h1>Test technique / SpinforEat</h1>

  <NumberInput
  @phoneNumber="(n) => {this.numberAPI(n)}">
  </NumberInput>
  
  <div>
    Validité du numéro : 
    <span v-if="validator" style="color : green">Valide</span>
    <span v-else style="color : red">Invalide</span>
  </div>

  <div>
        Pays du numéro : {{countryName}}
    </div>
  </div>

</template>

<script>
import NumberInput from '../components/NumberInput.vue'

export default {
     name : "MainPage",
    components : {NumberInput},
    data() {
       
        return {
        countryName : 'Inconnu',
        validator : false,
        // à modifier quand le nombre d'appel est depassé
        apikey : "5xIcIgWEbXL6rYucqg0GcD4FRDehbCGa"
        }
    },
    
    methods: {
      // appel fonction asynchrone API
      numberAPI(number) {

      this.fetchNumberApi(number)
        .then((data) => {
            //console.log(data);

            if(data.valid){
            this.validator = true;
            this.countryName = data.country_name; 
            }

            else {
            this.countryName = "";
            this.validator = false;
            return
            }
            
          if (!data) {
            alert("la requete a échoué");
          }
        })
        .catch((e) => console.log(e));
    },
    //GET API externe NumVerify
    async fetchNumberApi(number) {
        //chemin API externe NumVerify
      let path = `https://api.apilayer.com/number_verification/validate?number=`;

      let response = await fetch(path+ number, {
        method: "GET",
        headers: {
          //required auth key, 100 call month
          apikey: this.apikey,
        },
      });
    
      if (!response.ok) {
        // get error message from body or default to response status
        const error = (data && data.message) || response.status;
        //console.log('not response ok, error : ' + error);
        alert("une erreur innattendue s'est produite");
        return Promise.reject(error);
      }
      return await response.json();
    },
    },

}
</script>

<style>

</style>