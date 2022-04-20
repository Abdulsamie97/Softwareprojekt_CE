<template>
  <div class="home">
    <h3>Hello World</h3>
    {{pokemons}}
  </div>
</template>

<script>
import {reactive, toRefs} from "vue";

export default {
  name: 'HomeView',
    setup(){

      const state=reactive(
        {
          pokemons: [],
          urlIdLookup: {}
        }
      )


    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
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
