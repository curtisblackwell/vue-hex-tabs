<template>
  <ol v-if="fixedHeight" class="tab-panels" :style="'height:' + height + 'px'">
    <slot></slot>
  </ol>

  <ol v-else class="tab-panels">
    <slot></slot>
  </ol>
</template>


<script>
  export default {
    name: 'HexTabPanels',


    props: {
      /**
       * Set a fixed height for the tabs?
       *
       * Delay height setting by passing an object with `delay` property set to number of ms.
       *
       * @type {Boolean|Object}
       */
      fixedHeight: {
        type:    [Boolean, Object],
        default: true,
      },
    },


    data() {
      return {
        activeTabPanel:  1,
        height: 0,
      };
    },


    created() {
      HexBus.$on('HexTabPanel:activated', (uid, index) => {
        // If this is the activated tab panel's parent, update `activeTabPanel`.
        if (uid === this._uid) {
          this.activeTabPanel = index;
        }
      });

      HexBus.$on('HexTabPanel:heightMeasured', (uid, height) => {
        if (uid === this._uid) {
          this.height = height > this.height ? height : this.height;
        }
      });
    },
  }
</script>
