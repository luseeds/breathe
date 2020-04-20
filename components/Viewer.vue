<template>
  <div class="mt-4">
    <canvas ref="canvas" class="mx-auto" width="300" height="300"></canvas>
  </div>
</template>
<script>
const COLORS = {
  inhale: 'rgb(112, 206, 255)',
  exhale: 'rgb(189, 232, 255)',
  hold: 'rgb(90, 164, 204)'
}
const actionToColor = (action) => COLORS[action] || COLORS.inhale

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
      return this.canvas.width * 0.42 // radius is 42% of circle width
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
    this.draw()
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
    drawArc(arc, { radius = this.radius, width = 15 } = {}) {
      this.ctx.beginPath()
      this.ctx.arc(
        this.center.x,
        this.center.y,
        radius,
        arc.start * Math.PI,
        arc.end * Math.PI
      )
      this.ctx.lineWidth = width
      this.ctx.strokeStyle = actionToColor(arc.action)
      this.ctx.stroke()

      this.ctx.closePath()
    },

    drawArcs(arcs) {
      arcs.map(this.drawArc)
    },

    drawCursor({ x, y }) {
      this.ctx.beginPath()
      this.ctx.arc(x, y, 15, 0, 2 * Math.PI)
      this.ctx.fillStyle = 'rgba(255, 255, 255, .8)'
      this.ctx.fill()
      this.ctx.lineWidth = 1
      this.ctx.strokeStyle = 'rgba(45, 55, 72, .3)'
      this.ctx.stroke()
      this.ctx.closePath()
    }
  }
}
</script>
