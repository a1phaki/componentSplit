<script setup>
defineProps({
  products:{
    type:Array,
    default:()=>[],
  },
  addCart:{
    type:Function,
    default:()=>()=>{},
  }
});


</script>

<template>
  <h2 class="mb-3">商品列表</h2>
  <div class="row">
    <div class="col-md-4 mb-4" v-for="product in products" :key="product.id">
      <div class="card h-100">
        <img :src="product.imgUrl" class="card-img-top">
        <div class="card-body">
          <h5 class="card-title">{{ product.title }}</h5>
          <p class="card-text">{{ product.description }}</p>
          <p class="fw-bold text-primary">$ {{ product.price }}</p>
          <button class="btn btn-success w-100" @click="addCart(product.id)"> 加入購物車 </button>
        </div>
      </div>
    </div>
  </div>
</template>

<style scoped>
.card-img-top {
  height: 150px;
  object-fit: cover;
}
</style>
