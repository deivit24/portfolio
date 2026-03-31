<script setup>
import { ref } from 'vue'
import NavSidebar from '../components/NavSidebar.vue'

// type: 'edu' | 'work'
// end: null means ongoing / still running
// bullets: only present on interactive work entries
const EXPERIENCE = [
  {
    start: '2011',
    end:   '2014',
    type:  'edu',
    org:   'Lipscomb University',
    role:  'BA Accounting',
    loc:   'Nashville, TN',
    bio:   'I attended Lipscomb on a tennis scholarship, competing at the D1 level. I loved my team and genuinely cherish that experience. That said, I never felt a real pull toward an accounting career — I chose the major because it seemed like the practical thing to do. Like most people in their early twenties, I didn\'t fully know what I wanted yet. I just knew I still loved tennis and wanted to stay connected to it. So when the opportunity came to help coach the men\'s team at the United States Military Academy at West Point, I took it without hesitation.',
  },
  {
    start: '2014',
    end:   '2015',
    type:  'work',
    org:   'West Point',
    role:  'Asst. Men\'s Tennis Coach',
    loc:   'New York',
    bio:   'An amazing experience. I helped coach during the 2014 fall and 2015 spring seasons — and we won the conference. I learned an enormous amount from head coach Jim Poling, who I owe a great deal to. Through him I also met Randy Rowley, who ran the summer camps and was Associate Head Coach at the time (now Head Coach). Randy really pushed me to pursue this opportunity, and I\'m grateful he did. I should mention — some of the cadets I was "coaching" were the same age as me and far more mature. I mean, it\'s USMA. That kind of puts things in perspective. Around this time I got engaged, and my wife and I were both at a crossroads. She\'s a tennis player too, and we were in the exact same boat — so we both applied to become tennis coaches and have our Masters paid for at Durham University in the United Kingdom. We both got accepted. On to the next chapter.',
  },
  {
    start: '2015',
    end:   '2017',
    type:  'edu',
    org:   'Durham University',
    role:  'MSc Finance',
    loc:   'United Kingdom',
    bio:   'I was completing my Masters in Finance while also participating as a tennis player and coach — which made for a full schedule. The experience was genuinely amazing. Durham is a beautiful, historic town. The program was structured differently from anything I\'d done before: heavy on verbal presentations, almost no multiple choice exams, and enough flexibility to complete coursework and dedicate real time to my dissertation. Worth noting — it is supposedly the driest part of the country, and it still rained about half the time. Classic. Toward the end of the program I was offered a position at N+1 Singer as a market maker. Then Brexit happened. The offer was rescinded. I ended up landing a job at Edward Jones and moved to St. Louis — a solid opportunity, but finance was starting to feel less like a calling and more like a fallback. Around this same time I started programming on my own, mostly because it was genuinely fun. That curiosity never really went away.',
  },
  {
    start: '2017',
    end:   null,
    type:  'work',
    org:   'Armis Financial',
    role:  'Owner — Independent Advisory',
    loc:   '',
    bio:   'After a brief stint at Edward Jones as a Financial Advisor, I realized the role wasn\'t for me. I genuinely loved working with individuals and families — but 90% of the job was cold-calling and knocking on doors. That part I could not get behind. So I thought: what if I just opened my own practice? I knew how to program by this point, so I built the site myself — raw JavaScript and CSS. And somewhere in the middle of building it, I fell in love with programming all over again. I ended up building two sites: one for Armis Financial and one for my contracting work. Armis became a place to park my licenses and work with a small handful of people where I could actually make a difference — not chasing quotas, just helping. It was a good chapter. But honestly, my heart was always in the code.',
  },
  {
    start: '2017',
    end:   '2020',
    type:  'work',
    org:   'Fuegoleon Development',
    role:  'Frontend Engineer',
    loc:   '',
    bio:    'This was my self-starter moment. I was still running Armis Financial while picking up contracting jobs on the side. My strategy at the beginning was simple — I\'d walk into small businesses and offer my services for free. The first few months, I worked with a handful of businesses that didn\'t expect much, but figured: why not, it\'s free. That was the plan. I learned how to scope projects, gather requirements, manage client expectations — all of it from scratch. Eventually I started getting tips, and after about a year I began charging real money. That\'s when I started to figure out what my work was actually worth. A friend of mine who was already working as a software engineer — and who I\'d helped out on his own projects — kept telling me I could just apply to jobs. So I enrolled in Springboard, a bootcamp focused less on curriculum and more on networking and mentorship, which was exactly what I was missing. I ended up landing a job at P3F before I even finished. They gave me the certificate anyway.',
  },
  {
    start: '2020',
    end:   '2021',
    type:  'work',
    org:   'P3F (Point3 Federal)',
    role:  'Software Engineer',
    loc:   '',
    bullets: [
      'Joined as a junior engineer — quickly moved from fixing small bugs to owning feature development end-to-end.',
      'Built REST APIs with Django and integrated them with Vue.js frontends.',
      'Participated in sprint planning and code review processes in an Agile environment.',
    ],
  },
  {
    start: '2021',
    end:   '2022',
    type:  'work',
    org:   'P3F (Point3 Federal)',
    role:  'Software Engineer II',
    loc:   '',
    bullets: [
      'Managed client communication and strategic planning for a Government project, ensuring seamless collaboration and a clear understanding of client needs.',
      'Orchestrated the deployment of applications and performed essential DevOps operations, optimizing the efficiency and reliability of systems.',
      'Developed a Python-based video uploading tool integrated with Minio, enhancing the value of cybersecurity lessons by providing a targeted and effective platform for content delivery.',
      'Led complex feature implementations and software refactoring across multiple services.',
      'Served as the primary client point of contact, translating technical concepts for non-technical stakeholders.',
    ],
  },
  {
    start: '2022',
    end:   '2023',
    type:  'work',
    org:   'CreatorIQ / Tribe Dynamics',
    role:  'Fullstack Software Engineer',
    loc:   '',
    bullets: [
      'Held full ownership of features and cycle projects using JavaScript/Vue.js and Python/Django.',
      'Architected and implemented an AWS Lambda/API Gateway service using Python and Flask for the Gifting and Seeding project.',
      'Enabled clients to send gifts to creators and efficiently track engagement, enhancing the overall functionality of the platform.',
      'Led the client interface build using React, collaborating closely with PMs and Designers for the Gifting and Seeding project.',
      'Worked within a larger engineering team with strong product org processes and well-defined sprint cycles.',
    ],
  },
  {
    start: '2023',
    end:   null,
    type:  'work',
    org:   'Aledade',
    role:  'Senior Software / Data Engineer',
    loc:   '',
    bullets: [
      'Built dbt + Snowflake data models for Aledade\'s IPO Readiness project, including patient attribution models that saved the analytics team hundreds of hours of manual work.',
      'Built an internal Git-based deprecation scanner using Python with Airflow orchestration during a large-scale model migration. The scanner performs weekly repo scans to detect deprecated assets, outputs CSVs consumed by dbt models, and surfaces utilization metrics in Snowflake dashboards. Integrates with PRs to automatically warn developers when referencing deprecated assets.',
      'Developed a Snowflake cost-tracking dashboard that provided visibility into compute and storage usage. Identified a critical bug preventing deletion of production database snapshots, saving the company over $100K in wasted storage costs.',
      'Designed and implemented a Snowflake-to-Postgres sync pipeline using Kafka and Postgres sinks. Built an alerting system to detect record misalignments and a tombstone script to correctly handle deletes and restore synchronization.',
      'Architected and deployed a solution leveraging Snowflake streams with Kafka to generate real-time patient events for the Patient Domain team, enabling timely identification of patients needing outreach from their primary care providers.',
    ],
  },
]

