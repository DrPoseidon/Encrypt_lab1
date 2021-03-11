<template>
  <div class="main">
    <div class="types">
      <div class="type">
        <label>Шифр Виженера (базовый)</label>
        <input type="radio" v-model="type" value="vigener" name="type" />
      </div>
      <div class="type">
        <label>Шифр Виженера + гаммирование</label>
        <input type="radio" v-model="type" value="gamma" name="type" />
      </div>
    </div>
    <div class="acm">
      <div class="inputs">
        <label v-show="type === 'gamma'">a</label>
        <input v-show="type === 'gamma'" type="text" v-model="a" class="inp" />
      </div>
      <div class="inputs">
        <label v-show="type === 'gamma'">c</label>
        <input v-show="type === 'gamma'" type="text" v-model="c" class="inp" />
      </div>
      <div class="inputs">
        <label v-show="type === 'gamma'">m</label>
        <input v-show="type === 'gamma'" type="text" v-model="m" class="inp" />
      </div>
    </div>
    <div class="items">
      <div class="item">
        <label>Введите то, что нужно зашифровать</label>
        <input type="text" v-model="encryptedWord" />
        <label v-show="type !== 'gamma'">Введите ключ слово</label>
        <input
          v-show="type !== 'gamma'"
          type="text"
          v-model="encryptedCodeWord"
        />
        <button v-if="type === 'vigener'" @click="encrypt()">
          Зашифровать
        </button>
        <button v-else @click="gammaEncrypt()">
          Зашифровать
        </button>
        <p v-show="encryptedResult">Результат: {{ encryptedResult }}</p>
      </div>

      <div class="item">
        <label>Введите то, что нужно расшифровать</label>
        <input type="text" v-model="decryptedWord" />
        <label v-show="type !== 'gamma'">Введите ключ слово</label>
        <input
          v-show="type !== 'gamma'"
          type="text"
          v-model="decryptedCodeWord"
        />
        <button v-if="type === 'vigener'" @click="decrypt()">
          Расшифровать
        </button>
        <button v-else @click="gammaDecrypt()">
          Расшифровать
        </button>
        <p v-show="decryptedResult">Результат: {{ decryptedResult }}</p>
      </div>
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
      type: "vigener",
      a: "",
      c: "",
      m: "",
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
    gammaEncrypt() {
      const a = Number(this.a);
      const c = Number(this.c);
      const m = Number(this.m);
      const encryptedWord = this.encryptedWord;
      const x = [a];

      for (let i = 0; i < encryptedWord.length - 1; i++) {
        x.push((a * x[i] + c) % m);
      }
      let encryptedResult = [];
      for (let i = 0; i < encryptedWord.length; i++) {
        encryptedResult.push(
          String.fromCharCode(encryptedWord[i].charCodeAt() + x[i])
        );
      }
      this.encryptedResult = encryptedResult.join("");
      this.decryptedWord = this.encryptedResult;
    },
    gammaDecrypt() {
      const a = Number(this.a);
      const c = Number(this.c);
      const m = Number(this.m);
      const x = [a];
      const decryptedWord = this.decryptedWord;
      for (let i = 0; i < decryptedWord.length - 1; i++) {
        x.push((a * x[i] + c) % m);
      }
      let decryptedResult = [];
      for (let i = 0; i < decryptedWord.length; i++) {
        decryptedResult.push(
          String.fromCharCode(decryptedWord[i].charCodeAt() - x[i])
        );
      }
      this.decryptedResult = decryptedResult.join("");
    },
  },
};
</script>
<style>
.acm {
  display: flex;
  flex-direction: column;
}
.main {
  display: flex;
  flex-direction: column;
  margin: 100px 25%;
  font-size: 20px;
  align-items: center;
}
.types {
  margin-bottom: 20px;
}
.type {
  display: flex;
  justify-content: space-between;
  align-items: center;
  width: 400px;
}
.items {
  display: flex;
  flex-direction: row;
}
.item {
  display: flex;
  flex-direction: column;
  width: 400px;
  margin: 0 20px;
}
.inputs {
  display: flex;
  justify-content: space-between;
  align-items: center;
}
.inp {
  width: 20px;
  margin-left: 20px;
  margin-bottom: 10px;
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
