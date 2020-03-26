<template>
    <div class="w-full flex justify-center items-center min-h-screen">   
        
        <div class="w-full max-w-xs">
          <form class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4" @submit.prevent="logIn()">
            <div class="mb-4">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="username">
                Username
              </label>
              <input class="shadow appearance-none border rounded w-full py-2 px-3 text-gray-700 leading-tight focus:outline-none focus:shadow-outline" id="username" type="text" placeholder="Username" v-model="data.username">
            </div>
            <div class="mb-6">
              <label class="block text-gray-700 text-sm font-bold mb-2" for="password">
                Password
              </label>
              <input class="shadow appearance-none border border rounded w-full py-2 px-3 text-gray-700 mb-3 leading-tight focus:outline-none focus:shadow-outline" id="password" type="password" v-model="data.password">
            </div>
            <div class="flex items-center justify-between">
              <button type="submit" class="bg-blue-500 hover:bg-blue-700 text-white font-bold py-2 px-4 rounded focus:outline-none focus:shadow-outline">
                Sign In
              </button>
              <a class="inline-block align-baseline font-bold text-sm text-blue-500 hover:text-blue-800" href="#">
                Forgot Password?
              </a>
            </div>
          </form>
          <p class="text-center text-gray-500 text-xs">
            &copy;2020 Acme Corp. All rights reserved.
          </p>
        </div>
        <div>{{ this.users.username }}</div>
    </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
    data(){
        return{
            data:{
              username: '',
              password: ''
            },
        }
    },
    methods: {
        ...mapActions({
          fetchUser: 'fetchUser'
        }),
        logIn(){
          var getIndexUser = this.users.findIndex(obj => obj.username === this.data.username && obj.password === this.data.password);
          var getRedircet = this.$route.query.hasOwnProperty('redirect') ? this.$route.query.redirect : 'product';
          if(getIndexUser == -1) {
            this.loginError = true;
            return
          }

          

          this.$cookies.set('session',this.users[getIndexUser].username,'23h');
          this.$router.push({ name: getRedircet });
        }
    },
    computed: {
      ...mapGetters({
        users: 'users',
      }),
    },
    created(){
      this.fetchUser();
    },
    beforeRouteEnter (to, from, next) {
      $cookies.get('session') ? next({ name: 'product' }) : next()
    },
}
</script>

<style>
</style>
