<template>

  <div class="container">
    <section class="section">
      <div class="columns">
        <div class="column">
          <img :src="`${series.thumbnail.path}/detail.${series.thumbnail.extension}`" alt="">
        </div>
        <div class="column">
          <h2 class="title">{{series.title}}</h2>
          <div class="column--header">
            <p v-if="series.type"><b>Type:</b> {{series.type}}</p>
            <p v-if="series.rating"><b>Rating:</b> {{series.rating}}</p>
            <p v-if="series.startYear"><b>Date:</b> {{series.startYear}} - {{series.endYear}}</p>
            <p v-if="series.creators.items.length > 0">
              <b>Creators: </b>
              <span v-for="(creator, index) in creators">
                {{creator.name}}<span v-if="index+1 < creators.length">, </span>
              </span>
            </p>
            <p v-if="series.comics.items.length > 0">
              <b>Comics: </b>
              <span v-for="(comic, index) in comics">
                {{comic.name}}<span v-if="index+1 < comics.length">, </span>
              </span>
            </p>
            <p v-if="series.characters.items.length > 0">
              <b>Characters: </b>
              <span v-for="(character, index) in characters">
                {{character.name}}<span v-if="index+1 < characters.length">, </span>
              </span>
            </p>
          </div>
          <div v-if="series.description" class="column--content">
            {{series.description}}
          </div>
        </div>
      </div>
    </section>
  </div>

</template>

<script>
import axios from 'axios'

export default {
  name: 'serie-detail',

  async asyncData ({ params }) {
    const data = await
    axios.get(`http://gateway.marvel.com/v1/public/series/${params.id}?apikey=50ede0c40704d09749e334857d9e4bda`)
    return { series: data.data.data.results[0], characters: data.data.data.results[0].characters.items, comics: data.data.data.results[0].comics.items, creators: data.data.data.results[0].creators.items   }
  },

  mounted () {
    console.log(this.series)
  }

}

</script>


<style lang="scss" scoped>
.columns{
  padding: 2rem;
  background: $default;
  .column{
    &--header{
      p{
        font-size: 0.9rem;
        margin-bottom: 0.3rem;
      }
    }
    &--content{
      margin-top: 2rem;
    }
    &--footer{
      margin-top: 2rem;
    }
  }
}
</style>
