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
    message: 'Ты у меня такая роскошная и опасная, реально "Fashion Killa"... будто Рианна, а я твой прити мазафакер😎.',
    songTitle: 'Fashion Killa',
    artist: 'A$AP Rocky',
    audio: '/music/A_AP-Rocky-Fashion-Killa-_Official-Audio_.mp3',
    lyrics: [
      { time: 14, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 17.6, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 20.65, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 24.45, text: "'Cause she a fashion killa, and I'm a trendy nigga" },
      { time: 27.39, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 31.35, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 34.24, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 37.9, text: "'Cause she a fashion killa, and I'm a jiggy nigga, uh" },
      { time: 41.34, text: "I said, rockin', rollin', swaggin' to the max" },
      { time: 44.74, text: 'My bitch a fashion killa, she be busy poppin’ tags' },
      { time: 48.45, text: 'She got a lotta Prada, that Dolce and Gabbana' },
      { time: 51.84, text: "I can't forget Escada, and that Balenciaga" },
      { time: 56, text: "I'm sippin' purple syrup, come be my Aunt Jemima" },
      { time: 59.2, text: 'And if you is a rider, we’ll go shopping like mañana' },
      { time: 62.85, text: 'Her attitude Rihanna, she get it from her mama' },
      { time: 66, text: 'She jiggy like Madonna, but she trippy like Nirvana' },
      { time: 69.9, text: "'Cause everything designer, her jeans is Helmut Lang" },
      { time: 72.9, text: 'Shoes is Alexander Wang and her shirt the newest Donna' },
      { time: 76.8, text: 'Karan, wearing all the Cartier frames' },
      { time: 80, text: "Jean Paul Gaultiers 'cause they match with her persona" },
      { time: 83.68, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 86.82, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 90.2, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 93.35, text: "'Cause she a fashion killa, and I'm a trendy nigga" },
      { time: 96.4, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 99.9, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 103.4, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 106.76, text: "'Cause she a fashion killa, and I'm a jiggy nigga" },
      { time: 110.3, text: 'I said, I see your Jil Sanders, Oliver Peoples' },
      { time: 115, text: 'Costume National, your Ann Demeuelemeester' },
      { time: 118.1, text: 'See Visvim be the sneaker, Lanvin or Balmain' },
      { time: 121.5, text: 'Goyard by the trunk, her Isabel Marant' },
      { time: 124.95, text: 'I love your Linda Farrow, I adore your Dior' },
      { time: 128.28, text: 'Your Damir Doma, Vena Cava from the store' },
      { time: 131.87, text: 'I crush down with that top down, bossy see how I ride ’round' },
      { time: 135.25, text: 'Mami in that Tom Ford, Papi in that Thom Browne' },
      { time: 138.55, text: 'Rick Owens, Raf Simons, boy she got it by the stock' },
      { time: 141.95, text: "She ball until she fall, that means she shop 'til she drop" },
      { time: 145.2, text: 'And Versace got a lot, but she may never wear it' },
      { time: 148.87, text: 'But she save it so our babies will be flyer than their parents' },
      { time: 151.92, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 155.14, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 158.38, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 161.65, text: "'Cause she a fashion killa, and I'm a trendy nigga" },
      { time: 165.5, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 168.95, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 171.98, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 175.48, text: "'Cause she a fashion killa, and I'm a jiggy nigga" },
      { time: 178.85, text: 'I said her pistol go' },
      { time: 180, text: 'Scoop back tees, breeze in the coupé' },
      { time: 183.3, text: 'Smilin’ is your treasure, you’re so well put together' },
      { time: 186.25, text: 'Bags and links, jeans and shoes' },
      { time: 190.1, text: 'Spikes and patent leathers, different fabrics mixed together' },
      { time: 193.7, text: 'You and me, me and you' },
      { time: 197.1, text: 'Go away together, we could get away forever' },
      { time: 199.99, text: 'All emotions clashin’, thrashin’, someone turn the light out' },
      { time: 203.8, text: 'I met my baby expressed my passion on my fashion night out' },
      { time: 207.32, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 210.52, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 213.2, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 217.13, text: "'Cause she a fashion killa, and I'm a trendy nigga" },
      { time: 220.2, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 224.22, text: 'Her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 226.63, text: 'I said her pistol go (Bang-bang, boom-boom, pop-pop)' },
      { time: 230.28, text: "'Cause she a fashion killa, and I'm a jiggy nigga" },
      { time: 234.12, text: 'I said her pistol go (I said her pistol go)' },
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
      { time: 49.5, text: 'Ты моя звезда' },
      { time: 52, text: 'А я твой номер один, baby, я твой номер один' },
      { time: 59, text: 'Знаешь? А я твой номер один, baby, я твой номер один' },
      { time: 67, text: 'А я твой номер один' },
      { time: 71, text: 'От тебя схожу с ума' },
      { time: 74, text: 'Только лишь с тобой, только твой Дима Билан' },
      { time: 78, text: 'Так владеешь телом, детка, ближе к делу (а-а-а)' },
      { time: 82.2, text: 'Люби меня, этой ночью ты моя' },
      { time: 87.8, text: 'Детка, ты за мной иди (ты за мной иди)' },
      { time: 92.2, text: 'Я тот, кого так хочешь ты (кого так хочешь ты)' },
      { time: 97, text: 'Я вижу блеск в твоих глазах' },
      { time: 101, text: 'Ты моя звезда' },
      { time: 103, text: 'А я твой номер один, baby, я твой номер один' },
      { time: 110, text: 'Знаешь? А я твой номер один, baby, я твой номер один' },
      { time: 117.3, text: 'А я твой номер один' },
      { time: 119.5, text: 'Только твой Дима Билан, только твой Дима Билан' },
      { time: 123.7, text: 'Только твой Дима Билан, только твой Дима Билан' },
      { time: 128, text: 'Только твой Дима Билан, только твой Дима Билан' },
      { time: 132.4, text: 'Только твой Дима Билан, только твой Дима Билан' },
      { time: 138, text: 'Детка, ты за мной иди' },
      { time: 142.5, text: 'Я тот, кого так хочешь ты' },
      { time: 147, text: 'Я вижу блеск в твоих глазах' },
      { time: 151, text: 'Ты моя звезда' },
      { time: 153.8, text: 'А я твой номер один, baby, я твой номер один' },
      { time: 161, text: 'Знаешь? А я твой номер один, baby, я твой номер один' },
      { time: 169.7, text: 'А я твой номер один, baby, я твой номер один' },
      { time: 177, text: 'Знаешь? А я твой номер один, baby, я твой номер один' },
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
