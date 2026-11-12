<template>
  <q-page class="fairytale-page">
    <!-- FULL WEBSITE VIDEO BACKGROUND -->
    <video
      class="site-background-video"
      autoplay
      muted
      loop
      playsinline
      preload="auto"
    >
      <source src="/videos/background.mp4" type="video/mp4" />
    </video>

    <!-- VIDEO OVERLAY -->
    <div class="site-background-overlay"></div>
    <!-- FLOATING PARTICLES -->
    <div class="particle-layer">
      <span
        v-for="n in 24"
        :key="n"
        class="particle"
        :style="{
          '--i': n,
          '--delay': `${n * -0.7}s`,
          '--duration': `${9 + (n % 5)}s`
        }"
        >✦</span
      >
    </div>

    <!-- NAVIGATION -->
    <nav class="floating-nav">
      <a class="nav-brand" href="#home">H.P.B <span>♡</span></a>
      <div class="nav-links">
        <a href="#story">STORY</a>
        <a href="#memories">MEMORIES</a>
        <a href="#garden">GARDEN</a>
        <a href="#secret">SECRET</a>
      </div>
    </nav>

    <!-- HERO -->
    <section class="hero-section" id="home">
      <div class="moon-glow"></div>
      <div class="hero-flower flower-left">✿</div>
      <div class="hero-flower flower-right">❀</div>

      <div class="hero-content">
        <p class="hero-eyebrow">AN ENCHANTED WORLD CREATED FOR</p>
        <div class="hero-symbol">✧</div>

        <h1>
          HARI PERAYAAN
          <span>BUMI</span>
        </h1>

        <div class="hero-divider"> <span></span> ♡ <span></span> </div>

        <p class="hero-description">
          Some stories are not written in books. Some beautiful moments simply
          happen and become a part of our universe.
        </p>

        <p class="hero-for">A little universe for Dini Handayani</p>

        <button class="enter-button" @click="enterWorld">
          ENTER THIS WORLD <span>✧</span>
        </button>

        <div class="scroll-hint">
          SCROLL TO EXPLORE
          <span>↓</span>
        </div>
      </div>
    </section>

    <!-- INTRO -->
    <section class="intro-section" id="story">
      <div class="section-kicker">A STORY BEGINS</div>
      <div class="intro-decoration">❀</div>

      <h2>
        Every little thing
        <span>has its own meaning.</span>
      </h2>

      <p class="intro-text">
        Tidak semua cerita dimulai dengan sesuatu yang luar biasa. Terkadang
        semuanya bermula dari pertemuan sederhana, percakapan singkat, dan waktu
        yang berjalan tanpa kita sadari.
      </p>

      <div class="intro-signature">And this is our little story ♡</div>
    </section>

    <!-- STORY JOURNEY -->
    <section class="story-section">
      <div class="section-kicker">THE LITTLE CHRONICLES</div>

      <h2 class="section-title">
        Four Chapters,
        <span>One Beautiful Journey.</span>
      </h2>

      <div class="timeline">
        <article
          v-for="(chapter, index) in chapters"
          :key="chapter.number"
          class="chapter-card"
          :class="{ 'chapter-reverse': index % 2 !== 0 }"
        >
          <div class="chapter-number">{{ chapter.number }}</div>

          <div class="chapter-content">
            <span class="chapter-label">{{ chapter.label }}</span>
            <h3>{{ chapter.title }}</h3>
            <p>{{ chapter.description }}</p>
            <div class="chapter-ornament">{{ chapter.symbol }}</div>
          </div>
        </article>
      </div>

      <div class="story-ending">
        <span>✧</span>
        <p>And the story is still being written...</p>
      </div>
    </section>

    <!-- MEMORY GALLERY -->
    <section class="memory-section" id="memories">
      <div class="section-kicker">CAPTURED IN TIME</div>

      <h2 class="section-title">
        Little Moments,
        <span>Lasting Memories.</span>
      </h2>

      <p class="memory-description">
        Some moments deserve a little place in our universe.
      </p>

      <div class="memory-grid">
        <div v-for="memory in memories" :key="memory.id" class="memory-card">
          <div class="memory-image">
            <img
              v-if="memory.image"
              :src="memory.image"
              :alt="memory.caption"
            />

            <div v-else class="memory-placeholder">
              <span>{{ memory.symbol }}</span>
              <small>YOUR MEMORY GOES HERE</small>
            </div>
          </div>
          <p>{{ memory.caption }}</p>
        </div>
      </div>
    </section>

    <!-- FLOWER MINI GAME -->
    <section class="garden-section" id="garden">
      <div class="garden-overlay"></div>

      <div class="garden-content">
        <div class="section-kicker">A LITTLE INTERACTIVE MOMENT</div>

        <h2>
          Garden of
          <span>Three Wishes</span>
        </h2>

        <p class="garden-description">
          In this little garden, three flowers are waiting. Choose each one and
          discover the wishes hidden inside.
        </p>

        <div class="flower-progress">
          <span
            v-for="n in 3"
            :key="n"
            :class="{ collected: collectedFlowers.length >= n }"
            >✦</span
          >
        </div>

        <div class="flower-grid">
          <button
            v-for="flower in flowers"
            :key="flower.id"
            class="flower-choice"
            :class="{ selected: collectedFlowers.includes(flower.id) }"
            :disabled="collectedFlowers.includes(flower.id)"
            @click="chooseFlower(flower)"
          >
            <span class="flower-art">{{ flower.symbol }}</span>
            <span class="flower-name">{{ flower.name }}</span>
            <small>
              {{
                collectedFlowers.includes(flower.id)
                  ? 'DISCOVERED ✓'
                  : 'TAP TO DISCOVER'
              }}
            </small>
          </button>
        </div>

        <transition name="wish-reveal">
          <div v-if="activeWish" class="wish-card">
            <span>{{ activeWish.symbol }}</span>
            <h3>{{ activeWish.name }}</h3>
            <p>{{ activeWish.message }}</p>
          </div>
        </transition>

        <p class="game-progress">
          {{ collectedFlowers.length }} / 3 flowers discovered
        </p>

        <transition name="wish-reveal">
          <button
            v-if="gameCompleted"
            class="secret-unlock-button"
            @click="goToSecret"
          >
            UNLOCK THE SECRET ROOM ✧
          </button>
        </transition>
      </div>
    </section>

    <!-- SECRET ROOM -->
    <section class="secret-section" id="secret">
      <div class="secret-content">
        <template v-if="!gameCompleted">
          <div class="secret-lock">♙</div>
          <div class="section-kicker">A PLACE BEYOND THE GARDEN</div>
          <h2>The Secret Room</h2>
          <p>
            Three little wishes are waiting to be discovered before this room
            can open.
          </p>
          <div class="locked-label">✦ STILL LOCKED ✦</div>
        </template>

        <template v-else>
          <div class="secret-unlocked">✧</div>
          <div class="section-kicker">YOU FOUND YOUR WAY HERE</div>

          <h2>
            Welcome to
            <span>The Secret Room.</span>
          </h2>

          <div class="secret-letter">
            <div class="secret-flower">🌷</div>
            <p class="secret-to">A LETTER FOR DINI</p>
            <h3>Dear Dini,</h3>

            <p>
              Dari sekian banyak hal yang bisa terjadi dalam kehidupan,
              terkadang ada pertemuan sederhana yang diam-diam menjadi sesuatu
              yang berarti.
            </p>

            <p>
              Aku tidak tahu akan sejauh apa sebuah cerita berjalan, tetapi aku
              ingin mengapresiasi setiap hal baik yang sudah hadir sejauh ini.
            </p>

            <p>
              Semoga dunia selalu punya cara untuk menghadirkan kebahagiaan,
              ketenangan, dan banyak alasan untuk tersenyum dalam setiap
              langkahmu.
            </p>

            <p> Terima kasih sudah menjadi bagian dari cerita kecil ini. </p>

            <div class="secret-signature">
              With appreciation,
              <span>Someone who appreciates you ♡</span>
            </div>
          </div>

          <p class="final-message">
            May your universe always bloom beautifully.
          </p>
        </template>
      </div>
    </section>

    <!-- FOOTER -->
    <footer class="footer-section">
      <div>✧ HARI PERAYAAN BUMI ✧</div>
      <p>Made with affection, just for Dini.</p>
      <span>♡</span>
    </footer>

    <!-- MUSIC CONTROL -->
    <button
      class="music-toggle"
      @click="toggleMusic"
      :aria-label="isPlaying ? 'Pause music' : 'Play music'"
    >
      {{ isPlaying ? 'Ⅱ' : '♫' }}
    </button>

    <audio
      ref="audio"
      src="/music/background.mp3"
      loop
      @play="isPlaying = true"
      @pause="isPlaying = false"
    ></audio>
  </q-page>
