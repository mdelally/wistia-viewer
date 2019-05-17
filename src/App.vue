<template>
    <div id="app">

        <div class="container mx-auto text-grey-dark">
            <header class="w-full p-3 flex justify-between align-center mt-4">
                <h1 class="text-4xl text-orange-600">wistia viewer</h1>

                <input type="text"
                       class="bg-gray-700 p-2 rounded shadow-md w-1/4"
                       placeholder="Search..."
                       v-model="search"
                >
            </header>

            <section v-if="medias.length === 0"
                class="text-center m-16 text-gray-300"
            >
                <h2 class="text-3xl"><em>Polling videos...</em></h2>
            </section>
            
            <section class="flex align-center justify-end p-3">
                <span class="p-2 mr-2 text-gray-700 font-bold">Toggle View</span>
                <button class="bg-orange-600 hover:bg-orange-800 w-8 p-2 rounded mr-2"
                        :class="{ 'bg-orange-500': view === 'list' }"
                        @click="view = 'list'"
                >
                    <svg class="fill-current text-orange-200" xmlns="http://www.w3.org/2000/svg"
                         viewBox="0 0 20 20">
                        <path d="M0 3h20v2H0V3zm0 4h20v2H0V7zm0 4h20v2H0v-2zm0 4h20v2H0v-2z"/>
                    </svg>
                </button>
                <button class="bg-orange-600 hover:bg-orange-800 w-8 p-2 rounded"
                        :class="{ 'bg-orange-500': view === 'grid' }"
                        @click="view = 'grid'"
                >
                    <svg class="fill-current text-orange-200" xmlns="http://www.w3.org/2000/svg" viewBox="0 0 20 20">
                        <path d="M0 0h9v9H0V0zm2 2v5h5V2H2zm-2 9h9v9H0v-9zm2 2v5h5v-5H2zm9-13h9v9h-9V0zm2 2v5h5V2h-5zm-2 9h9v9h-9v-9zm2 2v5h5v-5h-5z"/>
                    </svg>
                </button>
            </section>

            <section class="bg-gray-800 p-4 my-8 md:rounded-lg text-gray-500" v-if="medias.length > 0">

                <div v-if="medias.length === searchedMedia.length">
                    <h3 class="text-2xl">Showing all {{ medias.length }} videos.</h3>
                </div>
                <div v-else>
                    <h3 class="text-2xl">
                        Showing {{ searchedMedia.length }} videos containing <em class="text-orange-600">{{ search
                        }}</em>.
                    </h3>
                </div>

                <transition-group name="fade" mode="out-in" class="flex flex-wrap justify-center"
                    :class="{
                        'flex-row': view === 'grid',
                        'flex-col': view === 'list'
                     }"
                >
                    <media v-for="(media, index) in searchedMedia"
                           :key="'media_' + index"
                           :media="media"
                    ></media>
                </transition-group>
            </section>

            <footer class="text-center mt-4 mb-8 text-gray-700 font-bold">
                Built by <a href="https://github.com/mdelally" class="text-orange-600 hover:text-orange-900">Michael
                DeLally</a>
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
        view: 'list',
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
    body {
        @apply bg-gray-900 border-t-4 border-orange-700;
    }

    .fade-enter-active, .fade-leave-active {
        transition: all .5s ease-in-out;
    }
    .fade-enter, .fade-leave-to /* .fade-leave-active below version 2.1.8 */ {
        opacity: 0;
        transform: translateX(16px);
    }
</style>
