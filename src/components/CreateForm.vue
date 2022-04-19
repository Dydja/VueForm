<template>
<link href="https://cdn.jsdelivr.net/npm/bootstrap@5.0.2/dist/css/bootstrap.min.css" rel="stylesheet" integrity="sha384-EVSTQN3/azprG1Anm3QDgpJLIm9Nao0Yz1ztcQTwFspd3yD65VohhpuuCOmLASjC" crossorigin="anonymous">
  <h1 id="title">Reservation billet d'avion</h1>
    <!-- 
      - Notre formulaire a traité en vuejs
      - v-model permet de lier la valeur de l'input a celle de la variable au niveau de vue.
      - @submit.prevent permet d soumettre le formulaire en appeelant une methode vue.
    --> 
      <div class="card">
        <div class="card-header">
           Reservation de billet
        </div>
         <!-- {{users}} ticket c'est une variable que j'affecte -->
         <!--{{users}}-->
        <div class="card-body">
          <blockquote class="blockquote mb-0">
            <p>A well-known quote, contained in a blockquote element.</p>
            <footer class="blockquote-footer">Someone famous in <cite title="Source Title">Source Title</cite></footer>
          </blockquote>
           
           <!-- le formaulaire -->
            <form method="post" class="row g-3" @submit.prevent="registerTickets">
           
                <div class="col-md-6">
                  <label for="voyage" class="form-label">Nom</label>
                  <input type="text" name="voyage"  v-model="ticket.voyage" class="form-control" id="voyage" placeholder="Aller-retour">
                </div>
                <div class="col-md-6">
                  <label for="inputcabine" class="form-label">Cabine</label>
                  <input type="text" name="cabine" class="form-control"  v-model="ticket.cabine" id="inputcabine" placeholder="Economy">
                </div>
                <div class="col-12">
                  <label for="passagers" class="form-label">Nombre de passagers</label>
                  <input type="text" class="form-control"  v-model="ticket.passagers" id="passagers" placeholder="1-adulte">
                </div>
                <div class="col-6">
                  <label for="departDe" class="form-label">Depart de</label>
                  <input type="text" class="form-control"  v-model="ticket.departDe" name="departDe" id="departDe" placeholder="ex:Bamako">
                </div>
                <div class="col-md-6">
                  <label for="inputCity" class="form-label">date du voyage</label>
                  <input type="date"  name="dateVoyage" class="form-control"  v-model="ticket.dateVoyage" id="dateVoyage">
                </div>
               
             
                <div class="col-md-6">
                  <label for="inputZip" class="form-label">Clients</label>
                  <select id="users" name="user_id" v-model="ticket.user_id" class="form-select"> <!-- De relancer encore -->
                    <option selected>Selectionner le client</option>
                    <option>...</option>
                    <option v-for=" customers in users " :key="customers.id" :value="customers.id">{{ customers.lastname }}</option>
                  </select>
                </div>

                
                <div class="col-md-6">
                  <label for="inputZip" class="form-label">Destination</label>
                  <select id="destination" name="destination_id" v-model="ticket.destination_id" class="form-select">
                    <option selected>Selectionner la destination</option>
                    <option>...</option>
                    <option  v-for="items in destinations " :key="items.id" :value="items.id">{{ items.name }}</option>
                  </select>
                </div>
                
                <div class="col-12">
                  <button type="submit" class="btn btn-primary">Envoyer</button>
                </div>

                
            </form>
        </div>
      </div>

      
      
        <!-- <div v-for="items in destinations " :key="items.id"> 
            <li> {{ items.name }}</li>
      </div> -->
 
</template>

<script>
// axios librairie pour faire les requetes HTTP
import axios from "axios";
export default {
  name: "App",
  components: {},
//Declaration des variables(data)
  data() {
    return {
       users: {
         lastname:""
       },
       ticket: {
         voyage:"",
         cabine:"",
         passagers:"",
         departDe:"", 
         dateVoyage:"",
         user_id :"",
         destination_id:"",
       },
      destinations:{
        name: "",
      },
      success: "",
    };
  },
  
  //Affiche des data with api Methode appelée quant le composant est chargé(page)
  mounted() {
     axios
      .get("http://localhost:8001/api/destinations/")
      .then((response) => {
        this.destinations = response.data;
      })
      .catch((error) => {
        console.log(error);
      });

    
      axios
      .get("http://127.0.0.1:8000/api/users/")
      .then((response) => {
        this.users = response.data;
      })
      .catch((error) => {
        console.log(error);
      });
  },

  //Enregistrement des data //Method pour faire les appeles axios en POST pour sauvegarder des données
  methods: {
    async registerTickets() {
     await axios
        .post("http://127.0.0.1:8000/api/tickets/", this.ticket)
        .then((response) => {
          this.ticket.voyage ="";
          this.ticket.cabine ="";
          this.ticket.passagers ="";
          this.ticket.departDe ="";
          this.ticket.dateVoyage ="";
          this.ticket.user_id ="";
          this.ticket.destination_id ="";
          //console.log(response);
        })
        .catch((response) => {
          console.log(response);
        });
        // .catch((error) => {
        //   console.log(error);
        // });
    },
  },// je parle de ça d'accords
};

</script>


<style>
#app {
  margin-left: auto;
  font-family: Avenir, Helvetica, Arial, sans-serif;
  font-size: 20px;
  font-weight: bold;
}
ul {
  list-style: none;
  display: flex;
  text-align: center;
  justify-content: center;
}
ul,
li {
  margin: 10px;
  font-size: 20px;
  color: gray;
}
a {
  color: red;
  text-decoration: none;
}
a:hover {
  color: #fff;
}
#title {
  text-align: center;
  font-size: 30px;
}
</style>