const hoveredWork = ref(null)

function dateRange(exp) {
  return exp.end ? `${exp.start} – ${exp.end}` : `${exp.start} – Present`
}
</script>

<template>
  <div class="page">
    <div class="layout">

      <aside class="col-nav">
        <NavSidebar active="profile" />
      </aside>

      <!-- ────────────── TIMELINE ────────────── -->
      <section class="timeline">
          <p class="section-name">[ David Salazar ]</p>
          <div class="gap-xl" />
          <p class="timeline-hint">[ hover entries to explore ]</p>
          <div class="gap-md" />

          <div
            v-for="(exp, i) in EXPERIENCE"
            :key="i"
            class="exp-block"
            :class="{ interactive: exp.bullets || exp.bio }"
            @mouseenter="(exp.bullets || exp.bio) ? hoveredWork = exp : null"
            @mouseleave="(exp.bullets || exp.bio) ? hoveredWork = null : null"
          >
            <!-- type badge + date range -->
            <div class="exp-header">
              <span class="exp-tag" :class="exp.type">
                {{ exp.type === 'edu' ? '[E]' : '[W]' }}
              </span>
              <span class="exp-dates">{{ dateRange(exp) }}</span>
            </div>
            <!-- details -->
            <div class="exp-detail">
              <div class="exp-org">{{ exp.org }}</div>
              <div class="exp-role">{{ exp.role }}</div>
              <div v-if="exp.loc" class="exp-loc">{{ exp.loc }}</div>
            </div>
            <div class="gap-md" />
          </div>

          <!-- legend -->
          <div class="legend">
            <span class="exp-tag edu">[E]</span> Education &nbsp;&nbsp;
            <span class="exp-tag work">[W]</span> Work
          </div>
        </section>

        <!-- Right: who am I / hovered work detail -->
        <section class="bio">
          <Transition name="bio-fade" mode="out-in">

            <!-- Hovered entry -->
            <div v-if="hoveredWork" :key="hoveredWork.org + hoveredWork.role">
              <p class="section-name">{{ hoveredWork.org }}</p>
              <div class="gap-sm" />
              <p class="bio-role-title">{{ hoveredWork.role }} &nbsp;·&nbsp; {{ dateRange(hoveredWork) }}</p>
              <div class="gap-xl" />
              <ul v-if="hoveredWork.bullets" class="bio-bullets">
                <li v-for="(b, bi) in hoveredWork.bullets" :key="bi">{{ b }}</li>
              </ul>
              <p v-else-if="hoveredWork.bio" class="bio-body">{{ hoveredWork.bio }}</p>
            </div>

            <!-- Default: Who Am I -->
            <div v-else key="default">
              <p class="section-name">Who Am I</p>
              <div class="gap-xl" />

              <p class="bio-body">
                Born in McAllen, TX and raised a tennis player. I've lived across
                the country and abroad:
              </p>

              <ul class="bio-list">
                <li><span class="bio-city">Nashville</span> — Lipscomb University, BA in Accounting</li>
                <li><span class="bio-city">New York</span> — Assistant Men's Tennis Coach at West Point. Go Army, Beat Navy!</li>
                <li><span class="bio-city">United Kingdom</span> — Durham University, Masters in Finance</li>
                <li><span class="bio-city">St. Louis</span> — Large financial services firm</li>
              </ul>

              <div class="gap-md" />

              <p class="bio-body">
                My home base is Austin, TX — but I spend summers in Long Island and
                make it a point to visit Japan once or twice a year. I founded
                Armis Financial, build web applications on the side, and currently
                work at Aledade as a Senior Software Engineer.
              </p>

              <div class="gap-md" />

              <p class="bio-body">
                I previously worked at P3F (Point3 Federal), where I started fixing
                small bugs and grew into leading complex feature implementations,
                software refactoring, and serving as the client point of contact —
                moving from Software Engineer to Software Engineer II.
              </p>

              <div class="gap-md" />

              <p class="bio-body">
                I then joined Tribe Dynamics, a CreatorIQ company — a larger team
                with stronger processes and a great product org. I managed more
                complex features and grew a lot there. Unfortunately a company-wide
                layoff ended that chapter.
              </p>

              <div class="gap-md" />

              <p class="bio-body">
                Thanks to the network I built at P3F, I landed at Aledade as a
                Senior Software Engineer. With 1,500+ employees and 300+ in
                Engineering & Product, I work on the data engineering team with
                dbt and Snowflake — designing and maintaining the data pipeline
                from raw data to actionable insights.
              </p>

              <div class="gap-md" />

              <p class="bio-body">
                I discovered my passion for programming while living in the UK in
                2016. In 2017 I began contracting — building frontend applications
                for small businesses. In January 2020 I enrolled in the Springboard
                Software Engineering Bootcamp, focusing on data structures,
                algorithms, and networking.
              </p>
            </div>

          </Transition>
        </section>

    </div>
  </div>
