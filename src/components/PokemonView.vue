<template>

<div id="pokemon">
    <div class="card">
        <div class="card-image">
        <figure>
        <img :src="currentImg" alt="Pokémon front">
        </figure>
  </div>
  <div class="card-content">
    <div class="media">
        <div class="media-content">
            <p class="title is-4">{{ idFixo }} - {{ upper(name) }}</p>
            <hr>
            <p class="subtitle is-6">Types:<strong v-for="(type) in pokemon.type" :key="type.type"><p>{{ upper(type.type.name) }}</p></strong></p>
            <hr>
            <p class="subtitle is-6">Base Stats:</p>
            <p>HP: {{ baseHp }}</p>
            <p>Attack: {{ baseAtk }}</p>
            <p>Defense: {{ baseDef }}</p>
            <p>Sp. Attack: {{ baseSpatk }}</p>
            <p>Sp. Defense: {{ baseSpdef }}</p>
            <p>Speed: {{ baseSpd }}</p>
            <hr>
        </div>
    </div>
    <div class="content buttons are-medium column is-full">
        <button class="button is-responsive" @click="mudarSprite">Normal</button>
        <button class="button is-responsive" @click="mudarShiny">Shiny</button>
    </div>
  </div>
</div>
</div>


</template>

<script>
import axios from 'axios';


export default{
    created: function(){
        axios.get(this.url).then(res =>{
            this.pokemon.type = res.data.types;
            this.pokemon.front = res.data.sprites.front_default;
            this.pokemon.back = res.data.sprites.back_default;
            this.pokemon.frontShiny = res.data.sprites.front_shiny;
            this.pokemon.backShiny = res.data.sprites.back_shiny;
            this.currentImg = this.pokemon.front;
            this.idFixo = res.data.id;
            
            // stats

            this.baseHp = res.data.stats[0].base_stat;
            this.baseAtk = res.data.stats[1].base_stat;
            this.baseDef = res.data.stats[2].base_stat;
            this.baseSpatk = res.data.stats[3].base_stat;
            this.baseSpdef = res.data.stats[4].base_stat;
            this.baseSpd =  res.data.stats[5].base_stat;

        })
    },
    data(){
        return{
            isFront: true,
            isShiny: false,
            currentImg: '',
            pokemon: {
                type: '',
                front: '',
                frontShiny: '',
                backShiny: '',
                back: '',
                idFixo: '',
                baseHp: '',
                baseAtk: '',
                baseDef: '',
                baseSpatk: '',
                baseSpdef: '',
                baseSpd: ''
            }
        }
    },
    props:{
        num: Number,
        name: String,
        url: String
    },
    methods: {
        upper: function(value){
            var newName = value[0].toUpperCase() + value.slice(1);
            return newName;
        },
        mudarSprite: function(){
            this.isShiny = false;
                if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.back;
                } else {
                    this.isFront = true;
                    this.currentImg = this.pokemon.front;
                }
            
        },
        mudarShiny: function(){
            this.isShiny = true;
                if(this.isFront){
                this.isFront = false;
                this.currentImg = this.pokemon.backShiny;
                } else {
                    this.isFront = true;
                    this.currentImg = this.pokemon.frontShiny;
                }
            }
    }
}
</script>

<style>

#pokemon {
    margin-top: 2%;
}

</style>