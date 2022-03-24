// HTML CONTENT
<template>
  <main ref="main" :style="`transform:scale(${mainSizeRatio});`">
    <div id="center">
      <div id="topRow">
        <div>
          <h3>HEAD</h3>
          <Wardrobe
            :equipped="equipped"
            :assets="assets"
            :x-scale="xScale"
            align="top"
            part="head"
          />
        </div>
        <div>
          <h3>EYES</h3>
          <Wardrobe
            :equipped="equipped"
            :assets="assets"
            :x-scale="xScale"
            align="top"
            part="eyes"
          />
        </div>
        <div>
          <h3>NOSE</h3>
          <Wardrobe
            :equipped="equipped"
            :assets="assets"
            :x-scale="xScale"
            align="top"
            part="nose"
          />
        </div>
        <div>
          <h3>MOUTH</h3>
          <Wardrobe
            :equipped="equipped"
            :assets="assets"
            :x-scale="xScale"
            align="top"
            part="mouth"
          />
        </div>
      </div>
      <div id="middleRow">
        <div id="leftWardrobes">
          <div>
            <Wardrobe
              :equipped="equipped"
              :assets="assets"
              :x-scale="xScale"
              align="top"
              part="arms"
              :arm="xScale === 1 ? '0' : '1'"
            />
            <h3>RIGHT ARM</h3>
          </div>
          <div>
            <h3>RIGHT LEG</h3>
            <Wardrobe
              :equipped="equipped"
              :assets="assets"
              :x-scale="xScale"
              align="bottom"
              part="legs"
              :leg="xScale === 1 ? '0' : '1'"
            />
          </div>
        </div>
        <Preview
          :equipped="equipped"
          :assets="assets"
          :styles="styles"
          :x-scale="xScale"
        />
        <div id="rightWardrobes">
          <div>
            <Wardrobe
              :equipped="equipped"
              :assets="assets"
              :x-scale="xScale"
              align="top"
              part="arms"
              :arm="xScale === -1 ? '0' : '1'"
            />
            <h3>LEFT ARM</h3>
          </div>
          <div>
            <h3>LEFT LEG</h3>
            <Wardrobe
              :equipped="equipped"
              :assets="assets"
              :x-scale="xScale"
              align="bottom"
              part="legs"
              :leg="xScale === -1 ? '0' : '1'"
            />
          </div>
        </div>
      </div>
    </div>
    <div>
      <h3>BODY</h3>
      <Wardrobe
        :equipped="equipped"
        :assets="assets"
        :x-scale="xScale"
        align="bottom"
        part="body"
      />
    </div>
    <button
      id="downloadButton"
      ref="downloadButton"
      @click="
        (e) => {
          download(e)
        }
      "
    >
      DOWNLOAD SVG
      <img src="@/assets/dl.png" alt="download icon" height="20" />
    </button>
  </main>
</template>

// TYPESCRIPT CONTENT
<script lang="ts">
/** import libs **/
import { Component, Mixins } from 'vue-property-decorator'
/** import components **/
import Preview from '@/components/Preview'
import Wardrobe from '@/components/Wardrobe'
/** import miscellaneous **/
import { EventBus } from '@/tools/EventBus'
import { UtilMixins } from '@/tools/mixins'

@Component({
  components: {
    Preview,
    Wardrobe
  }
})
export default class Index extends Mixins(UtilMixins) {
  openedSomewhere: string | null = null
  xScale = 1
  /**
   * Defining the amount of options for each part
   */
  types = ['asian', 'caucasian', 'latin', 'mideast', 'african']
  armsSignRange = 5
  armsPoseRange = 6
  bodyRange = 4
  eyesRange = 12
  headHairRange = 10
  legsPoseRange = 7
  mouthRange = 5
  noseRange = 6

  equipped = {
    arms0: ``,
    arms1: ``,
    body: ``,
    eyes: ``,
    head: ``,
    legs0: ``,
    legs1: ``,
    mouth: ``,
    nose: ``
  }

