<template>
  <div v-show="isActive">
    <slot>{{ title }}</slot>
  </div>
</template>

<script>
  export default {
    name: 'Tab',
    data() {
      return {
        isActive: true,
      };
    },
    props: {
      title: {
        type: String,
        default: 'Tab',
      },
    },
  };
</script>
