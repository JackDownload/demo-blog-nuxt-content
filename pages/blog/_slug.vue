<template>
  <article>
    <div
      class="flex lg:h-screen w-screen lg:overflow-hidden xs:flex-col lg:flex-row"
    >
      <div class="relative lg:w-1/5 xs:w-full xs:h-screen lg:h-full post-left">
        <img
          :src="article.img"
          :alt="article.alt"
          class="absolute h-full w-full object-cover"
        />
        <div class="overlay"></div>
      </div>
      <div
        class="relative xs:py-8 xs:px-8 lg:py-32 lg:px-16 lg:w-1/2 xs:w-full h-full overflow-y-scroll markdown-body post-right custom-scroll"
      >
        <h1>{{ article.title }}</h1>
        <p v-if="article.description" class="lead">
          {{ article.description }}
        </p>
        <nuxt-content :document="article" />
        <div class="mb-3 rebound-block">
          <h2>Cela pourrait vous intéresser</h2>
          <p>
            Mes recettes sont organisés par catégorie. Retrouvez les catégories
            en lien avec cette recette.
          </p>
          <div>
            <a
              v-if="article.time < 31"
              class="link mr-2"
              href="/categories/trente-minutes-chrono"
            >
              Recettes en moins de 30 min
            </a>
            <a
              v-if="article.theme == 'Pâtes'"
              class="link mr-2"
              href="/categories/pates"
            >
              Pâtes
            </a>
            <a
              v-if="article.theme == 'Tartines'"
              class="link mr-2"
              href="/categories/burgers-et-tartines"
            >
              Tartines
            </a>
            <a
              v-if="article.theme == 'Burgers'"
              class="link mr-2"
              href="/categories/burgers-et-tartines"
            >
              Burgers
            </a>
            <a
              v-if="article.main_ingredient == 'Chocolat'"
              class="link mr-2"
              href="/categories/chocolat"
            >
              Desserts au chocolat
            </a>
            <a
              v-if="article.theme == 'Gâteaux'"
              class="link mr-2"
              href="/categories/gateaux"
            >
              Gâteaux
            </a>
            <a
              v-if="article.veggie"
              class="link mr-2"
              href="/categories/vegetarien"
            >
              Végératien
            </a>
            <a
              v-if="
                article.main_ingredient == 'Saumon' ||
                article.main_ingredient == 'Thon' ||
                article.main_ingredient == 'Daurade' ||
                article.main_ingredient == 'Lotte' ||
                article.main_ingredient == 'Crevette' ||
                article.main_ingredient == 'Bar' ||
                article.main_ingredient == 'Cabillaud' ||
                article.main_ingredient == 'Saint-Jacques' ||
                article.main_ingredient == 'Huître' ||
                article.main_ingredient == 'Crabe' ||
                article.main_ingredient == 'Truite' ||
                article.main_ingredient == 'Coques' ||
                article.main_ingredient == 'Palourde' ||
                article.main_ingredient == 'Moule' ||
                article.main_ingredient == 'Calamar'
              "
              class="link mr-2"
              href="/categories/poissons-et-crustaces"
            >
              Poissons et crustacés
            </a>
            <a
              v-if="
                article.main_ingredient == 'Boeuf' ||
                article.main_ingredient == 'Poulet' ||
                article.main_ingredient == 'Dinde' ||
                article.main_ingredient == 'Veau' ||
                article.main_ingredient == 'Canard' ||
                article.main_ingredient == 'Chapon' ||
                article.main_ingredient == 'Agneau' ||
                article.main_ingredient == 'Lapin' ||
                article.main_ingredient == 'Boudin' ||
                article.main_ingredient == 'Porc'
              "
              class="link mr-2"
              href="/categories/viandes"
            >
              Viandes
            </a>
            <a
              v-if="article.theme == 'Salades'"
              class="link mr-2"
              href="/categories/salades"
            >
              Salades
            </a>
            <a
              v-if="
                article.main_ingredient == 'Asperge' ||
                article.with == 'Asperge'
              "
              class="link mr-2"
              href="/categories/asperge"
            >
              Asperges
            </a>
          </div>
          <div class="last-post mt-4">
            <h3>Ma dernière recette publiée</h3>
            <p>
              Chaque semaine, je publie 4 nouvelles recettes. Découvrez la
              dernière recette publiée sur le site.
            </p>
            <b-row id="posts" class="row-cols-1">
              <b-col class="mb-3">
                <b-card class="post horizontal" no-body>
                  <NuxtLink
                    :to="{ name: 'blog-slug', params: { slug: last.slug } }"
                  >
                    <b-row>
                      <b-col md="5">
                        <div class="img-container">
                          <b-card-img-lazy
                            :src="last.thumbnail"
                            left
                            blank-color="#d0b8ac"
                            loading="lazy"
                            alt=""
                          >
                          </b-card-img-lazy>
                        </div>
                      </b-col>
                      <b-col md="2">
                        <b-card-body>
                          <b-card-title title-tag="h3">
                            {{ last.title }}
                          </b-card-title>
                          <p class="lead mb-3">{{ last.description }}</p>
                          <div class="infos-card">
                            <div v-if="last.veggie">
                              <i class="fas fa-seedling"></i>
                              Recette végétarienne
                            </div>
                            <div v-if="last.country">
                              <i class="fas fa-globe-americas"></i>
                              {{ last.country }}
                            </div>
                            <div v-if="last.difficulty">
                              <i class="fas fa-check-double"></i>
                              {{ last.difficulty }}
                            </div>
                            <div v-if="article.time">
                              <i class="fas fa-stopwatch"></i>
                              {{ last.time }} min
                            </div>
                          </div>
                        </b-card-body>
                      </b-col>
                    </b-row>
                  </NuxtLink>
                </b-card>
              </b-col>
            </b-row>
          </div>
        </div>
      </div>
    </div>
  </article>
