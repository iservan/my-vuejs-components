<template>
  <b-link
    :class="[variant, { 'text-center': bigIcon == true }, 'icon-link']"
    :href="to"
    v-bind="$attrs"
    @click="$emit('click', $event)"
  >
    <slot v-if="!svgIcon" name="icon">
      <i v-if="bigIcon" :class="[iconName, 'fas fa-2x big-icon']"></i>
      <i v-else :class="[iconName, 'fas mr-2']"></i>
    </slot>
    <slot v-else name="svg-icon"></slot>
    <span :class="[{ 'big-text': bigIcon == true }]">
      <slot v-if="linkText" name="linkText">{{ linkText }}</slot>
      <slot></slot>
    </span>
    <i v-if="hasDropdown" class="fa fa-caret-down"></i>
  </b-link>
</template>

<script>
  export default {
    name: 'IconLink',
    props: {
      to: String,
      iconName: String,
      linkText: String,
      variant: String,
      bigIcon: {
        type: Boolean,
        default: false,
      },
      svgIcon: {
        type: Boolean,
        default: false,
      },
      hasDropdown: {
        type: Boolean,
        default: false,
      },
    },
  };
</script>

<style lang="scss" scoped>
  @import '../../sass/config/variables';
  .icon-link {
    display: inline-block;
    white-space: nowrap;
    transition: none;
    color: $body-color;
    &.delete {
      color: $red-base;
      &:hover {
        color: darken($red-base, 13);
      }
    }
    &:hover {
      color: lighten($body-color, 15);
      text-decoration: none;
      .big-text {
        color: lighten($body-color, 15);
      }
    }
    .big-icon {
      display: block;
    }
    .big-text {
      font-size: 0.7rem;
      font-weight: 600;
      text-transform: uppercase;
    }
  }
  body.dark {
    .dropdown .icon-link,
    .icon-link {
      color: $white;
      &:hover {
        color: lighten($body-color, 15);
      }
    }
  }
</style>
