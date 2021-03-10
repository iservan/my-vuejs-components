<template>
  <div>

    <chat-panel
        class="mb-4"
        active
        size="XL"
        :description="panel.description"
        :icon="panel.icon"
        :name="panel.name"
        :parentRoute="panel.parentRoute"
        :route="panel.route"
        :showFooter="false">

        <!-- Target child view -->
        <router-view
            :panel="panel"
        />

    </chat-panel>

  </div>
</template>

<script>

export default {
  name: 'knowledge-base-quick-replies-view',

  props: {
    service_id: {
      type: String,
      required: true,
    },
    panel: {
      type: Object,
      required: true
    },
  },

}
</script>

<style lang="scss" scoped>
  @import '../styles/variables/colors';

  .invalid-state {
    width: 100%;
    margin-top: 0.25rem;
    font-size: 80%;
    color: $red;
  }
  .list-view {
    &:hover {
      background-color: $gray-blue-light;
    }
    .quick-reply-body {
      color: $text-muted;
      font-size: 14px;
      letter-spacing: 0.1px;
      line-height: 1.43;
      max-height: 40px;
      overflow: hidden;
    }
  }
</style>
