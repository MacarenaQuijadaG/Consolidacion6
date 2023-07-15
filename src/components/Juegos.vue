<template>
    <div id="Juegos">
        <h1>Juegos Disponibles</h1>

        <div class="contenedorPadre">
                <div v-for="(juego,index) in juegos" v-bind:key="index" class="card m-2" style="width: 18rem;">
                        <img v-bind:src="juego.background_image" class="card-img-top" alt="logo">
                        <div class="card-body">
                            <h5 class="card-title">{{juego.name}}</h5>
                            <p class="card-text">ESRB Rating: {{  juego.esrb_rating.name }}</p>
                        </div>
                        <ul class="list-group list-group-flush">
                            <li class="list-group-item">Rating: {{ juego.rating }}</li>
                            <li class="list-group-item">Released: {{  juego.released }}</li>
                            <li class="list-group-item">Updated: {{  juego.updated }}</li>
                        </ul>
                        <div class="card-body">
                            <a v-on:click="mostrarOpiniones(juego.name)" class="btn btn-secondary">Opinar</a><br>
                            <a v-on:click="irAdministracion(juego.name)" class="btn btn-primary">Me Gusta</a>
                        </div>
                </div>
        </div>

    </div>
</template>

<script>
import axios from 'axios';
export default{
    name:'Juegos',
    data:function(){
        return{
            cantidadJuegos:0,
            juegos:[],
        }
    },
    methods:{
        consumirApi:function(){
            let url='https://api.rawg.io/api/games?key=2fc721a57531406fb663e0e8d4b00b82';
            axios(url)
            .then(respuesta =>{
                //respuesta
                this.cantidadJuegos = respuesta.data.results.length;
                for(let i=0; i < this.cantidadJuegos; i++){
                    this.juegos.push(respuesta.data.results[i]);
                }
            })
            .catch(error=>{
                console.log(error);
            });
        },
        mostrarOpiniones:function(nombreJuego){
            this.$router.push(`/opiniones/${nombreJuego}`);
        },
        irAdministracion:function(nombreJuego){
            this.$router.push(`/administracion/${nombreJuego}`);
        },
    },
    created(){
        this.consumirApi();
    }
}
</script>

<style scoped>
#Juegos{
    background-color: rgb(173, 203, 243);
}

.contenedorPadre{
    display:flex;
    flex-wrap: wrap;
    justify-content: center;
}
</style>