</template>

<script setup>
import { ref, computed, onBeforeUnmount } from 'vue'

const audio = ref(null)
const isPlaying = ref(false)

const collectedFlowers = ref([])
const activeWish = ref(null)

const gameCompleted = computed(
  () => collectedFlowers.value.length === flowers.length
)

const chapters = [
  {
    number: '01',
    label: 'CHAPTER ONE',
    title: 'A Random Beginning',
    description:
      'Sebuah pertemuan sederhana di antara kesibukan hari-hari biasa. Tidak ada yang tahu bahwa momen kecil itu akan menjadi awal dari sebuah cerita.',
    symbol: '✿'
  },
  {
    number: '02',
    label: 'CHAPTER TWO',
    title: 'Little Conversations',
    description:
      'Percakapan singkat yang awalnya sederhana, perlahan menjadi momen yang dinantikan. Ada kehangatan dalam hal-hal kecil.',
    symbol: '♡'
  },
  {
    number: '03',
    label: 'CHAPTER THREE',
    title: 'Between Ashar and Maghrib',
    description:
      'Dari waktu-waktu singkat setelah Ashar hingga menjelang Maghrib, sebuah cerita mulai menemukan jalannya.',
    symbol: '☾'
  },
  {
    number: '04',
    label: 'CHAPTER FOUR',
    title: 'A Story Beyond the Moment',
    description:
      'Dari obrolan sederhana hingga percakapan yang berlanjut melalui pesan. Tidak perlu terburu-buru, setiap cerita punya waktunya sendiri.',
    symbol: '✧'
  }
]

