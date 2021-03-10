<template>
  <div class="main">
    <div class="item">
      <label>Введите то, что нужно зашифровать</label>
      <input type="text" v-model="encryptedWord" />
      <label>Введите ключ слово</label>
      <input type="text" v-model="encryptedCodeWord" />
      <button @click="encrypt()">Зашифровать</button>
      <p v-show="encryptedResult">Результат: {{ encryptedResult }}</p>
    </div>
    <div class="item">
      <label>Введите то, что нужно расшифровать</label>
      <input type="text" v-model="decryptedWord" />
      <label>Введите ключ слово</label>
      <input type="text" v-model="decryptedCodeWord" />
      <button @click="decrypt()">Расшифровать</button>
      <p v-show="decryptedResult">Результат: {{ decryptedResult }}</p>
    </div>
  </div>
</template>
<script>
import alphabet from "../alphabet";
export default {
  data() {
    return {
      encryptedWord: "",
      encryptedCodeWord: "",
      decryptedWord: "",
      decryptedCodeWord: "",
      encryptedResult: "",
      decryptedResult: "",
    };
  },
  methods: {
    encrypt() {
      let encryptedWord = this.encryptedWord.toLowerCase();
      let encryptedCodeWord = this.encryptedCodeWord.toLowerCase();
      const indexes = [];
      for (let i = 0; i < encryptedCodeWord.length; i++) {
        indexes.push(alphabet.indexOf(encryptedCodeWord[i]));
      }

      let k = 0;
      let encryptedResult = [];
      for (let i = 0; i < encryptedWord.length; i++) {
        if (alphabet.indexOf(encryptedWord[i]) !== -1) {
          if (
            indexes[k] + alphabet.indexOf(encryptedWord[i]) - 1 <
            alphabet.length - 1
          ) {
            encryptedResult.push(
              alphabet[indexes[k] + alphabet.indexOf(encryptedWord[i])]
            );
          } else {
            encryptedResult.push(
              alphabet[
                indexes[k] +
                  alphabet.indexOf(encryptedWord[i]) -
                  alphabet.length
              ]
            );
          }
          k < indexes.length - 1 ? k++ : (k = 0);
        } else {
          encryptedResult.push(encryptedWord[i]);
        }
      }

      this.encryptedResult = encryptedResult.join("");
      this.decryptedWord = this.encryptedResult;
      this.decryptedCodeWord = this.encryptedCodeWord;
    },
    decrypt() {
      const decryptedWord = this.decryptedWord.toLowerCase();
      const decryptedCodeWord = this.decryptedCodeWord.toLowerCase();
      const indexes = [];
      let k = 0;
      let decryptedResult = [];
      for (let i = 0; i < decryptedCodeWord.length; i++) {
        indexes.push(alphabet.indexOf(decryptedCodeWord[i]));
      }
      for (let i = 0; i < decryptedWord.length; i++) {
        if (alphabet.indexOf(decryptedWord[i]) !== -1) {
          if (alphabet.indexOf(decryptedWord[i]) - indexes[k] >= 0) {
            decryptedResult.push(
              alphabet[alphabet.indexOf(decryptedWord[i]) - indexes[k]]
            );
          } else {
            decryptedResult.push(
              alphabet[
                alphabet.length +
                  alphabet.indexOf(decryptedWord[i]) -
                  indexes[k]
              ]
            );
          }

          k < indexes.length - 1 ? k++ : (k = 0);
        } else {
          decryptedResult.push(decryptedWord[i]);
        }
      }
      this.decryptedResult = decryptedResult.join("");
    },
  },
};
</script>
<style>
.main {
  display: flex;
  flex-direction: row;
  align-items: center;
  justify-content: space-between;
  margin: 100px 50px;
  font-size: 20px;
}
.item {
  display: flex;
  flex-direction: column;
  width: 400px;
}
input {
  font-size: 20px;
}
button {
  margin: 20px 0;
  font-size: 20px;
}
p {
  color: red;
}
</style>
