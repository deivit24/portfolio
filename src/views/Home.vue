<script setup>
import { ref, computed, onMounted, onBeforeUnmount } from 'vue'
import { RouterLink } from 'vue-router'

const FONT_SIZE_DESKTOP = 13
const FONT_SIZE_MOBILE  = 10
const LINE_HEIGHT = 1.35
const WORD_PAD = '      ' // 4-space breathing room on each side of a word

const vw = ref(window.innerWidth)
const vh = ref(window.innerHeight)

function onResize() {
  vw.value = window.innerWidth
  vh.value = window.innerHeight
}
onMounted(() => window.addEventListener('resize', onResize))
onBeforeUnmount(() => window.removeEventListener('resize', onResize))

const isMobile = computed(() => vw.value < 600)
const padding  = computed(() => isMobile.value ? 15 : 40)
const fontSize = computed(() => isMobile.value ? FONT_SIZE_MOBILE : FONT_SIZE_DESKTOP)

// Monospace: char width ≈ 0.601 × font size (Courier New)
const charW    = computed(() => fontSize.value * 0.601)
const bracketW = computed(() => charW.value * 2) // width of one '[]' pair

// Available content area after subtracting padding on both sides
const innerW = computed(() => vw.value - padding.value * 2)
const innerH = computed(() => vh.value - padding.value * 2)

const WORDS = [
  { rowPct: 7,  colPct: 4,  mobileColPct: 4,  text: 'David Salazar',    bold: true, link: '/about' },
  { rowPct: 7,  colPct: 22, mobileColPct: 22, text: '<<=====' },
  { rowPct: 21, colPct: 52, mobileColPct: 18, text: 'Software Engineer' },
  { rowPct: 36, colPct: 8,  mobileColPct: 8,  text: 'Data Engineer'     },
  { rowPct: 50, colPct: 71, mobileColPct: 5,  text: 'Python'            },
  { rowPct: 63, colPct: 27, mobileColPct: 12, text: 'Javascript'        },
  { rowPct: 78, colPct: 63, mobileColPct: 5,  text: 'Architect'         },
]

const lines = computed(() => {
  const lineH     = fontSize.value * LINE_HEIGHT
  const totalRows = Math.ceil(innerH.value / lineH) + 2
  const totalCols = Math.ceil(innerW.value / bracketW.value) + 6
  const mobile    = isMobile.value

  const result = []

  for (let r = 0; r < totalRows; r++) {
    const wordsOnRow = WORDS.filter(
      w => Math.round((w.rowPct / 100) * totalRows) === r
    )

    if (!wordsOnRow.length) {
      result.push([{ type: 'b', text: '[]'.repeat(totalCols) }])
      continue
    }

    const segs   = []
    let cursor   = 0

    const sorted = [...wordsOnRow].sort((a, b) => {
      const ac = mobile && a.mobileColPct != null ? a.mobileColPct : a.colPct
      const bc = mobile && b.mobileColPct != null ? b.mobileColPct : b.colPct
      return ac - bc
    })

    for (const w of sorted) {
      const pct      = mobile && w.mobileColPct != null ? w.mobileColPct : w.colPct
      // padded text includes breathing spaces
      const padded   = `${WORD_PAD}${w.text}${WORD_PAD}`
      // clamp so padded word fits inside the grid
      const maxStart = totalCols - Math.ceil(padded.length / 2) - 1
      const wordCol  = Math.min(Math.floor((pct / 100) * totalCols), maxStart)

      if (wordCol > cursor) {
        segs.push({ type: 'b', text: '[]'.repeat(wordCol - cursor) })
      }

      segs.push({ type: 'w', text: padded, bold: w.bold || false, link: w.link })

      // each char ≈ 0.5 bracket-pair width → length chars ≈ length/2 pairs
      cursor = wordCol + Math.ceil(padded.length / 2)
    }

    if (cursor < totalCols) {
      segs.push({ type: 'b', text: '[]'.repeat(totalCols - cursor) })
    }

    result.push(segs)
  }

  return result
})
</script>

<template>
  <div class="page" :style="{ padding: padding + 'px' }">
    <div class="grid" :style="{ fontSize: fontSize + 'px', lineHeight: '1.35' }">
      <div v-for="(line, i) in lines" :key="i" class="line">
        <template v-for="(seg, j) in line" :key="j">
          <span v-if="seg.type === 'b'" class="br">{{ seg.text }}</span>
          <RouterLink
            v-else-if="seg.link"
            :to="seg.link"
            class="word"
            :class="{ bold: seg.bold }"
          >{{ seg.text }}</RouterLink>
          <span
            v-else
            class="word"
            :class="{ bold: seg.bold }"
          >{{ seg.text }}</span>
        </template>
      </div>
    </div>
  </div>
</template>

<style scoped>
.page {
  width: 100vw;
  height: 100vh;
  background: #fff;
  box-sizing: border-box;
  overflow: hidden;
}

.grid {
  font-family: 'Courier New', Courier, monospace;
  color: #000;
  background: #fff;
  width: 100%;
  height: 100%;
  overflow: hidden;
  user-select: none;
}

.line {
  display: block;
  white-space: nowrap;
  overflow: hidden;
}

.br {
  color: #000;
  letter-spacing: 0;
}

.word {
  color: #000;
  text-decoration: none;
  cursor: default;
  white-space: pre; /* preserve the spaces */
}

.word.bold {
  font-weight: 700;
}

a.word {
  cursor: pointer;
}

a.word:hover {
  color: orangered;
}
</style>
