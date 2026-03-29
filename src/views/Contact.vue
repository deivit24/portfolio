<script setup>
import NavSidebar from '../components/NavSidebar.vue'
import resumeUrl from '../assets/David_Salazar_Resume_2026.pdf?url'

// Inline SVGs — minimal, 15×15, filled currentColor
const ICONS = {
  location: `<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2C8.13 2 5 5.13 5 9c0 5.25 7 13 7 13s7-7.75 7-13c0-3.87-3.13-7-7-7zm0 9.5c-1.38 0-2.5-1.12-2.5-2.5s1.12-2.5 2.5-2.5 2.5 1.12 2.5 2.5-1.12 2.5-2.5 2.5z"/></svg>`,
  phone:    `<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M6.62 10.79c1.44 2.83 3.76 5.14 6.59 6.59l2.2-2.2c.27-.27.67-.36 1.02-.24 1.12.37 2.33.57 3.57.57.55 0 1 .45 1 1V20c0 .55-.45 1-1 1-9.39 0-17-7.61-17-17 0-.55.45-1 1-1h3.5c.55 0 1 .45 1 1 0 1.25.2 2.45.57 3.57.11.35.03.74-.25 1.02l-2.2 2.2z"/></svg>`,
  email:    `<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M20 4H4c-1.1 0-2 .9-2 2v12c0 1.1.9 2 2 2h16c1.1 0 2-.9 2-2V6c0-1.1-.9-2-2-2zm0 4l-8 5-8-5V6l8 5 8-5v2z"/></svg>`,
  linkedin: `<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M19 3a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2H5a2 2 0 0 1-2-2V5a2 2 0 0 1 2-2h14m-.5 15.5v-5.3a3.26 3.26 0 0 0-3.26-3.26c-.85 0-1.84.52-2.32 1.3v-1.11h-2.79v8.37h2.79v-4.93c0-.77.62-1.4 1.39-1.4a1.4 1.4 0 0 1 1.4 1.4v4.93h2.79M6.88 8.56a1.68 1.68 0 0 0 1.68-1.68c0-.93-.75-1.69-1.68-1.69a1.69 1.69 0 0 0-1.69 1.69c0 .93.76 1.68 1.69 1.68m1.39 9.94v-8.37H5.5v8.37h2.77z"/></svg>`,
  github:   `<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M12 2A10 10 0 0 0 2 12c0 4.42 2.87 8.17 6.84 9.5.5.08.66-.23.66-.5v-1.69c-2.77.6-3.36-1.34-3.36-1.34-.46-1.16-1.11-1.47-1.11-1.47-.91-.62.07-.6.07-.6 1 .07 1.53 1.03 1.53 1.03.87 1.52 2.34 1.07 2.91.83.09-.65.35-1.09.63-1.34-2.22-.25-4.55-1.11-4.55-4.92 0-1.11.38-2 1.03-2.71-.1-.25-.45-1.29.1-2.64 0 0 .84-.27 2.75 1.02.79-.22 1.65-.33 2.5-.33.85 0 1.71.11 2.5.33 1.91-1.29 2.75-1.02 2.75-1.02.55 1.35.2 2.39.1 2.64.65.71 1.03 1.6 1.03 2.71 0 3.82-2.34 4.66-4.57 4.91.36.31.69.92.69 1.85V21c0 .27.16.59.67.5C19.14 20.16 22 16.42 22 12A10 10 0 0 0 12 2z"/></svg>`,
  instagram:`<svg xmlns="http://www.w3.org/2000/svg" width="15" height="15" viewBox="0 0 24 24" fill="currentColor"><path d="M7.8 2h8.4C19.4 2 22 4.6 22 7.8v8.4a5.8 5.8 0 0 1-5.8 5.8H7.8C4.6 22 2 19.4 2 16.2V7.8A5.8 5.8 0 0 1 7.8 2m-.2 2A3.6 3.6 0 0 0 4 7.6v8.8C4 18.39 5.61 20 7.6 20h8.8a3.6 3.6 0 0 0 3.6-3.6V7.6C20 5.61 18.39 4 16.4 4H7.6m9.65 1.5a1.25 1.25 0 0 1 1.25 1.25A1.25 1.25 0 0 1 17.25 8 1.25 1.25 0 0 1 16 6.75a1.25 1.25 0 0 1 1.25-1.25M12 7a5 5 0 0 1 5 5 5 5 0 0 1-5 5 5 5 0 0 1-5-5 5 5 0 0 1 5-5m0 2a3 3 0 0 0-3 3 3 3 0 0 0 3 3 3 3 0 0 0 3-3 3 3 0 0 0-3-3z"/></svg>`,
}