  assets = {
    arms: {
      sideback: {
        sign0: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        },
        sign1: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        },
        sign2: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        },
        sign3: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        },
        sign4: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        }
      },
      sidefront: {
        sign0: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        },
        sign1: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        },
        sign2: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        },
        sign3: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        },
        sign4: {
          pose0: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose1: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose2: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose3: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose4: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          },
          pose5: {
            asian: '',
            caucasian: '',
            latin: '',
            mideast: '',
            african: ''
          }
        }
      }
    },
    body: ['', '', '', ''],
    eyes: ['', '', '', '', '', '', '', '', '', '', '', ''],
    head: {
      hair0: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair1: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair2: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair3: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair4: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair5: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair6: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair7: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair8: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      },
      hair9: {
        asian: '',
        caucasian: '',
        latin: '',
        mideast: '',
        african: ''
      }
    },
    legs: {
      pose0: '',
      pose1: '',
      pose2: '',
      pose3: '',
      pose4: '',
      pose5: '',
      pose6: ''
    },
    mouth: ['', '', '', '', ''],
    nose: ['', '', '', '', '', '']
  }

  styles = {
    arms: {
      sign0: {
        pose0: {
          x: '-35,27.5',
          y: 115,
          height: 150
        },
        pose1: {
          x: '-45,17',
          y: 115,
          height: 150
        },
        pose2: {
          x: '-57.5,5',
          y: 115,
          height: 125
        },
        pose3: {
          x: '-66,-2.5',
          y: 115,
          height: 88
        },
        pose4: {
          x: '-62,1',
          y: 115,
          height: 89
        },
        pose5: {
          x: '-50,12',
          y: 115,
          height: 86
        }
      },
      sign1: {
        pose0: {
          x: '-35,27.5',
          y: 115,
          height: 150
        },
        pose1: {
          x: '-45,17',
          y: 115,
          height: 150
        },
        pose2: {
          x: '-57.5,5',
          y: 115,
          height: 125
        },
        pose3: {
          x: '-66,-2.5',
          y: 115,
          height: 88
        },
        pose4: {
          x: '-62,1',
          y: 115,
          height: 89
        },
        pose5: {
          x: '-50,12',
          y: 115,
          height: 86
        }
      },
      sign2: {
        pose0: {
          x: '-35,27.5',
          y: 115,
          height: 150
        },
        pose1: {
          x: '-45,17',
          y: 115,
          height: 150
        },
        pose2: {
          x: '-57.5,5',
          y: 115,
          height: 125
        },
        pose3: {
          x: '-66,-2.5',
          y: 115,
          height: 88
        },
        pose4: {
          x: '-62,1',
          y: 115,
          height: 89
        },
        pose5: {
          x: '-50,12',
          y: 115,
          height: 86
        }
      },
      sign3: {
        pose0: {
          x: '-35,27.5',
          y: 115,
          height: 150
        },
        pose1: {
          x: '-45,17',
          y: 115,
          height: 150
        },
        pose2: {
          x: '-57.5,5',
          y: 115,
          height: 125
        },
        pose3: {
          x: '-66,-2.5',
          y: 115,
          height: 88
        },
        pose4: {
          x: '-62,1',
          y: 115,
          height: 89
        },
        pose5: {
          x: '-50,12',
          y: 115,
          height: 86
        }
      },
      sign4: {
        pose0: {
          x: '-35,27.5',
          y: 115,
          height: 150
        },
        pose1: {
          x: '-45,17',
          y: 115,
          height: 150
        },
        pose2: {
          x: '-57.5,5',
          y: 115,
          height: 125
        },
        pose3: {
          x: '-66,-2.5',
          y: 115,
          height: 88
        },
        pose4: {
          x: '-62,1',
          y: 115,
          height: 89
        },
        pose5: {
          x: '-50,12',
          y: 115,
          height: 86
        }
      }
    },
    head: {
      hair0: {
        x: -10,
        y: 20,
        height: 106
      },
      hair1: {
        x: -5,
        y: 34,
        height: 92
      },
      hair2: {
        x: -8,
        y: 34,
        height: 92
      },
      hair3: {
        x: -9,
        y: 25,
        height: 102
      },
      hair4: {
        x: -11,
        y: 26,
        height: 100
      },
      hair5: {
        x: -9,
        y: 33,
        height: 92
      },
      hair6: {
        x: -8,
        y: 33,
        height: 92
      },
      hair7: {
        x: -8,
        y: 33,
        height: 92
      },
      hair8: {
        x: -8,
        y: 27,
        height: 98
      },
      hair9: {
        x: -10,
        y: 27,
        height: 98
      }
    },
    eyes: [
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      }
    ],
    nose: [
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      }
    ],
    mouth: [
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      },
      {
        x: -8,
        y: 32,
        height: 92
      }
    ],
    body: [
      {
        x: 0,
        y: 100,
        height: 145
      },
      {
        x: 0,
        y: 100,
        height: 145
      },
      {
        x: 0,
        y: 100,
        height: 145
      },
      {
        x: 0,
        y: 100,
        height: 145
      }
    ],
    legs: {
      pose0: {
        x: '-23,18',
        y: 200,
        height: 160
      },
      pose1: {
        x: '-13,28',
        y: 200,
        height: 165
      },
      pose2: {
        x: '-1,37',
        y: 200,
        height: 165
      },
      pose3: {
        x: '9,50',
        y: 200,
        height: 145
      },
      pose4: {
        x: '9,50',
        y: 200,
        height: 110
      },
      pose5: {
        x: '9,50',
        y: 200,
        height: 100
      },
      pose6: {
        x: '4,45',
        y: 200,
        height: 100
      }
    }
  }

  get assetsLoaded() {
    const flatAssets = this.flattenObject(this.assets)
    const emptyAssets = Object.values(flatAssets).filter(
      (value) => value === ''
    )
    return emptyAssets.length === 0
  }

  get mainSizeRatio() {
    return window.innerHeight / 850
  }

  async mounted() {
    await this.loadArmAssets()
    await this.loadBodyAssets()
    await this.loadHeadAssets()
    await this.loadEyesAssets()
    await this.loadLegsAssets()
    await this.loadMouthAssets()
    await this.loadNoseAssets()
    const main = this.$refs.main as HTMLElement
    document.body.onclick = (e) => {
      if (e.target === document.body || e.target === main) {
        EventBus.$emit('wardrobe-opens', null)
      }
    }

    EventBus.$emit('assets-ready')
    EventBus.$on('wardrobe-opens', (wardrobeName: string) => {
      this.openedSomewhere = wardrobeName
    })
    EventBus.$on('flip', () => {
      this.xScale *= -1
    })
    EventBus.$on(
      'equip',
      (data: { wardrobeName: string; assetPath: string }) => {
        const targetAsset = this.flattenObject(this.assets)[data.assetPath]
        let stylePath = data.assetPath
        this.types.forEach((type) => {
          stylePath = stylePath
            .replace(`.${type}`, '')
            .replace('.sideback', '')
            .replace('.sidefront', '')
        })

        let styledAsset = `<svg `
        styledAsset += `height="${
          this.flattenObject(this.styles)[stylePath + '.height']
        }"`

        let coordX = this.flattenObject(this.styles)[stylePath + '.x']

        if (data.wardrobeName === 'arms0') coordX = coordX.split(',')[0]
        if (data.wardrobeName === 'arms1') coordX = coordX.split(',')[1]
        if (data.wardrobeName === 'legs0') coordX = coordX.split(',')[0]
        if (data.wardrobeName === 'legs1') coordX = coordX.split(',')[1]
        styledAsset += ` x="${coordX}" `
        styledAsset += ` y="${
          this.flattenObject(this.styles)[stylePath + '.y']
        }" `
        styledAsset += targetAsset.split('<svg ')[1]

        styledAsset = styledAsset.replace('red', 'transparent !important')
        this.equipped[data.wardrobeName] = styledAsset
      }
    )

    window.addEventListener('resize', () => {
      const ratio = window.innerHeight / 850
      main.style.transform = `scale(${ratio})`
    })

    this.equipDefaults()
  }

  /**  load arms **/
  loadArmAssets() {
    // for each sign
    for (let s = 0; s < this.armsSignRange; s++) {
      // for each pose
      for (let p = 0; p < this.armsPoseRange; p++) {
        // for each type
        this.types.forEach(async (type) => {
          // for back side of the arm
          // const pathBack = `@/assets/arms/sideback/sign${s}/pose${p}/${type}.svg?raw`
          const rawSvgBack =
            (await require(`@/assets/arms/sideback/sign${s}/pose${p}/${type}.svg?raw`)) as string
          // const rawSvgBack =
          //   require(pathBack) as string
          const svgBack = rawSvgBack
            .split('cls')
            .join(`clsarmback${s}${p}${type}`) // avoid conflicting styles
          const labeledSvgBack =
            `<svg data-label=arms.sideback.sign${s}.pose${p}.${type}` +
            svgBack.split('<svg')[1]
          this.assets.arms.sideback[`sign${s}`][`pose${p}`][type] =
            labeledSvgBack

          // for front side of the arm
          const rawSvgFront =
            (await require(`@/assets/arms/sidefront/sign${s}/pose${p}/${type}.svg?raw`)) as string
          const svgFront = rawSvgFront
            .split('cls')
            .join(`clsarmfront${s}${p}${type}`) // avoid conflicting styles
          const labeledSvgFront =
            `<svg data-label=arms.sidefront.sign${s}.pose${p}.${type}` +
            svgFront.split('<svg')[1]
          this.assets.arms.sidefront[`sign${s}`][`pose${p}`][type] =
            labeledSvgFront
        })
      }
    }
  }

  /**  load body **/
  async loadBodyAssets() {
    // for each body shape
    for (let b = 0; b < this.bodyRange; b++) {
      const rawSvg = (await require(`@/assets/body/body${b}.svg?raw`)) as string
      const svg = rawSvg.split('cls').join(`clsbody${b}`) // avoid conflicting styles
      const labeledSvg = `<svg data-label=body.${b}` + svg.split('<svg')[1]
      this.assets.body[b] = labeledSvg
    }
  }

  /**  load heads **/
  loadHeadAssets() {
    // for each hair style
    for (let h = 0; h < this.headHairRange; h++) {
      // for each type
      this.types.forEach(async (type) => {
        const rawSvg =
          (await require(`@/assets/head/hair${h}/${type}.svg?raw`)) as string
        const svg = rawSvg.split('cls').join(`clshead${h}${type}`) // avoid conflicting styles
        const labeledSvg =
          `<svg data-label=head.hair${h}.${type}` + svg.split('<svg')[1]
        this.assets.head[`hair${h}`][type] = labeledSvg
      })
    }
  }

  /**  load eyes **/
  async loadEyesAssets() {
    // for each eyes option
    for (let e = 0; e < this.eyesRange; e++) {
      const rawSvg = (await require(`@/assets/eyes/eyes${e}.svg?raw`)) as string
      const svg = rawSvg.split('cls').join(`clseyes${e}`) // avoid conflicting styles
      const labeledSvg = `<svg data-label=eyes.${e}` + svg.split('<svg')[1]
      this.assets.eyes[e] = labeledSvg
    }
  }

  /**  load legs **/
  async loadLegsAssets() {
    // for each leg pose
    for (let p = 0; p < this.legsPoseRange; p++) {
      const rawSvg = (await require(`@/assets/legs/pose${p}.svg?raw`)) as string
      const svg = rawSvg.split('cls').join(`clslegpose${p}`) // avoid conflicting styles
      const labeledSvg = `<svg data-label=legs.pose${p}` + svg.split('<svg')[1]
      this.assets.legs[`pose${p}`] = labeledSvg
    }
  }

  /**  load mouth **/
  async loadMouthAssets() {
    // for each mouth option
    for (let m = 0; m < this.mouthRange; m++) {
      const rawSvg =
        (await require(`@/assets/mouth/mouth${m}.svg?raw`)) as string
      const svg = rawSvg.split('cls').join(`clsmouth${m}`) // avoid conflicting styles
      const labeledSvg = `<svg data-label=mouth.${m}` + svg.split('<svg')[1]
      this.assets.mouth[m] = labeledSvg
    }
  }

  /**  load nose **/
  async loadNoseAssets() {
    // for each nose option
    for (let n = 0; n < this.noseRange; n++) {
      const rawSvg = (await require(`@/assets/nose/nose${n}.svg?raw`)) as string
      const svg = rawSvg.split('cls').join(`clsnose${n}`) // avoid conflicting styles
      const labeledSvg = `<svg data-label=nose.${n}` + svg.split('<svg')[1]
      this.assets.nose[n] = labeledSvg
    }
  }

  equipDefaults() {
    const defaults = [
      { wardrobeName: 'head', assetPath: 'head.hair3.latin' },
      { wardrobeName: 'nose', assetPath: 'nose.0' },
      { wardrobeName: 'mouth', assetPath: 'mouth.0' },
      { wardrobeName: 'eyes', assetPath: 'eyes.0' },
      { wardrobeName: 'legs0', assetPath: 'legs.pose0' },
      { wardrobeName: 'legs1', assetPath: 'legs.pose1' },
      { wardrobeName: 'arms0', assetPath: 'arms.sidefront.sign1.pose3.latin' },
      { wardrobeName: 'body', assetPath: 'body.2' },
      { wardrobeName: 'arms1', assetPath: 'arms.sideback.sign0.pose2.latin' }
    ]

    defaults.forEach((data) => {
      EventBus.$emit('equip', data)
    })
  }

  /** Download function **/
  download() {
    const svgData = document.getElementById('avatar')!.outerHTML
    const svgBlob = new Blob([svgData], { type: 'image/svg+xml;charset=utf-8' })
    const svgUrl = URL.createObjectURL(svgBlob)
    const downloadLink = document.createElement('a')
    downloadLink.href = svgUrl
    downloadLink.download = 'newesttree.svg'
    document.body.appendChild(downloadLink)
    downloadLink.click()
    document.body.removeChild(downloadLink)
  }
}
</script>

