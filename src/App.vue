<template>
  <div class="chuseok-page">
    <div class="background">
      <div class="moon"></div>
      <div class="floating-messages">
        <span
            v-for="(msg, index) in messages"
            :key="index"
            class="floating-text"
            :style="{
            top: `${randomHeights[index]}%`,
            color: getTextColor(randomHeights[index]),
            animationDelay: `${index * 3}s`
          }"
        >
          {{ msg }}
        </span>
      </div>
    </div>

    <header class="hero">
      <h1>🌕 한가위, 마음을 나누는 날 🌾</h1>
      <p>풍요로운 달빛 아래에서 함께하는 따뜻한 이야기</p>
    </header>

    <main>
      <section class="intro">
        <h2>추석이란?</h2>
        <p>
          추석은 음력 8월 15일, 한 해의 수확을 감사하고 조상에게 예를 올리는 한국의 대표 명절입니다.
          가족이 모여 송편을 빚고, 조상께 차례를 올리며, 이웃과 정을 나누는 날이지요.
        </p>
      </section>

      <section class="origin">
        <h2>추석의 유래</h2>
        <p>
          추석은 신라 시대의 ‘가배(嘉俳)’ 풍습에서 유래했습니다.
          두 편으로 나누어 한 달 동안 길쌈을 하고, 승패에 따라 잔치를 벌였던 풍습이
          오늘날의 수확 감사 절기로 발전한 것이지요.
        </p>
      </section>

      <section class="foods">
        <h2>전통 음식</h2>
        <ul>
          <li><strong>송편</strong> — 햇곡으로 빚은 반달 모양 떡. 가족의 소원을 담습니다.</li>
          <li><strong>육전</strong> — 얇게 썬 고기에 달걀옷을 입혀 지진 명절 대표 음식.</li>
          <li><strong>잡채</strong> — 여러 채소와 당면을 함께 볶은 화합의 음식.</li>
        </ul>
      </section>

      <section class="games">
        <h2>전통 놀이</h2>
        <ul>
          <li><strong>쥐불놀이</strong> — 불을 돌리며 풍년을 기원하는 명절 놀이.</li>
          <li><strong>제기차기</strong> — 제기를 높이 차 올리며 즐기는 운동 겸 놀이.</li>
        </ul>
      </section>

      <section class="guestbook">
        <h2>🌾 한가위 방명록</h2>
        <form @submit.prevent="addMessage">
          <input v-model="newMessage" placeholder="추석 인사를 남겨주세요!" required />
          <button type="submit">남기기</button>
        </form>

        <ul class="guest-list">
          <li v-for="(msg, idx) in messages" :key="idx">
            {{ msg }}
          </li>
        </ul>
      </section>
    </main>

    <footer>
      <p>© 2025 한가위의 마음 | 풍요로운 추석 되세요 🌕</p>
    </footer>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'

const messages = ref<string[]>([
  '보름달처럼 밝은 한가위 되세요 🌕',
  '가족과 함께 즐거운 명절 보내세요 🎑',
  '풍성한 마음으로 행복을 나누세요 🍂',
  '멀리 있어도 마음은 함께 🌾',
  '건강하고 따뜻한 추석 보내세요 💛',
])

const newMessage = ref('')
const randomHeights = ref<number[]>([])

onMounted(() => {
  randomHeights.value = messages.value.map(() =>
      Math.floor(Math.random() * 70) + 10
  )
})

function getTextColor(topPercent: number) {
  if (topPercent < 40 && topPercent > 10) {
    return '#f0e5a2'
  }
  return '#ffffffb3'
}

function addMessage() {
  if (newMessage.value.trim()) {
    messages.value.push(newMessage.value.trim())
    randomHeights.value.push(Math.floor(Math.random() * 70) + 10)
    newMessage.value = ''
  }
}
</script>

<style scoped>
.chuseok-page {
  font-family: 'Pretendard', sans-serif;
  color: #f5f5f5;
  background: radial-gradient(circle at 70% 20%, #3b2c5a, #1b152a 80%);
  min-height: 100vh;
  overflow-x: hidden;
  position: relative;
}

.moon {
  position: absolute;
  top: 80px;
  right: 15%;
  width: 200px;
  height: 200px;
  background: radial-gradient(circle, #fffde8 60%, #f7eeb5 85%, #e0d38f 100%);
  border-radius: 50%;
  box-shadow: 0 0 40px 20px rgba(255, 255, 200, 0.3);
  animation: pulse 6s ease-in-out infinite;
}

@keyframes pulse {
  0%, 100% {
    transform: scale(1);
    opacity: 0.95;
  }
  50% {
    transform: scale(1.05);
    opacity: 1;
  }
}

.floating-messages {
  position: absolute;
  top: 0;
  left: 0;
  width: 100%;
  height: 100%;
  overflow: hidden;
  pointer-events: none;
}

.floating-text {
  position: absolute;
  left: 100%;
  white-space: nowrap;
  font-size: 1rem;
  font-weight: 500;
  text-shadow: 0 0 6px rgba(0, 0, 0, 0.6);
  animation: floatText 22s linear infinite;
  pointer-events: none;
  opacity: 0.9;
}

@keyframes floatText {
  0% {
    transform: translateX(100%) translateY(0);
  }
  50% {
    transform: translateX(0%) translateY(3px);
  }
  100% {
    transform: translateX(-120%) translateY(0);
  }
}

.hero {
  text-align: center;
  padding: 120px 20px 60px;
}

.hero h1 {
  font-size: 2.4rem;
  margin-bottom: 12px;
}

.hero p {
  font-size: 1.1rem;
  color: #ffda8b;
}

section {
  max-width: 800px;
  margin: 60px auto;
  padding: 0 20px;
  background: rgba(255, 255, 255, 0.05);
  border-radius: 12px;
  backdrop-filter: blur(6px);
}

section h2 {
  font-size: 1.5rem;
  border-bottom: 2px solid #f5d67c;
  padding-bottom: 6px;
  margin-bottom: 16px;
  color: #ffe8b5;
}

ul {
  list-style: none;
  padding: 0;
}

li {
  margin-bottom: 10px;
}

.guestbook form {
  display: flex;
  gap: 8px;
  margin-bottom: 16px;
}

.guestbook input {
  flex: 1;
  padding: 10px;
  border: none;
  border-radius: 6px;
}

.guestbook button {
  background: #f5d67c;
  border: none;
  border-radius: 6px;
  padding: 10px 16px;
  cursor: pointer;
  font-weight: bold;
  transition: 0.3s;
}

.guestbook button:hover {
  background: #ffecb3;
}

footer {
  text-align: center;
  margin: 60px 0;
  font-size: 0.9rem;
  color: #ccc;
}
</style>
