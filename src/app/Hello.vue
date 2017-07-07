<template>
  <div class="row">
    <h1>Vue JS 2 - Tooltipster componente use example</h1>
    <div>
      <vue-tooltipster label="I am the tooltip">
        <span>Hover me there is a tooltip with label property</span>
      </vue-tooltipster>
    </div>

    <div>
      <vue-tooltipster>
        <span>Html tooltip content</span>
        <div slot="content" style="background-color: red">
          This is html content for render inside tooltip.
          Use <h1>slot="content"</h1>
          <div>Also if you need to keep this dialog open, click over text and it not dissapear when mouse goes out! </div>
        </div>
      </vue-tooltipster>
    </div>

    <div>
      <h2>You can pass options to vue-tooltipster - same object options you can see at http://iamceege.github.io/tooltipster/</h2>
      <h3>An example with option "trigger: 'click'". Please click over the green svg circle</h3>
      <vue-tooltipster :label="label" :options="{trigger: 'click'}">
        <svg height="50" width="50">
          <circle cx="25" cy="25" r="25" stroke="black" stroke-width="0.5" fill="green" />
        </svg>
      </vue-tooltipster>
    </div>

  </div>
</template>

<script>
import VueTooltipster from './vue-tooltipster.vue';
import jQuery from 'jquery';
window.jQuery = jQuery;
require('bootstrap');
export default {
  name: 'Hello',
  data() {
    return {
      label: 'Yea tooltipster option trigger click!'
    };
  },
  components: {
    VueTooltipster
  }
};
</script>