// CSS CONTENT
<style>
@import url('https://fonts.googleapis.com/css2?family=Roboto:wght@100;300;400;500;700;900&display=swap');

body,
html {
  height: 100%;
  width: 100%;
  padding: 0px;
  margin: 0px;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  font-family: 'Roboto';
  font-weight: 100;
  overflow: hidden;
}
main {
  display: flex;
  flex-direction: column;
  align-items: center;
}
h3 {
  margin: 0px;
  text-align: center;
  font-size: 1rem;
  font-weight: 300;
}
#center {
  display: flex;
  flex-direction: column;
  align-items: center;
}
#topRow {
  position: relative;
  z-index: 10;
  display: flex;
  justify-content: center;
}
#middleRow {
  display: flex;
  align-items: center;
  height: 420px;
}
#leftWardrobes,
#rightWardrobes {
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: space-between;
  height: 100%;
}
#scroller svg {
  padding: 8px;
  opacity: 0.65;
  transition: opacity 0.3s ease;
  cursor: pointer;
}
#scroller svg.equipped {
  opacity: 1;
  border: 1px solid black;
  border-radius: 10px;
}
#scroller svg:hover {
  opacity: 1;
}

#downloadButton {
  display: flex;
  align-items: center;
  padding: 5px 20px;
  justify-content: space-around;
  background: transparent;
  cursor: pointer;
  border: 1px solid rgba(0, 0, 0, 0.8);
  border-radius: 10px;
  transition: all 0.3s ease;
}
#downloadButton img {
  margin-left: 20px;
}
#downloadButton:hover {
  border-color: transparent;
  padding: 5px 100px;
}
#flipIcon {
  position: absolute;
  z-index: 15;
  top: 10px;
  right: 10px;
  height: 40px;
  cursor: pointer;
  opacity: 0.8;
  transition: all 0.25s ease;
}
#flipIcon:hover {
  opacity: 0.5;
  transform: rotateY(180deg);
}
.clsbody0-1,
.clsbody1-1,
.clsbody2-1,
.clsbody3-1 {
  stroke: transparent !important;
}
</style>
