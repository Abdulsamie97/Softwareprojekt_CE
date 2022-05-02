<template>
<!--<div class="w-full flex justify-center">
  <input type="text" placeholder="Enter type here" 
         class="mt-10 p2 border-blue-500 border-2"
         v-model="text"/> 
</div>-->

<div class="mt-10 p-4 flex flex-wrap justify-center">
  <div class="ml-4 text-2x text-blue-500"
    v-for="(type, idx) in filteredType"
    :key="idx">
    <!--<router-link :to="`/about/${urlIdLookup[type.name]}`">
    {{type.name}}
    </router-link>-->
    <br />
    <router-link :to="`/type/${urlIdLookup[type.name]}`">
    {{type.name}}
    </router-link>
  </div>
</div>
</template>

<script>
import {reactive, toRefs, computed} from "vue";

export default {
  name: 'HomeView',
    setup(){

      function updateType(){
        if(state.text){
          return []
        }

        return state.pokemons.filter((type)=>
          type.name.includes(state.text)
        )
      }

      const state=reactive(
        {
          pokemons: [],
          urlIdLookup: {},
          text: "",
          filteredType:  computed(() => updateType()) // would be better named filteredPokemons  TB
        }
      )


    fetch("https://pokeapi.co/api/v2/type?offset=0")
    .then((res) => res.json())
    .then((data) => {
      console.log(data)
      state.pokemons = data.results;
      state.urlIdLookup = data.results.reduce((acc, cur, idx)  => 
        acc={...acc, [cur.name]:idx+1 }
        ,{})
    } )
    return {...toRefs(state)}
  }
}
</script>