</template>
<script>
export default {
  async asyncData({ $content, params }) {
    const article = await $content('articles', params.slug).fetch()
    const [last] = await $content('articles').sortBy('id', 'desc').fetch()
    return {
      article,
      last
    }
  },
  methods: {
    formatDate(date) {
      const options = { year: 'numeric', month: 'long', day: 'numeric' }
      return new Date(date).toLocaleDateString('fr', options)
    }
  },
  head() {
    return {
      title: this.article.title,
      script: [
        {
          type: 'application/ld+json',
          json: {
            '@context': 'https://schema.org',
            '@type': 'Recipe',
            name: this.article.title,
            url: 'https://http://192.168.1.11:3000/' + this.article.slug,
            image: this.article.thumbnail,
            author: {
              '@type': 'Person',
              name: 'Josie'
            },
            datePublished: this.article.date,
            description: this.article.description,
            totalTime: 'PT' + this.article.time + 'M'
          }
        }
      ],
      meta: [
        {
          hid: 'description',
          name: 'description',
          content: this.article.description
        },
        // Open Graph
        {
          hid: 'og:title',
          property: 'og:title',
          content: this.article.title
        },
        {
          hid: 'og:description',
          property: 'og:description',
          content: this.article.description
        },
        {
          hid: 'og:image',
          property: 'og:image',
          content: this.article.img
        },
        // Twitter Card
        {
          hid: 'twitter:title',
          name: 'twitter:title',
          content: this.article.title
        },
        {
          hid: 'twitter:description',
          name: 'twitter:description',
          content: this.article.description
        },
        {
          hid: 'twitter:image',
          property: 'twitter:image',
          content: this.article.img
        }
      ]
    }
  }
}
</script>
<style>
$salmon: #F73A64 !default;
$taupe: #d0b8ac !default;
$taupe-600: #f3d8c7 !default;
$taupe-400: #efe5dc !default;
$taupe-200: #fbfefb !default;


$theme-colors: (
  "primary": #d0b8ac,
  "secondary": #efe5dc,
  "tertiary":#FFFDFC,
);

//then bootstrap & bootstrap-vue should be added after 
//so that they inherit the custom SCSS variables


// Classic css
  
body {
    background-color:#d0b8ac; 
    color: #253237;
    font-family: 'Crimson Text', serif;
    min-height: 100vh;
}

input:focus {
    outline: 3px solid #F73A64;
}  

a {
    color : #75513e;
}


.lead {
    font-size: 1.125rem;
}

