<script setup>
import { ref,computed,provide } from 'vue';
import ProductList from './components/ProductList.vue';
import CartList from './components/CartList.vue';
import ToastView from './components/ToastView.vue';

const products = [
  {
    id:1,
    title: "耳罩式藍牙耳機",
    description: "舒適配戴，支援降噪技術",
    price: 2490,
    imgUrl:'https://images.unsplash.com/photo-1546435770-a3e426bf472b?q=80&amp;w=2065&amp;auto=format&amp;fit=crop&amp;ixlib=rb-4.1.0&amp;ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
  },
  {
    id:2,
    title: "真無線藍牙耳機",
    description: "輕巧便攜",
    price: 1990,
    imgUrl:'https://images.unsplash.com/photo-1722448113450-f6860b7fbfe5?q=80&w=1035&auto=format&fit=crop&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxwaG90by1wYWdlfHx8fGVufDB8fHx8fA%3D%3D'
  },
  {
    id:3,
    title: "運動防水耳機",
    description: "適合運動健身使用",
    price: 1590,
    imgUrl:'https://images.unsplash.com/photo-1636099362460-5717f3be17cf?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8Mnx8d2F0ZXJwcm9vZiUyMGVhcmJ1ZHN8ZW58MHx8MHx8fDA%3D'
  },
  {
    id:4,
    title: "高解析音質耳罩耳機",
    description: "享受細膩音質",
    price: 3290,
    imgUrl:'https://images.unsplash.com/photo-1704440278730-b420f5892700?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8MTF8fGhpZ2glMjBxdWFsaXR5JTIwaGVhZHBob25lc3xlbnwwfHwwfHx8MA%3D%3D'
  },
  {
    id:5,
    title: "降噪真無線耳機",
    description: "主動降噪技術",
    price: 2790,
    imgUrl:'https://images.unsplash.com/photo-1733641839465-f9de0c9b9bde?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8NHx8bm9pc2UlMjBjYW5jZWxsaW5nJTIwZWFyYnVkc3xlbnwwfHwwfHx8MA%3D%3D'
  },
  {
    id:6,
    title: "遊戲專用藍牙耳機",
    description: "低延遲模式，提升遊戲體驗",
    price: 2390,
    imgUrl:'https://images.unsplash.com/photo-1674641900670-4444c8fe3ef0?w=900&auto=format&fit=crop&q=60&ixlib=rb-4.1.0&ixid=M3wxMjA3fDB8MHxzZWFyY2h8M3x8ZXNwb3J0cyUyMGhlYWRwaG9uZXN8ZW58MHx8MHx8fDA%3D'
  }
];

const cartList = ref([]);
const toastMessage = ref('');
const toastState = ref('success');

const addCart = (id) =>{
  const product = products.find(item=>item.id===id);
  const exist = cartList.value.find(item=>item.id===id);

  if(exist){
    exist.count += 1;
    toastMessage.value = `${product.title} 數量 +1`;
    toastState.value = 'success';
  }else{
    cartList.value.push({
      id:product.id,
      name:product.title,
      price:product.price,
      count:1
    })
    toastMessage.value = `${product.title} 已加入購物車`;
    toastState.value = 'success';
  }
}
const deleteCart = (id) =>{
  const cart = cartList.value.find(item => item.id === id)
  cartList.value = cartList.value.filter(cart=>cart.id !== id);
  toastMessage.value = `${cart.name} 已移出購物車`;
  toastState.value = 'error';
}
const totalPrice = computed(()=>{
  return cartList.value.reduce((sum,cart)=> sum+cart.price*cart.count,0);
})

provide('toastMessage',toastMessage);
provide('toastState',toastState);

</script>

<template>
  <div id="app" class="container py-4">
    <div class="row">
      <!-- 商品列表區 -->
      <div class="col-md-8">
        <ProductList :products="products" :add-cart="addCart"/>  
      </div>

      <!-- 購物車區 -->
      <div class="col-md-4">
        <CartList :CartList="cartList" @delete-cart="deleteCart"/>
        <p class="fw-bold" >總金額：{{ totalPrice }}</p>
      </div>
    </div>

    <!-- 通知元件 -->
    <div class="position-fixed top-0 end-0 p-3 " style="z-index: 1050">
      <ToastView/>
    </div>
  </div>
</template>