<template>
  <div class="home">
    <h1 class="title">Resolution Finder</h1>
    <div class="selector">
      <span>적용 해상도(dp):</span>
      <select v-model="selectedResolutionIndex" class="select-box resolution">
        <option :value="index" v-for="(resolution, index) in resolutions">{{resolution.acronym}}: {{resolution.width}}X{{resolution.height}}</option>
      </select>
      <input type="number" v-model="resolutions[selectedResolutionIndex].width" :disabled="selectedResolutionIndex !== (resolutions.length - 1)"/> X <input type="number" v-model="resolutions[selectedResolutionIndex].height" :disabled="selectedResolutionIndex !== (resolutions.length - 1)"/>
      <span>스크린 사이즈(inch):</span>
      <select v-model="selectedScreenSize" class="select-box screen-size">
        <option disabled value="-1">스크린 사이즈를 선택하세요.</option>
        <option :value="screenSize" v-for="(screenSize, index) in screenSizes">{{screenSize}} inch</option>
      </select>
      <span>윈도우 시스템 배율(%):</span>
      <select v-model="selectedSystemMagnification" class="select-box system-magnification">
        <option disabled value="-1">시스템 배율을 선택하세요.</option>
        <option :value="systemMagnification" v-for="(systemMagnification, index) in systemMagnifications">{{systemMagnification}}%</option>
      </select>
    </div>
    <div class="reference">
      <h3>해당 사항: {{resolutions[selectedResolutionIndex].acronym}}</h3>
      <h3>Aspect Ratio: {{aspectRatio}}</h3>
      <h3>Computed Resolution(dp): {{realResolution}}</h3>
      <h3>1dp = {{pixelSize}}mm</h3>
      <h3>16dp = {{physicalSize}}mm</h3>
      <div class="border"></div>
      <a href="https://material.io/tools/devices/">Device Metrics</a>
      <iframe src="https://material.io/tools/devices/" width="100%" height="1000px" frameborder="0"></iframe>
      <a href="http://gs.statcounter.com/">Resolution Market Share</a>
      <iframe src="http://gs.statcounter.com/" width="100%" height="1000px" frameborder="0"></iframe>
      <a href="https://netmarketshare.com/">Device Metrics</a>
      <iframe src="https://netmarketshare.com/" width="100%" height="1000px" frameborder="0"></iframe>
    </div>
  </div>
</template>

<script>
export default {
  name: 'home',
  computed: {
    physicalSize () {
      let resolution = this.resolutions[this.selectedResolutionIndex]
      let w = resolution.width
      let h = resolution.height
      let d = Math.sqrt(Math.pow(w, 2) + Math.pow(h, 2))
      return (this.selectedScreenSize / d * 16 * 2.54 * this.selectedSystemMagnification / 10).toFixed(3)
    },
    pixelSize () {
      let resolution = this.resolutions[this.selectedResolutionIndex]
      let w = resolution.width
      let h = resolution.height
      let d = Math.sqrt(Math.pow(w, 2) + Math.pow(h, 2))
      return (this.selectedScreenSize / d * 2.54 * this.selectedSystemMagnification / 10).toFixed(3)
    },
    realResolution () {
      let resolution = this.resolutions[this.selectedResolutionIndex]
      let w = resolution.width
      let h = resolution.height
      return Math.round(w / this.selectedSystemMagnification * 100) + 'X' + Math.round(h / this.selectedSystemMagnification * 100)
    },
    aspectRatio () {
      function gcd (a, b) {
        return (b === 0) ? a : gcd (b, a%b)
      }
      if(this.selectedResolutionIndex >= 0) {
        let resolution = this.resolutions[this.selectedResolutionIndex]
        let w = resolution.width
        let h = resolution.height
        let r = gcd (w, h)
        return (w / r) + ':' + (h / r)
      } else {
        return '해상도를 선택하세요.'
      }
    }
  },
  data () {
    return {
      selectedResolutionIndex: 0,
      selectedScreenSize: 11,
      selectedSystemMagnification: 100,
      systemMagnifications: [75, 100, 125, 150, 175, 200, 225, 250, 275, 300],
      screenSizes: [5, 6, 7, 8, 9, 10, 11, 12, 13, 14, 15, 16, 17, 18, 19, 20, 21, 22, 23, 24, 25, 26, 27, 28, 29, 30],
      resolutions: [
        {
          width: 3840,
          height: 2160,
          acronym: '4k 모니터'
        },
        {
          width: 2560,
          height: 1440,
          acronym: '3k 모니터'
        },
        {
          width: 1920,
          height: 1080,
          acronym: '2k 모니터'
        },
        {
          width: 1680,
          height: 1050,
          acronym: '???'
        },
        {
          width: 1600,
          height: 900,
          acronym: '4:3 모니터'
        },
        {
          width: 1440,
          height: 990,
          acronym: '???'
        },
        {
          width: 1366,
          height: 768,
          acronym: '울트라북, 맥북 에어'
        },
        {
          width: 1536,
          height: 864,
          acronym: '???'
        },
        {
          width: 1280,
          height: 1024,
          acronym: '???'
        },
        {
          width: 1280,
          height: 800,
          acronym: '???'
        },
        {
          width: 1280,
          height: 720,
          acronym: '???'
        },
        {
          width: 1024,
          height: 768,
          acronym: '태플릿 Landscape'
        },
        {
          width: 360,
          height: 640,
          acronym: 'Android'
        },
        {
          width: 411,
          height: 731,
          acronym: 'Android 대화면'
        },
        {
          width: 360,
          height: 740,
          acronym: 'Galaxy S8'
        },
        {
          width: 0,
          height: 0,
          acronym: '사용자 정의'
        },
        {
          width: 0,
          height: 0,
          acronym: '사용자 정의'
        }
      ]
    }
  }
}
</script>
<style>
  .selector > span {
    margin: 0 0 0 20px;
  }
  .select-box {
    font-size: 16px;
    margin: 0 10px;
  }
  .reference {
    border-top: 2px solid #dddddd;
    margin-top: 60px;
  }
  .reference a {
    margin-top: 36px;
    margin-bottom: 12px;
    display: block;
    font-size: 24px;
  }
  .border {
    height: 2px;
    background-color: #dddddd;
  }
  iframe {
    width: calc(100% - 120px);
    height: 768px;
    outline: none;
  }
</style>
