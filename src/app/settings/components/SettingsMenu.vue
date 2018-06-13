<template>
  <div class="settings-menu">
    <ul class="menu-main">
      <li v-for="(item, index) in menu" :key="index">
        <router-link :to="{ name: item.name }">
          <span>{{ item.label }}</span>
          <feather-arrow-right />
        </router-link>
      </li>
      <li class="menu-version">
        <span>Version</span><span>{{ version }}</span>
      </li>
    </ul>

    <ul class="menu-theme">
      <li v-for="item in themes"
        :key="item.type"
        :class="{ 'selected': isActiveTheme(item.type) }">
        <button type="button" @click="onChangeTheme">
          {{ item.label }}
        </button>
      </li>
    </ul>
  </div>
</template>

<script>
  import pkg from '#/package.json'
  import { mapState } from 'vuex'

  export default {
    name: 'SettingsMenu',

    components: {
      'feather-arrow-right': () =>
        import('vue-feather-icon/components/arrow-right' /* webpackChunkName: "settings" */)
    },

    data () {
      return {
        version: pkg.version,
        themes: [
          { type: 'light', label: 'Light' },
          { type: 'dark', label: 'Dark' }
        ],
        menu: [
          { label: 'Tags', name: 'SettingsTags' },
          { label: 'Plant Data', name: 'SettingsData' },
          { label: 'About', name: 'SettingsAbout' }
        ]
      }
    },

    computed: {
      ...mapState({
        theme: state => state.settings.theme
      })
    },

    methods: {
      onChangeTheme () {
        console.log('change theme')
      },
      isActiveTheme (type) {
        return (
          type === this.theme ||
          (type === 'light' && this.theme === undefined)
        )
      }
    }
  }
</script>

<style lang="postcss" scoped>
  .menu-main li {
    display: flex;
    align-items: center;

    &:not(:last-child) {
      border-bottom: 3px solid rgba(0, 0, 0, 0.05);
    }

    & a {
      font-weight: 600;
      font-size: var(--text-size-medium);
      padding: var(--base-gap);

      &:focus {
        outline: none;
        background: var(--brand-green);
        color: var(--text-color-inverse);
      }

      &:focus svg {
        filter: invert(100%);
      }
    }

    & a,
    &.menu-version {
      width: 100%;
      display: flex;
      justify-content: space-between;
      align-items: center;
    }

    &.menu-version {
      font-size: 80%;
      color: var(--text-color-secondary);
      padding: var(--base-gap);
    }

    & svg {
      transform: scale(0.8);
    }
  }

  .menu-theme {
    display: flex;
    justify-content: center;
    list-style: none;
  }
</style>
