<template>
    <nav class="navbar container" role="navigation" aria-label="main navigation">
        <div class="navbar-brand">
            <a class="navbar-item" href="/">
                <strong class="is-size-3">MandiDaliya</strong>
            </a>
            <div class="navbar-burger" @click="showNav = !showNav" :class="{ 'is-active': showNav }">
                <span ></span>
                <span ></span>
                <span ></span>
                <span ></span>
            </div>
        </div>
        <div class="navbar-menu" :class="{ 'is-active': showNav }">
            <div class="navbar-start">
                <router-link to="/" class="navbar-item">Home</router-link>
                <router-link to="/mandi" class="navbar-item">Mandi</router-link>
                <router-link to="/favourites" class="navbar-item">Favourites</router-link>
                <router-link to="/about" class="navbar-item">About</router-link>
            </div>
            <div class="navbar-end">
                <div class="navbar-item">
                    <button class="button is-block is-danger" v-if="user.authenticated"  @click="logout">Logout</button>
                    <Login v-if="!user.authenticated"></Login>
                    <Signup v-if="!user.authenticated"></Signup>
                </div>
            </div>
        </div>
    </nav>
</template>

<script>
import Login from '@/components/auth/Login';
import Signup from '@/components/auth/Signup';
import axios from 'axios';
import router from '../../router';
export default {
    name: 'Nav',
    components: {
        Login,
        Signup,
    },
    data() {
      return {
          showNav: false,
          user:  {
            name: "DummyUser",
            authenticated: false,
          },
      }
  },
  methods: {
      getUserData: function (){
          let self = this;
          axios.get('/api/user')
          .then((response) => {
              self.$set(this, "user", response.data.user);
          })
          .catch((errors) => {
              console.log(errors);
          });
      },
      logout: function(e){
        axios.get('/api/logout')
        .then(() => {
            router.push('/');
            location.reload();
        })
        .catch(() => {
            console.log(e);
        })
    },
  },
  mounted() {
      this.getUserData();
  },

}
</script>
<style lang="scss" scoped>
    nav {
        margin-top: 25px;
        margin-bottom: 30px;
        a {
            font-weight: bold;
            color: #2c3e50;
            &.router-link-exact-active {
                color: #d88d00;
            }
        }
    }
    .navbar-end {
        .a {
            color: white;
        }
    }
</style>