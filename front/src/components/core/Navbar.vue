<template>
  <nav class="navbar bg-white fixed-top navbar-expand navbar-light mb-4 px-md-5 px-2">
      <a v-if="backRoute" @click="goBackOne">
              <img class="goBackArrow" src="/assets/back.svg" />
      </a>
    <img @click="goHomePage" src="/assets/logo.svg" class="logoNav me-4 ms-3" alt="logo">
    <button class="navbar-toggler" type="button" data-toggle="collapse" data-target="#navbarNavAltMarkup" aria-controls="navbarNavAltMarkup" aria-expanded="false" aria-label="Toggle navigation">
      <span class="navbar-toggler-icon"></span>
    </button>
    <div class="collapse navbar-collapse me-3" id="navbarNavAltMarkup">
      <div v-if="isAdmin" class="navbar-nav w-100">
        <router-link class="nav-item nav-link" to="/auth">Home</router-link>
        <router-link class="nav-item nav-link" to="/auth/users/">Utenti</router-link>
        <router-link class="nav-item nav-link" to="/auth/config">Configurazione</router-link>
        <router-link @click.prevent="logout" class="nav-item nav-link ms-auto" to="/logout">Logout</router-link>
      </div>
      <div v-else-if="isAdmin === false" class="navbar-nav w-100">

        <img class="profileImage ms-auto" src="/assets/profilo.svg" @click="profile" />
      </div>
      <div v-else class="navbar-nav">
<!--        <router-link class="nav-item nav-link" to="/login">Login</router-link>-->
      </div>
    </div>
  </nav>

</template>

<script>
export default {
  name: "Navbar",
  props:{
    isAdmin: {},
  },
  methods:{
    logout() {
      this.$store.dispatch('logout')
    },
    profile() {
      this.$router.push({ name: "Profile" });
    },
    goBackOne(){
      return this.$router.push(this.backRoute)
    },
    goHomePage(){
      return this.$router.push({name: 'Home'})
    }
  },


  computed:{
    // checkRoute(){
    //   return this.$route.name
    // },
    backRoute(){
      return this.$store.getters.getBackPage
    }
  },


}
</script>

<style scoped lang="scss">
@import "src/sass/variables.scss";
.logoNav{
  width: 64px;
  display: block;
}
.navbar{

}
.goBackArrow{
  cursor: pointer;
  margin-left: 10px;
  margin-right: 10px;
  width: 18px;
}

.profileImage{
  cursor: pointer;

  width: 42px;
}
</style>
