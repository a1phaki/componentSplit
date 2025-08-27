<script setup>
import { inject,ref,watch,nextTick } from 'vue';
import { Toast } from 'bootstrap';
const toasts = ref([]);

const toastMessage = inject('toastMessage');
const toastState = inject('toastState');

function addToast(message, type = 'success', timeout = 1000) {
  const id = Date.now();
  const toast = { id, message, type, timeout };
  toasts.value.push(toast);

  
  nextTick(() => {
    const toastEl = document.getElementById(`toast-${toast.id}`);
    if (toastEl) {
      const instance = new Toast(toastEl, { delay: toast.timeout, autohide: true });
      instance.show();
    }
  })
}

watch([toastMessage,toastState], ([newMessage, newState]) => {
  if ( newMessage && newState ) {
    addToast(newMessage,newState);
  }
});

</script>


<template>
  <div
    v-for="toast in toasts"
    :key="toast.id"
    :id="`toast-${toast.id}`"
    class="toast align-items-center text-white bg-success border-0 m-3"
    :class="{
      'bg-success': toast.type === 'success',
      'bg-danger': toast.type === 'error',
    }"
    role="alert"
    aria-live="assertive"
    aria-atomic="true"
  >
    <div class="d-flex">
      <div class="toast-body">{{ toast.message }}</div>
      <button type="button" class="btn-close btn-close-white me-2 m-auto" @click="toasts = toasts.filter(t => t.id !== toast.id)"></button>
    </div>
  </div>
</template>