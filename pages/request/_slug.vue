<template>
  <div class="container">
    <div class="markdown-body">
      <vue-markdown :source="data.pull_request.body" />
    </div>
    <div class="form-container">
      <div class="form-header">
        <img class="avatar" :src="data.owner.avatar_url" />
        <p class="request">request approval</p>
      </div>
      <div class="form-content">
        <div class="comment-approve">
          <input type="text" class="comment" v-model="comment" />
          <button class="button" @click="handleClick">
            {{ approved ? 'disapprove' : 'approve' }}
          </button>
        </div>
        <div class="submit">
          <button class="submit-button" @click="handleSubmit">Submit</button>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import VueMarkdown from 'vue-markdown'
import axios from 'axios'
import 'github-markdown-css'

export default {
  asyncData({ route: { params } }) {
    return axios
      .get(`http://approvli.herokuapp.com/checks/${params.slug}`)
      .then(function({ data }) {
        return { data }
      })
      .catch(function(error) {
        console.log({ error })
      })
  },
  components: {
    'vue-markdown': VueMarkdown
  },
  data() {
    return {
      approved: false,
      comment: '',
      done: false
    }
  },
  methods: {
    handleClick() {
      return (this.approved = !this.approved)
    },
    handleSubmit() {
      return axios
        .post(
          `http://approvli.herokuapp.com/checks/${this.$route.params.slug}`,
          {
            approved: this.approved,
            comment: this.comment
          }
        )
        .then(function() {
          console.log('done')
          return { done: true }
        })
    }
  }
}
</script>

<style>
/* Sample `apply` at-rules with Tailwind CSS
.container {
  @apply min-h-screen flex justify-center items-center text-center mx-auto;
}
*/
.container {
  display: flex;
  flex-direction: row;
  align-items: center;
}
.markdown-body {
  background: #d3d3d3;
  box-sizing: border-box;
  margin: 0 auto;
  max-width: 60%;
  padding: 45px;
}
.form-container {
  width: 30%;
}
.form-header {
  background: #778899;
  display: flex;
  flex-direction: row;
  padding: 20px;
  justify-content: space-between;
  align-items: center;
}
.request {
  border: 1px solid white;
  border-radius: 25px;
  text-align: center;
  padding: 0 15px;
  text-transform: capitalize;
}
.avatar {
  height: 50px;
  width: 50px;
}
.comment-approve {
  background: black;
  display: flex;
  flex-direction: column;
  align-items: center;
}
.comment {
  padding: 20px;
  height: 150px;
  width: 85%;
  margin-top: 20px;
}
.button {
  color: white;
}
.submit {
  background: pink;
  margin-top: 20px;
  align-items: center;
  display: flex;
}
.submit-button {
  background: red;
  width: 100%;
  margin: 20px;
}
</style>
