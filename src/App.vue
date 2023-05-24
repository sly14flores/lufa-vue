<template>
  <div
    id="app"
    :class="{ 'has-mouse': hasMouse }"
    @touchstart="hasMouse = false"
  >
    <Flipbook
      class="flipbook"
      :pages="pages"
      :pagesHiRes="pagesHiRes"
      :startPage="pageNum"
      v-slot="flipbook"
      ref="flipbook"
      @flip-left-start="onFlipLeftStart"
      @flip-left-end="onFlipLeftEnd"
      @flip-right-start="onFlipRightStart"
      @flip-right-end="onFlipRightEnd"
      @zoom-start="onZoomStart"
      @zoom-end="onZoomEnd"
    >
      <div class="action-bar">
        <left-icon
          class="btn left"
          :class="{ disabled: !flipbook.canFlipLeft }"
          @click="flipbook.flipLeft"
        />
        <plus-icon
          class="btn plus"
          :class="{ disabled: !flipbook.canZoomIn }"
          @click="flipbook.zoomIn"
        />
        <span class="page-num">
          Page {{ flipbook.page }} of {{ flipbook.numPages }}
        </span>
        <minus-icon
          class="btn minus"
          :class="{ disabled: !flipbook.canZoomOut }"
          @click="flipbook.zoomOut"
        />
        <right-icon
          class="btn right"
          :class="{ disabled: !flipbook.canFlipRight }"
          @click="flipbook.flipRight"
        />
      </div>
    </Flipbook>
    <p class="credit"></p>
  </div>
</template>

<script>
import 'vue-material-design-icons/styles.css'
import LeftIcon from 'vue-material-design-icons/ChevronLeftCircle'
import RightIcon from 'vue-material-design-icons/ChevronRightCircle'
import PlusIcon from 'vue-material-design-icons/PlusCircle'
import MinusIcon from 'vue-material-design-icons/MinusCircle'
import Flipbook from 'flipbook-vue'
import Ribbon from './Ribbon'

