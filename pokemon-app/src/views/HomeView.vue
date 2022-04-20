<template>
  <div class="home">
    <h3>Hello World</h3>
  </div>
</template>

<script>
// @ is an alias to /src

export default {
  name: 'HomeView',
    setup(){

    fetch("https://pokeapi.co/api/v2/pokemon?offset=0")
    .then((res) => res.json())
    .then((data) => {
      console.log(data)
    } )
  }
}
</script>
