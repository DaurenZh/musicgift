<script setup>
import { ref, computed, nextTick, watch, onBeforeUnmount } from 'vue'

const currentMemory = ref(null)
const audioPlayer = ref(null)
const currentTime = ref(0)
const duration = ref(0)
const isPlaying = ref(false)
const lyricsContainer = ref(null)

const memories = ref([
  {
    id: 1,
    caption: 'Sulugym menin',
    photo: '/photos/us_mountains.jpeg',
    message: 'Если бы все вернулось на момент нашего знакомства, я бы все равно подошел к тебе и сказал: "Токтаныз, сұлу қыз, танысқым келеді менің сізбен."',
    songTitle: 'Сулу кыз',
    artist: 'Дос-Мукасан',
    audio: '/music/Сұлу-қыз.mp3',
    lyrics: [
      { time: 9, text: 'Таңертеңмен бір сұлу қыз' },
      { time: 13, text: 'Суға келеді' },
      { time: 17, text: 'Суға келеді' },
      { time: 21, text: 'Суға келеді' },
      { time: 25.5, text: 'Ат жетектеп бір жас жігіт' },
      { time: 30, text: 'Еріп келеді' },
      { time: 34, text: 'Еріп келеді' },
      { time: 38, text: 'Еріп келеді' },
      { time: 42, text: 'Тоқтаңыз сұлу қыз су іздеген' },
      { time: 46, text: 'Танысқым келеді менің сізбен' },
      { time: 49.5, text: 'Айтыңыз атыңыз кім болады' },
      { time: 53.5, text: 'Танысып көрмеген бұрын қызбен' },
      { time: 66.5, text: 'Менің атым неге керек' },
      { time: 70.5, text: 'Айтшы жас жігіт' },
      { time: 75, text: 'Айтшы жас жігіт' },
      { time: 79, text: 'Айтшы жас жігіт' },
      { time: 83, text: 'Бір көргеннен қалдыңыз ба' },
      { time: 87, text: 'Мені шын сүйіп' },
      { time: 91, text: 'Мені шын сүйіп' },
      { time: 95, text: 'Мені шын сүйіп' },
      { time: 98.5, text: 'Тоқтаңыз сұлу қыз су іздеген' },
      { time: 102.5, text: 'Танысқым келеді менің сізбен' },
      { time: 106, text: 'Айтыңыз атыңыз кім болады' },
      { time: 110, text: 'Танысып көрмеген бұрын қызбен' },
      { time: 154.5, text: 'Тоқтаңыз сұлу қыз су іздеген' },
      { time: 158, text: 'Танысқым келеді менің сізбен' },
      { time: 162, text: 'Айтыңыз атыңыз кім болады' },
      { time: 166, text: 'Танысып көрмеген бұрын қызбен' },
    ],
  },
  {
    id: 2,
    caption: 'Partners in crime',
    photo: '/photos/gta.png',
    message: 'Ты у меня такая роскошная и опасная, реально "Fashion Killa"... будто Рианна, я твой прити мазафакер😎.',
    songTitle: 'Fashion Killa',
    artist: 'A$AP Rocky',
    audio: '/music/A_AP-Rocky-Fashion-Killa-_Official-Audio_.mp3',
    lyrics: [
      { time: 14.4, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 18.09, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 21.15, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 24.38, text: "'Cause she a fashion killa, and I'm a trendy nigga" },
      { time: 27.89, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 31.55, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 34.74, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 38.15, text: "'Cause she a fashion killa, and I'm a jiggy nigga, uh" },
      { time: 41.64, text: "I said, rockin', rollin', swaggin' to the max" },
      { time: 45.24, text: 'My bitch a fashion killa, she be busy poppin’ tags' },
      { time: 48.78, text: 'She got a lotta Prada, that Dolce and Gabbana' },
      { time: 52.14, text: "I can't forget Escada, and that Balenciaga" },
      { time: 55.61, text: "I'm sippin' purple syrup, come be my Aunt Jemima" },
      { time: 59.07, text: 'And if you is a rider, we’ll go shopping like mañana' },
      { time: 62.52, text: 'Her attitude Rihanna, she get it from her mama' },
      { time: 65.92, text: 'She jiggy like Madonna, but she trippy like Nirvana' },
      { time: 69.34, text: "'Cause everything designer, her jeans is Helmut Lang" },
      { time: 72.56, text: 'Shoes is Alexander Wang and her shirt the newest Donna' },
      { time: 76.57, text: 'Karan, wearing all the Cartier frames' },
      { time: 79.4, text: "Jean Paul Gaultiers 'cause they match with her persona" },
      { time: 83.08, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 86.52, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 89.62, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 93.08, text: "'Cause she a fashion killa, and I'm a trendy nigga" },
      { time: 96.38, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 100.1, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 103.4, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 106.76, text: "'Cause she a fashion killa, and I'm a jiggy nigga" },
      { time: 110.13, text: 'I said, I see your Jil Sanders, Oliver Peoples' },
      { time: 113.96, text: 'Costume National, your Ann Demeuelemeester' },
      { time: 117.32, text: 'See Visvim be the sneaker, Lanvin or Balmain' },
      { time: 120.85, text: 'Goyard by the trunk, her Isabel Marant' },
      { time: 124.15, text: 'I love your Linda Farrow, I adore your Dior' },
      { time: 127.68, text: 'Your Damir Doma, Vena Cava from the store' },
      { time: 130.97, text: 'I crush down with that top down, bossy see how I ride ’round' },
      { time: 134.66, text: 'Mami in that Tom Ford, Papi in that Thom Browne' },
      { time: 138.07, text: 'Rick Owens, Raf Simons, boy she got it by the stock' },
      { time: 141.17, text: "She ball until she fall, that means she shop 'til she drop" },
      { time: 144.62, text: 'And Versace got a lot, but she may never wear it' },
      { time: 148.01, text: 'But she save it so our babies will be flyer than their parents' },
      { time: 151.65, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 155.14, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 158.28, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 161.55, text: "'Cause she a fashion killa, and I'm a trendy nigga" },
      { time: 165.09, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 168.72, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 171.98, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 175.23, text: "'Cause she a fashion killa, and I'm a jiggy nigga" },
      { time: 178.68, text: 'I said her pistol go' },
      { time: 179.25, text: 'Scoop back tees, breeze in the coupé' },
      { time: 182.53, text: 'Smilin’ is your treasure, you’re so well put together' },
      { time: 185.97, text: 'Bags and links, jeans and shoes' },
      { time: 189.37, text: 'Spikes and patent leathers, different fabrics mixed together' },
      { time: 192.85, text: 'You and me, me and you' },
      { time: 196.26, text: 'Go away together, we could get away forever' },
      { time: 199.76, text: 'All emotions clashin’, thrashin’, someone turn the light out' },
      { time: 203.15, text: 'I met my baby expressed my passion on my fashion night out' },
      { time: 206.66, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 209.92, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 213.19, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 216.53, text: "'Cause she a fashion killa, and I'm a trendy nigga" },
      { time: 219.95, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 223.64, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 226.88, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 230.13, text: "'Cause she a fashion killa, and I'm a jiggy nigga" },
      { time: 233.52, text: 'I said her pistol go (I said her pistol go)' },
    ],
  },
  {
    id: 3,
    caption: 'Ты моя звезда...',
    photo: '/photos/us_rest.jpeg',
    message: 'Ты для меня воистину сияешь ярче солнца, ты моя звезда...',
    songTitle: 'Я твой номер один',
    artist: 'Дима Билан',
    audio: '/music/dima-bilan-number-one.mp3',
    lyrics: [
      { time: 18, text: 'Взгляд твой как власть, та, что разжигает страсть' },
      { time: 22.4, text: 'Чья ты bad girl? Кто тебя сюда привёл?' },
      { time: 26.5, text: 'А-а, автограф свой ногтями на спине' },
      { time: 31.5, text: 'Этой ночью можешь ты оставить мне' },
      { time: 37, text: 'Детка, ты за мной иди' },
      { time: 41, text: 'Я тот, кого так хочешь ты' },
      { time: 45, text: 'Я вижу блеск в твоих глазах' },
      { time: 48.5, text: 'Ты моя звезда' },
      { time: 51, text: 'А я твой номер один, baby, я твой номер один' },
      { time: 58.5, text: 'Знаешь? А я твой номер один, baby, я твой номер один' },
      { time: 66.3, text: 'А я твой номер один' },
      { time: 71, text: 'От тебя схожу с ума' },
      { time: 74, text: 'Только лишь с тобой, только твой Дима Билан' },
      { time: 76.7, text: 'Так владеешь телом, детка, ближе к делу (а-а-а)' },
      { time: 81.5, text: 'Люби меня, этой ночью ты моя' },
      { time: 87, text: 'Детка, ты за мной иди (ты за мной иди)' },
      { time: 91.3, text: 'Я тот, кого так хочешь ты (кого так хочешь ты)' },
      { time: 95.7, text: 'Я вижу блеск в твоих глазах' },
      { time: 98.7, text: 'Ты моя звезда' },
      { time: 101, text: 'А я твой номер один, baby, я твой номер один' },
      { time: 109, text: 'Знаешь? А я твой номер один, baby, я твой номер один' },
      { time: 116, text: 'А я твой номер один' },
      { time: 119.5, text: 'Только твой Дима Билан, только твой Дима Билан' },
      { time: 123, text: 'Только твой Дима Билан, только твой Дима Билан' },
      { time: 127, text: 'Только твой Дима Билан, только твой Дима Билан' },
      { time: 131, text: 'Только твой Дима Билан, только твой Дима Билан' },
      { time: 138, text: 'Детка, ты за мной иди' },
      { time: 142, text: 'Я тот, кого так хочешь ты' },
      { time: 146, text: 'Я вижу блеск в твоих глазах' },
      { time: 149, text: 'Ты моя звезда' },
      { time: 151, text: 'А я твой номер один, baby, я твой номер один' },
      { time: 159, text: 'Знаешь? А я твой номер один, baby, я твой номер один' },
      { time: 168, text: 'А я твой номер один, baby, я твой номер один' },
      { time: 175, text: 'Знаешь? А я твой номер один, baby, я твой номер один' },
    ],
  },
])

