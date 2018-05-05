<template>
  <div class="container">
    <h1>スプラライブ</h1>
    <div class="media" v-for="movie in movies" :key="movie.etag">
      <a :href="movie.id.videoId | url" target="_blank">
        <img class="d-flex mr-3 img" :src="movie.snippet.thumbnails.medium.url">
      </a>
      <div class="media-body">
        <a :href="movie.id.videoId | url" target="_blank">
          <h5 class="mt-0">{{ movie.snippet.title }}</h5>
        </a>
        <h5 class="mt-0">{{ movie.snippet.title }}</h5>
        <span class="text-muted">{{ movie.snippet.started_at | fromNow }}</span>
      </div>
    </div>
  </div>
</template>

<script>
import moment from 'moment-timezone'
moment.locale('ja')

export default {
  async asyncData({ params, app}) {
    const api_key = 'AIzaSyBP6-9ekseVKVlXdmdSW1xoeWqsvMae5FU'
    const word = encodeURIComponent('スプラトゥーン')
    const url = `https://www.googleapis.com/youtube/v3/search?q=${word}&key=${api_key}&maxResults=50&type=video&part=snippet&order=date`
    const result = await app.$axios.$get(url)
    return { movies: result.items }
  },
  filters: {
    fromNow: function (date) {
      return moment(date).tz('Asia/Tokyo').fromNow()
    },
    url: function(id) {
      return `https://www.youtube.com/watch?v=${id}`
    }
  }
}
</script>

<style>
.img {
  width: 200px;
}
</style>
