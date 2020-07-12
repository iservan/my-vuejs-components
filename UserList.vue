<template>
  <fragment>
    <b-list-group flush>
      <b-list-group-item v-for="(user, index) in users" :key="index" :user="user">
        <user-media
          :user="user"
          :show-institution="false"
          class="float-left"
          :with-follow-button="withFollowButton"
        ></user-media>
        <slot name="actions" class="float-right"></slot>
        <dots-settings-menu v-if="dotsMenu" class="float-right" tooltip-title="User actions" :show.sync="created">
          <slot name="links"></slot>
        </dots-settings-menu>
      </b-list-group-item>
    </b-list-group>
  </fragment>
</template>

<script>
  import UserMedia from '../Molecules/UserMedia';
  import DotsSettingsMenu from '../Molecules/DotsSettingsMenu';

  export default {
    name: 'UserList',

    data() {
      return {
        selected: '',
      };
    },

    components: {
      UserMedia,
      DotsSettingsMenu,
    },

    props: {
      withFollowButton: Boolean,
      users: Array,
      dotsMenu: {
        type: Boolean,
        default: false,
      },
    },

    methods: {
      created(content) {
        this.$emit('dots-menu-slot', content);
      },
    },
  };
</script>

<style lang="scss" scoped>
  ::v-deep {
    .follow-btn {
      position: absolute;
      right: 0;
      top: 15px;
    }
  }
</style>
