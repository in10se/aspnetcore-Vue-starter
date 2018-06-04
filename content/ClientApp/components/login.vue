<template>
  <div>
    <h1>Login</h1>

    <p>Login here.</p>

    <template>
      <div>
        <form class="login" @submit.prevent="login">
          <h1>Sign in</h1>
          <label>User name</label>
          <input required v-model="username" type="text" placeholder="Your username here" />
          <label>Password</label>
          <input required v-model="password" type="password" placeholder="Password" />
          <hr />
          <button type="submit">Login</button>
        </form>
      </div>
    </template>
  </div>
</template>

<script>
  import { AUTH_REQUEST } from  '../store/actions/auth'

  export default {
    name: 'login',
    computed: {
      totalPages: function () {
        return Math.ceil(this.total / this.pageSize)
      }
    },

    data () {
      return {
        forecasts: null,
        total: 0,
        pageSize: 5,
        currentPage: 1
      }
    },

    methods: {
      async loadPage (page) {
        // ES2017 async/await syntax via babel-plugin-transform-async-to-generator
        // TypeScript can also transpile async/await down to ES5
        this.currentPage = page

        try {
          var from = (page - 1) * (this.pageSize)
          var to = from + this.pageSize
          let response = await this.$http.get(`/api/weather/forecasts?from=${from}&to=${to}`)
          console.log(response.data.forecasts)
          this.forecasts = response.data.forecasts
          this.total = response.data.total
        } catch (err) {
          window.alert(err)
          console.log(err)
        }
        // Old promise-based approach
        // this.$http
        //    .get('/api/SampleData/WeatherForecasts')
        //    .then(response => {
        //        console.log(response.data)
        //        this.forecasts = response.data
        //    })
        //    .catch((error) => console.log(error))*/
      },
      login: function () {
        const { username, password } = this
        this.$store.dispatch(AUTH_REQUEST, { username, password }).then(() => {
          this.$router.push('/')
        })
      }
    },

    async created () {
      this.loadPage(1)
    }
  }
</script>

<style>
</style>