</template>

<style scoped>
/* ── Base ────────────────────────────────────────────── */
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

/* ── Grid layout ─────────────────────────────────────── */
.layout {
  display: grid;
  grid-template-columns: 250px 260px 1fr;
  grid-template-areas: "nav timeline bio";
  gap: 50px;
  align-items: start;
  max-width: 1600px;
  margin: 0 auto;
}

.col-nav  { grid-area: nav; }
.timeline { grid-area: timeline; }
.bio      { grid-area: bio; }

/* Tablet: nav left, bio above timeline on right */
@media (max-width: 900px) {
  .layout {
    grid-template-columns: 220px 1fr;
    grid-template-areas:
      "nav bio"
      "nav timeline";
    gap: 32px;
  }
  .col-nav { grid-row: 1 / 3; }
}

/* Mobile: single column */
@media (max-width: 599px) {
  .page { padding: 15px; }
  .layout {
    grid-template-columns: 1fr;
    grid-template-areas:
      "nav"
      "bio"
      "timeline";
    gap: 24px;
  }
  .col-nav { grid-row: auto; }
}

/* ── Section heading ─────────────────────────────────── */
.section-name {
  font-size: 17px;
  font-weight: 700;
  margin: 0;
  line-height: 1.2;
}

/* ── Timeline entries ────────────────────────────────── */
.exp-block { margin: 0; }

