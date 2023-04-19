<script lang="ts">
// export default {
// let uuid = 0;
// this.$el.querySelector("input").innerHTML = "#scroll-sync-template"

// data() {
//     return {
//       topNode: null
//     }
//   },
//   beforeCreate() {
//     this.uuid = uuid.toString();
//     uuid += 1;
//   },
//   mounted() {
//     let parent = this.$parent
//     while (parent) {
//       this.topNode = parent
//       parent = this.topNode.$parent
//     }

//     const vue = this;
//     this.topNode.$on('scroll-sync', function(data) {
//       if (data.emitter === vue.uuid) {
//         return
//       }
//       const {
//         scrollTop,
//         scrollHeight,
//         clientHeight,
//         barHeight
//       } = data

//       const scrollTopOffset = scrollHeight - clientHeight

//       vue.$el.onscroll = null
//       if (scrollTopOffset > barHeight) {
//         vue.$el.scrollTop = scrollTop;
//       }
//       window.requestAnimationFrame(() => {
//         vue.$el.onscroll = vue.handleScroll
//       })
//     })
//     this.$el.onscroll = this.handleScroll
//   },
//   methods: {
//     handleScroll: function(e) {
//       const vue = this
//       window.requestAnimationFrame(() => {
//         const {
//           scrollTop,
//           scrollHeight,
//           clientHeight,
//           offsetHeight
//         } = e.target

//         this.topNode.$emit('scroll-sync', {
//           scrollTop,
//           scrollHeight,
//           clientHeight,
//           barHeight: offsetHeight - clientHeight,
//           emitter: vue.uuid
//         })
//       })
//     }
//   }
// }

import { defineComponent, getCurrentInstance } from 'vue'

export default defineComponent({
  data: () => {
    return {
      topNode: null as any | null,
      uuid: 0
    }
  },
  beforeCreate(): void {
    this.$el.querySelector('input').innerHTML = '#scroll-sync-template'
    this.uuid += 1
  },
  mounted(): void {
    let parent = this.$parent

    while (parent) {
      this.topNode = parent
      parent = this.topNode.$parent
    }

    // eslint-disable-next-line @typescript-eslint/no-this-alias
    const vue = this

    //
    console.log('hola')
    if ((this.topNode = getCurrentInstance()!.parent!.subTree.el)) {
      this.topNode.addEventListener('scroll-sync', function (data: any) {
        if (data.emitter === vue.uuid) {
          return
        }
        console.log(data)
        const { scrollTop, scrollHeight, clientHeight, barHeight } = data

        const scrollTopOffset = scrollHeight - clientHeight

        vue.$el.onscroll = null
        if (scrollTopOffset > barHeight) {
          vue.$el.scrollTop = scrollTop
        }
        window.requestAnimationFrame(() => {
          vue.$el.onscroll = vue.handleScroll
        })
      })
      this.$el.onscroll = this.handleScroll
    }
    //

    //
    // if (this.topNode) {
    //   this.topNode.$on('scroll-sync', function (data: any) {
    //     if (data.emitter === vue.uuid) {
    //       return
    //     }
    //     const { scrollTop, scrollHeight, clientHeight, barHeight } = data

    //     const scrollTopOffset = scrollHeight - clientHeight

    //     vue.$el.onscroll = null
    //     if (scrollTopOffset > barHeight) {
    //       vue.$el.scrollTop = scrollTop
    //     }
    //     window.requestAnimationFrame(() => {
    //       vue.$el.onscroll = vue.handleScroll
    //     })
    //   })
    //   this.$el.onscroll = this.handleScroll
    // }
    //
  },
  beforeDestroy() {
    this.topNode.removeEventListener('scroll-sync')
  },
  methods: {
    handleScroll: function (e: any) {
      // eslint-disable-next-line @typescript-eslint/no-this-alias
      const vue = this

      window.requestAnimationFrame(() => {
        const { scrollTop, scrollHeight, clientHeight, offsetHeight } = e.target

        if (this.topNode) {
          this.topNode.$emit('scroll-sync', {
            scrollTop,
            scrollHeight,
            clientHeight,
            barHeight: offsetHeight - clientHeight,
            emitter: vue.uuid
          })
        }
      })
    }
  }
})
</script>
<template>
  <div class="scroll-sync-container">
    <slot></slot>
  </div>
</template>
<style>
.scroll-sync-container {
  height: 100%;
  width: 100%;
  position: relative;
  overflow: auto;
}
</style>
