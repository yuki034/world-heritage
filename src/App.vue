<template>
  <body class="room">

    <div class="size_test">
      <p>世界遺産はかせ</p>
    </div>
      <label>
        <div class="size_test2">
          世界遺産を入力しよう
        </div>
      
       <input 
        v-model="userInput"
        type="text">
      </label>
      <br />
    <div class="button_set">
     <button @click="confirmSelection">確定</button>
    </div>
    <img :src= "imgOutput(imageOutput)">
      <div class="perfOutput_set">
        <p>{{prefOutput}}</p>
      </div> 
      <div class="gazou">
        <img src="@/assets/sekaiisanichiran.png" alt="世界遺産一覧" />
      </div> 
  </body>
</template>

<script setup>
import { ref } from 'vue';

const answerPref = () => {
  prefOutput.value = heritageToPref[userInput.value];
  userInput.value = "";
  };
const userInput = ref("")
const prefOutput = ref("")
const heritageToPref = {
  "富士山": "静岡,山梨",
  "知床半島": "北海道",
  "姫路城": "兵庫",
  "屋久島": "鹿児島",
  "白神山地": "青森、秋田",
  "白川郷": "岐阜、富山",
  "原爆ドーム": "広島",
  "厳島神社": "広島",
  "首里城": "沖縄",
  "石見銀山": "島根",
  "小笠原諸島": "東京",
  "富岡製糸場":"群馬県",
  "佐渡島":"新潟県",
};

const imgOutput = (fileName) => {
  return new URL(`./assets/${fileName}`, import.meta.url).href
};

const answerImage =() => {
  imageOutput.value =heritageToImage[userInput.value];
  userInput.value = "";
};
const imageOutput = ref("")
const heritageToImage = {
  "富士山": "Mt_Fujiimage.jpeg",
  "知床半島": "shiretoko_hanntou.jpeg",
  "姫路城" : "Himeji_jyou.jpeg",
  "屋久島": "yakushima.png",
  "白神山地": "shirakamisanchi.png",
  "白川郷":"shirakawagou.png",
  "原爆ドーム":"genbakudomu.png",
  "厳島神社":"itukushima_jinjya.png",
  "首里城":"shurijyou.png",
  "石見銀山":"iwamiginzan.png",
  "小笠原諸島":"ogasawarashotou.png",
  "富岡製糸場":"tomiokaseishijyou.png",
  "佐渡島":"sadogashima.png",
}

const confirmSelection = () => {
  imageOutput.value = heritageToImage[userInput.value.trim()] || "";
  prefOutput.value =　heritageToPref[userInput.value];

};
</script>

<style scoped>
.size_test {
  font-size: 40px;
  height: 120px;
  font-family: serif;
}
.size_test2 {
  font-size: 20px;
  
  color: crimson;
}
.button_set{
  font-size: 40px;
}
 .room{ 
  background-color: tan;
  padding: 120px;
  margin: auto; 
}
.perfOutput_set{
  font-size: 50px;
  color: red;
}
.gazou{
  margin-left: 300px;
  height: 300px;
}


</style>