article .lead {
    font-size: 1.25rem;
    font-weight: 600;
}

.lead.lead-home {
    font-size: 1.5rem;
}

nav.customnav {
    /* Permalink - use to edit and share this gradient: https://colorzilla.com/gradient-editor/#d0b8ac+1,d0b8ac+100&1+65,0+93 */
    background:none;
    position: fixed;
    width: 100%;
    padding-bottom: 1.5rem;
}

#josie.logo {
    width :5.5rem; 
    filter: drop-shadow(0 0 6px rgba(0, 0, 0, 0.5));
}
.container-lg {
    max-width: none;
    padding-top: 7rem;
}

h1::first-letter {
    text-transform: uppercase;
}

h3, .h3 {
    font-size: 1.5rem;
}

h2.subtitle-home {
    color: #75513e
}

.btn-tertiary.nuxt-link-exact-active {
    background-color: #F73A64;
    border-color: #F73A64;
    color:#FFFDFC;
}

.link {
    text-transform: uppercase;
    font-weight: 800;
    font-size: 0.9rem;
    transition: 500ms ease-in-out;
    color: #F73A64;
    white-space:nowrap;
}

.link:hover {
    text-decoration: none;
    color: #75513e;
}

article h1 {
    font-size: 3rem;
    color: #F73A64;
}

article h2 {
    border-bottom: 4px solid #F73A64;
}

article li {
    list-style-type: none;
}

article li::before {
    color: #F73A64;
    content : "•";
    padding-right: 0.7rem;
    font-weight: 1000;
}

article .detail-infos-card {
    color: #75513e;
    i {
        color : #F73A64
    }
}

article .icon-insta i  {
    color: #75513e;
    transition: ease-in 0.1s;
    -webkit-transition: ease-in 0.3s;
}
  
article .icon-insta:hover i ,article .icon-insta:focus i  {
    color: #F73A64;
}



// nav 

.navbar-dark .navbar-nav .nav-link {
    color:rgba(132,87,76,0.8);
}

.navbar-dark .navbar-nav .nav-link,.navbar-dark .navbar-nav .nav-link:focus{
    color:white;
    padding:0;
    align-self: bottom;
}

.navbar-dark .navbar-nav .nav-link:hover{
    color:white;
    text-decoration: underline;

}

.navbar-text.title-home {
    display: none;
}

.halloween .navbar-text.title-home {
    display: inline-block;
}

.navbar-nav {
    width: 100%;
}


.insta-link {
    display: none;
}
.dropdown-menu {
    display: block;
    visibility: hidden;
    opacity:0;
    transition:1s ease all;
    -webkit-transition:1s ease all;
}
.dropdown-menu.show {
    display: block;
    visibility: visible;
    opacity:1;
    transition:1s ease all;
    -webkit-transition:1s ease all;
}

.customnav .dropdown-menu {
    overflow: scroll;
    max-height: 90vh;
}

::-webkit-input-placeholder { /* Edge */
    color: rgba(132,87,76,0.7) !important;
}

:-ms-input-placeholder { /* Internet Explorer 10-11 */
    color: rgba(132,87,76,0.7) !important;
}

::placeholder {
    color: rgba(132,87,76,0.7) !important;
}

.customnav .dropdown-toggle::after {
    display: none;
}

.filter-icon {
    width:20px;
    height:20px;
    margin-top:2px;
    vertical-align: top;
    fill: #FFF;
}

.search-container {
    width: 100%;
}

.search-recipes, .search-list-recipes {
    color: #000;
    border: 1px solid #d0b8ac;
    border-radius: 0.25rem;
}

.search-recipes {
    padding: 0.3rem;
    padding-left:0.5rem;
    overflow: hidden;
    text-overflow: ellipsis;
    white-space: nowrap;
    width: 100%;
    background-color: #efe5dc;
}

.search-list-recipes {
    background-color: #fff;
    overflow-x: hidden !important;
    width: auto;
    position: absolute;
    max-height: 60vh;
    overflow-y: auto;
    >li {
        padding: 0.4rem;
    }
    a {
        color:#000;
    }
}

.search-container-home .search-recipes {
    font-size: 1.5rem;
}

.search-container-home .search-list-recipes {
    font-size: 1.5rem;
    z-index: 300;
    width: 90%;
    height: 30vh;
}

