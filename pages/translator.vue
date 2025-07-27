<template>
  <div class="translate-container">
    <div class="translate-card">
      <h2 class="title">Language Translator</h2>

      <div class="text-input">
        <div class="box">
          <label>From</label>
          <textarea v-model="fromText" placeholder="Enter text" spellcheck="false"></textarea>
        </div>
        <div class="box">
          <label>To</label>
          <textarea v-model="toText" placeholder="Translation" spellcheck="false" readonly disabled></textarea>
        </div>
      </div>

      <ul class="controls">
        <li class="row">
          <select ref="selectFrom"></select>
          <i class="fas fa-volume-up" @click="speak(fromText, fromLang)"></i>
          <i class="fas fa-copy" @click="copy(fromText)"></i>
        </li>

        <li class="exchange" @click="exchangeText">
          <i class="fas fa-exchange-alt"></i>
        </li>

        <li class="row">
          <select ref="selectTo"></select>
          <i class="fas fa-volume-up" @click="speak(toText, toLang)"></i>
          <i class="fas fa-copy" @click="copy(toText)"></i>
        </li>
      </ul>

      <v-btn class="translate-btn" color="primary" @click="translateText">Translate</v-btn>
    </div>
  </div>
</template>


<script>
export default {
    data() {
        return {
            fromText: '',
            toText: '',
            fromLang: 'en-GB',
            toLang: 'tl-PH'
        };
    },
    mounted() {
        const selectFrom = this.$refs.selectFrom;
        const selectTo = this.$refs.selectTo;

        selectFrom.innerHTML = `
      <option value="en-GB" selected>English</option>
      <option value="tl-PH">Tagalog</option>
    `;
        selectTo.innerHTML = `
      <option value="en-GB">English</option>
      <option value="tl-PH" selected>Tagalog</option>
    `;

        selectFrom.addEventListener('change', (e) => {
            this.fromLang = e.target.value;
        });
        selectTo.addEventListener('change', (e) => {
            this.toLang = e.target.value;
        });
    },
    methods: {
        translateText() {
            const apiUrl = `https://api.mymemory.translated.net/get?q=${encodeURIComponent(
                this.fromText
            )}&langpair=${this.fromLang}|${this.toLang}`;

            fetch(apiUrl)
                .then((res) => res.json())
                .then((data) => {
                    this.toText = data.responseData.translatedText;
                });
        },
        exchangeText() {
            const tempText = this.fromText;
            this.fromText = this.toText;
            this.toText = tempText;

            const tempLang = this.fromLang;
            this.fromLang = this.toLang;
            this.toLang = tempLang;

            this.$refs.selectFrom.value = this.fromLang;
            this.$refs.selectTo.value = this.toLang;
        },
        copy(text) {
            navigator.clipboard.writeText(text);
        },
        speak(text, lang) {
            const utterance = new SpeechSynthesisUtterance(text);
            utterance.lang = lang;
            speechSynthesis.speak(utterance);
        }
    }
};
</script>

<style scoped>
.translate-container {
  display: flex;
  justify-content: center;
  align-items: center;
  min-height: 60vh;
  background: #121212;
}

.translate-card {
  background: #1e1e1e;
  padding: 25px;
  border-radius: 20px;
  box-shadow: 0 8px 20px rgba(0,0,0,0.4);
  width: 90%;
  max-width: 850px;
}

.title {
  color: #fff;
  font-size: 1.6rem;
  font-weight: bold;
  margin-bottom: 20px;
  text-align: center;
}

.text-input {
  display: flex;
  gap: 20px;
  flex-wrap: wrap;
}

.box {
  flex: 1;
  display: flex;
  flex-direction: column;
}

.box label {
  color: #ccc;
  margin-bottom: 8px;
  font-size: 0.95rem;
}

.text-input textarea {
  width: 100%;
  height: 140px;
  padding: 12px;
  font-size: 16px;
  resize: none;
  border-radius: 10px;
  border: 1px solid #333;
  background-color: #fff;
  color: #000;
  box-sizing: border-box;
}

.controls {
  display: flex;
  justify-content: space-between;
  margin: 20px 0;
  color: #ddd;
}

.controls li {
  display: flex;
  align-items: center;
  gap: 10px;
}

.controls select {
  padding: 8px;
  border-radius: 6px;
  border: none;
  background: #2a2a2a;
  color: #fff;
}

.controls i {
  cursor: pointer;
  transition: transform 0.2s ease;
}

.controls i:hover {
  transform: scale(1.2);
  color: #4dabf7;
}

.exchange {
  font-size: 22px;
  cursor: pointer;
  color: #4dabf7;
}

.translate-btn {
  display: block;
  margin: 0 auto;
  font-size: 1rem;
  padding: 10px 20px;
  border-radius: 10px;
}
</style>
