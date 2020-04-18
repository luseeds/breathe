<template>
  <div>
    <canvas ref="canvas" width="800" height="400"></canvas>
  </div>
</template>
<script>
export default {
  props: {
    arcs: {
      type: Array,
      default: () => []
    },
    diff: {
      type: Number,
      default: 0
    }
  },
  watch: {
    diff() {
      this.draw()
    }
  },
  mounted() {
    this.canvas = this.$refs.canvas
    this.ctx = this.canvas.getContext('2d')
    this.center = {
      x: this.canvas.width / 2,
      y: this.canvas.height / 2
    }
    this.cursor = {
      x: 0,
      y: 0
    }
    this.angle = 1.5 * Math.PI
  },
  methods: {
    draw() {
      // Background
      this.ctx.fillStyle = 'lavender'
      this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height)

      this.drawArcs(this.arcs)
      this.angle += (this.diff * 0.0005) % (2 * Math.PI)

      this.cursor.x = this.center.x + 180 * Math.cos(this.angle)
      this.cursor.y = this.center.y + 180 * Math.sin(this.angle)

      this.drawCursor(this.cursor)
    },
    drawArc(arc, { radius = 180, width = 15, color = 'lightSkyBlue' } = {}) {
      this.ctx.beginPath()
      this.ctx.arc(
        this.center.x,
        this.center.y,
        radius,
        arc.start * Math.PI,
        arc.end * Math.PI
      )
      this.ctx.lineWidth = width
      this.ctx.strokeStyle = this.getBreatheColor(arc.action) || color
      this.ctx.stroke()

      this.ctx.closePath()
    },

    drawArcs(arcs) {
      arcs.map(this.drawArc)
    },
    drawCursor({
      x,
      y,
      radius = 15,
      color = 'rgba(255, 255, 255, .9)',
      width = 1,
      borderColor = 'rgba(0, 0, 0, .3)'
    }) {
      this.ctx.beginPath()
      this.ctx.arc(x, y, radius, 0, 2 * Math.PI)
      this.ctx.fillStyle = color
      this.ctx.fill()
      this.ctx.lineWidth = width
      this.ctx.strokeStyle = borderColor
      this.ctx.stroke()
      this.ctx.closePath()
    },
    getBreatheColor(action) {
      const breatheColors = {
        inhale: 'lightSkyBlue',
        exhale: 'lightBlue',
        hold: 'royalBlue'
      }

      return breatheColors[action]
    }
  }
}
</script>
