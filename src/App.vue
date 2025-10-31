<template>
  <div class="page">
    <!-- 배경 레이어: 방명록 문구가 흘러감 -->
    <div class="floating-layer" aria-hidden="true">
      <span
          v-for="(msg, i) in messages"
          :key="'bg-' + i"
          class="floating"
          :style="{
          top: getLane(i) + '%',
          animationDelay: (i * 2) + 's'
        }"
      >
        {{ msg }}
      </span>
    </div>

    <!-- 메인 헤더: 보름달을 가장 위에 -->
    <header class="hero">
      <div class="moon" aria-hidden="true"></div>
      <h1>한가위, 풍요와 나눔의 날</h1>
      <p class="subtitle">달빛이 가장 둥근 날, 마음까지 둥글게</p>
    </header>

    <main>
      <section>
        <h2>추석이란 무엇일까?</h2>
        <p>
          추석은 음력 8월 15일, 한 해의 수확에 감사하고 조상에게 예를 올리는 우리나라의 대표 명절입니다.
          가족이 모여 안부를 나누고, 송편을 빚으며, 서로의 마음을 나누는 따뜻한 날이지요.
        </p>
      </section>

      <section>
        <h2>추석의 유래</h2>
        <p>
          추석의 기원은 신라 시대의 ‘가배(嘉俳)’ 풍습에서 비롯되었다고 전해집니다.
          두 편으로 나뉘어 한 달 동안 길쌈을 겨루고, 승패에 따라 잔치를 베풀던 풍습이
          농경 사회의 수확 시기와 결합되어 오늘날의 추석으로 발전했습니다.
        </p>
        <p>
          현대에 와서는 차례와 성묘, 가족 모임, 전통 음식과 놀이를 통해
          ‘감사’와 ‘나눔’의 의미를 기리며 이어지고 있습니다.
        </p>
      </section>

      <section>
        <h2>추석의 전통 음식</h2>
        <ul>
          <li>
            <strong>송편</strong> — 햇곡으로 빚은 반달 모양의 떡.
            가족의 소원을 담아 함께 빚고 나눠 먹습니다.
          </li>
          <li>
            <strong>육전</strong> — 얇게 썬 고기에 달걀옷을 입혀 부친 명절 별미로,
            담백한 맛과 풍성한 식감이 특징입니다.
          </li>
          <li>
            <strong>잡채</strong> — 여러 채소와 당면을 어우러지게 볶아낸 음식으로
            조화와 풍요를 상징합니다.
          </li>
        </ul>
      </section>

      <section>
        <h2>추석의 전통 놀이</h2>
        <ul>
          <li>
            <strong>쥐불놀이</strong> — 들판 해충을 없애고 풍년을 기원하며
            불붙인 깡통을 원을 그리며 돌리던 놀이입니다.
          </li>
          <li>
            <strong>제기차기</strong> — 제기를 발로 차서 오래 띄우는 놀이로,
            남녀노소 함께 즐길 수 있는 명절 놀이입니다.
          </li>
        </ul>
      </section>

      <section class="guestbook">
        <h2>한가위 방명록</h2>
        <form @submit.prevent="addMessage" class="form">
          <input
              v-model="newMessage"
              type="text"
              placeholder="추석 인사를 남겨주세요!"
              required
              aria-label="방명록 입력"
          />
          <button type="submit">남기기</button>
        </form>

        <ul class="list">
          <li v-for="(msg, i) in messages" :key="'list-' + i">{{ msg }}</li>
        </ul>
      </section>
    </main>

    <footer class="footer">
      <p>© 2025 한가위의 마음 | 풍요로운 추석 되세요 🌕</p>
    </footer>
  </div>
</template>

<script lang="ts" setup>
import { ref, onMounted } from 'vue'

const messages = ref<string[]>([
  '보름달처럼 환한 한가위 되세요 🌕',
  '가족과 함께 따뜻한 시간 보내세요 🎑',
  '풍성한 수확처럼 행복이 가득하길 🍂',
])

const newMessage = ref<string>('')

/** 배경 텍스트의 Y위치를 위한 “레인” 값들(%) */
const lanes = ref<number[]>([])

const LANE_MIN = 10
const LANE_MAX = 80
const LANE_COUNT = 6

const clamp = (v: number, lo: number, hi: number): number =>
    Math.min(hi, Math.max(lo, v))

/** 균등하게 분포된 레인(약간의 지터 포함) 생성 */
function buildLaneSet(count: number): number[] {
  const gap = (LANE_MAX - LANE_MIN) / Math.max(1, LANE_COUNT - 1)
  const base = Array.from({ length: LANE_COUNT }, (_, i) => LANE_MIN + i * gap)
  const jittered = base.map(v => clamp(v + (Math.random() * 6 - 3), LANE_MIN, LANE_MAX))
  const out: number[] = []
  for (let i = 0; i < count; i++) {
    out.push(jittered[i % LANE_COUNT])
  }
  return out
}

/** i번째 메시지가 사용할 안전한 레인 값(항상 number 반환) */
function getLane(i: number): number {
  const arr = lanes.value
  if (arr.length === 0) return 50
  const lane = arr[i % arr.length]
  return typeof lane === 'number' ? lane : 50
}

