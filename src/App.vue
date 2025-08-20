<template>
  <div class="app-container">
    <!-- Header -->
    <header class="header">
      <div class="header-content">
        <h1 class="title">🏛️ 世界遺産はかせ</h1>
        <p class="subtitle">日本の美しい世界遺産を探索しよう</p>
      </div>
    </header>

    <!-- Main Content -->
    <main class="main-content">
      <div class="search-section">
        <div class="search-card">
          <h2 class="search-title">世界遺産を検索</h2>
          <div class="input-group">
            <input 
              v-model="userInput"
              type="text"
              class="search-input"
              placeholder="例：富士山、姫路城、法隆寺、古都京都..."
              @keyup.enter="confirmSelection"
            >
            <button @click="confirmSelection" class="search-button">
              <span class="search-icon">🔍</span>
              検索
            </button>
          </div>
        </div>
      </div>

      <!-- Results Section -->
      <div class="results-section" v-if="prefOutput || imageOutput">
        <div class="result-card">
          <div class="image-container" v-if="imageOutput">
            <img :src="imgOutput(imageOutput)" :alt="lastSearchedHeritage" class="heritage-image">
          </div>
          <div class="info-container" v-if="prefOutput">
            <h3 class="heritage-name">{{ lastSearchedHeritage }}</h3>
            <p class="prefecture-info">
              <span class="label">所在地：</span>
              <span class="prefecture">{{ prefOutput }}</span>
            </p>
          </div>
        </div>
      </div>

      <!-- Heritage List -->
      <div class="heritage-list-section">
        <h2 class="section-title">日本の世界遺産一覧</h2>
        <div class="heritage-grid">
          <div 
            v-for="(pref, heritage) in heritageToPref" 
            :key="heritage"
            class="heritage-item"
            @click="selectHeritage(heritage)"
          >
            <div class="heritage-card">
              <img :src="imgOutput(heritageToImage[heritage])" :alt="heritage" class="card-image">
              <div class="card-content">
                <h3 class="card-title">{{ heritage }}</h3>
                <p class="card-location">{{ pref }}</p>
              </div>
            </div>
          </div>
        </div>
      </div>
    </main>
  </div>
</template>

<script setup>
import { ref } from 'vue';

const userInput = ref("");
const prefOutput = ref("");
const imageOutput = ref("");
const lastSearchedHeritage = ref("");
const heritageToPref = {
  // 既存の世界遺産
  "富士山": "静岡県・山梨県",
  "知床半島": "北海道",
  "姫路城": "兵庫県",
  "屋久島": "鹿児島県",
  "白神山地": "青森県・秋田県",
  "白川郷": "岐阜県・富山県",
  "原爆ドーム": "広島県",
  "厳島神社": "広島県",
  "首里城": "沖縄県",
  "石見銀山": "島根県",
  "小笠原諸島": "東京都",
  "富岡製糸場": "群馬県",
  "佐渡島": "新潟県",
  
  // 追加の世界遺産
  "法隆寺": "奈良県",
  "古都京都": "京都府・滋賀県",
  "古都奈良": "奈良県",
  "日光の社寺": "栃木県",
  "琉球王国": "沖縄県",
  "紀伊山地": "奈良県・和歌山県・三重県",
  "平泉": "岩手県",
  "明治日本の産業革命遺産": "福岡県・佐賀県・長崎県・熊本県・鹿児島県・山口県・岩手県・静岡県",
  "ル・コルビュジエの建築作品": "東京都",
  "百舌鳥・古市古墳群": "大阪府",
  "北海道・北東北の縄文遺跡群": "北海道・青森県・岩手県・秋田県",
  "奄美大島・徳之島・沖縄島北部・西表島": "鹿児島県・沖縄県",
  "鎌倉": "神奈川県",
  "熊野古道": "和歌山県・奈良県・三重県",
  "高野山": "和歌山県",
};

