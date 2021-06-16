<template lang="pug">
div#app
  h1 Diamond
  input(
    type="number"
    :value="value"
    @input="updateValue"
  )
  div.wrap
    div.row(
      v-for="(row, index) in rows"
      :key="index"
    )
      Square.square(
        v-for="(active, j) in row"
        :key="`${index}-${j}`"
        :width="width"
        :height="width"
        :active="active"
      )
</template>

<script>
import Square from '@/components/Square.vue'

export default {
  name: 'App',
  components: {
    Square
  },
  data() {
    return {
      value: '11',
      timerId: null
    }
  },
  computed: {
    rows() {
      if (!this.value) return []
      if (this.value % 2 === 0) return []

      const count = Number(this.value)
      const midPoint = Math.ceil(count / 2)
      const temp = [...new Array(count)].map(() => {
        return [...new Array(count)].map(() => false)
      })
      const result = temp.reduce((all, current, index) => {
        const line = index + 1
        const min = 1
        const step = 2
        const quantity = min + (index * step)
        const start = Math.ceil((count - quantity) / 2)

        if (line <= midPoint) {
          const activeList = [...new Array(quantity)].map(() => true)
          current.splice(start, quantity, ...activeList)
          all[index] = current
        } else {
          all[index] = all[count - line]
        }

        return all
      }, temp)

      return result
    },
    width() {
      const count = Number(this.value)
      return `${600 / count}px`
    }
  },
  methods: {
    updateValue(e) {
      if (this.timerId) {
        clearTimeout(this.timerId)
      }
      this.timerId = setTimeout(() => {
        this.value = e.target.value
      }, 500)
    }
  }
}
</script>

<style lang="scss">
#app {
  font-family: Avenir, Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  text-align: center;
  color: #2c3e50;
  margin-top: 30px;
}
.wrap {
  width: 600px;
  margin: 50px auto;
  box-sizing: border-box;
}
.row {
  display: flex;
  border-left: 1px solid #000;
  &:last-child {
    border-bottom: 1px solid #000;
  }
}
.square {
  box-sizing: border-box;
  border-top: 1px solid #000;
  border-right: 1px solid #000;
}
</style>
