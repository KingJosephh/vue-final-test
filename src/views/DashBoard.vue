<template>
  <nav-bar></nav-bar>
    <router-view/>
</template>

<script>
import NavBar from '../components/NavBar.vue'
export default {
  components: { NavBar },
  created () {
    const token = document.cookie.replace(/(?:(?:^|.*;\s*)hexToken\s*=\s*([^;]*).*$)|^.*$/, '$1')
    console.log(token)
    this.$http.defaults.headers.common.Authorization = token
    const api = `${process.env.VUE_APP_API}api/user/check`
    console.log(api)
    this.$http.post(api, this.user)
      .then((res) => {
        if (!res.data.success) {
          this.$router.push('/login')
        }
      })
  }
}
</script>
