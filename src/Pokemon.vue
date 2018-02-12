<template>
  <div>
    <div class="box pokemon-box">
      <article class="media">
        <div class="media-left">
          <figure class="image">
            <img :src="pokemon.sprites.front_default">
          </figure>
        </div>
        <div class="media-content">
          <div class="content">
            <p>
            <strong>{{ pokemon.name }}</strong> <small>@pokedex</small> <small>{{ pokemon.base_experience }}</small>
            <br>
            {{ pokemon.description }}
            </p>
        </div>
        </div>
      </article>
    </div>
  </div>
</template>

<script>
export default {
  name: 'pokemon-component',
  props: ['pokemon']
}
</script>

<style>
.pokemon-box {
  margin-bottom: 10px;
}
</style>
