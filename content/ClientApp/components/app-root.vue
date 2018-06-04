<template>
    <div id="app" class="container-fluid">
        <div class="row">
            <div class="col-md-3">
                <nav-menu params="route: route"></nav-menu>
            </div>
            <div class="col-sm-9">
                <router-view></router-view>
            </div>
        </div>
    </div>
</template>

<script>
  import NavMenu from './nav-menu'
  import { USER_REQUEST } from '../store/actions/user'

    export default {
      components: {
        'nav-menu': NavMenu
      },
    created: function () {
      if (this.$store.getters.isAuthenticated) {
        this.$store.dispatch(USER_REQUEST)
      }
    },
    data () {
        return {}
      }
    }
</script>

<style>
</style>