.dropdown-item {
    color:rgba(132,87,76,0.8);
}

.dropdown-item:hover,.dropdown-item:focus {
    color:rgba(132,87,76,0.9);
    background: transparent;
}

// card
.recipe-filter a {
    font-size: 0.8rem;
}

.card.post .badge {
    font-size: 0.65rem;
}

.recipe-filter a, .card.post .badge {
    text-transform: uppercase;
    font-weight: 400;
}

.recipe-filter a:hover {
    text-decoration: underline;
    background-color: transparent;
}

.navbar-dark .navbar-nav .nav-link:hover,.navbar-dark .navbar-nav .nav-link:focus,.navbar-dark .navbar-nav .show > .nav-link {
    color:rgba(132,87,76,0.9); 
}

article {
    color: #000;
    .post-left {
        -webkit-box-shadow: inset 0 200px 100px -100px rgba(208,184,172,0.2);
        -moz-box-shadow: inset 0 200px 100px -100px rgba(208,184,172,0.2);
        box-shadow: inset 0 200px 100px -100px rgba(208,184,172,0.2);
        img {
            z-index: -2;
        }
    }
    .post-right {
        background-color: #d0b8ac;
    }
}

.push-category {
    transition: all 0.3s ease-in-out;
    border-color : rgba(0,0,0,0.1);
    background-color: #bf8378;
    color:#fbfefb;
    h2 {
        font-size: 2rem;
    }
    h3 {
        margin: 0;
    }
}

.push-category:hover {
    color:#fff;
    background-color: #b57d72;

}

.push-category.push-halloween {
    background-color: #0D0126;
    background-image : url(~assets/images/pattern-halloween.svg);
    color:#F27405;
    h2 {
        color:#F27405;
    }
    p {
        color:#FFF
    }
}
.push-category.push-halloween:hover {
    color:#c45b00;
}

.halloween .img-container {
    box-shadow: inset 0 0 90px rgba(13,1,38,0.9);
    height: initial;
    overflow: hidden;
    img {
        z-index: -2;
        position: relative;
        opacity: 0.8;
    }
}

.card.horizontal a {
    overflow: hidden;

    img {
        transition: all 1s ease;
        -webkit-transition: all 1s ease;
    }
}

.halloween .intro .lead, .noel .intro .lead,.saint-valentin .intro .lead {
    color:#CBB1A4;
    font-size: 1.5rem;
}


.category-page h1, .halloween h1, .noel h1, .saint-valentin h1{
    font-size: 3rem;
}

.halloween h1 {
    text-transform: uppercase;
    color : #F27405
}

.saint-valentin h1, .saint-valentin .intro .lead {
    color:#fff;
}

.easter h1, .easter .intro .lead {
    color:#FF3294;
}

.push-category.push-christmas {
    background-color: #BF1131;
    background-image : url(~assets/images/pattern-noel.svg);
    color:#FFF;
    border: solid 5px #8C0D17;
    h2 {
        color:#FFF;
    }
    p {
        color:#FFF
    }
    .btn {
        background-color: #006523;
        color: #fff;
        border-color: #00491a;
    }
}

.push-category.push-valentin {
    background-color: #FF084A;
    background-image : url(~assets/images/pattern-saint-valentin.svg);
    color:#FFF;
    border: solid 5px #FF93AB;
    h2 {
        color:#FFF;
    }
    p {
        color:#FFF
    }
    .btn {
        background-color: #FFC2CD;
        color: #FF084A;
        border-color: #FF6289;
    }
}

.push-category.push-easter {
    background-color:#fce3ae;
    background-image : url(~assets/images/pattern-easter.svg);
    color:#FF3294;
    border: solid 5px #ffd374;
    h2 {
        color:#FF3294;
    }
    p {
        color:#FF3294;
    }
    .btn {
        background-color: #FF3294;
        color: #fce3ae;
        border-color: #fce3ae;
    }
    .btn:hover {
        background-color: #fce3ae;
        color: #FF3294;
    }
}

.push-category.push-valentin .btn:hover {
    background-color: #FF084A;
    color: #FFC2CD;
}

.push-category.push-christmas .btn:hover {
    background-color: #fff;
    color: #006523;
}

