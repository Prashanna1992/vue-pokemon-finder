<template>
  <div class="w-full flex justify-center">
    <input type="text" placeholder="Search" class="mt-10 p-2 border-blue-500 border-2" v-model="keyword"/>
  </div>
  <div class="mt-10 p-4 flex flex-wrap justify-center">
    <div class="ml-4 text-2xl text-blue-600">
      <div v-for="(pokemon, index) in filteredPokemon" :key="index">
        <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
          {{pokemon.name}}
        </router-link>
      </div>
    </div>
  </div>
  <div class="home">
  </div>
</template>

<script>
// @ is an alias to /src

import {reactive} from "vue";
import { toRefs, computed } from "vue";

export default {
  name: 'HomeView',
  setup(){
    const state = reactive({
      pokemons : [],
      urlIdLookup : {},
      keyword : "",
      filteredPokemon: computed(()=> updatePokemon()),
    })

    function updatePokemon() {
        if(!state.keyword){
          return [];
        }

        return state.pokemons.filter((pokemon) => pokemon.name.includes(state.keyword));
    }

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      .then((res) => res.json())
      .then((data) => {
        state.pokemons = data.results;
        state.urlIdLookup = data.results.reduce((accumulator, currentValue, index)=>
          accumulator = {...accumulator, [currentValue.name]: index + 1}, {});
      })
      return {...toRefs(state)}
  },
  components: {
    
  }
}
</script>
