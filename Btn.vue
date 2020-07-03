<template>
  <b-button v-bind="$attrs" :disabled="busy" :class="[{ 'btn-fab': fab == true }]" @click="$emit('click', $event)">
    <slot v-if="!svgIcon" name="icon">
      <i v-if="icon" :class="[iconName, 'fas']"></i>
    </slot>
    <slot v-else name="svg-icon"></slot>
    <slot name="btnText">{{ btnText }}</slot>
    <i v-if="iconRight" :class="[iconName, 'fas ml-1']"></i>
    <slot></slot>
  </b-button>
</template>

<script>
  export default {
    name: 'Btn',

    data() {
      return {
        busy: false,
      };
    },
    props: {
      iconName: String,
      btnText: String,
      icon: {
        type: Boolean,
        default: false,
      },
      iconRight: {
        type: Boolean,
        default: false,
      },
      fab: {
        type: Boolean,
        default: false,
      },
      svgIcon: {
        type: Boolean,
        default: false,
      },
    },
    methods: {
      onClick() {
        this.busy = true;
      },
    },
  };
</script>
<!-- Buttons styles are all included at _custom-button.scss -->
