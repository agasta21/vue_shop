<template>
  <div class="fixed w-64 trolly top-0 right-0 bg-blue-800" :class="{ show : ShowTrolly && !emptyTrolly }">
    <div class="relative min-h-screen py-16 px-2 flex justify-center items-center">
      <div class="trolly__wrapper w-full overflow-hidden">
        <div class="w-full bg-blue-200 mb-2 p-2 rounded" v-for="(data, index) in sumTrolly" :key="index">
          <div class="pb-1 text-blue-800">
            {{ data.name }}
          </div>
          <hr class="border-blue-800">
          <div class="pt-1 flex justify-between text-blue-800">
            <div>{{ data.price | convertToRupiah }} <span class="font-bold">x {{ itemLength(data) }}</span></div>
            <div class="font-bold">{{ subtotal(data) | convertToRupiah}}</div>
          </div>
          <div class="flex justify-between pt-2">
            <div class="w-2/4">
              <button class="bg-blue-400 hover:bg-blue-500 text-white font-bold rounded focus:outline-none px-2" @click="remove(data)">Remove</button>
            </div>
            <div class="flex w-2/4 justify-end">
              <button class="bg-blue-400 hover:bg-blue-500 text-white font-bold mr-1 rounded focus:outline-none w-1/4" @click="increase(data)">+</button>
              <button class="bg-blue-400 hover:bg-blue-500 text-white font-bold rounded focus:outline-none w-1/4" @click="decrease(data)">-</button>
            </div>
          </div>
        </div>
        <div class="mb-2 text-white text-right">
          Total: <span class="font-bold text-2xl">{{ total(trolly) | convertToRupiah }}</span>
        </div>
        <button class="mb-4 bg-blue-400 hover:bg-blue-500 text-white font-bold py-2 px-4 rounded focus:outline-none w-full" @click="checkOut(sumTrolly)">
          Checkout
        </button>
        <button class="bg-blue-400 hover:bg-blue-500 text-white font-bold py-2 px-4 rounded focus:outline-none w-full" @click="ShowTrolly =! ShowTrolly">
          Close
        </button>
      </div>
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  data(){
    return{
      ShowTrolly: false,
      emptyTrolly: true
    }
  },
  computed: {
    ...mapGetters({
      trolly: 'trolly',
      sumTrolly: 'sumTrolly'
    }),
  },
  watch:{
    trolly(){
      if(this.trolly != ''){
        this.emptyTrolly = false
        this.ShowTrolly = true
      }else{
        this.emptyTrolly = true
        this.ShowTrolly = false
      }
    }
  },
  methods:{
    ...mapActions({
      Trollyadd: 'increaseTrolly',
      decreaseTrolly: 'decreaseTrolly',
      removeTrolly: 'removeTrolly'
    }),
    checkOut(a){
      this.ShowTrolly = false;
      this.$router.push({ name: 'checkout', query: { back : this.$route.fullPath } });
    },
    itemLength(a){
      return this.trolly.filter(d => d.id === a.id).length;
    },
    subtotal(a){
      return a.price * this.itemLength(a);
    },
    total(a){
      return a.reduce(function(total, num){
        return total + num.price
      }, 0);
    },
    increase(a){
      this.Trollyadd(a);
    },
    decrease(a){
      this.decreaseTrolly(a);
    },
    remove(a){
      this.removeTrolly(a);
    }
  }
}
</script>
<style scoped>
  .trolly{
    transform: translateX(100%);
    transition: transform .4s cubic-bezier(.46,.54,.17,1);
  }
  .trolly.show{
    transform: translateX(0%);
  }
  .trolly__wrapper{
    overflow-y: scroll;
    height: 90vh;
    padding-right: 8px;
  }
  
</style>