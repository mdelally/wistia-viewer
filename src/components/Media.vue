<template>
    <div class="bg-gray-700 p-2 m-2 rounded-lg shadow-md flex-1 flex-grow-0">
        <div class="flex flex-wrap md:justify-start justify-center">
            <img :src="media.thumbnail.url" class="rounded-lg">
            <div class="p-2">
                <h3>
                    <a :href="'https://pli.wistia.com/medias/' + media.hashed_id"
                       class="text-orange-600 hover:text-orange-400 font-bold text-xl"
                    >
                        <span v-if="media.name.length > 50">
                            {{ media.name.substring(0, 50) }}...
                        </span>
                        <span v-else>
                            {{ media.name }}
                        </span>
                    </a>
                </h3>

                <div class="p-2 bg-gray-600 text-gray-800 rounded-lg mt-1">
                    <small><strong>URL</strong></small>
                    <br>
                    <span class="console">
                        https://pli.wistia.com/medias/{{ media.hashed_id.trim() }}
                    </span>
                </div>
            </div>
        </div>
    </div>
</template>

<script>
  export default {
    name: 'media',
    props: ['media']
  }
</script>

<style lang="scss">
    span.console {
        font-family: 'Consolas', 'Courier New', monospace;
    }
</style>
