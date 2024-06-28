<template>
  <div>
    <h3>Virtual List ({{list.length}})</h3>

    <div class="container">
      <div class="list-view" ref="scrollBox" @scroll="onScroll">
        <div class="list-view-shadow" :style="{ height: contentHeight }"></div>

        <div class="list-view-content" ref="content">
          <div
             v-for="item in viewData"
             :key="item.val"
             :style="{height: item.height + 'px'}"
             class="list-view-item"
          >
            {{item}}
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'VirtualList',
  data() {
    return {
      start: 0,
      minHeight: 35,
      maxItem: 100000,
      list: [],
      computedData: [],
      viewData: [],
    }
  },
  computed: {
    contentHeight() {
      return this.computedData[this.computedData.length - 1]?.bom + 'px'
    }
  },
  watch: {
    start: {
      deep: true,
      handler (newVal, oldVal)  {
        if (newVal !== oldVal) {
          this.updateVisibleData()
        }
      }
    }
  },
  created() {
    this.init()
  },
  mounted() {
    this.updateVisibleData()
  },
  methods: {
    init() {
      for (let i = 0; i < this.maxItem; i++) {
        this.list.push({val: i, height: this.minHeight + Math.random() * 20})
      }

      for (let i = 0; i < this.list.length; i++) {
        let bom = this.list[i].height + (i === 0 ? 0 : this.computedData[i - 1].bom)
        this.computedData[i] = {val: this.list[i].val, bom}
      }
    },
    updateVisibleData() {
      const visibleCount = Math.ceil(this.$refs.scrollBox?.clientHeight / this.minHeight)
      const end = this.start + visibleCount
      this.viewData = this.list.slice(this.start, end)
      let scrollLen = this.computedData[this.start].bom - this.list[this.start].height
      this.$refs.content.style.webkitTransform = `translate3d(0, ${scrollLen}px, 0)`
    },
    onScroll() {
      this.start = this.findStart()
    },
    findStart() {
      let left = 0
      let right = this.computedData.length

      while ((left + 1) !== right && right > 0) {
        let mid = Math.floor((left + right) / 2)
        if (this.computedData[mid].bom > this.$refs.scrollBox.scrollTop) {
          right = mid
        } else {
          left = mid
        }
      }

      return left === 0 ? 0 : right
    }
  }
}
</script>

<style scoped>
.container {
  width: 500px;
  margin: 10px auto;
}

.list-view {
  position: relative;
  height: 400px;
  overflow: auto;
  border: 1px solid #aaa;
}

.list-view-shadow {
  position: absolute;
  left: 0;
  top: 0;
  right: 0;
  z-index: -1;
}

.list-view-content {
  position: absolute;
  left: 0;
  right: 0;
  top: 0;
}

.list-view-content div:nth-child(odd) {
  background-color: lightgreen;
}

.list-view-item {
  display: flex;
  align-items: center;
  justify-content: center;
  width: 100%;
  color: #666;
  background-color: rgb(60, 217, 234);
  border: 1px solid snow;
  box-sizing: border-box;
}
</style>
