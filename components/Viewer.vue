<template>
  <div class="mt-4">
    <canvas ref="canvas" class="mx-auto" width="300" height="300"></canvas>
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
  computed: {
    radius() {
      return this.canvas.width * 0.45 // radius is 45% of circle width
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
  },
  methods: {
    draw() {
      // Background
      this.ctx.fillStyle = 'white'
      this.ctx.fillRect(0, 0, this.canvas.width, this.canvas.height)

      // Arcs
      this.drawArcs(this.arcs)

      this.angle = 1.5 * Math.PI + this.diff // % (2 * Math.PI)

      // Cursor
      this.cursor.x = this.center.x + this.radius * Math.cos(this.angle)
      this.cursor.y = this.center.y + this.radius * Math.sin(this.angle)

      this.drawCursor(this.cursor)
    },
    drawArc(
      arc,
      { radius = this.radius, width = 15, color = 'lightSkyBlue' } = {}
    ) {
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