.noel h1 {
    color : #FFF;
}

.halloween h1::first-letter, .noel h1::first-letter {
    font-size: 3rem;
}

.saint-valentin .lead a {
    color: #ffd9e0;
}

.noel .card {
    min-height: 300px;
    height: 40vh;
    border : 5px solid #8C0D17;
    background-image : url(~assets/images/pattern-noel-card.svg);
    background-color: #F2EADF;
    display: flex;
    flex-direction: column;
    .card-body {
        display: flex;
        flex: 1;
        .card-title {
            font-size: 1.3rem;
            font-weight: 600;
            text-shadow: 0 0 4px #fff; 
            span {
                display: block;
                font-size: 3.5rem;
                color: #006523;
            }
        }
        .btn {
            transition: all 0.3s ease-in-out;
            background-color: #BF1131;
            color: #fff;
            border-color: #8C0D17;
        }
        .btn:hover {
            background-color: #8C0D17;
            color: #D98E04;
        }
    }
    .card-text {
        padding: 0 1rem;
    }
    .card-text.title-recipe{
        font-size: 1.2rem;
        text-shadow: 0 0 5px #fff; 
        transition:all 1s ease-in-out;
    } 

    .card-footer {
        border: none;
        font-style: italic;
        color: #8C0D17;
        font-size:1.3rem;
    }

    .overlay-calendar {
        transition: all 2s ease-in-out;
        background-color: rgba($color: #fff, $alpha: 0.5);
        border-radius: 0.25rem;
        width: 100%;
        align-items: center;
        display: flex;
        flex:1;
        .overlay-container {
            width: 100%;
        }
    }
}

.noel .reveal-calendar {
    background-size: cover;
    .card-body .overlay-calendar {
        background-color: rgba($color: #fff, $alpha: 0.8);
    }
}

.noel .card.reveal-calendar:hover {
    .overlay-calendar {
        background-color: rgba($color: #fff, $alpha: 0.3);
        .card-text.title-recipe{
            opacity: 0;
        }
    }
}


.card.horizontal:hover a img {
    transform: scale(1.1);
}

#posts .card {
    border:0;
    transition: all 1s ease;
    -webkit-transition: all 1s ease;
    -webkit-box-shadow: 0 4px 5px 0 rgba(208,184,172,0.5);
    -moz-box-shadow: 0 4px 5px 0 rgba(208,184,172,0.5);
    box-shadow: 0 4px 5px 0 rgba(208,184,172,0.5);
    overflow: hidden;
    a img {   
        opacity: 0.9;
    }
    .img-container {
        overflow: hidden;
        height: initial;
        margin: 20px 20px 0 20px;
    }
    .card-body {
        transition: 1s ease;
        -webkit-transition: 1s ease;
    }
}

.halloween #posts .card {
    -webkit-box-shadow: none;
    -moz-box-shadow: none;
    box-shadow: none;
}

.saint-valentin #posts .card {
    -webkit-box-shadow: 0 4px 5px 0 rgba(255,147,171,0.2);
    -moz-box-shadow: 0 4px 5px 0 rgba(255,147,171,0.2);
    box-shadow: 0 4px 5px 0 rgba(255,147,171,0.2);
}

#posts .card:hover {    
    -webkit-box-shadow: 0 5px 6px 0 rgba(132, 87, 76, 0.5);
    -moz-box-shadow: 0 5px 6px 0 rgba(132, 87, 76, 0.5);;
    box-shadow: 0 5px 6px 0 rgba(132, 87, 76, 0.5);    
    .icon {
        background: #F73A64;
    }
    .icon:hover {
        background: #253237;
    }
}

#posts .card.bg-tertiary:hover {
    background-color: #efe5dc !important;
}

#posts .card-img {
    border-bottom-right-radius: 0;
    border-bottom-left-radius: 0;
}

.saint-valentin #posts .card:hover {    
    -webkit-box-shadow: 0 5px 6px 0 rgba(255,147,171, 0.7);
    -moz-box-shadow: 0 5px 6px 0 rgba(255,147,171, 0.7);;
    box-shadow: 0 5px 6px 0 rgba(255,147,171, 0.7);    
}

