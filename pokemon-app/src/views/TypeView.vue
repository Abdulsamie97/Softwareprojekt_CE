<template>
  <div class="about">
    <div
      v-if="type"
      className="w-3/12 m-auto bg-purple-100 mt-4 shadow-2xl flex justify-center flex-col items-center"
    >
        <h3 className="text-2xl text-green-900 uppercase">{{ type.name }}</h3>
         <div v-for="(typer, idx) in type.pokemon" :key="idx">
          <h5 className="text-blue-900">{{ typer.pokemon.name}}</h5>
         </div>
    
    </div>
  </div>
</template>

<script>
import { reactive, toRefs } from "vue";
import { useRoute } from "vue-router";
export default {
  setup() {
    const route = useRoute();
    const type = reactive({
      type: null 
    });
    fetch(`https://pokeapi.co/api/v2/type/${route.params.slug}/`)
      .then((res) => res.json())
      .then((data) => {
        type.type = data;
        console.log(data);
      });
    return { ...toRefs(type) };
  }
};
</script>