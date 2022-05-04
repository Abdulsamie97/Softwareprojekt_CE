<template>
<div class="w-full flex justify-center">
  <input type="text" placeholder="Enter Pokemon here" 
         class="mt-10 p2 border-blue-500 border-2"
         v-model="text"/>
</div>
<div style="width: 100%">
  <p style="text-align:center;margin-top:2%;margin-bottom:2%"> {{filteredPokemons.counter}}  </p>
  <table style="font-family: arial, sans-serif;border-collapse: collapse;width: 70%;text-align:center;margin-left:15%;margin-right:15%">
    <tr style="border: 1px solid #dddddd;">
      <th>Name</th>
      <th>Types</th>
    </tr>
    <tr v-for="(pokemon, idx) in filteredPokemons.pokemonsViewList" :key="idx" style="border: 1px solid #dddddd;">
      <td class="ml-4 text-2x text-blue-500">
        <router-link :to="`/about/${urlIdLookup[pokemon.name]}`">
          {{pokemon.name}}
        </router-link>
      </td>
      <td>
        <div v-for="(pokemonTypes, idx1) in pokemon.types" :key="idx1">
          <h5 class="text-blue-900">{{pokemonTypes}}</h5>
        </div>
      </td>
    </tr>
  </table>
</div>
</template>

<script>
import {reactive, toRefs, computed, onMounted} from "vue";

export default {
  name: 'HomeView',
    setup(){

      function updatePokemon(){
        if(!state.text){
          return { pokemonsViewList: state.pokemons}
        }

        const pokemonsViewList = state.pokemons.filter((pokemon)=>
          pokemon.name.startsWith(state.text)
        )
        return {counter: pokemonsViewList.length, pokemonsViewList}
      }

      const getPokemonType = async(pokemonId) => {
       let types = []
       const response = await fetch(`https://pokeapi.co/api/v2/pokemon/${pokemonId}/`)
       const dataType = await response.json()
       types = [...dataType.types]
       return types
      }

      const getPokemonList = (pokemonList, urlId) => {
        const pokemonsNewList = pokemonList.map(async(pokemon) => {
          let types = await getPokemonType(urlId[pokemon.name])
          types = types.map(type => type.type.name)
          return { ...pokemon, types }
        })
        return Promise.all(pokemonsNewList);
      }

      const state=reactive(
        {
          pokemons: [],
          urlIdLookup: {},
          text: "",
          filteredPokemons:  computed(() => updatePokemon())
        }
      )

    onMounted(async () => {
      const response = await fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
      const data = await response.json();

      state.pokemons = data.results;
      state.urlIdLookup = data.results.reduce((acc, cur, idx)  => 
        acc={...acc, [cur.name]:idx+1 }
      ,{})
        
      state.pokemons = await getPokemonList(state.pokemons, state.urlIdLookup)
      console.log(state.pokemons)
    })

    return {...toRefs(state)}
  }
}
</script>