.halloween #posts .card:hover {    
    -webkit-box-shadow: none;
    -moz-box-shadow: none;
    box-shadow: none;
}
.card.post .card-header {
    z-index: 10;
    border: 0;
    background: transparent;
}

.card.post .card-header .tag {
    padding: 0.25rem;
    padding-left: 0;
    background: #efe5dc;
    color: #253237;
}

.card.post {
    a  {
        h3 {
            color: #F73A64;
        }
        p.lead  {
            color: #CBB1A4;
            margin-bottom: 0;
            font-size:1.125rem;
            font-weight: 400;
        }
        .infos-card {
            color: #75513e;
            i {
                color : #F73A64
            }
        }
    }
    a:hover {
        text-decoration: none;
        h3 {
            color: #F73A64;
            text-decoration: underline;
        }
        p {
            color: #b48e7b;
        }
    }
}


a.see-more {
    color: #B99B8B;
    background-color: #fff;
    -webkit-box-shadow: 0 4px 5px 0 rgba(208,184,172,0.5);
    -moz-box-shadow: 0 4px 5px 0 rgba(208,184,172,0.5);
    box-shadow: 0 4px 5px 0 rgba(208,184,172,0.5);
    transition: 1s ease;
    -webkit-transition: 1s ease;
}

a.see-more:hover {
    color: #b48e7b;
    -webkit-box-shadow: 0 5px 6px 0 rgba(132, 87, 76, 0.5);
    -moz-box-shadow: 0 5px 6px 0 rgba(132, 87, 76, 0.5);;
    box-shadow: 0 5px 6px 0 rgba(132, 87, 76, 0.5);
}


// content 



.nuxt-content ul {
    list-style-type: disc;
    padding-left: 1rem;
}

.halloween {
    background-color: #0D0126;
    background-image : url(~assets/images/pattern-halloween.svg);
    min-height: 100vh;
}

.noel {
    background-color: #BF1131;
    background-image : url(~assets/images/pattern-noel.svg);
    min-height: 100vh;
}

.saint-valentin {
    background-color:#FF084A;
    background-image : url(~assets/images/pattern-saint-valentin.svg);
    min-height: 100vh;
}

.easter {
    background-color:#fce3ae;
    background-image : url(~assets/images/pattern-easter.svg);
    min-height: 100vh;
}


.halloween nav, .noel nav {
    background: transparent;
}


.halloween .card.post {
    border:none;
    background: rgba(51,3,64,0.4) !important;
    transition : 1s all ease-in-out;
    a h3 {
        color: #F27405;
    }
    
}

.easter .card.post {
    background-color: #ffd374;
    border: #FF3294;

    a  {
        h3 {
            color: #FF3294;
        }
        p {
            color: #FF3294;
        }
        p.date {
            color:#FF3294;
        }
    }
}

.easter .intro .lead a {
    color:#FF3294;
}

.saint-valentin .card.post {
    background-color: #ffd9e0;
    border: #FF084A;

    a  {
        h3 {
            color: #FF084A;
        }
        p {
            color: #ff779c;
        }
        p.date {
            color:#ff779c;
        }
    }
    a:hover {
        text-decoration: none;
        h3 {
            color: #FF084A;
            text-decoration: underline;
        }
    }
}

.easter .card.post a:hover {
    text-decoration: none;
    h3 {
        color: #FF3294;
        text-decoration: underline;
    }
}


.saint-valentin .card.post:hover {
    background-color: #FFC2CD;
}

.easter .card.post:hover {
    background-color: #ffdd94;
}

.halloween .card.post:hover {
    background: rgba(51,3,64,0.2) !important;
}


@media (min-width: 320px){
    .xs\:h-screen {
        height: 80vh;
    }
}

// media queries

@media (min-width: 575px){
    .card-columns {
        column-count: 2;
    }

    .navbar-text.title-home {
        display: inline-block;
    }

    .push-category.btn-category {
        height:20vh;
        align-items: center;

        .card-body {
            align-items: center;
            display: flex;
        }
    }

    h1 {
        font-size: 1.8rem;
    }
    h1::first-letter {
        font-size: 2rem;
    }
    .navbar-nav {
        width: auto;
    }
    
    .search-container,.search-list-recipes {
        width:13rem;
    }

    article .search-container {
        display: block;
    }
}

