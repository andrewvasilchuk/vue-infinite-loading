<template>
  <component :is="spinnerView"></component>
</template>

<script>
import config from '../config';

const SPINNERS = {
  DEFAULT: {
    render(createElement) {
      return createElement('i', {
        attrs: {
          class: 'loading-default',
        },
      });
    },
  },
};

export default {
  name: 'Spinner',
  computed: {
    spinnerView() {
      return (
        SPINNERS[(this.$attrs.spinner || '').toUpperCase()]
        || this.spinnerInConfig // fallback to spinner of config
      );
    },
    spinnerInConfig() {
      let result;

      if (config.slots.spinner && typeof config.slots.spinner === 'string') {
        // as spinner slot config a pure text spinner
        result = {
          render() {
            return this._v(config.slots.spinner); // eslint-disable-line no-underscore-dangle
          },
        };
      } else if (typeof config.slots.spinner === 'object') {
        // as spinner slot config a Vue component
        result = config.slots.spinner;
      } else {
        // fallback to spinner property config
        /* istanbul ignore next */
        result = SPINNERS[config.props.spinner.toUpperCase()] || SPINNERS.DEFAULT;
      }

      return result;
    },
  },
};
</script>

<style lang="less" scoped>
@import '../styles/spinner';
</style>
