<template>
  <div class="chuseok-page">
    <!-- ✅ ① 메인 카피 -->
    <header class="hero">
      <h1>🍁 한가위, 마음을 나누는 날 🍁</h1>
      <p>풍요로운 보름달 아래, 함께하는 따뜻한 추석 이야기</p>
    </header>

    <main>
      <!-- ✅ ② 추석 소개 섹션 -->
      <section class="intro">
        <h2>추석이란 무엇일까?</h2>
        <p>
          추석은 음력 8월 15일, 한 해의 수확을 감사하고 조상에게 예를 올리는 우리나라의 대표 명절입니다.
          ‘한가위’라고도 불리며, 온 가족이 모여 즐거운 시간을 보내는 날이지요.
        </p>
      </section>

      <!-- ✅ ③ 추석의 유래 섹션 -->
      <section class="origin">
        <h2>추석의 유래</h2>
        <p>
          추석의 기원은 삼국시대 신라의 ‘가배(嘉俳)’ 풍습에서 비롯되었다고 전해집니다.
          당시에는 여성들이 편을 나누어 한 달간 길쌈을 겨루고,
          그 결과에 따라 이긴 편은 잔치를 열고 진 편은 술과 음식을 대접했다고 합니다.
        </p>
        <p>
          이후 농경사회가 정착되면서 수확의 기쁨을 나누는 풍습으로 발전했고,
          현재까지도 조상 제사, 성묘, 음식 나눔, 전통 놀이 등으로 이어지고 있습니다.
        </p>
      </section>

      <!-- ✅ ④ 전통 음식 소개 섹션 -->
      <section class="foods">
        <h2>추석의 전통 음식</h2>
        <ul>
          <li>
            <strong>송편</strong> — 햇곡으로 빚은 반달 모양의 떡으로,
            소원을 담아 가족이 함께 만들어 먹습니다.
          </li>
          <li>
            <strong>육전</strong> — 고기를 얇게 저며 달걀옷을 입혀 지진 음식으로,
            명절 상차림에 빠질 수 없는 별미입니다.
          </li>
          <li>
            <strong>잡채</strong> — 여러 채소와 당면을 함께 볶아낸 음식으로
            풍성한 색감과 맛을 자랑합니다.
          </li>
        </ul>
      </section>

      <!-- ✅ ⑤ 전통 놀이 소개 섹션 -->
      <section class="games">
        <h2>추석의 전통 놀이</h2>
        <ul>
          <li>
            <strong>쥐불놀이</strong> — 들판의 해충을 없애고 풍년을 기원하며
            불붙은 깡통을 돌리는 놀이입니다.
          </li>
          <li>
            <strong>제기차기</strong> — 제기를 발로 차서 오래 띄우는 놀이로,
            아이들과 어른이 함께 즐기는 명절 놀이입니다.
          </li>
        </ul>
      </section>
    </main>

    <!-- ✅ ⑥ 푸터 -->
    <footer>
      <p>© 2025 한가위의 마음 - 모두에게 행복이 가득하길 🌕</p>
    </footer>
  </div>
</template>

<script lang="ts" setup>
// ✅ ⑦ (선택) 추석 테마 색상 혹은 애니메이션 로직을 추가할 수 있습니다.
</script>

<style scoped>
/* ✅ ⑧ 스타일 구성 */
.chuseok-page {
  font-family: 'Pretendard', sans-serif;
  color: #333;
  background: #fffdf8;
  line-height: 1.7;
  padding-bottom: 60px;
}

.hero {
  text-align: center;
  background: linear-gradient(to bottom, #ffe4b5, #fffaf0);
  padding: 80px 20px;
}

.hero h1 {
  font-size: 2.2rem;
  margin-bottom: 10px;
}

.hero p {
  font-size: 1.1rem;
  color: #5a4a2f;
}

section {
  max-width: 800px;
  margin: 60px auto;
  padding: 0 20px;
}

section h2 {
  font-size: 1.6rem;
  color: #8b4513;
  border-bottom: 2px solid #deb887;
  padding-bottom: 8px;
  margin-bottom: 16px;
}

ul {
  list-style: none;
  padding-left: 0;
}

li {
  margin-bottom: 12px;
}

footer {
  text-align: center;
  margin-top: 80px;
  color: #777;
  font-size: 0.9rem;
}
</style>
