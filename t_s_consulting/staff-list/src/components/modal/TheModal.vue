<template>
  <!-- Полупрозрачный ovelay, который развернут на весь экран, и контейнера для формы или предупреждения перед удалением. Клик на контейнер ничего не делает (@mousedown.stop предотвращает event bubbling), а вот на сам оверлей - вызывает функцию hideOverlay, которая через emit передает сигнал в App.vue закрыть модальник  -->
  <div @mousedown="hideOverlay" id="overlay">
    <div class="container">
      <div @mousedown.stop class="card mx-auto my-5" style="max-width: 30rem">
        <!-- slot, на место которого, в зависимости от modalHead, встает либо DataForm, либо DeleteWarning -->
        <slot></slot>
      </div>
    </div>
  </div>
</template>

<script setup>
const emit = defineEmits(["hideOverlay"]);
function hideOverlay() {
  emit("hideOverlay");
}
</script>

<style scoped>
#overlay {
  position: fixed;
  width: 100%;
  height: 100%;
  top: 0;
  left: 0;
  right: 0;
  bottom: 0;
  background-color: rgba(0, 0, 0, 0.5);
  z-index: 1;
}
</style>
