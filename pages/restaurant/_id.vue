<template>
  <div>
    <div class="section">
      <section class="hero has-background is-large">
        <img
          v-if="restaurant.hero_image"
          :src="restaurant.hero_image.url"
          alt=""
          class="hero-background is-transparent"
        />

        <div class="hero-body">
          <div class="container">
            <h1 class="title has-text-white">
              {{ restaurant.restaurant }}
            </h1>
          </div>
        </div>
      </section>
    </div>
    <div class="section">
      <section>
        {{ restaurant.description }}

        <div class="notification">
          <nuxt-link to="/" class="button">Back</nuxt-link>
        </div>
      </section>
    </div>
  </div>
</template>

<script>
import axios from 'axios'

export default {
  data() {
    return {
      restaurant: {},
      id: this.$route.params.id.substr(0, this.$route.params.id.indexOf('-'))
    }
  },
  async mounted() {
    const { data } = await axios({
      url: 'https://still-journey-60454.herokuapp.com/graphql',
      method: 'post',
      data: {
        query: `
          {restaurant(id:${this.id}) {
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

    this.restaurant = data.data.restaurant
  }
}
</script>

<style></style>
