<template>
  <div class="service">
    <div class="service__container">
      <div class="service__container__content">
        <div
          v-for="(icon, index) in icons"
          :key="index"
          class="service__container__content__element"
        >
          <span class="service__container__content__element__line" />
          <img
            class="service__container__content__element__icon"
            :src="'/assets/images/service/' + icon + '.png'"
            @mousedown="onIconClick(index)"
          >
        </div>
      </div>
    </div>
  </div>
</template>

<script>

export default {
  name: 'ServiceIndex',
  data () {
    return {
      iconActiveIndex: null,
      icons: [
        'bitcoin',
        'avalanche',
        'ethereum'
      ]
    }
  },
  mounted () {
    const iconsSize = 120
    let isMouseDown = false
    const icons = document.querySelectorAll('.service__container__content__element__icon')
    const container = document.querySelector('.service__container')
    const containerHeight = container.clientHeight

    // Set icons initials positions
    for (let i = 0; i < icons.length; i++) {
      if (i === 1) {
        icons[i].style.top = 'calc(50% - ' + iconsSize + 'px * 0.5)'
      } else {
        icons[i].style.top = 'calc(25% - ' + iconsSize + 'px * 0.5)'
      }
    }

    // Grab animation
    const onMouseUp = (e) => {
      isMouseDown = false
    }
    const onMouseDown = (e) => {
      document.addEventListener('mousemove', onMouseMove)
      isMouseDown = true
    }
    const updateOtherIconsPostion = (pixelMoved) => {
      const percentIconMoved = (pixelMoved + iconsSize) * 100 / containerHeight
      const percentOtherIcon = (100 - percentIconMoved) * 0.5

      console.log(percentOtherIcon)

      for (let i = 0; i < icons.length; i++) {
        if (i !== this.iconActiveIndex) {
          icons[i].style.top = percentOtherIcon + '%'
        }
      }
      // icons[0].style.top = percentOtherIcon + '%'
      // icons[2].style.top = percentOtherIcon + '%'
      // console.log(percentIconMoved)
    }
    const onMouseMove = (e) => {
      if (isMouseDown) {
        const getIconPosition = e.clientY - containerHeight * 0.5
        const setIconPosition = e.clientY - containerHeight * 0.5 - iconsSize * 0.5
        if (getIconPosition + iconsSize * 0.5 > iconsSize && getIconPosition + iconsSize * 0.5 < containerHeight) {
          e.target.style.top = setIconPosition + 'px'
          updateOtherIconsPostion(setIconPosition)
        }
      }
    }
    document.addEventListener('mousedown', onMouseDown)
    document.addEventListener('mouseup', onMouseUp)
  },
  methods: {
    onIconClick (index) {
      this.iconActiveIndex = index
      console.log(index)
    }
  }
}
</script>

<style
  lang="scss"
  scoped
>

  .service{
    width: 100vw;
    height: 100vh;
    background-color: hsla(224, 39%, 10%, 1);
    display: flex;
    justify-content: center;
    align-items: center;

    &__container{
        width: 50%;
        height: 50%;
        border-radius: 16px;
        border: white 1px solid;
        display: flex;
        justify-content: center;
      &__content{
          width: 40%;
          height: 100%;
          display: flex;
          justify-content: space-between;

        &__element{
          width: 4px;
          height: 100%;
          /* background-color: yellow; */
          position: relative;
          display: flex;

          &__line{
            width: 100%;
            height: 100%;
          }
          &__icon{
            position: absolute;
            width: 120px;
            height: 120px;
            left: calc(50% - 60px);
            transform: rotate(-15deg);
            cursor: grab;
            -webkit-user-drag: none;
          }
        }
        &__element:nth-child(1){
          .service__container__content__element__line{
            background-color: hsla(54, 88%, 59%, 1);
          }
        }
        &__element:nth-child(2){
          .service__container__content__element__line{
            background-color: hsla(352, 85%, 61%, 1);
          }
        }
        &__element:nth-child(3){
          .service__container__content__element__line{
            background-color: hsla(292, 94%, 67%, 1);
          }
        }
      }
    }
  }
</style>
