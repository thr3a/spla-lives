<template>
  <div class="container">
    <nav class="navbar fixed-top navbar-dark bg-danger">
      <a class="navbar-brand" href="/">スプラライブ検索</a>
    </nav>
    <p>Youtubeで生放送中のスプラトゥーン動画を表示します</p>
    <div class="media mt-1" v-for="movie in movies" :key="movie.etag">
      <a :href="movie.id.videoId | url" target="_blank">
        <img class="d-flex mr-3 img" :src="movie.snippet.thumbnails.medium.url">
      </a>
      <div class="media-body">
        <a :href="movie.id.videoId | url" target="_blank">
          <h5 class="mt-0">{{ movie.snippet.title | truncate(30) }}</h5>
        </a>
        <span class="text-muted">{{ movie.snippet.publishedAt | fromNow }}</span>
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
    const url = `https://www.googleapis.com/youtube/v3/search?q=${word}&key=${api_key}&maxResults=50&type=video&part=snippet&order=date&eventType=live`
    const result = await app.$axios.$get(url)
    return { movies: result.items }
  },
  filters: {
    fromNow: function (date) {
      return moment(date).tz('Asia/Tokyo').fromNow()
    },
    url: function(id) {
      return `https://www.youtube.com/watch?v=${id}`
    },
    truncate: function(str, num) {
      return str.substring(0, num);
    }
  }
}
</script>

<style>
.container {
  padding-top: 80px;
}
.img {
  width: 200px;
}
</style>
