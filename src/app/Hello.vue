<template>
  <vue-tooltipster></vue-tooltipster>
</template>

<script>
import VueTooltipster from './vue-tooltipster.vue';
export default {
  name: 'Hello',
  data() {
    return {
      hello: 'Hello World!'
    };
  },
  components: {
    VueTooltipster
  }
};
</script>
