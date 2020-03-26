<template>
    <div>
      <app-header></app-header>
      <div class="py-24 px-24 mx-auto w-full"> 
        <div class="w-full flex flex-wrap">
          <div class="p-5 w-1/4" v-for="(data, index) in categoryItem" :key="index">
            <app-card :item="data"></app-card>
          </div>
        </div>
      </div>
    </div>
</template>
<script>
import AppHeader from './Header.vue'
import AppCard from './Card.vue'
import { mapGetters, mapActions } from 'vuex'
export default {
  components:{
      AppHeader,
      AppCard
  },
  data(){
    return{
      cat: this.$route.query.category,
    }
  },
  watch:{
    '$route'(to, from){
      this.cat = to.query.category;
      this.fetchData();
    }
  },
  methods:{
    ...mapActions({
      fetchData: 'fetchData'
    })
  },
  computed: {
    ...mapGetters({
      products: 'products',
    }),
    categoryItem(){
      return this.cat === undefined ? this.products : this.products.filter(d => d.category == this.cat)
    }
  },
  created(){
    this.fetchData();
  },
  beforeRouteEnter (to, from, next) {
    $cookies.get('session') ? next() : next({ name: 'login', query: { redirect: 'product' } })
  },
}
</script>