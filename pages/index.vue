<template>

  <div class="container">
    <div class="card shadow">
      <div class="card-body">
        <h1>Hello world!</h1>
        <p class="lead">A ready to go boilerplate with some modifications to bootstrap.</p>
        <p>Made with ❤️ by <a href="https://derianandre.com/">DerianAndre</a></p>
      </div>
    </div>

<b-col v-for="article of articles" :key="article.slug" class="col-sm-"6>
              <NuxtLink
                :to="{ name: 'blog-slug', params: { slug: article.slug } }"
              >  
<div class="card-container">
  <div class="card u-clearfix">
    <div class="card-body">
      <span class="card-number card-circle subtle">01</span>
      <span class="card-author subtle">John Smith</span>
      <h2 class="card-title">{{ article.title }} Brunch Recipe</h2>
      <span class="card-description subtle">These last few weeks I have been working hard on a new brunch recipe for you all.</span>
      <div class="card-read">Read</div>
      <span class="card-tag card-circle subtle">C</span>
    </div>
    <img                         :src="article.thumbnail"
                        left
                        blank-color="#d0b8ac"
                        loading="lazy"
                        alt="" />
  </div>
  <div class="card-shadow"></div>
</div>
              </NuxtLink>
  </b-col>
</div>

</template>

<script>

export default {
  components: { },
  async asyncData({ $content, params }) {
    const articles = await $content('articles', params.slug)
      .only([
        'id',
        'title',
        'slug',
        'description',
        'img',
        'thumbnail',
        'date',
        'url',
        'published',
        'theme',
        'main_ingredient',
        'time',
        'difficulty',
        'country',
        'veggie',
        'sweety_salty'
      ])
      .sortBy('id', 'desc')
      .where({ published: 1 })
      .limit(30)
      .fetch()
    return {
      articles
    }
  },
  data() {
    return {
      title: 'Les dernières recettes de Josie'
    }
  },
  head() {
    return {
      title: this.title,
      meta: [
        {
          hid: 'description',
          name: 'description',
          content:
            "Retrouvez sur mon site toutes les recettes que j'ai publiées sur mon compte Instagram @josiesrecipes"
        },
        {
          hid: 'og:image',
          name: 'og:image',
          content:
            'https://res.cloudinary.com/thibaults/image/upload/v1606424632/partage-home.png'
        }
      ]
    }
  }
}

  
</script>

<style lang="scss" scoped>
 
</style>