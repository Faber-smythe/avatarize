// HTML CONTENT
<template>
  <div id="door" :style="doorStyle" @click="doorClick">
    <img
      ref="closeIcon"
      :style="`display:${opened ? 'flex' : 'none'}`"
      class="closeIcon"
      src="@/assets/close.png"
      title="close"
      alt="cross icon"
      @click="closeDoor"
    />
    <div
      id="scroller"
      ref="scroller"
      :style="`visibility: ${opened ? 'visible' : 'hidden'}; `"
    >
      <div id="svgHolder" ref="svgHolder" style=""></div>
    </div>
    <div v-if="!opened" id="currentlySelected" v-html="currentSelection"></div>
  </div>
</template>

// TYPESCRIPT CONTENT
<script lang="ts">
/** import libs **/
import { Component, Mixins, Prop } from 'vue-property-decorator'
/** import components **/
/** import miscellaneous **/
import { EventBus } from '@/tools/EventBus'
import { UtilMixins } from '@/tools/mixins'

@Component({
  components: {}
})
export default class Wardrobe extends Mixins(UtilMixins) {
  @Prop({ type: Object, required: true }) readonly equipped!: any
  @Prop({ type: Object, required: true }) readonly assets!: any
  @Prop({ type: String, required: true }) readonly align!: string
  @Prop({ type: String, required: true }) readonly part!: string
  @Prop({ type: String, required: false }) readonly arm?: string
  @Prop({ type: String, required: false }) readonly leg?: string
  @Prop({ type: String, required: false }) readonly openedSomewhere?: string
  @Prop({ type: Number, required: true }) readonly xScale!: number

  opened = false
  closing = false
  optionDepth = 0
  currentPath = ``
  dropMenuHtml = ``

  get currentDepth() {
    return this.currentPath.split('.').filter((str) => str !== '').length
  }

  get wardrobeName() {
    const name = this.part + (this.arm ?? '') + (this.leg ?? '')
    return name
  }

  get doorStyle() {
    let style = ``
    // align
    if (this.align === 'top') {
      style += 'align-items: flex-start;'
    } else {
      style += 'align-items: flex-end;'
    }
    // border and cursor
    if (this.opened) {
      style += 'border: 1px solid transparent;'
      style += 'z-index: 100;'
    } else {
      style += 'cursor:pointer;'
    }
    return style
  }

  get currentSelection() {
    const asset = this.equipped[this.wardrobeName]
    // height = '130'
    const sizedAsset =
      `<svg height="130" transform="scale(${this.xScale} 1)" data-label` +
      asset.split(` data-label`)[1]
    return sizedAsset
  }

  mounted() {
    EventBus.$on('assets-ready', async () => {
      this.optionDepth = Object.keys(
        this.flattenObject(this.assets[this.part])
      )[0].split('.').length

      this.dropMenuHtml = await this.getOptionsForDepthLevel(1)
      this.updateHTML()

      const closeIcon = this.$refs.closeIcon as HTMLElement
      // align
      if (this.align === 'top') {
        closeIcon.style.borderTopLeftRadius = '10px'
        closeIcon.style.borderBottomRightRadius = '10px'
      } else {
        closeIcon.style.top = 'auto'
        closeIcon.style.bottom = '1px'
        closeIcon.style.borderTopRightRadius = '10px'
        closeIcon.style.borderBottomLeftRadius = '10px'
      }

      EventBus.$on('wardrobe-opens', (name: string) => {
        if (this.wardrobeName !== name || !name) {
          this.closeDoor()
        }
      })
    })
  }

  updateHTML() {
    const svgHolder = this.$refs.svgHolder as HTMLElement
    svgHolder.innerHTML = this.dropMenuHtml

    // set the listeners on injected html
    const scroller = this.$refs.scroller as HTMLElement
    Array.from(scroller.querySelectorAll('svg')).forEach((svg) => {
      svg.addEventListener('click', () => {
        this.handlePartClick(svg)
      })
    })
  }