function getLyricTime(lyric) {
  return lyric.time - (currentMemory.value?.id === 2 ? 0.5 : 0)
}

const activeLyricIndex = computed(() => {
  if (!currentMemory.value) return 0
  let index = -1

  currentMemory.value.lyrics.forEach((lyric, lyricIndex) => {
    if (currentTime.value >= getLyricTime(lyric)) index = lyricIndex
  })
  return index
})

function centerActiveLyric() {
  nextTick(() => {
    const container = lyricsContainer.value
    const activeLine = container?.querySelector('.lyric-line.active')
    if (!container || !activeLine) return

    const containerRect = container.getBoundingClientRect()
    const lineRect = activeLine.getBoundingClientRect()
    const targetScrollTop =
      container.scrollTop +
      lineRect.top -
      containerRect.top -
      (container.clientHeight - lineRect.height) / 2

    container.scrollTo({
      top: targetScrollTop,
      behavior: window.matchMedia('(prefers-reduced-motion: reduce)').matches ? 'auto' : 'smooth',
    })
  })
}

watch([activeLyricIndex, currentMemory], centerActiveLyric)

function openMemory(memory) {
  audioPlayer.value?.pause()
  currentMemory.value = memory
  currentTime.value = 0
  duration.value = 0
  isPlaying.value = false

  nextTick(async () => {
    if (!audioPlayer.value) return
    try {
      await audioPlayer.value.play()
      isPlaying.value = true
    } catch (error) {
      console.log('Playback could not start:', error)
    }
  })
}

