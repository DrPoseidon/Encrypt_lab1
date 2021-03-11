<template>
  <div class="main">
    <div class="types">
      <div class="type">
        <label>Шифр Виженера (базовый)</label>
        <input type="radio" v-model="type" value="vigenere" name="type" />
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
        <button v-if="type === 'vigenere'" @click="encrypt()">
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
        <button v-if="type === 'vigenere'" @click="decrypt()">
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
export default {
  data() {
    return {
      encryptedWord: "",
      encryptedCodeWord: "",
      decryptedWord: "",
      decryptedCodeWord: "",
      encryptedResult: "",
      decryptedResult: "",
      type: "vigenere",
      a: "",
      c: "",
      m: "",
      mid: 1103 - 1040,
      min: 1040,
      max: 1103,
    };
  },
  created() {
    console.log(String.fromCharCode(0));
  },
  methods: {
    encrypt() {
      let encryptedWord = this.encryptedWord;
      let encryptedCodeWord = this.encryptedCodeWord;
      const indexes = [];
      for (let i = 0; i < encryptedCodeWord.length; i++) {
        indexes.push(encryptedCodeWord[i].charCodeAt());
      }
      let k = 0;
      let encryptedResult = [];
      for (let i = 0; i < encryptedWord.length; i++) {
        if (
          encryptedWord[i].charCodeAt() >= this.min &&
          encryptedWord[i].charCodeAt() <= this.max
        ) {
          if (
            encryptedWord[i].charCodeAt() + indexes[k] >= this.min &&
            encryptedWord[i].charCodeAt() + indexes[k] <= this.max
          ) {
            encryptedResult.push(
              String.fromCharCode(encryptedWord[i].charCodeAt() + indexes[k])
            );
          } else {
            let v = encryptedWord[i].charCodeAt() + indexes[k];
            for (let j = 0; ; j++) {
              v -= this.mid;
              if (v >= this.min && v <= this.max) break;
            }
            encryptedResult.push(String.fromCharCode(v));
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
      const decryptedWord = this.decryptedWord;
      const decryptedCodeWord = this.decryptedCodeWord;
      const indexes = [];
      let k = 0;
      let decryptedResult = [];
      for (let i = 0; i < decryptedCodeWord.length; i++) {
        indexes.push(decryptedCodeWord[i].charCodeAt());
      }

      for (let i = 0; i < decryptedWord.length; i++) {
        if (
          decryptedWord[i].charCodeAt() >= this.min &&
          decryptedWord[i].charCodeAt() <= this.max
        ) {
          if (
            decryptedWord[i].charCodeAt() - indexes[k] >= this.min &&
            decryptedWord[i].charCodeAt() - indexes[k] <= this.max
          ) {
            decryptedResult.push(
              String.fromCharCode(decryptedWord[i].charCodeAt() - indexes[k])
            );
          } else {
            let v = decryptedWord[i].charCodeAt() - indexes[k];
            for (let j = 0; ; j++) {
              v += this.mid;
              if (v >= this.min && v <= this.max) break;
            }
            decryptedResult.push(String.fromCharCode(v));
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
        if (
          encryptedWord[i].charCodeAt() >= this.min &&
          encryptedWord[i].charCodeAt() <= this.max
        ) {
          if (
            encryptedWord[i].charCodeAt() + x[i] >= this.min &&
            encryptedWord[i].charCodeAt() + x[i] <= this.max
          ) {
            encryptedResult.push(
              String.fromCharCode(encryptedWord[i].charCodeAt() + x[i])
            );
          } else {
            let v = encryptedWord[i].charCodeAt() + x[i];
            for (let j = 0; ; j++) {
              v -= this.mid;
              if (v >= this.min && v <= this.max) break;
            }
            encryptedResult.push(String.fromCharCode(v));
          }
        } else {
          encryptedResult.push(encryptedWord[i]);
        }
        // encryptedResult.push(
        //   String.fromCharCode(encryptedWord[i].charCodeAt() + x[i])
        // );
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
        if (
          decryptedWord[i].charCodeAt() >= this.min &&
          decryptedWord[i].charCodeAt() <= this.max
        ) {
          if (
            decryptedWord[i].charCodeAt() - x[i] >= this.min &&
            decryptedWord[i].charCodeAt() - x[i] <= this.max
          ) {
            decryptedResult.push(
              String.fromCharCode(decryptedWord[i].charCodeAt() - x[i])
            );
          } else {
            let v = decryptedWord[i].charCodeAt() - x[i];
            for (let j = 0; ; j++) {
              v += this.mid;
              if (v >= this.min && v <= this.max) break;
            }
            decryptedResult.push(String.fromCharCode(v));
          }
        } else {
          decryptedResult.push(decryptedWord[i]);
        }
        // decryptedResult.push(
        //   String.fromCharCode(decryptedWord[i].charCodeAt() - x[i])
        // );
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