const heritageToImage = {
  // 既存の世界遺産
  "富士山": "Mt_Fujiimage.jpeg",
  "知床半島": "shiretoko_hanntou.jpeg",
  "姫路城": "Himeji_jyou.jpeg",
  "屋久島": "yakushima.png",
  "白神山地": "shirakamisanchi.png",
  "白川郷": "shirakawagou.png",
  "原爆ドーム": "genbakudomu.png",
  "厳島神社": "itukushima_jinjya.png",
  "首里城": "shurijyou.png",
  "石見銀山": "iwamiginzan.png",
  "小笠原諸島": "ogasawarashotou.png",
  "富岡製糸場": "tomiokaseishijyou.png",
  "佐渡島": "sadogashima.png",
  
  // 追加の世界遺産（プレースホルダー画像）
  "法隆寺": "sekaiisanichiran.png",
  "古都京都": "sekaiisanichiran.png",
  "古都奈良": "sekaiisanichiran.png",
  "日光の社寺": "sekaiisanichiran.png",
  "琉球王国": "shurijyou.png",
  "紀伊山地": "sekaiisanichiran.png",
  "平泉": "sekaiisanichiran.png",
  "明治日本の産業革命遺産": "sekaiisanichiran.png",
  "ル・コルビュジエの建築作品": "sekaiisanichiran.png",
  "百舌鳥・古市古墳群": "sekaiisanichiran.png",
  "北海道・北東北の縄文遺跡群": "sekaiisanichiran.png",
  "奄美大島・徳之島・沖縄島北部・西表島": "yakushima.png",
  "鎌倉": "sekaiisanichiran.png",
  "熊野古道": "sekaiisanichiran.png",
  "高野山": "sekaiisanichiran.png",
}

const imgOutput = (fileName) => {
  if (!fileName) return "";
  return new URL(`./assets/${fileName}`, import.meta.url).href;
};

const confirmSelection = () => {
  const trimmedInput = userInput.value.trim();
  if (!trimmedInput) return;

  if (heritageToPref[trimmedInput]) {
    imageOutput.value = heritageToImage[trimmedInput] || "";
    prefOutput.value = heritageToPref[trimmedInput];
    lastSearchedHeritage.value = trimmedInput;
    userInput.value = "";
  } else {
    alert("該当する世界遺産が見つかりませんでした。正確な名前を入力してください。");
  }
};

const selectHeritage = (heritage) => {
  userInput.value = heritage;
  confirmSelection();
};
</script>

<style scoped>
/* App Container */
.app-container {
  min-height: 100vh;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  font-family: 'Hiragino Kaku Gothic ProN', 'Hiragino Sans', 'Meiryo', sans-serif;
}

/* Header */
.header {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  box-shadow: 0 4px 20px rgba(0, 0, 0, 0.1);
  padding: 2rem 0;
  text-align: center;
}

.header-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 0 2rem;
}

.title {
  font-size: 3rem;
  font-weight: 700;
  color: #2d3748;
  margin: 0 0 0.5rem 0;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.1);
}

.subtitle {
  font-size: 1.2rem;
  color: #718096;
  margin: 0;
  font-weight: 300;
}

/* Main Content */
.main-content {
  max-width: 1200px;
  margin: 0 auto;
  padding: 3rem 2rem;
}

/* Search Section */
.search-section {
  margin-bottom: 3rem;
}

.search-card {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 2.5rem;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.15);
  text-align: center;
  transition: transform 0.3s ease;
}

.search-card:hover {
  transform: translateY(-5px);
}

.search-title {
  font-size: 1.8rem;
  color: #2d3748;
  margin: 0 0 2rem 0;
  font-weight: 600;
}

.input-group {
  display: flex;
  gap: 1rem;
  max-width: 500px;
  margin: 0 auto;
  flex-wrap: wrap;
}

.search-input {
  flex: 1;
  min-width: 300px;
  padding: 1rem 1.5rem;
  border: 2px solid #e2e8f0;
  border-radius: 50px;
  font-size: 1.1rem;
  outline: none;
  transition: all 0.3s ease;
  background: white;
}

.search-input:focus {
  border-color: #667eea;
  box-shadow: 0 0 0 3px rgba(102, 126, 234, 0.1);
}