function goBack() {
  audioPlayer.value?.pause()
  isPlaying.value = false
  currentMemory.value = null
  currentTime.value = 0
  duration.value = 0
}

async function togglePlayback() {
  if (!audioPlayer.value) return
  try {
    if (audioPlayer.value.paused) {
      if (audioPlayer.value.ended) {
        audioPlayer.value.currentTime = 0
        currentTime.value = 0
      }
      await audioPlayer.value.play()
      isPlaying.value = true
    } else {
      audioPlayer.value.pause()
      isPlaying.value = false
    }
  } catch (error) {
    console.log('Playback error:', error)
  }
}

function updateTime() {
  if (audioPlayer.value) currentTime.value = audioPlayer.value.currentTime
}

function updateDuration() {
  if (audioPlayer.value) duration.value = audioPlayer.value.duration
}

function formatTime(seconds) {
  if (!Number.isFinite(seconds)) return '0:00'
  return `${Math.floor(seconds / 60)}:${String(Math.floor(seconds % 60)).padStart(2, '0')}`
}

function seekTo(event) {
  if (!audioPlayer.value) return
  const value = Number(event.target.value)
  audioPlayer.value.currentTime = value
  currentTime.value = value
}

function seekToLyric(time) {
  if (!audioPlayer.value) return
  const adjustedTime = time - (currentMemory.value?.id === 2 ? 0.5 : 0)
  audioPlayer.value.currentTime = adjustedTime
  currentTime.value = adjustedTime
}

