<template>
  <div class="dialog" v-if="show" @click="hideDialog">
    <div @click.stop class="dialog_contents">
      <slot></slot>
    </div>
  </div>
</template>

<script>
import toggleMixin from "@/mixins/toggleMixin";

export default {
  name: "my_dialog",
  mixins: [toggleMixin]
}
</script>

<style>
.dialog {
  top: 0;
  bottom: 0;
  left: 0;
  right: 0;
  background: rgba(0, 0, 0, 0.5);
  position: fixed;
  display: flex;
}

.dialog_contents{
  margin: auto;
  border-right: 12px;
  min-height: 50px;
  min-width: 300px;
  background: white;
  padding: 20px;
}
</style>
