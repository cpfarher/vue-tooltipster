<!-- Code examples, use label or slot content if it is html:

    import VueTooltipster from './vue-tooltipster'

    components: {
      VueTooltipster
    }

    <vue-tooltipster label="tester">
      <span>tester1234</span>
    </vue-tooltipster>

    <vue-tooltipster>
      <span>tester</span>
      <div slot="content" style="background-color: red">tooltip content tester<h1>h1</h1></div>
    </vue-tooltipster>

    <vue-tooltipster :label="statusText" :options="{trigger: 'click'}">
      <svg :height="height" :width="width">
        <circle :cx="cxSize" :cy="cySize" :r="rSize" stroke="black" stroke-width="0.5" :fill="colorFill" />
      </svg>
    </vue-tooltipster>
  -->
<template>
  <span>
    <span ref="tooltipster" @click="toggleTip">
      <slot></slot> <!-- trigger del tooltip -->
    </span>

    <span class="hidden">
      <slot name="content"></slot>
    </span>
  </span>
</template>

<script>
  import 'tooltipster/dist/css/tooltipster.bundle.min.css';

  import $ from 'jquery';
  import 'tooltipster';
  import _ from 'lodash';
  import 'vue-awesome/icons';
  import Icon from 'vue-awesome/components/Icon.vue';

  export default {
    name: 'tooltip',
    mounted: function () {
      let self = this;
      $(this.$refs.tooltipster.children[0]).tooltipster(
        _.merge(this.defaultOptions, this.options, {content: this.label ? this.label : _.get(this.$slots, 'content[0].elm')})
      ).tooltipster('instance').on('close', function (event) {
        if (self.showTip) {
          event.stop();
        }
      });
    },
    components: {
      Icon
    },
    data: function () {
      return {
        showTip: false,
        showConfig: false,
        defaultOptions: {
          animationDuration: 100,
          contentAsHtml: true,
          delay: 0,
          interactive: true,
          respositionOnScroll: true, // ,
          trigger: 'custom',
          triggerOpen: {
            mouseenter: true,
            touchstart: true
          },
          triggerClose: {
            mouseleave: true,
            originClick: false,
            touchleave: true
          }
        }
      };
    },
    methods: {
      toggleTip: function () {
        this.showTip = !this.showTip;
        if (!this.showTip) {
          this.showConfig = false;
        }
      }
    },
    props: {
      label: {
        default: ''
      },
      options: { //  view possible options at http://iamceege.github.io/tooltipster/
        type: Object,
        default: function () {
          return {};
        }
      }
    },
    watch: {
      label: function (newLabel) { // if label change need to wath in order to update content to tooltipster!
        if (newLabel) {
          $(this.$refs.tooltipster.children[0]).tooltipster('content', newLabel);
        } else {
          $(this.$refs.tooltipster.children[0]).tooltipster('content', _.get(this.$slots, 'content[0].elm'));
        }
      }
    }
  }
</script>

<style scoped>
  .hidden {
    display: none;
  }
  div {
    color: black;
  }
</style>
