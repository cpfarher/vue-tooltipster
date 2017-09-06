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
    <div class="full-height">
      <div ref="tooltipster"  @click="toggleTip" class="full-height">
        <slot></slot> <!-- trigger del tooltip -->
      </div>
      <div class="hidden p-xs" style="color:black;">
        <div ref="content">
          <div class="row" v-if="showClosebutton"><div class="btn btn-danger btn-circle btn-outline pull-right" @click="closeTooltip"><icon name="close"></icon></div></div>
          <slot name="content"></slot>
        </div>
      </div>
    </div>
  </template>

  <script>
  import 'tooltipster/dist/css/tooltipster.bundle.min.css';
  // import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-borderless.min.css'
  // import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-light.min.css'
  import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-noir.min.css'
  // import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-punk.min.css'
  // import 'tooltipster/dist/css/plugins/tooltipster/sideTip/themes/tooltipster-sideTip-shadow.min.css'
  import $ from 'jquery'
  import 'tooltipster'
  import _ from 'lodash'
  import 'vue-awesome/icons'
  import Icon from 'vue-awesome/components/Icon.vue'

  export default {
    name: 'tooltip',
    beforeDestroy: function () {
      this.closeTooltip()
      this.tooltipsterInstance.close()
    },
    components: {
      Icon
    },
    mounted: function () {
      let self = this;
      this.tooltipsterInstance = $(this.$refs.tooltipster.children[0]).tooltipster(
        _.merge(this.defaultOptions, this.tooltipsterOptions, {content: this.label ? this.label : this.$refs.content})
        ).tooltipster('instance');
      this.tooltipsterInstance.on('close', function (event) {
        if (self.showTip) {
          event.stop();
        }
      });
    },
    data: function () {
      return {
        showTip: false,
        showConfig: false,
        defaultOptions: {
          animationDuration: 500,
          animation: 'fade',
          contentAsHtml: true,
          delay: 0,
          minWidth: 450,
          interactive: true,
          theme: 'tooltipster-noir',  // tooltipster-shadow, tooltipster-light, tooltipster-borderless, tooltipster-punk, , tooltipster-noir
          multiple: true,
          respositionOnScroll: true,
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
        this.showTip = !this.showTip
        if (!this.showTip) {
          this.showConfig = false
        }
      },
      closeTooltip: function () {
        this.toggleTip()
        this.showConfig = false
        this.showTip = false
      }
    },
    props: {
      label: {
        default: ''
      },
      tooltipsterOptions: { //  view possible options at http://iamceege.github.io/tooltipster/
        type: Object,
        default: function () { return {} }
      }
      showClosebutton: {
        type: Boolean,
        default: true
      }
    },
    watch: {
      label: function (newLabel) { // if label change need to wath in order to update content to tooltipster!
        if (newLabel) {
          $(this.$refs.tooltipster.children[0]).tooltipster({
            content: newLabel
          })
        } else {
          $(this.$refs.tooltipster.children[0]).tooltipster({
            content: _.get(this.$slots, 'content[0].elm')
          })
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
