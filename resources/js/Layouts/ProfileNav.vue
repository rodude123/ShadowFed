<template>
  <div class="profileNav">
      <img src="https://picsum.photos/id/64/300/320" alt="Profile Image">
      <h3>some name</h3>
      <button class="btn btnPrimary">Logout <logout-variant-icon size="30" class="icon"/></button>
  </div>
</template>
<script>

import { defineComponent } from 'vue';
import LogoutVariantIcon from 'vue-material-design-icons/LogoutVariant.vue';

export default defineComponent({
    components: { LogoutVariantIcon },
});
</script>