const memories = ref([
  {
    id: 1,
    image: '/images/dini-01.jpg',
    symbol: '🌷',
    caption: 'A moment worth remembering'
  },
  {
    id: 2,
    image: '/images/dini-02.jpg',
    symbol: '🌸',
    caption: 'Happiness in a little moment'
  }
])

const flowers = [
  {
    id: 1,
    name: 'The Tulip',
    symbol: '🌷',
    message: 'May you always find reasons to smile, even on ordinary days.'
  },
  {
    id: 2,
    name: 'The Sakura',
    symbol: '🌸',
    message:
      'May every new chapter bring beautiful opportunities and wonderful beginnings.'
  },
  {
    id: 3,
    name: 'The Rose',
    symbol: '🌹',
    message:
      'May you always remember how valuable, special, and worthy of happiness you are.'
  }
]

async function enterWorld() {
  document.getElementById('story')?.scrollIntoView({
    behavior: 'smooth'
  })

  if (audio.value && !isPlaying.value) {
    try {
      await audio.value.play()
    } catch (error) {
      console.log('Musik belum bisa diputar:', error)
    }
  }
}
function chooseFlower(flower) {
  if (collectedFlowers.value.includes(flower.id)) return

  collectedFlowers.value.push(flower.id)
  activeWish.value = flower
}

function goToSecret() {
  document.getElementById('secret')?.scrollIntoView({
    behavior: 'smooth'
  })
}

async function toggleMusic() {
  if (!audio.value) return

  if (isPlaying.value) {
    audio.value.pause()
  } else {
    try {
      await audio.value.play()
    } catch (error) {
      console.error('Music could not be played:', error)
    }
  }
}

