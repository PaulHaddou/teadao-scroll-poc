<template>
  <div class="physics">
    <div class="physics__canvas" />
  </div>
</template>

<script>
import Matter from 'matter-js'

// const paths = [
//   'M289.05,84.63L98.225,308.136c-6.35,7.44-9.126,17.291-7.594,26.954l10.514,66.144c1.32,8.312,8.484,14.441,16.901,14.44h66.971c9.785,0,19.079-4.283,25.434-11.725l206.015-241.29c5.93-6.943,5.104-17.379-1.841-23.305l-42.83-36.57c-6.946-5.928-17.38-5.104-23.31,1.837l-152.4,178.477c-3.312,3.872-1.941,7.557-0.754,15.562c0.647,4.358,4.409,7.572,8.817,7.528c8.201-0.082,11.959,0.677,15.254-3.186l104.745-122.653l30.031,25.641l-108.396,126.93c-5.823,6.821-14.307,10.795-23.272,10.904h-40.639c-10.349,0.124-19.191-6.943-20.691-17.18l-5.896-40.215c-1.304-8.875,1.292-17.878,7.118-24.697L321.757,75.111c18.261-21.39,50.409-23.929,71.803-5.664l50.576,43.18c21.395,18.265,23.932,50.414,5.666,71.806L235.457,435.479c-10.447,12.243-25.735,19.284-41.83,19.271h-89.321c-19.688-0.012-36.449-14.382-39.549-33.821l-14.072-88.213c-2.53-15.89,2.027-32.097,12.479-44.339L259.017,58.988L289.05,84.63z'
// ]

export default {
  name: 'IndexPage',
  mounted () {
    const Engine = Matter.Engine
    const Render = Matter.Render
    const Runner = Matter.Runner
    const Bodies = Matter.Bodies
    const Composite = Matter.Composite
    const MouseConstraint = Matter.MouseConstraint
    const Mouse = Matter.Mouse
    const canvas = document.querySelector('.physics__canvas')
    const canvasWidth = canvas.clientWidth
    const canvasHeight = canvas.clientHeight
    // create an engine
    const engine = Engine.create()

    // create a renderer
    const render = Render.create({
      element: canvas,
      engine,
      options: {
        background: '#D5DAF0',
        wireframes: false,
        width: canvasWidth,
        height: canvasHeight
      }
    })

    // create two boxes and a ground
    const servicesHeightDesktop = 77
    // const servicesHeightMobile = 49
    const services = [
      {
        name: 'haas',
        width: {
          mobile: 77,
          desktop: 115
        }
      },
      {
        name: 'staking',
        width: {
          mobile: 104,
          desktop: 156
        }
      },
      {
        name: 'liquiditea',
        width: {
          mobile: 122,
          desktop: 183
        }
      },
      {
        name: 'dao-vc',
        width: {
          mobile: 145,
          desktop: 97
        }
      }
    ]
    const boxs = []
    for (let i = 0; i < services.length; i++) {
      const box = Bodies.rectangle(
        (10 + i * 200),
        50,
        services[i].width.desktop,
        servicesHeightDesktop,
        {
          slop: 0,
          render: {
            sprite: {
              texture: './assets/images/home/services/' + services[i].name + '.png',
              xScale: 0.25,
              yScale: 0.25
            }
          }
        })
      boxs.push(box)
    }

    //  Canvas Limits
    const limitsWidth = 1
    const wallBottom = Bodies.rectangle(
      canvasWidth * 0.5,
      canvasHeight + limitsWidth,
      canvasWidth,
      limitsWidth,
      { isStatic: true }
    )
    const wallTop = Bodies.rectangle(
      canvasWidth * 0.5,
      0 - limitsWidth,
      canvasWidth,
      limitsWidth,
      { isStatic: true }
    )
    const wallRight = Bodies.rectangle(
      canvasWidth + limitsWidth,
      canvasHeight * 0.5,
      limitsWidth,
      canvasHeight,
      { isStatic: true }
    )
    const wallLeft = Bodies.rectangle(
      0 - limitsWidth,
      canvasHeight * 0.5,
      limitsWidth,
      canvasHeight,
      { isStatic: true }
    )
    boxs.push(wallBottom, wallTop, wallRight, wallLeft)
    // const boxA = Bodies.rectangle(400, 200, 80, 80, { isStatic: false, illStyle: '#ffffff' })
    // const boxB = Bodies.rectangle(450, 50, 100, 80, {
    //   render: {
    //     strokeStyle: '#000000',
    //     sprite: {
    //       texture: './assets/images/home/services/haas.png',
    //       xScale: 0.25,
    //       yScale: 0.25,
    //       restitution: 1
    //     }
    //   }
    // })

    // add mouse control
    const mouse = Mouse.create(render.canvas)
    const mouseConstraint = MouseConstraint.create(engine, {
      mouse,
      constraint: {
        stiffness: 0.2,
        render: {
          visible: false
        }
      }
    })

    // add all of the bodies to the world
    Composite.add(
      engine.world,
      boxs,
      mouseConstraint
    )

    // run the renderer
    Render.setPixelRatio(render, 'auto')
    Render.run(render)

    // create runner
    const runner = Runner.create()

    // run the engine
    Runner.run(runner, engine)
  }
}
</script>

<style
  lang="scss"
  scoped
>

  .physics{
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
    z-index: -2000;

    &__canvas{
      width: 700px;
      height: 230px;
      background-color: blue;
      display: flex;
    }
  }
</style>
