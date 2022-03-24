// HTML CONTENT
<template>
  <div style="position: relative">
    <img
      id="flipIcon"
      ref="flipIcon"
      title="symmetry"
      src="@/assets/flip.png"
      alt="symmetry icon"
    />
    <div id="avatar_preview" ref="preview" v-html="previewHtml"></div>
  </div>
</template>

// TYPESCRIPT CONTENT
<script lang="ts">
/** import libs **/
import { Component, Prop, Mixins } from 'vue-property-decorator'
/** import components **/
/** import miscellaneous **/
import { EventBus } from '@/tools/EventBus'
import { UtilMixins } from '@/tools/mixins'

@Component({
  components: {}
})
export default class Preview extends Mixins(UtilMixins) {
  @Prop({ type: Object, required: true }) readonly equipped!: any
  @Prop({ type: Object, required: true }) readonly assets!: any
  @Prop({ type: Number, required: true }) readonly xScale!: number
  parts = [
    'head',
    'nose',
    'mouth',
    'eyes',
    'legs0',
    'legs1',
    'arms0',
    'body',
    'arms1'
  ]

  get previewHtml() {
    let html = ''
    html += `<svg
      id="avatar"
      xmlns="http://www.w3.org/2000/svg"
      xmlns:xlink="http://www.w3.org/1999/xlink"
      x="0"
      y="0"
      width="290"
      height="380"
      transform="scale(${this.xScale} 1)"
    >`
    this.parts.forEach((part) => {
      html += this.findPreviewAsset(part)
    })
    html += `</svg>`
    return html
  }

  findPreviewAsset(part: string) {
    return this.equipped[part]
  }

  mounted() {
    const btn = this.$refs.flipIcon as HTMLElement
    btn.onclick = () => {
      EventBus.$emit('flip')
    }
  }
}
</script>

// CSS CONTENT
<style scoped>
#avatar_preview {
  padding: 15px;
  margin: 5px;
  position: relative;
  z-index: 10;
  top: 0px;
  border-radius: 10px;
  background: rgb(234 221 190);
}

#avatar {
  transform: scale(0.8);
  background: rgba(255, 0, 0, 0.1);
}
.part {
  border: 1px solid red;
}
</style>
