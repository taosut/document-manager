<template>
  <NestedTree
    :node="tree"
    :depth="1"
    :ignore-list="ignoreList"
    @openChildren="openChildren"
  />
</template>

<script>
import NestedTree from './NestedTree'

class IndexManager {
  constructor(array) {
    this.array = array
    this.size = array.length
    this.current = 0
  }

  getCurrent() {
    if (this.current >= this.size) {
      return null
    }
    return this.array[this.current]
  }

  goNext() {
    this.current++
  }
}

export default {
  components: {
    NestedTree
  },
  data: () => ({
    ignoreList: [],
    tree: {
      contents: { label: '1' },
      expand: false,
      index: 1,
      nodes: [
        {
          contents: { label: '2.1' },
          expand: false,
          index: 2,
          nodes: [
            {
              contents: { label: '3.1' },
              expand: false,
              index: 3,
              nodes: [
                { contents: { label: '4.1' }, expand: false, index: 4 },
                { contents: { label: '4.2' }, expand: false, index: 5 }
              ]
            },
            {
              contents: { label: '3.2' },
              expand: false,
              index: 6,
              nodes: [
                { contents: { label: '4.1' }, expand: false, index: 7 },
                { contents: { label: '4.2' }, expand: false, index: 8 }
              ]
            }
          ]
        },
        {
          contents: { label: '2.2' },
          expand: false,
          index: 9,
          nodes: [
            {
              contents: { label: '3.1' },
              expand: false,
              index: 10,
              nodes: [
                { contents: { label: '4.1' }, expand: false, index: 11 },
                { contents: { label: '4.2' }, expand: false, index: 12 }
              ]
            },
            {
              contents: { label: '3.2' },
              expand: false,
              index: 13,
              nodes: [
                { contents: { label: '4.1' }, expand: false, index: 14 },
                { contents: { label: '4.2' }, expand: false, index: 15 }
              ]
            }
          ]
        }
      ]
    }
  }),
  methods: {
    closeChildren(ignoreIndex) {
      // eslint-disable-next-line no-console
      console.log('top layer: ignoreIndex=' + ignoreIndex)
    },
    openChildren(openList) {
      console.log('TreeSidebar#openChilren() called.')
      console.log(openList)
      const mng = new IndexManager(openList)
      this.tree.expand = false
      //      this.tree.expand = true
      this.openElement(this.tree, mng)
    },
    openElement(node, mng) {
      console.log('openElement() called.')
      console.log(node)
      console.log(mng)
      const current = mng.getCurrent()

      console.log(`openElement()#current=${current} node.index=${node.index}`)

      if (current === null) {
        console.log('null')
        return
      }
      if (current === undefined) {
        console.log('undefined')
        return
      }
      if (current === node.index) {
        node.expand = true
        mng.goNext()
      }
      for (let i = 0; node.nodes && i < node.nodes.length; i++) {
        this.openElement(node.nodes[i], mng)
      }
    }
  }
}
</script>

<style></style>
