<script setup>
import { computed, ref, onMounted, onBeforeUnmount } from 'vue'
import { RouterLink } from 'vue-router'

const props = defineProps({
  active: { type: String, default: '' }, // 'profile' | 'contact'
})

// ── Constants ────────────────────────────────────────────────
const NAV_COLS       = 32
const DELAY_PER_CHAR = 7    // ms — controls overall speed of the wave
const WAVE_WIDTH_MS  = NAV_COLS * DELAY_PER_CHAR          // 224ms — one row's worth of time
const TOTAL_CHARS    = 14 * NAV_COLS                      // 14 skills × 32 chars = 448
const RESET_MS       = TOTAL_CHARS * DELAY_PER_CHAR       // last char-on fires at 3136ms
                     + WAVE_WIDTH_MS                      // + 224ms until it turns off
                     + 100                                // small buffer → 3460ms

const CYCLE_MS       = 10000 // ms between cascades

// ── Nav builders ─────────────────────────────────────────────
function bl() { return '[]'.repeat(NAV_COLS / 2) }

function navItem(label, opts = {}) {
  const suffix = ` ${label} ※※`
  const pre    = '※'.repeat(Math.max(0, NAV_COLS - suffix.length))
  return { type: 'item', pre, label, post: '※※', ...opts }
}

const BR = { type: 'br' }

const SKILL_LABELS = [
  'Python',
  'JavaScript',
  'Vue / React',
  'Django / FastAPI',
  'REST APIs',
  'SQL',
  'Data Engineering',
  'ETL',
  'Kafka',
  'Snowflake / Databricks',
  'AWS',
  'Docker',
  'Git',
  'Claude',
]

// Each character in every skill line gets a global index (ci).
// This index drives the animation delay via the CSS custom property --char-i.
const NAV = computed(() => {
  let ci = 0

  function skillLine(label) {
    const pre      = '※※※ '
    const trail    = '※'.repeat(Math.max(0, NAV_COLS - (pre + label + ' ').length))
    const fullText = pre + label + ' ' + trail
    const preLen   = pre.length
    const chars    = [...fullText].map((ch, li) => ({
      ch,
      i:       ci++,
      isLabel: li >= preLen && li < preLen + label.length,
    }))
    return { type: 'skill', chars }
  }

  return [
    BR, BR,
    navItem('David Salazar'),
    BR, BR,
    navItem('Profile', props.active === 'profile' ? { active: true } : { to: '/about' }),
    BR, BR,
    navItem('Contact', props.active === 'contact' ? { active: true } : { to: '/contact' }),
    BR, BR,
    navItem('Skills'),
    BR,
    ...SKILL_LABELS.map(skillLine),
    BR, BR, BR,
  ]
})

// ── Cascade animation ─────────────────────────────────────────
const navRef   = ref(null)
let cycleTimer = null
let resetTimer = null
let initTimer  = null

function triggerCascade() {
  const el = navRef.value
  if (!el) return

  clearTimeout(resetTimer)

  // Force reflow between remove→add so CSS animations restart from zero
  el.classList.remove('animating')
  void el.offsetWidth
  el.classList.add('animating')

  // Remove class after the last character has turned back off
  resetTimer = setTimeout(() => {
    el.classList.remove('animating')
  }, RESET_MS)
}

onMounted(() => {
  initTimer  = setTimeout(() => {
    triggerCascade()
    cycleTimer = setInterval(triggerCascade, CYCLE_MS)
  }, 2000)
})

onBeforeUnmount(() => {
  clearTimeout(initTimer)
  clearTimeout(resetTimer)
  clearInterval(cycleTimer)
})
</script>

<template>
  <nav ref="navRef" class="nav">
    <div v-for="(item, i) in NAV" :key="i" class="nav-line">

      <span v-if="item.type === 'br'">{{ bl() }}</span>

      <template v-else-if="item.type === 'item'">
        <span>{{ item.pre }}</span><!--
        --><RouterLink v-if="item.to" :to="item.to" class="nav-label nav-link"> {{ item.label }} </RouterLink><!--
        --><span v-else class="nav-label" :class="{ active: item.active }"> {{ item.label }} </span><!--
        --><span>{{ item.post }}</span>
      </template>

      <!--
        Each skill character is its own span.
        CSS custom property --char-i drives two staggered animations:
          char-on  fires at (i × 7ms)         → snaps to orangered
          char-off fires at (i × 7ms + 224ms)  → snaps back to black
        224ms = one row-width (32 chars × 7ms), so each row starts
        reverting exactly as the next row begins lighting up.
      -->
      <template v-else-if="item.type === 'skill'">
        <span
          v-for="c in item.chars"
          :key="c.i"
          class="skill-char"
          :class="{ 'skill-label': c.isLabel }"
          :style="{ '--char-i': c.i }"
        >{{ c.ch }}</span>
      </template>

    </div>
  </nav>
</template>

<style scoped>
.nav {
  font-family: 'Courier New', Courier, monospace;
  font-size: 13px;
  overflow: hidden;
}

.nav-line {
  display: flex;
  white-space: nowrap;
  overflow: hidden;
  line-height: 1.35;
}

/* ── Nav labels ──────────────────────────────────────── */
.nav-label {
  color: #000;
  text-decoration: none;
  cursor: default;
  white-space: pre;
  font-weight: 700;
}

.nav-label.active { color: orangered; }

.nav-link {
  color: #000;
  text-decoration: none;
  cursor: pointer;
  white-space: pre;
  font-weight: 700;
}

.nav-link:hover { color: orangered; }

/* ── Skill characters ────────────────────────────────── */
.skill-label { font-weight: 700; }

/*
 * Two 1ms animations per character, both fill-forward.
 * Because char-off is listed AFTER char-on, it wins when both
 * are in their fill state (CSS spec: later animation takes precedence).
 *
 *   char-on  delay = --char-i × 7ms          → snap to orangered
 *   char-off delay = --char-i × 7ms + 224ms  → snap back to #000
 *
 * 224ms = NAV_COLS(32) × DELAY_PER_CHAR(7ms) = one row-width.
 * Result: each row turns off just as the next row turns on — a
 * sliding-window wave that cascades down all 14 skill lines.
 */
@keyframes char-on  { to { color: orangered; } }
@keyframes char-off { to { color: #000; } }

.nav.animating .skill-char {
  animation:
    char-on  1ms linear forwards calc(var(--char-i) * 7ms),
    char-off 1ms linear forwards calc(var(--char-i) * 7ms + 224ms);
}
</style>