.search-button {
  padding: 1rem 2rem;
  background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
  color: white;
  border: none;
  border-radius: 50px;
  font-size: 1.1rem;
  font-weight: 600;
  cursor: pointer;
  transition: all 0.3s ease;
  display: flex;
  align-items: center;
  gap: 0.5rem;
  white-space: nowrap;
}

.search-button:hover {
  transform: translateY(-2px);
  box-shadow: 0 5px 20px rgba(102, 126, 234, 0.4);
}

.search-icon {
  font-size: 1.2rem;
}

/* Results Section */
.results-section {
  margin-bottom: 3rem;
  animation: fadeInUp 0.6s ease-out;
}

.result-card {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  padding: 2rem;
  box-shadow: 0 10px 40px rgba(0, 0, 0, 0.15);
  display: flex;
  gap: 2rem;
  align-items: center;
  flex-wrap: wrap;
}

.image-container {
  flex: 0 0 300px;
}

.heritage-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
  border-radius: 15px;
  box-shadow: 0 5px 20px rgba(0, 0, 0, 0.2);
}

.info-container {
  flex: 1;
  min-width: 250px;
}

.heritage-name {
  font-size: 2rem;
  color: #2d3748;
  margin: 0 0 1rem 0;
  font-weight: 700;
}

.prefecture-info {
  font-size: 1.3rem;
  color: #4a5568;
  margin: 0;
}

.label {
  font-weight: 600;
  color: #667eea;
}

.prefecture {
  font-weight: 700;
  color: #2d3748;
}

/* Heritage List Section */
.heritage-list-section {
  animation: fadeInUp 0.8s ease-out;
}

.section-title {
  font-size: 2.2rem;
  color: white;
  text-align: center;
  margin: 0 0 3rem 0;
  font-weight: 700;
  text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
}

.heritage-grid {
  display: grid;
  grid-template-columns: repeat(auto-fit, minmax(300px, 1fr));
  gap: 2rem;
}

.heritage-item {
  cursor: pointer;
  transition: transform 0.3s ease;
}

.heritage-item:hover {
  transform: translateY(-8px);
}

.heritage-card {
  background: rgba(255, 255, 255, 0.95);
  backdrop-filter: blur(10px);
  border-radius: 20px;
  overflow: hidden;
  box-shadow: 0 10px 30px rgba(0, 0, 0, 0.15);
  transition: all 0.3s ease;
}

.heritage-card:hover {
  box-shadow: 0 20px 50px rgba(0, 0, 0, 0.25);
}

.card-image {
  width: 100%;
  height: 200px;
  object-fit: cover;
}

.card-content {
  padding: 1.5rem;
}

.card-title {
  font-size: 1.3rem;
  color: #2d3748;
  margin: 0 0 0.5rem 0;
  font-weight: 700;
}

.card-location {
  font-size: 1rem;
  color: #718096;
  margin: 0;
  font-weight: 500;
}

/* Animations */
@keyframes fadeInUp {
  from {
    opacity: 0;
    transform: translateY(30px);
  }
  to {
    opacity: 1;
    transform: translateY(0);
  }
}

/* Responsive Design */
@media (max-width: 768px) {
  .title {
    font-size: 2.2rem;
  }
  
  .subtitle {
    font-size: 1rem;
  }
  
  .input-group {
    flex-direction: column;
    align-items: center;
  }
  
  .search-input {
    min-width: 100%;
  }
  
  .result-card {
    flex-direction: column;
    text-align: center;
  }
  
  .image-container {
    flex: none;
  }
  
  .heritage-grid {
    grid-template-columns: 1fr;
  }
  
  .main-content {
    padding: 2rem 1rem;
  }
}

@media (max-width: 480px) {
  .title {
    font-size: 1.8rem;
  }
  
  .heritage-name {
    font-size: 1.5rem;
  }
  
  .prefecture-info {
    font-size: 1.1rem;
  }
  
  .search-card,
  .result-card,
  .heritage-card {
    border-radius: 15px;
  }
}
</style>
