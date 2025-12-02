<template>
    <div class="nav-outer" v-if="allLinks && allLinks.length > 0">
      <IconBurger
        :class="`burger ${!navVisible ? 'visible' : ''}`"
        @click="navVisible = !navVisible"
      />
      <nav id="nav" v-if="navVisible">
        <!-- Render either router-link or anchor, depending if internal / external link -->
        <template v-for="(link, index) in allLinks">
          <router-link v-if="!isUrl(link.path)"
            :key="index"
            :to="link.path"
            class="nav-item"
          ><span class="nav-text">{{link.title}}</span>
          </router-link>
          <a v-else
            :key="index"
            :href="link.path"
            :target="determineTarget(link)"
            class="nav-item"
            rel="noopener noreferrer"
          ><span class="nav-text">{{link.title}}</span>
          </a>
        </template>
      </nav>
    </div>
</template>

<script>
import IconBurger from '@/assets/interface-icons/burger-menu.svg';
import { makePageSlug } from '@/utils/ConfigHelpers';
import { checkPageVisibility } from '@/utils/CheckPageVisibility';

export default {
  name: 'Nav',
  components: {
    IconBurger,
  },
  props: {
    links: Array,
  },
  data: () => ({
    navVisible: true,
    isMobile: false,
  }),
  computed: {
    /* Get links to sub-pages, and combine with nav-links */
    allLinks() {
      const subPages = this.$store.getters.pages.filter((page) => checkPageVisibility(page))
        .map((subPage) => ({
          path: makePageSlug(subPage.name, 'home'),
          title: subPage.name,
        }));
      const navLinks = this.links || [];
      return [...navLinks, ...subPages];
    },
  },
  created() {
    // Always show nav, do not collapse into burger menu
    this.navVisible = true;
    this.isMobile = this.detectMobile();
  },
  methods: {
    detectMobile() {
      const screenWidth = document.body.clientWidth;
      return screenWidth && screenWidth < 600;
    },
    isUrl: (str) => new RegExp(/(http|https):\/\/(\S+)(:[0-9]+)?/).test(str),
    determineTarget(link) {
      if (!link.target) return '_blank';
      switch (link.target) {
        case 'sametab': return '_self';
        case 'newtab': return '_blank';
        case 'parent': return '_parent';
        case 'top': return '_top';
        default: return undefined;
      }
    },
  },
};
</script>

<style scoped lang="scss">
@import '@/styles/style-helpers.scss';
@import '@/styles/media-queries.scss';

.nav-outer {
  nav {
    display: flex;
    align-items: center;
    .nav-item {
      display: flex;
      align-items: center;
      justify-content: center;
      padding: 0.5rem 1.2rem;
      margin: 0.5rem;
      min-width: 5rem;
      min-height: 70px;
      text-align: center;
      outline: none;
      border: none;
      border-radius: var(--curve-factor);
      box-shadow: var(--nav-link-shadow);
      background: var(--nav-link-background-color);
      border: 1px solid var(--nav-link-border-color);
      text-decoration: none;
      font-size: 1.1rem;
      font-weight: 500;

      .nav-text {
        background: linear-gradient(90deg,
          #ff0000 0%,
          #ff7f00 14%,
          #ffff00 28%,
          #00ff00 42%,
          #00ffff 57%,
          #0000ff 71%,
          #8b00ff 85%,
          #ff0000 100%
        );
        background-size: 200% 100%;
        -webkit-background-clip: text;
        -webkit-text-fill-color: transparent;
        background-clip: text;
        animation: rainbowFlow 10s linear infinite;
      }

      &.router-link-active, &:hover {
        transform: translateY(-2px);
        box-shadow: var(--nav-link-shadow-hover);
      }
    }
  }
  /* Mobile and Burger-Menu Styles */
  @extend .svg-button;
  @include phone {
    display: none !important;
  }
  
  .burger {
    display: none;
    /* Only show burger if there are many links, but for now user wants GitHub visible. 
       We will hide burger and show links directly as requested for the "folded github" issue. */
    /* &.visible { display: block; } */
    /* @include phone { display: block; } */
  }
}

@keyframes rainbowFlow {
  0% {
    background-position: 0% 50%;
  }
  100% {
    background-position: 200% 50%;
  }
}

</style>
