<template>
  <div class="container clearfix">
    <div v-if="loading" class="loader"></div>
    <div>
      <div class="text-right my-1">
        <p class="commentDate">{{ $t('news.added') }} {{ convertTimeStamp }}</p>
      </div>
      <h6 class="commentAuthor text-left">
        {{currentUser.name}} {{currentUser.surname}}
      </h6>
      <h6 class="commentContent">
        <router-link :to="{name: 'petDetails', params: { id: comment.petUuid }}" class="news-title">
          {{ comment.contents }}
        </router-link>
      </h6>
    </div>
  </div>
</template>
<script>
  import moment from 'moment'
  export default {
    data () {
      return {
        currentUser: {
          name: '',
          surname: ''
        },
        loading: true
      }
    },
    props: ['comment'],
    computed: {
      convertTimeStamp () {
        const date = moment(this.comment.created).locale(this.$t('common.code'))
        return date.format(this.$t('common.dateFormat'))
      }
    },
    created () {
      this.$http.get(this.apiUrl + 'users/' + this.comment.userUuid)
        .then(response => {
          this.currentUser.name = response.data.name
          this.currentUser.surname = response.data.surname
          this.loading = false
        })
        .catch(() => {
          this.loading = false
        })
    }
  }
</script>
<style lang="sass">
  @import "../assets/styles/loader.css"
  @import "../assets/styles/variables.sass"

  .commentDate
    margin: 0
    font-size: 1.2em

  .commentAuthor
    margin: 0 0 .2em 0
    padding: 0
    font-size: 1.8em

  .commentContent
    font-size: 1.6em
    border-bottom: 1px solid $silver
    padding-bottom: 1em
    a
      color: $green
      text-decoration: none
</style>
