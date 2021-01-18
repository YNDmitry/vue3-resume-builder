<template>
  <app-loader v-if="loader"></app-loader>
  <template v-else>
    <p>
      <app-btn
        v-if="loadButton"
        :color="'primary'"
        @action="loadComments"
      >Загрузить комментарии</app-btn>
    </p>

    <div class="card" v-if="comments.length">
      <h2>Комментарии</h2>
      <ul class="list">
        <li class="list-item" v-for="comment in comments" :key="comment.id">
          <div>
            <app-email>{{ comment.email }}</app-email>
            <app-text>{{ comment.body }}</app-text>
          </div>
        </li>
      </ul>
      <p>
        <app-btn
          :color="'danger'"
          @action="hideComments"
        >Скрыть комментарии</app-btn>
      </p>
    </div>
  </template>
</template>

<script>
import AppEmail from './CommentsElements/AppEmail'
import AppText from './CommentsElements/AppText'
import AppLoader from './uiElements/AppLoader.vue'
import AppBtn from './uiElements/AppBtn.vue'
import axios from 'axios'
export default {
  props: ['alert'],
  data () {
    return {
      comments: [],
      loader: false,
      commentsDataLink: 'https://jsonplaceholder.typicode.com/comments?_limit=5'
    }
  },
  methods: {
    async loadComments () {
      try {
        this.loader = true
        const { data } = await axios.get(this.commentsDataLink)
        this.comments = Object.keys(data).map(key => {
          return {
            id: key,
            ...data[key]
          }
        })
        this.loader = false
      } catch (error) {
        console.log(error)
        this.loader = false
      }
    },
    hideComments () {
      this.comments = []
    }
  },
  computed: {
    loadButton () {
      return this.comments.length === 0
    }
  },
  components: {
    AppEmail,
    AppText,
    AppBtn,
    AppLoader
  }
}
</script>

<style></style>
