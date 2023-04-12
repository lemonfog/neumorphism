<script setup>
import { ref, provide } from 'vue';
import Configuration from './components/Configuration.vue';
import IconStar from './components/svg/IconStar.vue';
const preview = ref(null)
provide('preview', preview)
</script>
<template>
  <div class="title">
    <h1><a href="/">Neumorphism</a></h1>
    <h2>Generate <b>Soft-UI</b> CSS code</h2>
  </div>
  <div class="container">
    <div class="preview " ref="preview">
      <div class="soft-element soft-shadow"></div>
    </div>
    <Configuration></Configuration>
  </div>
  <div class="star-github">
    <a class="" href="https://github.com/lemonfog/neumorphism" target="_blank"
      aria-label="Star adamgiebl/neumorphism on GitHub">
      <IconStar></IconStar>
      <span>Star on Github</span>
    </a>
  </div>
</template>

<style scoped lang="less">
.title {
  text-align: center;
  margin-bottom: 30px;
}

a {
  text-decoration: none;
  color: inherit;
}

.container {
  display: flex;
  align-items: center;
  position: relative;

}

.preview {
  display: flex;
  align-items: center;
  justify-content: center;
  padding: 10px;
  position: relative;
  width: 400px;
  height: 400px;
  margin-right: 50px;

  @media screen and (min-width:1440px) {
    margin-right: 100px;
  }

  @media screen and (max-width:1000px) {
    width: 350px;
    height: 350px;
  }

  @media screen and (max-width:800px) {
    width: 280px;
    height: 280px;
    margin-right: 20px;
  }

}

@media only screen and (max-width:680px) {
  .container {
    flex-direction: column;
  }

  .preview {
    width: 300px;
    height: 300px;
    margin-right: 0;
    margin-bottom: 20px;
  }
}

.soft-element {
  width: var(--size);
  height: var(--size);
  border-radius: var(--radius);
}

.soft-shadow {
  background: var(--background);
  box-shadow: var(--box-shadow);
  // box-shadow: var(--positionX) var(--positionY) var(--blur) var(--dark-color), calc(var(--positionX)* -1) calc(var(--positionY)* -1) var(--blur) var(--light-color);
  // box-shadow: var(--box-shadow);
}

.star-github a {
  text-decoration: none;
  outline: none;
  display: block;
  background: #eff3f6;
  border: 1px solid rgba(27, 31, 35, .2);
  padding: 5px 10px;
  border-radius: .25em;
  position: fixed;
  right: 30px;
  top: 30px;
  color: #24292e;
  font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Oxygen, Ubuntu, Cantarell, 'Open Sans', 'Helvetica Neue', sans-serif;

  line-height: 20px;
  font-size: 12px;

  &:hover {
    background-color: #e6ebf1;
  }

  svg {
    vertical-align: text-top;
    margin-right: 5px;
  }

  span {
    font-weight: 600;
  }
}
</style>
