<script setup>
import { ref, computed } from 'vue'

  const inputSent = ref('')
  const testSent = ref('')
  const testSentKeepFront = ref('')
  const testSentKeepBack = ref('')
  const wordNumFront = ref(0)
  const wordNumBack = ref(0)
  const symbolFront = '('
  const symbolBack = ')'
  const symbolLine = ' / '

  //英単語を入れ替える。
  const replacedEnglishWord = computed(() => {
    // ()がある場合、そこだけ切り出す
    wordNumFront.value = inputSent.value.indexOf(symbolFront);
    wordNumBack.value = inputSent.value.indexOf(symbolBack);
    
    if (wordNumFront.value > 0 && wordNumBack.value > 0) {
      testSent.value = inputSent.value.substring(wordNumFront.value + 1, wordNumBack.value);
      testSentKeepFront.value = inputSent.value.substring(0, wordNumFront.value);
      testSentKeepBack.value = inputSent.value.substring(wordNumBack.value, inputSent.length);
    } else {
      testSent.value = inputSent.value;
      testSentKeepFront.value = '';
      testSentKeepBack.value = '';
    }
    var reEnglishWord;
  	if(inputSent.value) {
      reEnglishWord = shuffle(testSent.value.split(" ")).join(symbolLine);
      if(testSentKeepFront.value) {
        reEnglishWord = testSentKeepFront.value + symbolFront + reEnglishWord;
        if(testSentKeepBack.value){
          reEnglishWord = reEnglishWord + testSentKeepBack.value
        }
      }
    }
    return reEnglishWord;
	})
  const shuffle = ([...array]) => {
    for (var i = array.length - 1; i >= 0; i--) {
      var j = Math.floor(Math.random() * (i + 1));
      [array[i], array[j]] = [array[j], array[i]];
    }
    return array;
  }
</script>

<template>
  <main>
    <h1>英語テスト作成</h1>
    <textarea v-model="inputSent" placeholder="add inputSent"></textarea>
    
    <h2>英語テスト作成結果</h2>
		<p>
      以下の語を、正しい英文になるように並び替えなさい。
    </p>
    <template v-for="value in replacedEnglishWord">
        {{ value }}
    </template>
  </main>
</template>


<style scoped>
header {
  line-height: 1.5;
}

.logo {
  display: block;
  margin: 0 auto 2rem;
}

@media (min-width: 1024px) {
  header {
    display: flex;
    place-items: center;
    padding-right: calc(var(--section-gap) / 2);
  }

  .logo {
    margin: 0 2rem 0 0;
  }

  header .wrapper {
    display: flex;
    place-items: flex-start;
    flex-wrap: wrap;
  }
  textarea {
	width: 300px;
	height: 100px;
  }
}
</style>
