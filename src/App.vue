<script setup>
import { ref, computed } from 'vue'

  const inputSentence = ref('')

  const replacedEnglishWord = computed(() => {
    
    let testSentence = ''
    let testSentenceKeepFront = ''
    let testSentenceKeepBack = ''
    let wordNumFront = 0
    let wordNumBack = 0
    const symbolFront = '('
    const symbolBack = ')'
    const symbolLine = ' / '
    const symbolDot = '.'
    
    wordNumFront = inputSentence.value.indexOf(symbolFront);
    wordNumBack = inputSentence.value.indexOf(symbolBack);
    
    if (wordNumFront > 0 && wordNumBack > 0) {
      testSentence = inputSentence.value.substring(wordNumFront + 1, wordNumBack);
      testSentenceKeepFront = inputSentence.value.substring(0, wordNumFront);
      testSentenceKeepBack = inputSentence.value.substring(wordNumBack, inputSentence.length);
    } else {
      testSentence = inputSentence.value;
      testSentenceKeepFront = '';
      testSentenceKeepBack = '';
    }
    var reEnglishWord = '';
  	if(inputSentence.value) {
      reEnglishWord = shuffle(testSentence.split(" ")).join(symbolLine);
      if(testSentenceKeepFront) {
        reEnglishWord = testSentenceKeepFront + symbolFront + ' ' + reEnglishWord;
        if(testSentenceKeepBack){
          reEnglishWord = reEnglishWord + testSentenceKeepBack;
        }
      }
    	reEnglishWord = reEnglishWord + ' ' + symbolDot
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
    <p>
      テキストボックスに英文を入力してください。<br/>
      <ul>
        <li>一文ずつ入力する</li>
        <li>単語と単語の間は半角スペースを開ける</li>
        <li>一部分だけ並べ替えたい場合は、並べ替えたい部分を半角括弧()で囲む</li>
        <li>.はつけない</li>
        <li>改行を入れない</li>
      </ul>
    </p>
    <textarea v-model="inputSentence" placeholder="add inputSentence"></textarea>
    
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