onMounted(() => {
  lanes.value = buildLaneSet(messages.value.length)
})

function addMessage(): void {
  const text = newMessage.value.trim()
  if (!text) return
  messages.value.push(text)

  // 새 메시지용 레인 추가 (항상 number 보장)
  const arrLen = lanes.value.length > 0 ? lanes.value.length : 1
  const base = buildLaneSet(arrLen)
  const next = base[(messages.value.length - 1) % arrLen]
  lanes.value.push(next)
  newMessage.value = ''
}
</script>

<style scoped>
.page {
  min-height: 100vh;
  color: #f7f7f7;
  background:
      radial-gradient(circle at 60% -10%, rgba(255,255,255,0.08), transparent 40%),
      radial-gradient(circle at 10% 20%, rgba(255,255,255,0.06), transparent 35%),
      linear-gradient(180deg, #190f2a 0%, #0f0b1a 60%, #0a0712 100%);
  font-family: "Pretendard", system-ui, -apple-system, Segoe UI, Roboto, sans-serif;
  overflow-x: hidden;
  position: relative;
  padding-bottom: 72px;
}

/* 배경 흐르는 텍스트 (고정) */
.floating-layer {
  position: fixed;
  inset: 0;
  pointer-events: none;
  z-index: 1;
  opacity: 0.9;
}
.floating {
  position: absolute;
  left: 100%;
  white-space: nowrap;
  font-size: 0.95rem;
  color: rgba(255, 240, 200, 0.85);
  text-shadow: 0 0 8px rgba(0,0,0,0.6);
  animation: float-left 24s linear infinite;
}
@keyframes float-left {
  0%   { transform: translateX(100%);   opacity: 0.9; }
  10%  { opacity: 1; }
  90%  { opacity: 1; }
  100% { transform: translateX(-130%); opacity: 0.9; }
}

/* 헤더/보름달 */
.hero {
  position: relative;
  z-index: 2;
  padding: 56px 20px 24px;
  text-align: center;
}
.moon {
  width: 240px;
  height: 240px;
  margin: 24px auto 20px;
  border-radius: 50%;
  background:
      radial-gradient(circle at 45% 40%, #fffbe9 0%, #fff3b8 55%, #edd787 80%, #d9c06e 100%);
  box-shadow:
      0 0 32px 8px rgba(255, 245, 200, 0.3),
      0 0 120px 40px rgba(255, 245, 200, 0.08);
  animation: moon-pulse 7s ease-in-out infinite;
}
@keyframes moon-pulse {
  0%, 100% { transform: scale(1);   opacity: 0.96; }
  50%      { transform: scale(1.04); opacity: 1; }
}
.hero h1 {
  font-size: 2.35rem;
  margin: 6px 0 6px;
  letter-spacing: 0.2px;
}
.subtitle {
  color: #ffe8a8;
  opacity: 0.95;
}

/* 본문 섹션 */
main { position: relative; z-index: 3; }
section {
  max-width: 860px;
  margin: 28px auto;
  padding: 24px 20px;
  border-radius: 14px;
  background: rgba(255, 255, 255, 0.06);
  backdrop-filter: blur(6px);
  border: 1px solid rgba(255, 255, 255, 0.08);
}
section h2 {
  font-size: 1.4rem;
  margin-bottom: 12px;
  color: #ffe7a1;
  border-bottom: 2px solid rgba(255, 231, 161, 0.35);
  padding-bottom: 6px;
}
section ul { list-style: none; padding-left: 0; }
section li { margin: 10px 0; line-height: 1.7; }

/* 방명록 */
.guestbook .form {
  display: flex;
  gap: 10px;
  margin: 14px 0 16px;
}
.guestbook input {
  flex: 1;
  padding: 11px 12px;
  border-radius: 10px;
  border: 1px solid rgba(255, 255, 255, 0.14);
  background: rgba(255, 255, 255, 0.08);
  color: #fff;
  outline: none;
}
.guestbook input::placeholder { color: rgba(255, 255, 255, 0.7); }
.guestbook button {
  padding: 11px 16px;
  border: none;
  border-radius: 10px;
  background: linear-gradient(135deg, #f2d37a, #ffd98f);
  color: #2a1f42;
  font-weight: 700;
  cursor: pointer;
  transition: transform 0.15s ease, filter 0.15s ease;
}
.guestbook button:hover { transform: translateY(-1px); filter: brightness(1.05); }
.guestbook .list {
  margin-top: 10px;
  display: grid;
  gap: 8px;
}
.guestbook .list li {
  padding: 10px 12px;
  background: rgba(255, 255, 255, 0.06);
  border: 1px solid rgba(255, 255, 255, 0.08);
  border-radius: 10px;
  line-height: 1.6;
}

/* 푸터 */
.footer {
  position: relative;
  z-index: 3;
  text-align: center;
  margin: 36px 0 12px;
  color: #e6e2f0;
  opacity: 0.9;
  font-size: 0.92rem;
}
</style>