onBeforeUnmount(() => {
  audio.value?.pause()
})
</script>

<style scoped>
@import url('https://fonts.googleapis.com/css2?family=Playfair+Display:ital,wght@0,400;0,500;0,600;1,400;1,500&family=Poppins:wght@300;400;500;600&display=swap');

/* GLOBAL */
.fairytale-page {
  --pink: #e7a9c0;
  --rose: #bb7997;
  --cream: #fff4ef;
  --dark: #211923;

  font-family: 'Poppins', sans-serif;
  color: var(--cream);
  background: var(--dark);
  position: relative;
  isolation: isolate;
  background: transparent;
  overflow-x: hidden;
  width: 100%;
}

.fairytale-page *,
.fairytale-page *::before,
.fairytale-page *::after {
  box-sizing: border-box;
}
/* FULL WEBSITE VIDEO BACKGROUND */
.site-background-video {
  position: fixed;
  inset: 0;
  width: 100%;
  height: 100%;
  object-fit: cover;
  object-position: center;
  z-index: -2;
  pointer-events: none;
}

.site-background-overlay {
  position: fixed;
  inset: 0;
  background: rgba(25, 15, 29, 0.3);
  z-index: -1;
  pointer-events: none;
}

/* KEEP WEBSITE CONTENT ABOVE VIDEO */
.fairytale-page > section,
.fairytale-page > nav,
.fairytale-page > footer {
  position: relative;
  z-index: 2;
}

.fairytale-page > .floating-nav {
  position: absolute;
  z-index: 10;
}

.fairytale-page > .particle-layer {
  position: fixed;
  z-index: 5;
}

html {
  scroll-behavior: smooth;
}

/* FLOATING PARTICLES */
.particle-layer {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 5;
  overflow: hidden;
}

.particle {
  position: absolute;
  top: -30px;
  left: calc(var(--i) * 4.17%);
  color: #f6cbdc;
  opacity: 0.45;
  font-size: 12px;
  animation: particleFall var(--duration) linear infinite;
  animation-delay: var(--delay);
}

@keyframes particleFall {
  to {
    transform: translate3d(35px, 110vh, 0) rotate(420deg);
  }
}

/* NAVIGATION */
.floating-nav {
  position: absolute;
  z-index: 10;
  top: 0;
  left: 0;
  width: 100%;
  padding: 27px 7%;
  display: flex;
  align-items: center;
  justify-content: space-between;
}

.nav-brand {
  font-family: 'Playfair Display', serif;
  font-size: 20px;
  letter-spacing: 3px;
  color: #fff4ef;
  text-decoration: none;
}

.nav-brand span {
  color: var(--pink);
}

.nav-links {
  display: flex;
  gap: 28px;
}

.nav-links a {
  color: #f5dce7;
  font-size: 9px;
  letter-spacing: 2px;
  text-decoration: none;
  transition: 0.3s;
}

.nav-links a:hover {
  color: var(--pink);
}

/* HERO */
.hero-section {
  min-height: 760px;
  height: 100svh;
  position: relative;
  display: grid;
  place-items: center;
  background:
    radial-gradient(
      ellipse at 50% 40%,
      rgba(116, 80, 101, 0.35) 0%,
      transparent 40%
    ),
    radial-gradient(
      ellipse at 15% 80%,
      rgba(117, 67, 95, 0.3) 0%,
      transparent 35%
    ),
    linear-gradient(
      160deg,
      rgba(33, 25, 35, 0.28) 0%,
      rgba(57, 37, 56, 0.32) 55%,
      rgba(28, 23, 35, 0.3) 100%
    );
}

