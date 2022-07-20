<template>
  <div class="home">
    <img
      class="home__background"
      src="/assets/images/background.png"
    >
    <div class="home__container">
      <div class="home__container__slide">
        <img
          :class="'home__container__slide__cube home__container__slide__cube-1'"
          src="/assets/images/floor-1.png"
        >
      </div>
      <div class="home__container__slide">
        <img
          :class="'home__container__slide__cube home__container__slide__cube-2'"
          src="/assets/images/floor-2.png"
        >
      </div>
      <!-- <div class="home__container__slide">
        <img
          :class="'home__container__slide__cube home__container__slide__cube-3'"
          src="/assets/images/cube.png"
        >
      </div>
      <div class="home__container__slide">
        <img
          :class="'home__container__slide__cube home__container__slide__cube-4'"
          src="/assets/images/cube.png"
        >
      </div>
      <div class="home__container__slide">
        <img
          :class="'home__container__slide__cube home__container__slide__cube-5'"
          src="/assets/images/cube.png"
        >
      </div> -->
    </div>
  </div>
</template>

<script>
export default {
  name: 'IndexPage',
  mounted () {
    // For global animation
    const viewportHeight = window.innerHeight
    const home = document.querySelector('.home')
    // For Slides animation
    let currentSlide = 0
    const container = document.querySelector('.home__container')
    const slides = document.querySelectorAll('.home__container__slide')
    const cubes = document.querySelectorAll('.home__container__slide__cube')
    const numberOfSlides = slides.length // Also used for background paralax
    const translateYSlides = 243 * 2
    // For Background animation
    const background = document.querySelector('.home__background')
    const backgroundTranslateY = 30
    const backgroundParalaxSpeed = 0.1
    // const getScrollableAreSize = numberOfSlides * viewportHeight - viewportHeight

    home.style.height = numberOfSlides * 100 + 'vh'
    for (let i = 0; i < cubes.length; i++) {
      cubes[i].style.transform = 'translateY(' + translateYSlides * i + 'px)'
      slides[i].style.zIndex = -10 * i
    }

    const onScroll = (e) => {
      for (let i = 0; i < numberOfSlides; i++) {
        // Basic scroll animation for Slides
        if (window.scrollY > viewportHeight * i && window.scrollY < viewportHeight * (i + 1)) {
          slides[i].style.top = viewportHeight * i - window.scrollY + 'px'
          if (window.scrollY < viewportHeight * i + translateYSlides) {
            container.style.transform = 'translateY(-' + (translateYSlides * i + (window.scrollY - viewportHeight * i)) + 'px)'
          }
          if (window.scrollY < translateYSlides) {
            container.style.transform = 'translateY(-' + window.scrollY + 'px)'
          }
          currentSlide = i
        }

        // Basic scroll animation for Background
        if (window.scrollY > viewportHeight * i && window.scrollY < viewportHeight * (i + 1)) {
          // background.style.top = viewportHeight * i - window.scrollY + 'px'
          if (window.scrollY < viewportHeight * i + backgroundTranslateY * (1 / backgroundParalaxSpeed)) {
            background.style.transform = 'translateY(-' + (backgroundTranslateY * i + (window.scrollY - viewportHeight * i) * backgroundParalaxSpeed) + 'px)'
          }
          if (window.scrollY < backgroundTranslateY) {
            background.style.transform = 'translateY(-' + window.scrollY * backgroundParalaxSpeed + 'px)'
          }
        }

        // To correct the problems of final position of slides in case of too fast scroll
        if (i !== currentSlide && window.scrollY < viewportHeight * i) {
          slides[i].style.top = 0 + 'px'
        }
        if (window.scrollY === 0) {
          slides[i].style.top = 0 + 'px'
          background.style.transform = 'translateY(0px)'
        }
      }
    }
    document.addEventListener('wheel', onScroll)
  }
}
</script>

<style
  lang="scss"
  scoped
>

  .home{
    width: 100vw;
    /* background-image: url(/assets/images/background.png);
    background-size: contain;
    background-repeat: no-repeat;
    background-position: top center; */
    display: flex;
    position: relative;
    z-index: -2000;
    &__background{
      width: 100%;
      object-fit: contain;
      object-position: top center;
      position: fixed;
      transition: transform .1s ease;
    }
    &__container{
      position: fixed;
      top: 0;
      left: 0;
      display: flex;
      justify-content: center;
      align-items: center;
      flex-direction: column;
      width: 100%;
      height: 100vh;
      &__slide{
        position: fixed;
        top: 0;
        left: 0;
        display: flex;
        justify-content: center;
        align-items: center;
        width: 100%;
        height: 100vh;

        &__cube{
          width: 1600px;
          height: 1600px;
          object-fit: contain;
          object-position: center;
          /* border: red 1px solid; */
        }
      }
    }
  }
</style>
