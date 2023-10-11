<script setup lang="ts">
import {Canvas, Rect, Text, Point} from 'fabric'
import {onMounted, ref} from "vue";

const canvasRef = ref()
const wrapperRef = ref()
let canvas: any = null

function setCanvasSize() {
  const parent = wrapperRef.value
  console.log('parent', parent, parent.offsetWidth, parent.offsetHeight)
  // canvasRef.value.width = parent.offsetWidth
  // canvasRef.value.height = parent.offsetHeight
  // canvas.setDimensions({
  //   width: parent.offsetWidth,
  //   height: parent.offsetHeight
  // })
  canvas.renderAll()
}

onMounted(() => {
  console.log(canvasRef.value)

  canvas = new Canvas(canvasRef.value, {})
  // canvas.setViewportTransform(iMatrix.concat())
  const center = canvas.getCenterPoint()
  canvas.backgroundColor = '#fff'
  setCanvasSize()

  function getZoom() {
    const width = document.documentElement.clientWidth
    const height = document.documentElement.clientHeight
    if (width / height < 1440 / 900) {
      return 1440 / width
    }
    return 900 / height
  }

  canvas.zoomToPoint(new Point(center.left, center.top), Math.min(0.8, getZoom() - 0.08))
  const rect = new Rect({
    top: 10,
    left: 10,
    width: 1440,
    height: 900,
    fill: '#aa96da'
  })
  const text = new Text('this is text', {
    left: 30,
    top: 40,
    fill: '#f00'
  })
  text.on('selected', () => {
    console.log('text is selected')
    console.log(text)
    text.set({fill: '#0f0'})
    setTimeout(() => {
      text.set({fill: '#00f'})
      canvas.renderAll()
    }, 3000)
  })
  // 将矩形添加到画布中
  canvas.add(rect)
  canvas.add(text)

  window.onresize = (e) => {
    console.log('onresize', e)
    setCanvasSize()
  }
})

const formatOutput = () => {
  console.log(canvas.toObject())

}

</script>

<template>
  <div class="app">
    <div class="sider"></div>
    <div class="main" ref="wrapperRef">
      <div style="position: relative">
        <canvas id="painter" ref="canvasRef"/>
      </div>
    </div>
    <div class="menu"></div>
  </div>

</template>

<style scoped>
.app {
  display: flex;
  background-color: #efefef;
  flex: auto;

  .sider, .menu {
    width: 300px;
    height: 100vh;
  }

  .main {
    flex: 1;
  }
}
</style>
