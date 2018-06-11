<template>
  <main id="home" class="main">

    <section id="title" class="section">
      <div class="container">

        <div v-masonry transition-duration="0.3s" item-selector=".column" class="columns is-multiline">

          <div v-masonry-tile class="column is-mobile is-one-third-tablet is-one-quarter-desktop" v-for="serie in series">

            <div class="card">
              <nuxt-link class="card-link" :to="{ path: `${serie.id}/${convertToSlug(serie.title)}` }" append>
                <div class="card-image">
                  <figure class="image is-4by3">
                    <img :src="`${serie.thumbnail.path}/landscape_incredible.${serie.thumbnail.extension}`" :alt="serie.title">
                  </figure>
                </div>
                <div class="card-content">
                  <div class="media">
                    <div class="media-content">
                      <p class="title is-5">{{serie.title}}</p>
                      <div class="category"><i class="fas fa-map-marker-alt"></i>
                      <p class="subtitle is-6">{{serie.startYear}}</p>
                      </div>
                    </div>
                  </div>
                  <div id="overview" class="content">
                  {{serie.description}}
                  </div>
                </div>
              </nuxt-link>
            </div>

          </div>
        </div>

      </div>
    </section>


  </main>
</template>

<script>
import axios from 'axios'

export default {

  name: 'home',

  head: {
    title: 'Home'
  },

  async asyncData () {
    const data = await axios.get('http://gateway.marvel.com/v1/public/series?apikey=50ede0c40704d09749e334857d9e4bda')
    return { series: data.data.data.results }
  },

  mounted () {
    if (typeof this.$redrawVueMasonry === 'function') {
      this.$redrawVueMasonry()
    }
    this.truncate()
  },

  methods: {
    truncate () {
      let overviews = document.querySelectorAll('#overview')
      for (var overview of overviews) {
        let strings = overview.innerHTML.substring(0,155);
        overview.innerHTML = strings + '...'
      }
    },
    update (val) {
      this.value = val
    },
    convertToSlug(text) {
      return text
          .toLowerCase()
          .replace(/[^\w ]+/g,'')
          .replace(/ +/g,'-')
    }
  }

}
</script>

<style lang="scss" scoped>
.card{
  border-radius: 0.2rem;
  .title{
    color: #717171;
    margin-bottom: 1rem;
    transition: 0.5s all;
  }
  .category{
    display: flex;
    flex-direction: row;
    align-items: center;
    i{
      color: #ACAAAA;
      margin-right: .4rem;
      font-size: .7rem;
    }
    .subtitle{
      color: #ACAAAA;
      &:first-letter{
        text-transform: uppercase;
      }
    }
  }
  .card-image{
    border-top-left-radius: 0.2rem;
    border-top-right-radius: 0.2rem;
    img{
      border-top-left-radius: 0.2rem;
      border-top-right-radius: 0.2rem;
    }
  }
  .content{
    color: #9a9a9a;
  }
  .card-footer{
    i{
      color: #4468B0;
    }
    .card-footer-item{
      display: flex;
      flex-direction: row;
      justify-content: space-between;
    }
  }
}
.card-link:hover{
  .title{
    color: #ED1B2A;
  }
}
.starability-basic{
  min-height: 0;
}

@media only screen and (max-width: 768px) {
  .column{
    width: 100%;
  }
}
</style>