.hero-section::before {
  content: '';
  position: absolute;
  inset: 0;
  opacity: 0.18;
  background-image: radial-gradient(#ffffff 1px, transparent 1px);
  background-size: 48px 48px;
}

.moon-glow {
  position: absolute;
  width: 340px;
  height: 340px;
  top: 13%;
  left: 50%;
  transform: translateX(-50%);
  background: #e5b0c8;
  filter: blur(120px);
  opacity: 0.19;
  border-radius: 50%;
}

.hero-content {
  position: relative;
  z-index: 2;
  text-align: center;
  padding: 80px 20px;
  max-width: 750px;
  animation: heroAppear 1.5s ease both;
}

@keyframes heroAppear {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

.hero-eyebrow,
.section-kicker {
  color: #e5b2c8;
  font-size: 9px;
  letter-spacing: 5px;
  text-align: center;
}

.hero-symbol {
  color: #e8b6ca;
  font-size: 45px;
  margin: 28px 0 15px;
}

.hero-content h1 {
  font-family: 'Playfair Display', serif;
  font-size: clamp(44px, 9vw, 92px);
  font-weight: 400;
  line-height: 1.15;
  letter-spacing: 5px;
  margin: 0;
}

.hero-content h1 span {
  display: block;
  color: #e9b1c9;
  font-style: italic;
  letter-spacing: 12px;
}

.hero-divider {
  display: flex;
  justify-content: center;
  align-items: center;
  gap: 18px;
  color: #e9b1c9;
  margin: 28px 0;
}

.hero-divider span {
  width: 55px;
  height: 1px;
  background: #b9869d;
}

.hero-description {
  max-width: 470px;
  margin: auto;
  font-family: 'Playfair Display', serif;
  font-size: 18px;
  line-height: 1.9;
  color: #e3ced8;
}

.hero-for {
  margin-top: 20px;
  color: #d7a6bb;
  font-size: 11px;
  letter-spacing: 2px;
}

.enter-button {
  margin-top: 35px;
  padding: 16px 32px;
  border: 1px solid #d9a1b9;
  background: #d9a1b9;
  color: #2b1d29;
  font-size: 9px;
  font-weight: 600;
  letter-spacing: 3px;
  cursor: pointer;
  transition: 0.4s;
}

.enter-button span {
  margin-left: 20px;
}

.enter-button:hover {
  background: transparent;
  color: white;
  box-shadow: 0 0 35px #d9a1b955;
}

.hero-flower {
  position: absolute;
  font-size: 90px;
  color: #e3a7c0;
  opacity: 0.25;
  animation: flowerDrift 6s ease-in-out infinite;
}

.flower-left {
  left: 7%;
  bottom: 13%;
}

.flower-right {
  right: 7%;
  top: 18%;
  animation-delay: 2s;
}

@keyframes flowerDrift {
  50% {
    transform: translateY(-20px) rotate(15deg);
  }
}

.scroll-hint {
  margin-top: 85px;
  font-size: 8px;
  letter-spacing: 4px;
  color: #d4b2c3;
}

.scroll-hint span {
  display: block;
  margin-top: 10px;
  font-size: 20px;
  animation: arrowDown 1.5s infinite;
}

@keyframes arrowDown {
  50% {
    transform: translateY(8px);
  }
}

/* INTRO */
.intro-section {
  padding: 140px 22px;
  text-align: center;
  background:
    radial-gradient(
      ellipse at 50% 50%,
      rgba(245, 216, 227, 0.82),
      transparent 70%
    ),
    rgba(255, 245, 241, 0.72);
  color: #604656;
}

.intro-decoration {
  font-size: 50px;
  color: #cf92ab;
  margin: 35px 0 15px;
}

.intro-section h2,
.section-title,
.garden-content h2,
.secret-content h2 {
  font-family: 'Playfair Display', serif;
  font-weight: 400;
  font-size: clamp(36px, 6vw, 55px);
  line-height: 1.35;
}

.intro-section h2 span,
.section-title span,
.garden-content h2 span,
.secret-content h2 span {
  display: block;
  font-style: italic;
  color: #c17c9a;
}

.intro-text {
  max-width: 540px;
  margin: 25px auto;
  font-family: 'Playfair Display', serif;
  font-size: 17px;
  line-height: 2;
  color: #816577;
}

.intro-signature {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  color: #c17c9a;
  margin-top: 35px;
}

/* STORY */
.story-section {
  padding: 120px 8%;
  background: linear-gradient(
    180deg,
    rgba(38, 28, 42, 0.76),
    rgba(52, 37, 56, 0.76)
  );
}

.section-title {
  text-align: center;
  color: #fff3f7;
  margin: 25px 0 80px;
}

.timeline {
  max-width: 900px;
  margin: auto;
  position: relative;
}

.timeline::before {
  content: '';
  position: absolute;
  width: 1px;
  top: 0;
  bottom: 0;
  left: 50%;
  background: linear-gradient(transparent, #d6a0b9, transparent);
}

.chapter-card {
  width: 50%;
  position: relative;
  padding: 0 55px 85px 0;
}

.chapter-reverse {
  margin-left: 50%;
  padding: 0 0 85px 55px;
}

.chapter-number {
  position: absolute;
  right: -22px;
  top: 0;
  width: 44px;
  height: 44px;
  display: grid;
  place-items: center;
  border: 1px solid #dba5bb;
  border-radius: 50%;
  color: #f0c4d6;
  background: #342538;
  font-family: 'Playfair Display', serif;
}

.chapter-reverse .chapter-number {
  left: -22px;
  right: auto;
}

.chapter-content {
  padding: 32px;
  border: 1px solid #78556e;
  background: #ffffff08;
  transition: 0.4s;
}

.chapter-content:hover {
  background: #ffffff13;
  border-color: #e2abc3;
  transform: translateY(-5px);
}

.chapter-label {
  font-size: 9px;
  color: #dda8c0;
  letter-spacing: 3px;
}

.chapter-content h3 {
  font-family: 'Playfair Display', serif;
  font-size: 28px;
  font-weight: 400;
  color: #fff0f5;
  margin: 20px 0;
}

.chapter-content p {
  font-size: 12px;
  line-height: 2.1;
  color: #cfb9c8;
}

.chapter-ornament {
  color: #e5a8c1;
  font-size: 25px;
  margin-top: 22px;
}

.story-ending {
  text-align: center;
  color: #e4b1c7;
  font-family: 'Playfair Display', serif;
  font-style: italic;
  font-size: 18px;
}

/* MEMORY */
.memory-section {
  padding: 120px 8%;
  background: rgba(255, 245, 241, 0.78);
  color: #604656;
  text-align: center;
}

.memory-description {
  color: #8c6d7c;
  font-family: 'Playfair Display', serif;
  font-style: italic;
  margin: -40px auto 55px;
}

.memory-grid {
  max-width: 1000px;
  margin: auto;
  display: grid;
  grid-template-columns: repeat(4, 1fr);
  gap: 20px;
}

.memory-card {
  padding: 10px 10px 25px;
  background: white;
  box-shadow: 0 12px 30px #98758916;
  transform: rotate(-2deg);
  transition: 0.4s;
}

.memory-card:nth-child(even) {
  transform: rotate(2deg);
}

.memory-card:hover {
  transform: translateY(-10px) rotate(0);
}

.memory-image {
  aspect-ratio: 3 / 4;
  background: #ecd4df;
  overflow: hidden;
}

.memory-image img {
  width: 100%;
  height: 100%;
  object-fit: cover;
}

.memory-placeholder {
  height: 100%;
  display: flex;
  align-items: center;
  justify-content: center;
  flex-direction: column;
  gap: 12px;
  background: linear-gradient(140deg, #f7dce7, #e8c4d4);
  color: #a66c88;
}

.memory-placeholder span {
  font-size: 45px;
}

.memory-placeholder small {
  font-size: 7px;
  letter-spacing: 2px;
}

.memory-card p {
  color: #906c7e;
  font-family: 'Playfair Display', serif;
  font-style: italic;
  font-size: 12px;
  margin: 17px 0 0;
}

/* GARDEN */
.garden-section {
  position: relative;
  padding: 130px 20px;
  background:
    radial-gradient(
      ellipse at 50% 60%,
      rgba(117, 75, 108, 0.55),
      transparent 55%
    ),
    linear-gradient(
      140deg,
      rgba(33, 26, 42, 0.78),
      rgba(73, 48, 70, 0.74),
      rgba(33, 26, 42, 0.78)
    );
  text-align: center;
}

.garden-content {
  position: relative;
  z-index: 2;
  max-width: 900px;
  margin: auto;
}

.garden-content h2 {
  color: #fff1f6;
  margin: 25px 0;
}

.garden-description {
  max-width: 490px;
  margin: auto;
  color: #dfc5d3;
  font-family: 'Playfair Display', serif;
  font-size: 17px;
  line-height: 1.9;
}

.flower-progress {
  display: flex;
  justify-content: center;
  gap: 30px;
  margin: 45px 0;
  color: #795a72;
  font-size: 25px;
}

.flower-progress span.collected {
  color: #f0bbd1;
  text-shadow: 0 0 18px #efb4d4;
}

.flower-grid {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  gap: 20px;
}

.flower-choice {
  min-height: 190px;
  border: 1px solid #a57a96;
  background: #ffffff09;
  color: white;
  cursor: pointer;
  display: flex;
  flex-direction: column;
  justify-content: center;
  align-items: center;
  gap: 13px;
  transition: 0.4s;
}

.flower-choice:hover:not(:disabled) {
  background: #ffffff16;
  transform: translateY(-10px);
  border-color: #f0bbd1;
  box-shadow: 0 10px 30px #ecb3d522;
}

.flower-choice.selected {
  background: #f0bbd118;
  border-color: #efb5cf;
  opacity: 0.75;
}

.flower-choice:disabled {
  cursor: default;
}

.flower-art {
  font-size: 48px;
  transition: 0.4s;
}

.flower-choice:hover .flower-art {
  transform: scale(1.2) rotate(10deg);
}

.flower-name {
  font-family: 'Playfair Display', serif;
  font-size: 20px;
}

.flower-choice small {
  color: #dcb5c9;
  font-size: 8px;
  letter-spacing: 2px;
}

.wish-card {
  max-width: 500px;
  margin: 40px auto 0;
  padding: 30px;
  background: #fff5f1;
  color: #74556a;
  border: 1px solid #e4b6ca;
}

.wish-card > span {
  font-size: 36px;
}

.wish-card h3 {
  font-family: 'Playfair Display', serif;
  font-weight: 400;
  font-size: 25px;
}

.wish-card p {
  font-family: 'Playfair Display', serif;
  line-height: 1.8;
}

.game-progress {
  color: #e2c4d3;
  font-size: 10px;
  letter-spacing: 2px;
  margin-top: 35px;
}

.secret-unlock-button {
  margin-top: 20px;
  padding: 17px 28px;
  background: #e4b0c5;
  color: #30202d;
  border: none;
  letter-spacing: 2px;
  font-size: 10px;
  cursor: pointer;
  transition: 0.3s;
}

.secret-unlock-button:hover {
  box-shadow: 0 0 35px #e5b4cb77;
  transform: translateY(-3px);
}

/* SECRET ROOM */
.secret-section {
  padding: 130px 20px;
  background:
    radial-gradient(
      ellipse at center,
      rgba(121, 84, 111, 0.58),
      transparent 60%
    ),
    rgba(33, 25, 35, 0.78);
  text-align: center;
  min-height: 650px;
}

.secret-content {
  max-width: 680px;
  margin: auto;
}

.secret-lock,
.secret-unlocked {
  font-size: 60px;
  color: #e8b6ca;
  margin-bottom: 30px;
}

.secret-content h2 {
  color: #fff1f6;
  margin: 25px 0;
}

.secret-content > p {
  color: #dec4d2;
  font-family: 'Playfair Display', serif;
  line-height: 1.8;
}

.locked-label {
  margin-top: 40px;
  font-size: 10px;
  letter-spacing: 4px;
  color: #c993ad;
}

.secret-letter {
  margin: 55px auto 35px;
  padding: 50px clamp(25px, 7vw, 60px);
  background: #fff8f1;
  color: #745668;
  text-align: left;
  box-shadow: 0 15px 60px #00000035;
}

.secret-flower {
  text-align: center;
  font-size: 40px;
}

.secret-to {
  color: #bd7f9c !important;
  text-align: center;
  font-size: 9px;
  letter-spacing: 4px;
}

.secret-letter h3 {
  font-family: 'Playfair Display', serif;
  font-style: italic;
  font-size: 28px;
  font-weight: 400;
}

.secret-letter p {
  font-family: 'Playfair Display', serif;
  font-size: 16px;
  line-height: 2;
  margin-bottom: 20px;
}

.secret-signature {
  margin-top: 45px;
  font-family: 'Playfair Display', serif;
  font-style: italic;
}

.secret-signature span {
  display: block;
  margin-top: 12px;
  color: #bd7f9c;
}

.final-message {
  font-style: italic;
  font-size: 18px;
}

/* FOOTER */
.footer-section {
  padding: 45px 20px;
  background: rgba(24, 19, 29, 0.88);
  text-align: center;
  color: #dba9c0;
  font-family: 'Playfair Display', serif;
  letter-spacing: 3px;
  font-size: 11px;
}

.footer-section p {
  color: #9d8092;
  font-family: 'Poppins', sans-serif;
  font-size: 9px;
  letter-spacing: 1px;
}

.footer-section span {
  font-size: 23px;
}

/* MUSIC */
.music-toggle {
  position: fixed;
  bottom: 25px;
  right: 25px;
  z-index: 20;
  width: 50px;
  height: 50px;
  border-radius: 50%;
  border: 1px solid #e6b1c8;
  color: white;
  background: #6b455e;
  cursor: pointer;
  font-size: 20px;
  box-shadow: 0 5px 25px #00000030;
}

/* TRANSITIONS */
.wish-reveal-enter-active {
  transition: all 0.6s ease;
}

.wish-reveal-enter-from {
  opacity: 0;
  transform: translateY(20px);
}

/* RESPONSIVE */
@media (max-width: 700px) {
  .floating-nav {
    padding: 22px 20px;
  }

  .nav-links {
    gap: 13px;
  }

  .nav-links a {
    font-size: 7px;
    letter-spacing: 1px;
  }

  .hero-section {
    min-height: 720px;
  }

  .hero-content h1 {
    letter-spacing: 2px;
  }

  .hero-content h1 span {
    letter-spacing: 7px;
  }

  .hero-description {
    font-size: 15px;
  }

  .hero-flower {
    font-size: 55px;
  }

  .story-section,
  .memory-section {
    padding: 90px 22px;
  }

  .timeline::before {
    left: 15px;
  }

  .chapter-card,
  .chapter-reverse {
    width: 100%;
    margin-left: 0;
    padding: 0 0 45px 45px;
  }

  .chapter-number,
  .chapter-reverse .chapter-number {
    left: -7px;
    right: auto;
    width: 44px;
    height: 44px;
  }

  .chapter-content {
    padding: 24px;
  }

  .memory-grid {
    grid-template-columns: repeat(2, 1fr);
    gap: 17px;
  }

  .flower-grid {
    grid-template-columns: 1fr;
    max-width: 320px;
    margin: auto;
  }

  .flower-choice {
    min-height: 150px;
  }

  .garden-section,
  .secret-section {
    padding: 95px 22px;
  }

  .secret-letter {
    padding: 35px 24px;
  }
}

@media (max-width: 400px) {
  .nav-brand {
    font-size: 15px;
  }

  .nav-links {
    gap: 8px;
  }

  .nav-links a {
    font-size: 6px;
    letter-spacing: 0.5px;
  }

  .hero-content h1 {
    font-size: 39px;
  }

  .memory-grid {
    gap: 10px;
  }
}

/* ACCESSIBILITY */
@media (prefers-reduced-motion: reduce) {
  *,
  *::before,
  *::after {
    animation-duration: 0.01ms !important;
    transition-duration: 0.01ms !important;
    scroll-behavior: auto !important;
  }
}
</style>