export default {
  components: { Flipbook, LeftIcon, RightIcon, PlusIcon, MinusIcon, Ribbon },
  data() {
    return {
      pages: [],
      pagesHiRes: [],
      hasMouse: true,
      pageNum: null,
    }
  },
  methods: {
    onFlipLeftStart(page) { console.log('flip-left-start', page) },
    onFlipLeftEnd(page) {
      console.log('flip-left-end', page)
      window.location.hash = '#' + page
    },
    onFlipRightStart(page) { console.log('flip-right-start', page) },
    onFlipRightEnd(page) {
      console.log('flip-right-end', page)
      window.location.hash = '#' + page
    },
    onZoomStart(zoom) {
      console.log('zoom-start', zoom)
    },
    onZoomEnd(zoom) {
      console.log('zoom-end', zoom)
    },
    setPageFromHash() {
      const n = parseInt(window.location.hash.slice(1), 10)
      if (isFinite(n)) this.pageNum = n
    },
  },
  mounted() {
    window.addEventListener('keydown', (ev) => {
      const flipbook = this.$refs.flipbook
      if (!flipbook) return
      if (ev.keyCode == 37 && flipbook.canFlipLeft) flipbook.flipLeft()
      if (ev.keyCode == 39 && flipbook.canFlipRight) flipbook.flipRight()
    })

    // Simulate asynchronous pages initialization
    setTimeout(() => {
      this.pages = [
        null,
        'https://storage.launion.gov.ph/lufa/3MB/00COVER.gif',
        'https://storage.launion.gov.ph/lufa/3MB/01ABTCOVER-1ST-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/02ABTCOVER-2ND-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/03TOC-1ST-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/04TOC-2ND-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/5President.gif',
        'https://storage.launion.gov.ph/lufa/3MB/6Vice-President.gif',
        'https://storage.launion.gov.ph/lufa/3MB/7Senate-President.gif',
        'https://storage.launion.gov.ph/lufa/3MB/8Speaker-of-the-house-representative.gif',
        'https://storage.launion.gov.ph/lufa/3MB/9DILG-Secretary.gif',
        'https://storage.launion.gov.ph/lufa/3MB/10DILG-Provincial-Director.gif',
        'https://storage.launion.gov.ph/lufa/3MB/11PNP-Chiefv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/12PNP-Provincial-Director.gif',
        'https://storage.launion.gov.ph/lufa/3MB/13Governorv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/14Vice-Governor.gif',
        'https://storage.launion.gov.ph/lufa/3MB/15First-District.gif',
        'https://storage.launion.gov.ph/lufa/3MB/16Second-District-Rep.gif',
        'https://storage.launion.gov.ph/lufa/3MB/17Governors-Galleryv2-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/18Governors-Galleryv2-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/19.Get-to-Know-Your-Governor-v3.gif',
        'https://storage.launion.gov.ph/lufa/3MB/20.Get-to-Know-Your-Governorv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/21PUSO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/22PUSO.jpg',
        'https://storage.launion.gov.ph/lufa/3MB/23PUSO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/24PUSO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/25Vice-Gov_s-Galleryv2-gifv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/26Vice-Gov_s-Galleryv2-gifv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/27SP-1ST-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/28SP-2ND-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/29AGOO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/30ARINGAY.gif',
        'https://storage.launion.gov.ph/lufa/3MB/31BACNOTAN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/32BAGULIN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/33BALAOAN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/34Bangar.gif',
        'https://storage.launion.gov.ph/lufa/3MB/35Bauang.gif',
        'https://storage.launion.gov.ph/lufa/3MB/36BURGOS.gif',
        'https://storage.launion.gov.ph/lufa/3MB/37CABA.gif',
        'https://storage.launion.gov.ph/lufa/3MB/38SFC.gif',
        'https://storage.launion.gov.ph/lufa/3MB/39LUNA.gif',
        'https://storage.launion.gov.ph/lufa/3MB/40NAGUILIAN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/41PUGO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/42Rosario.gif',
        'https://storage.launion.gov.ph/lufa/3MB/43SAN-GABRIEL.gif',
        'https://storage.launion.gov.ph/lufa/3MB/44SAN-JUAN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/45SANTO-TOMAS.gif',
        'https://storage.launion.gov.ph/lufa/3MB/46SANTOL.gif',
        'https://storage.launion.gov.ph/lufa/3MB/47SUDIPEN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/48TUBAO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/49.GOV-CABINET%20gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/50GOV-CABINET-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/51CALENDAR-1ST-PAGE-copy.gif',
        'https://storage.launion.gov.ph/lufa/3MB/52CALENDAR-2ND-PAGE-copy.gif',
        'https://storage.launion.gov.ph/lufa/3MB/53LUFA-EVENT-PHOTOS-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/54LUFA-EVENTS-PHOTOS-1st-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/55LUFA-EVENTS-PHOTOS-2nd-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/56LUFA-EVENTS-PHOTOS-3rd-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/57LUFA-EVENTS-PHOTOS-4th-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/58LUFA-EVENTS-PHOTOS-5th-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/59MLU-EVENTS-PHOTOS-6th-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/60Mutia-Ti-La-Union---Cover.gif',
        'https://storage.launion.gov.ph/lufa/3MB/61.sudipen.gif',
        'https://storage.launion.gov.ph/lufa/3MB/62bauang.gif',
        'https://storage.launion.gov.ph/lufa/3MB/63agoo.gif',
        'https://storage.launion.gov.ph/lufa/3MB/64san-juan.gif',
        'https://storage.launion.gov.ph/lufa/3MB/65bacnotan.gif',
        'https://storage.launion.gov.ph/lufa/3MB/66aringay.gif',
        'https://storage.launion.gov.ph/lufa/3MB/67bagulin.gif',
        'https://storage.launion.gov.ph/lufa/3MB/68balaoan.gif',
        'https://storage.launion.gov.ph/lufa/3MB/69bangar.gif',
        'https://storage.launion.gov.ph/lufa/3MB/70burgos.gif',
        'https://storage.launion.gov.ph/lufa/3MB/71caba.gif',
        'https://storage.launion.gov.ph/lufa/3MB/72luna.gif',
        'https://storage.launion.gov.ph/lufa/3MB/73naguilian.gif',
        'https://storage.launion.gov.ph/lufa/3MB/74pugo.gif',
        'https://storage.launion.gov.ph/lufa/3MB/75rosario.gif',
        'https://storage.launion.gov.ph/lufa/3MB/76san-fernando.gif',
        'https://storage.launion.gov.ph/lufa/3MB/77san-gabriel.gif',
        'https://storage.launion.gov.ph/lufa/3MB/78santol.gif',
        'https://storage.launion.gov.ph/lufa/3MB/79sto-tomas.gif',
        'https://storage.launion.gov.ph/lufa/3MB/80tubao.gif',
        'https://storage.launion.gov.ph/lufa/3MB/81MLU-EVENT-PHOTOS-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/82steercomv2-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/83steercomv2-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/84EDITORIAL-1ST-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/85EDITORIAL-2ND-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/86EDITORIAL-3RD-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/87.cover.gif',
        'https://storage.launion.gov.ph/lufa/3MB/88TIER-1_colourette.gif',
        'https://storage.launion.gov.ph/lufa/3MB/89TIER-2_KLAD.gif',
        'https://storage.launion.gov.ph/lufa/3MB/90TIER-2_AQN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/91TIER-2_aureo.gif',
        'https://storage.launion.gov.ph/lufa/3MB/92TIER-3_SMB-x-beut.gif',
        'https://storage.launion.gov.ph/lufa/3MB/93TIER-3_nisceskin-x-1590.gif',
        'https://storage.launion.gov.ph/lufa/3MB/94TIER-4_GatchallanTangalin-x-tbird.gif',
        'https://storage.launion.gov.ph/lufa/3MB/96TIER-5-6_1.gif',
        'https://storage.launion.gov.ph/lufa/3MB/95TIER-5-6_2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/98TIER-5-6_3.gif',
        'https://storage.launion.gov.ph/lufa/3MB/97TIER-5-6_4.gif',
        'https://storage.launion.gov.ph/lufa/3MB/99TIER-1_motul.gif',
        'https://storage.launion.gov.ph/lufa/3MB/100Back Page.gif',
        'https://storage.launion.gov.ph/lufa/3MB/100Back Cover.jpg',
      ]
      this.pagesHiRes = [
        null,
        'https://storage.launion.gov.ph/lufa/3MB/00COVER.gif',
        'https://storage.launion.gov.ph/lufa/3MB/01ABTCOVER-1ST-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/02ABTCOVER-2ND-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/03TOC-1ST-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/04TOC-2ND-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/5President.gif',
        'https://storage.launion.gov.ph/lufa/3MB/6Vice-President.gif',
        'https://storage.launion.gov.ph/lufa/3MB/7Senate-President.gif',
        'https://storage.launion.gov.ph/lufa/3MB/8Speaker-of-the-house-representative.gif',
        'https://storage.launion.gov.ph/lufa/3MB/9DILG-Secretary.gif',
        'https://storage.launion.gov.ph/lufa/3MB/10DILG-Provincial-Director.gif',
        'https://storage.launion.gov.ph/lufa/3MB/11PNP-Chiefv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/12PNP-Provincial-Director.gif',
        'https://storage.launion.gov.ph/lufa/3MB/13Governorv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/14Vice-Governor.gif',
        'https://storage.launion.gov.ph/lufa/3MB/15First-District.gif',
        'https://storage.launion.gov.ph/lufa/3MB/16Second-District-Rep.gif',
        'https://storage.launion.gov.ph/lufa/3MB/17Governors-Galleryv2-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/18Governors-Galleryv2-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/19.Get-to-Know-Your-Governor-v3.gif',
        'https://storage.launion.gov.ph/lufa/3MB/20.Get-to-Know-Your-Governorv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/21PUSO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/22PUSO.jpg',
        'https://storage.launion.gov.ph/lufa/3MB/23PUSO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/24PUSO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/25Vice-Gov_s-Galleryv2-gifv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/26Vice-Gov_s-Galleryv2-gifv2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/27SP-1ST-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/28SP-2ND-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/29AGOO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/30ARINGAY.gif',
        'https://storage.launion.gov.ph/lufa/3MB/31BACNOTAN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/32BAGULIN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/33BALAOAN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/34Bangar.gif',
        'https://storage.launion.gov.ph/lufa/3MB/35Bauang.gif',
        'https://storage.launion.gov.ph/lufa/3MB/36BURGOS.gif',
        'https://storage.launion.gov.ph/lufa/3MB/37CABA.gif',
        'https://storage.launion.gov.ph/lufa/3MB/38SFC.gif',
        'https://storage.launion.gov.ph/lufa/3MB/39LUNA.gif',
        'https://storage.launion.gov.ph/lufa/3MB/40NAGUILIAN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/41PUGO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/42Rosario.gif',
        'https://storage.launion.gov.ph/lufa/3MB/43SAN-GABRIEL.gif',
        'https://storage.launion.gov.ph/lufa/3MB/44SAN-JUAN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/45SANTO-TOMAS.gif',
        'https://storage.launion.gov.ph/lufa/3MB/46SANTOL.gif',
        'https://storage.launion.gov.ph/lufa/3MB/47SUDIPEN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/48TUBAO.gif',
        'https://storage.launion.gov.ph/lufa/3MB/49.GOV-CABINET%20gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/50GOV-CABINET-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/51CALENDAR-1ST-PAGE-copy.gif',
        'https://storage.launion.gov.ph/lufa/3MB/52CALENDAR-2ND-PAGE-copy.gif',
        'https://storage.launion.gov.ph/lufa/3MB/53LUFA-EVENT-PHOTOS-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/54LUFA-EVENTS-PHOTOS-1st-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/55LUFA-EVENTS-PHOTOS-2nd-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/56LUFA-EVENTS-PHOTOS-3rd-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/57LUFA-EVENTS-PHOTOS-4th-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/58LUFA-EVENTS-PHOTOS-5th-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/59MLU-EVENTS-PHOTOS-6th-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/60Mutia-Ti-La-Union---Cover.gif',
        'https://storage.launion.gov.ph/lufa/3MB/61.sudipen.gif',
        'https://storage.launion.gov.ph/lufa/3MB/62bauang.gif',
        'https://storage.launion.gov.ph/lufa/3MB/63agoo.gif',
        'https://storage.launion.gov.ph/lufa/3MB/64san-juan.gif',
        'https://storage.launion.gov.ph/lufa/3MB/65bacnotan.gif',
        'https://storage.launion.gov.ph/lufa/3MB/66aringay.gif',
        'https://storage.launion.gov.ph/lufa/3MB/67bagulin.gif',
        'https://storage.launion.gov.ph/lufa/3MB/68balaoan.gif',
        'https://storage.launion.gov.ph/lufa/3MB/69bangar.gif',
        'https://storage.launion.gov.ph/lufa/3MB/70burgos.gif',
        'https://storage.launion.gov.ph/lufa/3MB/71caba.gif',
        'https://storage.launion.gov.ph/lufa/3MB/72luna.gif',
        'https://storage.launion.gov.ph/lufa/3MB/73naguilian.gif',
        'https://storage.launion.gov.ph/lufa/3MB/74pugo.gif',
        'https://storage.launion.gov.ph/lufa/3MB/75rosario.gif',
        'https://storage.launion.gov.ph/lufa/3MB/76san-fernando.gif',
        'https://storage.launion.gov.ph/lufa/3MB/77san-gabriel.gif',
        'https://storage.launion.gov.ph/lufa/3MB/78santol.gif',
        'https://storage.launion.gov.ph/lufa/3MB/79sto-tomas.gif',
        'https://storage.launion.gov.ph/lufa/3MB/80tubao.gif',
        'https://storage.launion.gov.ph/lufa/3MB/81MLU-EVENT-PHOTOS-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/82steercomv2-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/83steercomv2-gif.gif',
        'https://storage.launion.gov.ph/lufa/3MB/84EDITORIAL-1ST-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/85EDITORIAL-2ND-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/86EDITORIAL-3RD-PAGE-3MB.gif',
        'https://storage.launion.gov.ph/lufa/3MB/87.cover.gif',
        'https://storage.launion.gov.ph/lufa/3MB/88TIER-1_colourette.gif',
        'https://storage.launion.gov.ph/lufa/3MB/89TIER-2_KLAD.gif',
        'https://storage.launion.gov.ph/lufa/3MB/90TIER-2_AQN.gif',
        'https://storage.launion.gov.ph/lufa/3MB/91TIER-2_aureo.gif',
        'https://storage.launion.gov.ph/lufa/3MB/92TIER-3_SMB-x-beut.gif',
        'https://storage.launion.gov.ph/lufa/3MB/93TIER-3_nisceskin-x-1590.gif',
        'https://storage.launion.gov.ph/lufa/3MB/94TIER-4_GatchallanTangalin-x-tbird.gif',
        'https://storage.launion.gov.ph/lufa/3MB/96TIER-5-6_1.gif',
        'https://storage.launion.gov.ph/lufa/3MB/95TIER-5-6_2.gif',
        'https://storage.launion.gov.ph/lufa/3MB/98TIER-5-6_3.gif',
        'https://storage.launion.gov.ph/lufa/3MB/97TIER-5-6_4.gif',
        'https://storage.launion.gov.ph/lufa/3MB/99TIER-1_motul.gif',
        'https://storage.launion.gov.ph/lufa/3MB/100Back Page.gif',
        'https://storage.launion.gov.ph/lufa/3MB/100Back Cover.jpg',
      ]
    }, 1)

    window.addEventListener('hashchange', this.setPageFromHash)
    this.setPageFromHash()
  },
}
</script>

