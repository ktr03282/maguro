<template>
  <div>
  </div>
</template>

<script>
import P5 from 'p5'

export default {
  name: 'maguro',
  data () {
    return {
      img: '',
      objects: [],
      config: {
        maguro: {
          num: 15,
          speed: 2
        }
      },
      body: {
        width: document.body.clientWidth,
        height: document.body.clientHeight
      }
    }
  },
  methods: {
    sketch: function (p) {
      p.setup = () => p.createCanvas(this.body.width, this.body.height)
      p.preload = () => {
        this.img = p.loadImage('src/renderer/assets/maguro.png')
      }

      p.draw = () => {
        p.clear()
        p.noStroke()
        p.smooth()

        p.fill(0)
        p.imageMode(p.CENTER)

        this.objects.forEach(e => {
          e.x = e.x + e.speed
          if (e.x < 0 - this.img.width / 5 / 2) {
            e.x = this.body.width + this.img.width / 5 / 2
            e.y = this.body.height * Math.random()
          }
          p.image(this.img, e.x, e.y, this.img.width / 5, this.img.height / 5)
        })
      }
    },
    createRandomSpeed: function () {
      return -4 * Math.random() - 1
    },
    resetSpeedAll: function () {
      this.objects.forEach(e => {
        e.speed = this.createRandomSpeed()
      })
    }
  },
  mounted: function () {
    this.objects = (() => {
      const objs = []
      for (let i = 0; i < this.config.maguro.num; i++) {
        const obj = {
          x: this.body.width * Math.random(),
          y: this.body.height * Math.random(),
          speed: -4 * Math.random() - 1
        }
        objs.push(obj)
      }
      return objs
    })()

    document.addEventListener('keydown', e => {
      const code = e.keyCode
      if (code === 32) {
        this.resetSpeedAll()
      } else if (code === 37) {
        this.objects.forEach(e => {
          e.speed += -1
        })
      } else if (code === 39) {
        this.objects.forEach(e => {
          e.speed += 1
        })
      } else if (code === 13) {
        this.objects.forEach(e => {
          e.x = Math.random() * this.body.width
          e.y = Math.random() * this.body.height
        })
      }
    })

    // eslint-disable-next-line no-new
    new P5(this.sketch)
  }
}
</script>

<style>

</style>
