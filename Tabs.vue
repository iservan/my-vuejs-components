<template>
  <fragment>
    <ul class="col tab-links mb-3">
      <li
        v-for="(tab, index) in tabs"
        :key="tab.title"
        :class="{ active: index == selectedIndex }"
        @click="selectTab(index)"
      >
        <span>{{ tab.title }}</span>
        <slot name="element"></slot>
      </li>
    </ul>
    <slot></slot>
  </fragment>
</template>

<script>
  export default {
    name: 'Tabs',
    data() {
      return {
        selectedIndex: 0,
        tabs: [],
      };
    },
    created() {
      this.tabs = this.$children;
    },
    mounted() {
      this.selectTab(0);
    },
    methods: {
      selectTab(i) {
        this.selectedIndex = i;

        // loop over all the tabs
        this.tabs.forEach((tab, index) => {
          tab.isActive = (index === i)
        });
      },
    },
  };
</script>

<style lang="sass" scoped>
</style>
