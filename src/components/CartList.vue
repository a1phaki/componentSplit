<script setup>
defineProps({
  CartList:{
    type:Array,
    default:()=>[],
  }
});

const emit = defineEmits(['deleteCart']);

const handleClick = (id) => {
  emit('deleteCart',id);
}

</script>

<template>
  <h2 class="mb-3">購物車</h2>
      <ul class="list-group mb-3">
        <li class="list-group-item d-flex justify-content-between align-items-center" v-for="(cart,index) in CartList" :key="index">
          <div>
            <h6 class="my-0">{{ cart.name }}</h6>
            <small class="text-muted">數量：{{ cart.count }}</small>
          </div>
          <div>
            <span class="text-muted">${{ cart.price*cart.count }}</span>
            <button class="btn btn-sm btn-outline-danger ms-2" @click="handleClick(cart.id)"> 移除 </button>
          </div>
        </li>
      </ul>
</template>