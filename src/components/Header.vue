<template>
  <header>
    <Logo />
    <div class="nav nav-pills">
      <div
        v-for="nav in navigations"
        :key="nav.name"
        class="nav-item">
        <RouterLink
          :to="nav.href"
          active-class="active"
          :class="{ active: isMatch(nav.path) }"
          class="nav-link">
          {{ nav.name }}
        </RouterLink>
      </div>
    </div>
    <div
      class="user"
      @click="toAbout">
      <img
        :src="image"
        :alt="name" />
    </div>
  </header>
</template>

<script>
import { mapState } from 'vuex'
import Logo from '~/components/Logo'

export default {
  components: {
    Logo
  },
  data() {
    return {
      navigations: [
        {
          name: 'Search',
          href: '/'
        },
        {
          name: 'Movie',
          href: '/movie/',
          path: /^\/movie/ // /movie로 시작하는 경로를 모두 일치시키는것.
        },
        {
          name: 'About',
          href: '/about'
        }
      ]
    }
  },
  computed: {
    ...mapState('about', [
      'image',
      'name'
    ])
    // image() {
    //   return this.$store.state.about.image
    // },
    // name() {
    //   return this.$store.state.about.name
    // }
  },
  methods: {
    isMatch(path) {
      if (!path) return false 
      return path.test(this.$route.fullPath)
    },
    toAbout() {
      this.$router.push('./about')
    }
  }
}
</script>

<style lang="scss" scoped>
header {
  height: 70px;
  padding: 0 40px;
  display: flex;
  align-items: center;
  position: relative;
  .logo {
    margin-right: 40px;
  }
  .user {
    width: 40px;
    height: 40px;
    padding: 6px;
    border-radius: 50%;
    box-sizing: border-box;
    background-color: $gray-200;
    cursor: pointer;
    position: absolute;
    top: 0;
    bottom: 0;
    right: 40px;
    margin: auto;
    transition: .4s;
    &:hover {
      background-color: darken($gray-200, 10%);
    }
    img {
      width: 100%;
    }
  }
  @include media-breakpoint-down(sm) {
    .nav {
      display: none;
      // 뷰포트의 크기가  small사이즈보다 작을 경우 display를 none 처리
    }
  }
}
</style>