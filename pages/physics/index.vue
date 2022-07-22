<template>
  <div class="physics">
    <div class="physics__container">
      <div class="physics__container__canvas" />
    </div>
  </div>
</template>

<script>
import Matter from 'matter-js'

export default {
  name: 'IndexPage',
  mounted () {
    const Engine = Matter.Engine
    const Render = Matter.Render
    const Runner = Matter.Runner
    const Bodies = Matter.Bodies
    // const Events = Matter.Events
    const Composite = Matter.Composite
    const MouseConstraint = Matter.MouseConstraint
    const Common = Matter.Common
    const Body = Matter.Body
    const Mouse = Matter.Mouse
    const canvas = document.querySelector('.physics__container__canvas')
    const canvasContainer = document.querySelector('.physics')
    const canvasWidth = canvas.clientWidth
    const canvasHeight = canvas.clientHeight
    // create an engine
    const engine = Engine.create()

    // create a renderer
    const render = Render.create({
      element: canvas,
      engine,
      options: {
        background: 'transparent',
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
          mobile: 97,
          desktop: 145
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
          slop: 0.5,
          friction: 1,
          frictionStatic: Infinity,
          density: 2,
          lineWidth: 0,
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
    Composite.add(
      engine.world,
      boxs
    )

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

    const shakeScene = function (engine) {
      const bodies = Composite.allBodies(engine.world)

      for (let i = 0; i < bodies.length; i++) {
        const body = bodies[i]

        if (!body.isStatic && body.position.y >= 0) {
          const forceMagnitude = 0.001 * body.mass * 3

          Body.applyForce(body, body.position, {
            x: (forceMagnitude + Common.random() * forceMagnitude) * Common.choose([1, -1]),
            y: -forceMagnitude + Common.random() * -forceMagnitude
          })
        }
      }
    }

    const onScroll = (e) => {
      if (window.scrollY > 0 && canvasContainer.offsetHeight + canvasContainer.scrollTop >= canvasContainer.scrollHeight) {
        if (e.wheelDelta >= 0) {
          shakeScene(engine)
        }
      }
    }
    document.addEventListener('wheel', onScroll)

    // add all of the bodies to the world
    Composite.add(
      engine.world,
      mouseConstraint
    )
    render.mouse = mouse

    // run the renderer
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
    height: 200vh;

    &__container{
    width: 100vw;
    height: 100vh;
    display: flex;
    justify-content: center;
    align-items: center;
      &__canvas{
        width: 700px;
        height: 230px;
        display: flex;
      }
    }
  }
</style>
