<template>
    <div class="flex justify-center py-40">
    <div class="w-full max-w-sm">
        <div class="bg-white shadow-md rounded px-8 pt-6 pb-8 mb-4">
  
        
            <div class="mb-4"><div class="flex justify-between"><div>Total</div><div>{{ total(trolly) | convertToRupiah }}</div></div></div>
            <div class="flex items-center justify-center">
                <button type="button" class="bg-blue-400 hover:bg-orange-500 text-white font-bold py-2 px-4 rounded focus:outline-none" @click="pay()"> Pay </button>
            </div>
        </div>
    </div>
</div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  computed: {
    ...mapGetters({
      trolly: 'trolly',
      counterTrolly: 'counterTrolly'
    }),
  },
  methods:{
    ...mapActions({
      printBill: 'printBill',
    }),
    cancel(){
      this.$router.push({ path: this.$route.query.back });
    },
    pay(){
      this.printBill();
      this.$router.push({ name: 'product' });
    },
    dataLength(a){
      return this.trolly.filter(d => d.id === a.id).length;
    },
    subtotal(a){
      return a.price * this.dataLength(a);
    },
    total(a){
      return a.reduce(function(total, num){
        return total + num.price
      }, 0);
    },
  },
  beforeRouteEnter (to, from, next) {
    $cookies.get('session') ? next() : next({ name: 'login', query: { redirect: 'product' } })
  },
}
</script>

<style>
</style>
