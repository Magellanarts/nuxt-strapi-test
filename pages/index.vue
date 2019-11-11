<template>
  <div class="container">
    <div>
      <div class="columns">
        <div
          v-for="restaurant in restaurants"
          :key="restaurant.id"
          class="column is-one-third"
        >
          <nuxt-link
            :to="
              `/restaurant/${restaurant.id}-${slugify(restaurant.restaurant)}`
            "
            class="card"
          >
            <div class="card-image">
              <figure class="image is-4by3">
                <img
                  v-if="restaurant.hero_image"
                  :src="restaurant.hero_image.url"
                  alt="Placeholder image"
                />
              </figure>
            </div>
            <div class="card-content">
              <p class="title is-4">{{ restaurant.restaurant }}</p>

              <div class="content">
                <VueShowdown
                  v-if="restaurant.description"
                  :markdown="restaurant.description"
                />
              </div>
            </div>
          </nuxt-link>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      restaurants: {}
    }
  },
  async mounted() {
    const { data } = await axios({
      url: 'https://still-journey-60454.herokuapp.com/graphql',
      method: 'post',
      data: {
        query: `
          {restaurants {
            id
            restaurant
            created_at
            description
            hero_image {
              url
            }
          } }`
      }
    })

    this.restaurants = data.data.restaurants
  },
  methods: {
    slugify(string) {
      const a = 'àáäâãåèéëêìíïîòóöôùúüûñçßÿœæŕśńṕẃǵǹḿǘẍźḧ·/_,:;'
      const b = 'aaaaaaeeeeiiiioooouuuuncsyoarsnpwgnmuxzh------'
      const p = new RegExp(a.split('').join('|'), 'g')

      return string
        .toString()
        .toLowerCase()
        .replace(/\s+/g, '-') // Replace spaces with
        .replace(p, (c) => b.charAt(a.indexOf(c))) // Replace special characters
        .replace(/&/g, '-and-') // Replace & with ‘and’
        .replace(/[^\w-]+/g, '') // Remove all non-word characters
        .replace(/--+/g, '-') // Replace multiple — with single -
        .replace(/^-+/, '') // Trim — from start of text
        .replace(/-+$/, '') // Trim — from end of text
    }
  }
}
</script>

<style>
.container {
  margin: 0 auto;
  min-height: 100vh;
  display: flex;
  justify-content: center;
  align-items: center;
  text-align: center;
}

.title {
  font-family: 'Quicksand', 'Source Sans Pro', -apple-system, BlinkMacSystemFont,
    'Segoe UI', Roboto, 'Helvetica Neue', Arial, sans-serif;
  display: block;
  font-weight: 300;
  font-size: 100px;
  color: #35495e;
  letter-spacing: 1px;
}

.subtitle {
  font-weight: 300;
  font-size: 42px;
  color: #526488;
  word-spacing: 5px;
  padding-bottom: 15px;
}

.links {
  padding-top: 15px;
}
</style>
