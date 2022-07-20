<template>
  <div class="layout">
    <Nuxt />
  </div>
</template>

<script>

export default {
  name: 'DefaultLayout',
  components: {
  }
}
</script>

<style
  lang='scss'
>
  html{
    scroll-behavior: smooth;
    overflow-x: hidden;
    overscroll-behavior-y: none;
  }
  body
  {
    margin: 0;
    position: relative;
    font-family: 'mukta', sans-serif;
  }
  .layout
  {
    width: 100vw;
    /* height: 300vh; */
    display: flex;
    flex-direction: column;
    position: relative;
    /* pointer-events: none; */

  }
  /* #__layout, #__nuxt, body, html{
    pointer-events: none;
  } */
</style>
