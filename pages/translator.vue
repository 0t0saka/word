<template>
    <div class="container">
        <div class="wrapper">
            <div class="text-input">
                <textarea v-model="fromText" placeholder="Enter text" spellcheck="false"></textarea>
                <textarea v-model="toText" placeholder="Translation" spellcheck="false" readonly disabled></textarea>
            </div>
            <ul class="controls">
                <li class="row from">
                    <select ref="selectFrom"></select>
                    <i class="fas fa-volume-up" @click="speak(fromText, fromLang)"></i>
                    <i class="fas fa-copy" @click="copy(fromText)"></i>
                </li>
                <li class="exchange" @click="exchangeText">
                    <i class="fas fa-exchange-alt"></i>
                </li>
                <li class="row to">
                    <select ref="selectTo"></select>
                    <i class="fas fa-volume-up" @click="speak(toText, toLang)"></i>
                    <i class="fas fa-copy" @click="copy(toText)"></i>
                </li>
            </ul>
        </div>
        <v-btn color="primary" @click="translateText">Translate</v-btn>
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
.container {
    max-width: 800px;
    margin: 40px auto;
    padding: 20px;
}

.wrapper {
    border: 1px solid #ccc;
    border-radius: 12px;
    padding: 20px;
}

.text-input {
    display: flex;
    gap: 20px;
}

.text-input textarea {
    width: 100%;
    height: 120px;
    padding: 10px;
    font-size: 16px;
    resize: none;
    background-color: #ffffff;
    color: #000000;
    border: 1px solid #ccc;
}

.controls {
    display: flex;
    justify-content: space-between;
    margin: 15px 0;
}

.controls li {
    display: flex;
    align-items: center;
    gap: 10px;
}

.controls select {
    padding: 5px;
}

.controls i {
    cursor: pointer;
}

.exchange {
    align-self: center;
    font-size: 20px;
    cursor: pointer;
}
</style>
