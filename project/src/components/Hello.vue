<template>
    <div class="hello">
        <header>
            <meta name="viewport" content="width=device-width, initial-scale=1, maximum-scale=1.0, user-scalable=no"/>

            <img class="logo" src="../assets/img/logo-chaordic.png" alt="Chaordic">
        </header>
        <div class="parallax-container">
            <div class="parallax">
                <h1 class="txt-branco roboto-slab">Encurte seus links.</h1>
                <p class="txt-branco">Links são longos. Encurte os links que você deseja compartilhar,</p>
                <p class="txt-branco">e acompanhe enquanto viajam pela internet.</p>
                <br><br>
                <form>
                    <input type="text" v-model="url"><button v-on:click.once="encurtar">{{ button }}</button>
                </form>
            </div>
        </div>

        <section class="links">
            <nav>
                <h2 class="color-primary roboto-slab">TOP 5</h2>

                <ul v-for="top in top5">
                    <li class="color-primary bold">{{ top.shortUrl }}</li>
                    <li class="color-apoio">{{ top.hits }}</li>
                </ul>
            </nav>
        </section>

        <section class="bg-cinza">
            <div class="arrow-down"></div>
            <h2 class="color-primary bold roboto-slab">HITS</h2>
            <h2 class="color-primary hits">35.713.571</h2>
            <p class="color-apoio bold bg-branco">Cliques em links</p>
        </section>

        <footer>
            <p class="color-primary align-left">chr.dc</p>
            <img class="social" src="../assets/img/icon-twitter.png">
            <img class="social" src="../assets/img/icon-facebook.png">
        </footer>
    </div>
</template>

<script>
  import axios from 'axios'

  export default {
    name: 'hello',
    data () {
      return {
        url: 'Cole a sua URL aqui',
        button: 'ENCURTAR',
        urlJSON: [],
        top5: []
      }
    },
    filters: {
      truncate: function (v) {
        var newline = v.indexOf('\n')
        return newline > 0 ? v.slice(0, newline) : v
      }
    },
    created: function () {
      this.fetchData()
    },
    methods: {
      fetchData: function () {
        axios.get('https://raw.githubusercontent.com/chaordic/frontend-intern-challenge/master/Assets/urls.json')
          .then(response => {
            this.urlJSON = response.data

            this.urlJSON.sort(function (a, b) {
              return b.hits - a.hits
            })

            this.top5 = this.urlJSON.slice(0, 5)
          })
      },
      encurtar: function () {
        this.button = 'COPIAR'

        for (var i = 0, l = Object.keys(this.urlJSON).length; i <= l; i++) {
          if (this.url === this.urlJSON[i].url) {
            this.url = this.urlJSON[i].shortUrl
            return this.url
          }
          console.log(this.urlJSON[i].url)
        }
      }
    }
  }
</script>
