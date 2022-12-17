<template>
  <div class="about">
    <div v-if="pokemon" class="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center">
      <h3 class="text-2xl mt-3 p-5 text-green-900 uppercase">
        {{ pokemon.name }}
      </h3>
      <div class="flex justify-center">
        <img class="w-48" :src="pokemon.sprites.front_shiny" alt="">
        <img :src="pokemon.sprites.back_shiny" alt="">
      </div>
      <h3 class="text-black mb-4">Type(s)</h3>
      <div class="mb-5" v-for="(type, index) in pokemon.types" :key="index">
        <span class="text-blue-900">
          {{ type.type.name.toUpperCase() }}
        </span>
      </div>
      <h3 class="text-black mt-3">
        <strong>
          Stats
        </strong>
      </h3>
      <table class="table-fixed text-left border-collapse border-gray-100">
        <thead>
          <tr>
            <th class="p-5">
              Stat Type
            </th>
            <th class="p-5">
              Value
            </th>
          </tr>
        </thead>
        <tbody>
          <tr v-for="(stat, index) in pokemon.stats" :key="index">
            <td class="p-5">
              <strong>{{ stat.stat.name }}:</strong>
            </td>
            <td class="p-5">
              {{ stat.base_stat }}
            </td>
          </tr>
        </tbody>
      </table>
    </div>
  </div>
</template>

<script>
import { useRoute } from 'vue-router'
import { reactive, toRefs } from 'vue';
export default {
  setup() {
    const route = useRoute();
    const state = reactive({
      pokemon: null
    });
    fetch(`https://pokeapi.co/api/v2/pokemon/${route.params.slug}`)
      .then((res) => res.json())
      .then((data) => {
        state.pokemon = data;
      })

    return { ...toRefs(state) }
  },
  components: {

  },
};
</script>