  getOptionsForDepthLevel(depth: number) {
    // flatten all the options for this part
    const flatOptions = this.flattenObject(this.assets[this.part])
    const outputKeys: string[] = []
    let pathWithoutPart = ''

    // first depth options ?
    if (depth <= 1) {
      const firstKeys = Object.keys(flatOptions).map(
        (option) => option.split('.')[0]
      )
      const uniqueKeys = []
      let nomatterPath = ''
      firstKeys.forEach((unikey) => {
        if (!uniqueKeys.includes(unikey)) {
          uniqueKeys.push(unikey)
          const nomatterPool = Object.keys(flatOptions).filter((key) =>
            key.includes(unikey)
          )
          if (nomatterPath === '' && this.optionDepth > 1) {
            nomatterPath = nomatterPool[
              Math.floor(Math.random() * nomatterPool.length)
            ].replace(unikey, '')
          }
          outputKeys.push(unikey + nomatterPath)
        }
      })
    }
    // already down some option depth ?
    else if (this.currentPath) {
      // console.log(this.currentPath)
      pathWithoutPart = this.currentPath.replace(`${this.part}.`, '')
      const involvedKeys = Object.keys(flatOptions)
        .filter((option) => option.includes(pathWithoutPart))
        .map((key) => key.replace(`${pathWithoutPart}.`, ''))

      // console.log('involvedKeys ', involvedKeys)
      const firstKeys = involvedKeys.map((option) => option.split('.')[0])
      // console.log('firstKeys ', firstKeys)
      const uniqueKeys = []
      let nomatterPath = ''
      firstKeys.forEach((unikey) => {
        if (!uniqueKeys.includes(unikey)) {
          uniqueKeys.push(unikey)
          const nomatterPool = involvedKeys.filter((key) =>
            key.includes(unikey)
          )
          if (this.optionDepth === this.currentDepth) {
            outputKeys.push(unikey)
          } else if (nomatterPath === '' && this.optionDepth > 1) {
            nomatterPath = nomatterPool[
              Math.floor(Math.random() * nomatterPool.length)
            ].replace(unikey, '')
            outputKeys.push(unikey + nomatterPath)
          } else {
            outputKeys.push(unikey + nomatterPath)
          }
        }
      })
    } else {
      console.error('current path has to be defined for non null depth')
    }

    // build the available wardrobe options from the output keys
    let htmlOptions = ``
    const remainingOptions = []
    if (pathWithoutPart !== '') pathWithoutPart += '.'
    outputKeys.forEach((key) => {
      htmlOptions +=
        `<svg height=100 transform="scale(${this.xScale} 1)" ` +
        flatOptions[pathWithoutPart + key].split('<svg')
      remainingOptions.push(flatOptions[pathWithoutPart + key])
    })
    // console.log(remainingOptions)
    return htmlOptions
  }

  doorClick() {
    if (!this.opened && !this.closing) {
      this.currentPath = this.part
      this.opened = true
      EventBus.$emit('wardrobe-opens', this.wardrobeName)
    }
  }

  handlePartClick(svg: SVGElement) {
    const partLabel = svg.dataset.label!
    const newOption = partLabel.split(`${this.currentPath}.`)[1].split('.')[0]
    if (this.optionDepth === this.currentDepth) {
      this.currentPath += '.' + newOption
      this.outputOption()
    } else {
      this.currentPath += '.' + newOption
      this.showNextDepth()
    }
  }

  outputOption() {
    EventBus.$emit('equip', {
      wardrobeName: this.wardrobeName,
      assetPath: this.currentPath
    })
    EventBus.$emit('wardrobe-opens', null)
  }

  closeDoor() {
    this.currentPath = ''
    this.closing = true
    this.opened = false
    this.showNextDepth()
    setTimeout(() => {
      this.closing = false
    }, 300)
  }

  async showNextDepth() {
    this.dropMenuHtml = await this.getOptionsForDepthLevel(this.currentDepth)
    this.updateHTML()
  }
}
</script>

// CSS CONTENT
<style scoped>
.closeIcon {
  height: 25px;
  padding: 3px;
  position: absolute;
  z-index: 5;
  left: 1px;
  top: 1px;
  transition: 0.15s ease;
  cursor: pointer;
}
.closeIcon:hover {
  background: rgba(0, 0, 0, 0.1);
}
#door {
  position: relative;
  display: flex;
  justify-content: center;
  align-items: center;
  width: 150px;
  height: 150px;
  margin: 7px;
  border: 1px solid black;
  border-radius: 10px;
  transition: border 0.3s ease;
}
#door:hover {
  border: 1px solid rgba(0, 0, 0, 0.2);
}
#svgHolder {
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 150px;
  background: rgba(245, 242, 238, 0.9);
}
#scroller {
  max-height: 500px;
  border: 2px solid black;
  border-right-color: transparent;
  border-radius: 10px;
  overflow-y: scroll;
  overflow-x: hidden;
}
#currentlySelected {
  position: absolute;
  pointer-events: none;
  display: flex;
  justify-content: center;
  align-items: center;
  height: 100%;
  width: 100%;
}
</style>