@media (min-width: 768px){
    body {
        background: rgba(239,229,220,1);
        background: linear-gradient(180deg, rgba(208,184,172,1) 5vw, rgba(239,229,220,1) 100vw);
        background-attachment: fixed;

    }
    .container-lg {
        padding-left:7rem;
        padding-right: 7rem;
    }

    .card-columns {
        column-count: 2;
    }

    h1::first-letter {
        font-size: 3.2rem;
    }

    .navbar h1 {
        position: fixed;
        bottom: -3rem;
        left: 1.5rem;
        transform : rotate(270deg);
        transform-origin :0 0;
        color:rgba(132,87,76,0.3);
        font-size: 3.6rem;
    }

    .navbar h1::first-letter {
        font-size: 3.8rem;
    }

    .dropdown-item {
        padding-right: 0;
    }
    .dropdown-menu {
        display: block;
        visibility: hidden;
        opacity:0;transform: translateY(-3rem);
        transition:1s ease all;
        -webkit-transition:1s ease all;
    }
    .dropdown-menu.show {
        display: block;
        visibility: visible;
        opacity:1;transform: translateX(0);
        transition:1s ease all;
        -webkit-transition:1s ease all;
    }
    
    .navbar-expand .navbar-nav .dropdown-menu {
        background: transparent;
        border: 0;
        text-align: right;
    }
    .insta-link {
        display: flex;
    }

    .saint-valentin .card.post .card-body {
        padding-left: 0;
    }
    #posts .card {
        .card-body {
            padding-left: 0;
        }
        .img-container {
            overflow: hidden;
            height: initial;
            margin-bottom: 20px;
            margin-right: 0;
        }
    }
}



@media (min-width: 1024px){
    .search-container-home .search-recipes {
        font-size: 2.5rem;
    }
    
    article h1 {
        position: fixed;
        bottom: -3rem;
        left: 1.5rem;
        transform : rotate(270deg);
        transform-origin :0 0;
        color: #fff;
        text-shadow:0 0 20px rgba(132,87,76,1);
        width:75vh;
    }
    .lg\:h-full {
        height: 100%;
    }
    .card-columns {
        column-count: 3;
    }
}

@media (min-width: 1280px){

    .container-lg {
        padding-right: 12rem;
    }

}

@media (min-width: 1440px){
    .card-columns {
        column-count: 4;
    }

}


// animation

.animate {
    animation-fill-mode: both;
    -webkit-animation-fill-mode: both;
}
.animate1 {
    animation-duration: 1s;
    -webkit-animation-duration: 1s;
}

.animate3 {
    animation-duration: 2s;
    -webkit-animation-duration: 2s;
}

@keyframes slideup {
    0% {
        transform: translateY(4rem);
        opacity: 0;
    }
    100% {
        transform:translateY(0);
        opacity: 1;
    }
    0% {
        transform: translateY(4rem);
        opacity: 0;
    }
}
  
@-webkit-keyframes slideup {
    0% {
        -webkit-transform: transform;
        -webkit-opacity: 0;
    }
    100% {
        -webkit-transform: translateY(0);
        -webkit-opacity: 1;
    }
}

@keyframes slideup2 {
    0% {
        transform: translateY(2rem);
        opacity: 0;
    }
    100% {
        transform:translateY(0);
        opacity: 1;
    }
}
  
@-webkit-keyframes slideup2 {
    0% {
        -webkit-transform: transform;
        -webkit-opacity: 0;
    }
    100% {
        -webkit-transform: translateY(0);
        -webkit-opacity: 1;
    }
}

.slideUp {
    -webkit-animation-name: slideup;
    animation-name: slideup;
}

.slideUp2 {
    -webkit-animation-name: slideup2;
    animation-name: slideup2;
}


@keyframes blur {
    0%,
    90% {
      -webkit-filter: blur(0);
      -moz-filter: blur(0);
      -o-filter: blur(0);
      -ms-filter: blur(0);
    }
    50% {
      -webkit-filter: blur(10px);
      -moz-filter: blur(10px);
      -o-filter: blur(10px);
      -ms-filter: blur(10px);
    }
  }

</style>