<style>
html, body {
  margin: 0;
  padding: 0;
}

#app {
  font-family: 'Avenir', Helvetica, Arial, sans-serif;
  -webkit-font-smoothing: antialiased;
  -moz-osx-font-smoothing: grayscale;
  height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  background-color: #333;
  color: #ccc;
  overflow: hidden;
}

a {
  color: inherit;
}

.action-bar {
  width: 100%;
  height: 30px;
  padding: 10px 0;
  display: flex;
  justify-content: center;
  align-items: center;
}

.action-bar .btn {
  font-size: 30px;
  color: #999;
}

.action-bar .btn svg {
  bottom: 0;
}

.action-bar .btn:not(:first-child) {
  margin-left: 10px;
}

.has-mouse .action-bar .btn:hover {
  color: #ccc;
  filter: drop-shadow(1px 1px 5px #000);
  cursor: pointer;
}

.action-bar .btn:active {
  filter: none !important;
}

.action-bar .btn.disabled {
  color: #666;
  pointer-events: none;
}

.action-bar .page-num {
  font-size: 12px;
  margin-left: 10px;
}

.flipbook .viewport {
  width: 90vw !important;
  height: calc(100vh - 50px - 40px) !important;
}

.flipbook .bounding-box {
  box-shadow: 0 0 20px #000;
}

.credit {
  font-size: 12px;
  line-height: 20px;
  margin: 10px;
}
</style>
