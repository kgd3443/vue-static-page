<template>
  <div class="page">
    <!-- ✅ ① 보름달 배경 -->
    <div class="background">
      <div class="moon"></div>

      <!-- ✅ ② 흘러가는 방명록 메시지 -->
      <div class="floating-messages">
        <span
            v-for="(msg, i) in messages"
            :key="i"
            class="floating"
            :style="{ top: `${positions[i]}%`, animationDelay: `${i * 2}s` }"
        >
          {{ msg }}
        </span>
      </div>
    </div>

    <!-- ✅ ③ 메인 카피 -->
    <header class="hero">
      <h1>🌕 한가위, 풍요와 나눔의 명절 🌾</h1>
      <p>달빛이 가장 아름다운 날, 마음까지 둥글게 나누는 시간</p>
    </header>

    <main>
      <!-- ✅ ④ 추석 소개 -->
      <section>
        <h2>추석이란 무엇일까?</h2>
        <p>
          추석은 음력 8월 15일, 한 해의 수확에 감사하고 조상에게 예를 올리는 한국의 대표 명절입니다.
          한가위라고도 하며, 가족이 모여 송편을 빚고 서로의 안부를 전하는 따뜻한 날입니다.
        </p>
      </section>

      <!-- ✅ ⑤ 유래 -->
      <section>
        <h2>추석의 유래</h2>
        <p>
          추석의 기원은 신라시대 ‘가배(嘉俳)’ 풍습에서 비롯되었습니다.
          두 편으로 나뉘어 한 달 동안 길쌈을 겨루고, 이긴 쪽이 잔치를 벌였다고 전해집니다.
          이후 농경 사회의 풍요로운 수확 시기와 결합되어 오늘날의 추석으로 이어졌습니다.
        </p>
      </section>

      <!-- ✅ ⑥ 전통 음식 -->
      <section>
        <h2>전통 음식</h2>
        <ul>
          <li><strong>송편</strong> — 햇곡으로 만든 반달 모양의 떡으로, 가족의 소원을 담습니다.</li>
          <li><strong>육전</strong> — 고기를 얇게 썰어 달걀옷을 입혀 부친 음식으로 명절의 별미입니다.</li>
          <li><strong>잡채</strong> — 여러 재료를 어우러지게 볶아낸 음식으로 풍요로움을 상징합니다.</li>
        </ul>
      </section>

      <!-- ✅ ⑦ 전통 놀이 -->
      <section>
        <h2>전통 놀이</h2>
        <ul>
          <li><strong>쥐불놀이</strong> — 들판의 해충을 없애며 풍년을 기원하는 불놀이.</li>
          <li><strong>제기차기</strong> — 제기를 발로 차서 오래 띄우며 즐기는 운동 겸 놀이.</li>
        </ul>
      </section>

      <!-- ✅ ⑧ 방명록 -->
      <section class="guestbook">
        <h2>🌾 한가위 방명록</h2>
        <form @submit.prevent="addMessage">
          <input v-model="newMessage" placeholder="추석 인사를 남겨주세요!" required />
          <button>남기기</button>
        </form>
        <ul>
          <li v-for="(msg, i) in messages" :key="i">{{ msg }}</li>
        </ul>
      </section>
    </main>

    <!-- ✅ ⑨ 푸터 -->
    <footer>
      <p>© 2025 풍요로운 한가위 | 보름달처럼 밝은 하루 되세요 🌕</p>
    </footer>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'

// ✅ 방명록 메시지 리스트
const messages = ref<string[]>([
  '🌕 행복이 가득한 추석 보내세요!',
  '🍂 가족과 함께 따뜻한 시간 보내세요!',
  '🎑 풍요로운 보름달처럼 마음도 가득 채우세요!',
])

// ✅ 새로운 메시지 입력
const newMessage = ref('')

// ✅ 각 메시지의 랜덤 높이 위치 (겹치지 않게)
const positions = ref<number[]>([])
onMounted(() => {
  positions.value = messages.value.map(() => Math.random() * 70 + 10)
})

// ✅ 메시지 추가
function addMessage() {
  if (!newMessage.value.trim()) return
  messages.value.push(newMessage.value.trim())
  positions.value.push(Math.random() * 70 + 10)
  newMessage.value = ''
}
</script>

<style scoped>
/* ✅ 전체 페이지 스타일 */
.page {
  min-height: 100vh;
  color: #111111;
  background: radial-gradient(circle at 60% 20%, #2e2246 0%, #120b1c 80%);
  font-family: "Pretendard", sans-serif;
  overflow-x: hidden;
  position: relative;
}

/* ✅ 보름달 */
.moon {
  position: absolute;
  top: 100px;
  right: 15%;
  width: 220px;
  height: 220px;
  border-radius: 50%;
  background: radial-gradient(circle, #fffbe6 60%, #f3e69b 85%, #d9c66d 100%);
  box-shadow: 0 0 50px 20px rgba(255, 250, 200, 0.3);
  animation: moonGlow 6s ease-in-out infinite;
}
@keyframes moonGlow {
  0%, 100% { opacity: 0.95; transform: scale(1); }
  50% { opacity: 1; transform: scale(1.05); }
}

/* ✅ 흘러가는 메시지 */
.floating-messages {
  position: absolute;
  inset: 0;
  overflow: hidden;
  pointer-events: none;
}
.floating {
  position: absolute;
  left: 100%;
  white-space: nowrap;
  animation: float 25s linear infinite;
  font-size: 1rem;
  text-shadow: 0 0 8px rgba(0,0,0,0.6);
  opacity: 0.9;
  color: #f5e8a3;
}
@keyframes float {
  0% { transform: translateX(100%); }
  100% { transform: translateX(-120%); }
}

/* ✅ 메인 카피 */
.hero {
  text-align: center;
  padding: 140px 20px 60px;
}
.hero h1 {
  font-size: 2.4rem;
  margin-bottom: 12px;
}
.hero p {
  font-size: 1.1rem;
  color: #ffeaa7;
}

/* ✅ 섹션 */
section {
  background: rgba(255, 255, 255, 0.08);
  backdrop-filter: blur(5px);
  border-radius: 12px;
  margin: 60px auto;
  padding: 30px 20px;
  max-width: 800px;
}
section h2 {
  font-size: 1.5rem;
  border-bottom: 2px solid #f5d67c;
  padding-bottom: 6px;
  margin-bottom: 16px;
  color: #ffeab3;
}
section ul {
  list-style: none;
  padding-left: 0;
}
section li {
  margin-bottom: 10px;
}

/* ✅ 방명록 입력 폼 */
.guestbook form {
  display: flex;
  gap: 8px;
  margin-bottom: 20px;
}
.guestbook input {
  flex: 1;
  padding: 10px;
  border: none;
  border-radius: 6px;
}
.guestbook button {
  background: #f6d77c;
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

/* ✅ 푸터 */
footer {
  text-align: center;
  margin: 80px 0 40px;
  color: #ddd;
  font-size: 0.9rem;
}
</style>