.exp-header {
  display: flex;
  align-items: baseline;
  gap: 0.5em;
}

/* [E] / [W] badge */
.exp-tag {
  font-weight: 700;
  font-size: 11px;
  flex-shrink: 0;
}

.exp-tag.edu  { color: orangered; }
.exp-tag.work { color: #000; }

.exp-dates {
  font-weight: 700;
  font-size: 12px;
}

.exp-detail {
  padding-left: 2.2em;
  margin-top: 0.15em;
}

.exp-org  { font-weight: 600; }
.exp-role { font-style: italic; color: #333; }
.exp-loc  { color: #777; font-size: 11px; }

/* legend */
.legend {
  margin-top: 1.2em;
  font-size: 11px;
  color: #555;
}

/* ── Bio ─────────────────────────────────────────────── */
.bio-body {
  line-height: 1.8;
  margin: 0;
}

.bio-list {
  margin: 0.6em 0 0 1.2em;
  padding: 0;
  line-height: 1.8;
}

.bio-list li { margin-bottom: 0.2em; }

.bio-city { font-weight: 700; }

/* ── Timeline hint ───────────────────────────────────── */
.timeline-hint {
  font-size: 10px;
  color: #aaa;
  margin: 0;
  letter-spacing: 0.04em;
}

/* ── Interactive timeline entries ────────────────────── */
.exp-block.interactive { cursor: pointer; }

.exp-block.interactive:hover .exp-org  { color: orangered; }
.exp-block.interactive:hover .exp-role { color: orangered; }

/* ── Bio fade transition ─────────────────────────────── */
.bio-fade-enter-active,
.bio-fade-leave-active {
  transition: opacity 0.18s ease;
}
.bio-fade-enter-from,
.bio-fade-leave-to {
  opacity: 0;
}

/* ── Hovered work detail ─────────────────────────────── */
.bio-role-title {
  font-style: italic;
  color: #555;
  margin: 0;
}

.bio-bullets {
  margin: 0 0 0 1.2em;
  padding: 0;
  line-height: 1.8;
}

.bio-bullets li {
  margin-bottom: 0.5em;
}

/* ── Spacing ─────────────────────────────────────────── */
.gap-xl { height: 1.8em; }
.gap-md { height: 1em; }
.gap-sm { height: 0.5em; }

</style>
