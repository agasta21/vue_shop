<template>
  <div>
    <div class="max-w-sm rounded overflow-hidden bg-white">
      <img class="w-full p-2" :src="item.image" :alt="item.name" >
        <div class="px-6 py-4">
          <div class="font-bold text-lg mb-2 ">{{ item.name }}</div>
          <div class="mb-2 text-blue-700">Price: <span class="font-bold">{{ item.price | convertToRupiah }}</span></div>
          <p class="text-black-700 text-xs mb-3">
            {{ item.description }}
          </p>
        
        
          <button class="bg-blue-400 hover:bg-blue-500 text-white font-bold py-2 px-4 rounded focus:outline-none" @click="send(item)">
          Add to Cart
        </button>
        </div>
        
    </div>
  </div>
</template>

<script>
import { mapGetters, mapActions } from 'vuex'
export default {
  props: {
    item : {
      type: Object,
      default: null
    }
  },
  methods:{
    ...mapActions({
      Trollyadd: 'increaseTrolly'
    }),
    send(a){
      this.Trollyadd(a);
    }
  }
}
</script>