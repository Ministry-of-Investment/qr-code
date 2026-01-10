<script setup>
import { Download } from 'lucide-vue-next'
import QRCodeStyling from 'qr-code-styling'

const props = defineProps({
  data: {
    type: String,
    required: true,
  },
  image: {
    type: String,
    default: '',
  },
})
const color = ref('#000000')
const options = {
  "type": "canvas",
  "shape": "square",
  "width": 1200,
  "height": 1200,
  "data": props.data,
  "margin": 10,
  "qrOptions": {
    "typeNumber": "0",
    "mode": "Byte",
    "errorCorrectionLevel": "Q"
  },
  "imageOptions": {
    "saveAsBlob": true,
    "hideBackgroundDots": false,
    "imageSize": 0,
    "margin": 0
  },
  "dotsOptions": {
    "type": "extra-rounded",
    "color": "#142e3f",
    "roundSize": true
  },
  "backgroundOptions": {
    "round": 0,
    "color": 'transparent',
    "gradient": null
  },
  "image": "10cc19bd484118dbcd0a7886a38ceddc.png",
  "dotsOptionsHelper": {
    "colorType": {
      "single": true,
      "gradient": false
    },
    "gradient": {
      "linear": true,
      "radial": false,
      "color1": "#6a1a4c",
      "color2": "#6a1a4c",
      "rotation": "0"
    }
  },
  "cornersSquareOptions": {
    "type": "",
    "color": "#95752c",
    "gradient": null
  },
  "cornersSquareOptionsHelper": {
    "colorType": {
      "single": true,
      "gradient": false
    },
    "gradient": {
      "linear": true,
      "radial": false,
      "color1": "#000000",
      "color2": "#000000",
      "rotation": "0"
    }
  },
  "cornersDotOptions": {
    "type": "",
    "color": "#000000"
  },
  "cornersDotOptionsHelper": {
    "colorType": {
      "single": true,
      "gradient": false
    },
    "gradient": {
      "linear": true,
      "radial": false,
      "color1": "#000000",
      "color2": "#000000",
      "rotation": "0"
    }
  },
  "backgroundOptionsHelper": {
    "colorType": {
      "single": true,
      "gradient": false
    },
    "gradient": {
      "linear": true,
      "radial": false,
      "color1": "#ffffff",
      "color2": "#ffffff",
      "rotation": "0"
    }
  }
}

const qrCode = new QRCodeStyling(options)
const qrCodeEl = ref(null)

function updateColor(newColor) {
  qrCode.update({
    dotsOptions: { type: 'dots', color: newColor, gradient: null },
    cornersSquareOptions: { type: 'extra-rounded', color: newColor },
    cornersDotOptions: { type: 'dot', color: newColor },
  })
}

watch(color, (newColor) => {
  updateColor(newColor)
})

function downloadQRCode() {
  const slug = props.data.split('/').pop()
  qrCode.download({
    extension: 'png',
    name: `qr_${slug}`,
  })
}

function downloadQRCodeSVG() {
  const slug = props.data.split('/').pop()
  qrCode.download({
    extension: 'svg',
    name: `qr_${slug}`,
  })
}

onMounted(() => {
  qrCode.append(qrCodeEl.value)
})
</script>

<template>
  <div class="flex flex-col items-center gap-4">
    <div
      ref="qrCodeEl"
      :data-text="data"
      class="rounded-lg bg-white p-1"
    />
    <div class="flex items-center gap-4">
      <div class="relative flex items-center">
        <div
          class="
            h-8 w-8 cursor-pointer overflow-hidden rounded-full border
            border-gray-300
            dark:border-gray-600
          "
          :style="{ backgroundColor: color }"
          title="Change QR code color"
        >
          <input
            v-model="color"
            type="color"
            class="absolute inset-0 h-full w-full cursor-pointer opacity-0"
            title="Change QR code color"
          >
        </div>
      </div>
      <Button
        variant="outline"
        size="sm"
        @click="downloadQRCode"
      >
        <Download class="mr-2 h-4 w-4" />
        {{ $t('links.download_qr_code') }} (PNG)
      </Button>
      <br>
      <Button
        variant="outline"
        size="sm"
        @click="downloadQRCodeSVG"
      >
        <Download class="mr-2 h-4 w-4" />
        {{ $t('links.download_qr_code') }} (SVG)
      </Button>
    </div>
  </div>
</template>