const CONTACTS = [
  {
    icon:  'location',
    label: 'Address',
    value: 'Austin, TX 78727',
  },
  {
    icon:  'phone',
    label: 'Phone',
    value: '956-867-6720',
    href:  'tel:9568676720',
  },
  {
    icon:  'email',
    label: 'E-mail',
    value: 'david.asal@hotmail.com',
    href:  'mailto:david.asal@hotmail.com',
  },
  {
    icon:  'linkedin',
    label: 'LinkedIn',
    value: 'linkedin.com/in/david-a-salazar',
    href:  'https://www.linkedin.com/in/david-a-salazar-aa6a2750/',
  },
  {
    icon:  'github',
    label: 'GitHub',
    value: 'github.com/deivit24',
    href:  'https://github.com/deivit24',
  },
  {
    icon:  'instagram',
    label: 'Instagram',
    value: 'instagram.com/deivit24',
    href:  'https://www.instagram.com/deivit24',
  },
]
</script>

<template>
  <div class="page">
    <div class="layout">

      <aside class="col-nav">
        <NavSidebar active="contact" />
      </aside>

      <main class="contact">
        <h1 class="heading">[ David Salazar ]</h1>

        <div class="gap" />

        <ul class="contact-list">
          <li v-for="item in CONTACTS" :key="item.label" class="contact-item">
            <span class="item-icon" v-html="ICONS[item.icon]" aria-hidden="true" />
            <div class="item-body">
              <div class="item-label">{{ item.label }}</div>
              <a
                v-if="item.href"
                :href="item.href"
                class="item-value link"
                target="_blank"
                rel="noopener noreferrer"
              >{{ item.value }}</a>
              <span v-else class="item-value">{{ item.value }}</span>
            </div>
          </li>
        </ul>

        <div class="gap" />
        <div class="gap" />

        <a
          :href="resumeUrl"
          download="David_Salazar_Resume_2026.pdf"
          class="resume-btn"
        >[ Download Resume ]</a>
      </main>

    </div>
  </div>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────── */
.page {
  font-family: 'Courier New', Courier, monospace;
  font-size: 13px;
  line-height: 1.5;
  color: #000;
  background: #fff;
  min-height: 100vh;
  padding: 40px;
  box-sizing: border-box;
}

/* ── Grid layout ─────────────────────────────────── */
.layout {
  display: grid;
  grid-template-columns: 250px 1fr;
  gap: 50px;
  align-items: start;
}

@media (max-width: 599px) {
  .page   { padding: 15px; }
  .layout { grid-template-columns: 1fr; gap: 24px; }
}

/* ── Heading ─────────────────────────────────────── */
.heading {
  font-size: 17px;
  font-weight: 700;
  margin: 0;
  letter-spacing: 0.04em;
}

/* ── Contact list ────────────────────────────────── */
.contact-list {
  list-style: none;
  padding: 0;
  margin: 0;
  display: flex;
  flex-direction: column;
  gap: 18px;
}

.contact-item {
  display: flex;
  align-items: flex-start;
  gap: 12px;
}

.item-icon {
  flex-shrink: 0;
  width: 15px;
  height: 15px;
  margin-top: 3px;
  display: flex;
  align-items: center;
  justify-content: center;
}

.item-label {
  font-weight: 700;
  font-size: 10px;
  text-transform: uppercase;
  letter-spacing: 0.1em;
  color: #888;
  margin-bottom: 1px;
}

.item-value {
  color: #000;
  display: block;
}

.item-value.link {
  text-decoration: none;
  cursor: pointer;
}

.item-value.link:hover { color: orangered; }

/* ── Resume button ───────────────────────────────── */
.resume-btn {
  display: inline-block;
  font-family: 'Courier New', Courier, monospace;
  font-size: 13px;
  font-weight: 700;
  color: #000;
  text-decoration: none;
  border: 1.5px solid #000;
  padding: 8px 18px;
  letter-spacing: 0.05em;
  transition: background 0.15s, color 0.15s;
}

.resume-btn:hover {
  background: #000;
  color: #fff;
}

/* ── Spacing ─────────────────────────────────────── */
.gap { height: 2em; }
</style>