function onSongEnded() {
  isPlaying.value = false
  currentTime.value = duration.value
}

function hideBrokenImage(event) {
  event.target.hidden = true
}

onBeforeUnmount(() => audioPlayer.value?.pause())
</script>

<template>
  <main class="app">
    <div class="app-shell">
      <section v-if="!currentMemory" class="gallery-screen">
        <h1 class="gallery-title">Небольшой подарок <em>тебе.</em></h1>
        <p class="gallery-subtitle">Для моей любимой жаныым, надеюсь тебе понравится. ♡</p>

        <div class="photo-grid">
          <button
            v-for="memory in memories"
            :key="memory.id"
            class="photo-card"
            type="button"
            @click="openMemory(memory)"
            :aria-label="`Open memory: ${memory.caption}`"
          >
            <div class="photo-frame">
              <img
                :src="memory.photo"
                :alt="memory.caption"
                class="gallery-photo"
                @error="hideBrokenImage"
              />
            <div class="photo-overlay" aria-hidden="true"></div>
            </div>
            <span class="photo-caption">{{ memory.caption }}</span>
          </button>
        </div>
        <p class="gallery-footer">made with love, just for you, my zhanym ♡</p>
      </section>

      <section
        v-else
        class="memory-detail"
        :style="{ '--memory-background': `url('${currentMemory.photo}')` }"
      >
        <div class="memory-detail-content">
          <button class="back-button" type="button" @click="goBack">
            <span aria-hidden="true">←</span>
            <span>Обратнооо</span>
          </button>

          <header class="memory-header">
            <p class="memory-message">{{ currentMemory.message }}</p>
            <span class="tiny-heart">♡</span>
          </header>

          <div class="memory-layout">
            <div class="polaroid-column">
              <div class="polaroid">
                <img
                  :src="currentMemory.photo"
                  :alt="currentMemory.caption"
                  class="polaroid-photo"
                  @error="hideBrokenImage"
                />
                <p class="polaroid-caption">{{ currentMemory.caption }}</p>
              </div>
              <p class="photo-note">Это мыыы ♡</p>
            </div>

            <div class="song-column">
              <div class="song-heading">
                <p class="song-label">THIS ONE IS FOR YOU 😘</p>
                <h2 class="song-title">{{ currentMemory.songTitle }}</h2>
                <p class="song-artist">{{ currentMemory.artist }}</p>
              </div>

              <div class="music-controls">
                <audio
                  ref="audioPlayer"
                  :src="currentMemory.audio"
                  preload="metadata"
                  @loadedmetadata="updateDuration"
                  @timeupdate="updateTime"
                  @ended="onSongEnded"
                  @play="isPlaying = true"
                  @pause="isPlaying = false"
                ></audio>
                <div class="progress-row">
                  <span>{{ formatTime(currentTime) }}</span>
                  <input
                    class="progress-slider"
                    type="range"
                    min="0"
                    :max="duration || 100"
                    :value="currentTime"
                    @input="seekTo"
                    aria-label="Song progress"
                  />
                  <span>{{ formatTime(duration) }}</span>
                </div>
                <button
                  class="play-button"
                  type="button"
                  @click="togglePlayback"
                  :aria-label="isPlaying ? 'Pause song' : 'Play song'"
                >
                  <span v-if="isPlaying" class="pause-icon" aria-hidden="true">Ⅱ</span>
                  <span v-else class="play-icon" aria-hidden="true"></span>
                </button>
              </div>

              <div class="lyrics-section">
                <div ref="lyricsContainer" class="lyrics-container">
                  <button
                    v-for="(lyric, index) in currentMemory.lyrics"
                    :key="index"
                    class="lyric-line"
                    type="button"
                    @click="seekToLyric(lyric.time)"
                    :class="{ active: index === activeLyricIndex, past: index < activeLyricIndex }"
                  >
                    {{ lyric.text }}
                  </button>
                </div>
              </div>
            </div>
          </div>
          <footer class="memory-footer">made with love, just for you, my zhanym <span>♡</span></footer>
        </div>
      </section>
    </div>
  </main>
</template>
