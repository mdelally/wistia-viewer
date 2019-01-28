<template>
    <div id="app">

        <div class="container mx-auto max-w-lg text-grey-darkest">
            <header class="w-full p-3 flex justify-between align-center">
                <h1>wistia viewer</h1>

                <input type="text"
                       class="bg-grey-light p-2 rounded"
                       placeholder="Search..."
                       v-model="search"
                >
            </header>

            <div v-if="medias.length === 0"
                class="text-center m-16 text-grey-dark"
            >
                <h2>Polling videos...</h2>
            </div>

            <div class="bg-grey-lighter p-4 my-8 md:rounded-lg" v-if="medias.length > 0">

                <div v-if="medias.length === searchedMedia.length">
                    <h3>Showing all {{ medias.length }} videos.</h3>
                </div>
                <div v-else>
                    <h3>
                        Showing {{ searchedMedia.length }} videos containing <em class="text-blue">{{ search }}</em>.
                    </h3>
                </div>

                <media v-for="(media, index) in searchedMedia"
                       :key="index"
                       :media="media"
                ></media>
            </div>

            <footer class="text-center mt-4 mb-8 text-grey-light">
                Built by Michael DeLally
            </footer>
        </div>

    </div>
</template>

<script>

  import Media from "./components/Media";
  export default {
    name: 'app',
    components: {Media},
    data() {
      return {
        medias: [],
        search: '',
        token: process.env.VUE_APP_WISTIA_TOKEN
      }
    },

    computed: {
      searchedMedia() {
        return this.medias.filter((media) => {


          return media.name.toLowerCase().includes(this.search.toLowerCase());
        });
      }
    },

    mounted() {
      this.$http
        .get('https://api.wistia.com/v1/medias.json?api_password=' + this.token)
        .then((response) => {
          this.medias = response.data;
        });
    }
  }
</script>

<style>

</style>
