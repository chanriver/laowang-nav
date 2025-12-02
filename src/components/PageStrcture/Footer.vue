<template>
  <footer v-if="visible">
    <!-- 硬编码的页脚，无法通过配置修改 -->
    <span>
      <a :href="projectUrl">LaoWang Nav</a> is a personal dashboard.
      © <a :href="authorUrl">{{authorName}}</a> {{currentYear}}.
    </span>
  </footer>
</template>

<script>

import { shouldBeVisible } from '@/utils/SectionHelpers';

export default {
  name: 'Footer',
  data() {
    return {
      // 硬编码的值，无法通过配置修改
      projectUrl: 'https://github.com/tony-wang1990/laowang-nav',
      authorName: 'LaoWang',
      authorUrl: 'https://github.com/tony-wang1990',
      currentYear: new Date().getFullYear(),
    };
  },
  computed: {
    visible() {
      return shouldBeVisible(this.$route.name);
    },
  },
};
</script>

<style scoped lang="scss">
@import '@/styles/media-queries.scss';

footer {
  width: calc(100% - 0.5rem);
  bottom: 0;
  padding: 0.25rem;
  text-align: center;
  color: var(--medium-grey);
  opacity: var(--dimming-factor);
  background: var(--footer-background);
  margin-top: 1.5rem;
  border-top: 1px solid var(--outline-color);
  @include tablet-down {
    display: none;
  }
  span.path-to-config {
    float: left;
    font-size: 0.75rem;
    margin: 0.1rem 0.5rem 0 0;
    opacity: var(--dimming-factor);
    max-width: 10rem;
    overflow: hidden;
    text-overflow: ellipsis;
    word-break: break-all;
    max-height: 1rem;
  }
}

footer a{
  color: var(--footer-text-color);
  text-decoration: none;
  &:hover {
    color: var(--footer-text-color-link);
  }
}

</style>
