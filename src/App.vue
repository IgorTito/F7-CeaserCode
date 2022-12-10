<template>
  <h1>Шифр Цезаря</h1>
  <div class="wrapper">
    <div class="input">
      <label for="n">Текст для шифрования</label>
      <input v-model="text_in" type="text" placeholder="введите текс" class="input-text">
      <input v-model="code_out" type="text" readonly class="output-code">
      <button @click="ceaserCode" class="code">Зашифровать</button>
    </div>
    <div class="input">
      <label for="n">Сдвиг</label>
      <input id="counter" v-model="step" type="number">
    </div>
    <div class="input">
      <label for="n">Шифр для раcшифрования</label>
      <input v-model="code_in" type="text" placeholder="введите шифр" class="input-code">
      <input v-model="text_out" type="text" readonly class="output-text">
      <button @click="ceaserDeCode" class="deCode">Расшифровать</button>
    </div>
  </div>

</template>

<script>
import {low_ru, upper_ru, symbols, capitalLetter} from "@/letters";

export default {
  data() {
    return {
      text_in: '',
      code_in: '',
      step: 1,
      code_out: '',
      text_out: ''
    }
  },
  methods: {
    ceaserCode() {
      const arr = this.text_in
      const n = this.step
      const arrayLength = low_ru.length - 1
      let new_arr = ''
      for (let i in arr) {
        const value = arr[i]

        //если элемент относится к заглавным то ищем среди заглавных..
        const indexArr = capitalLetter(value) ?
            upper_ru.indexOf(value) :
            low_ru.indexOf(value)

        //если элемент относится к символам
        if (symbols.includes(value))
          new_arr += value

        // если нашли букву
        else if (indexArr >= 0) {
          // если привышаем длину массива то берем сначала
          if (indexArr + n > arrayLength) {
            new_arr += capitalLetter(value) ?
                upper_ru[arrayLength - indexArr + n - 1] :
                low_ru[arrayLength - indexArr + n - 1]
            continue
          }
          //иначе берем как обычно
          new_arr += capitalLetter(value) ?
              upper_ru[indexArr + n] :
              low_ru[indexArr + n]
        }

      }

      this.code_out = new_arr
    },
    ceaserDeCode(){
      const arr = this.code_in
      const shift = this.step
      const arrayLength = low_ru.length
      let new_arr = ''
      for (let i in arr) {
        const value = arr[i]

        //заглавные буквы
        const indexArr = capitalLetter(value) ?
            upper_ru.indexOf(value) :
            low_ru.indexOf(value)

        //символы
        if (symbols.includes(value))
          new_arr += value

        //если вышел за предел
        else if (indexArr >= 0) {
          if (indexArr - shift < 0) {
            const startIndex = arrayLength - shift + indexArr
            console.log(startIndex)
            new_arr += capitalLetter(value) ?
                upper_ru[startIndex] :
                low_ru[startIndex]
            continue
          }
          //иначе берем как обычно
          new_arr += capitalLetter(value) ?
              upper_ru[indexArr - shift] :
              low_ru[indexArr - shift]
        }

      }

      this.text_out = new_arr
    }
  }
}
</script>

<style>
*{
  margin: 0;
  padding: 0;

}
.wrapper {
  margin-top: 20px;
  padding: 40px;
  display: flex;
  width: 100%;
  height: 100%;
  justify-content: center;
  font-size: 30px;
  border: 5px solid #151413;
  background-image: url('../public/fon.jpg');
  font-family: Bahnschrift;
}

/*.input {*/
/*  display: flex;*/
/*}*/

input, button {
  padding: 20px 20px;
  margin: 30px;
  font-size: 20px;
  border-radius: 15px;

}

.input{
  display: flex;
  flex-direction: column;
  align-items: center;


}

button {
  cursor: grab;
  background-color: #c2c2c2;
  border: 3px solid cadetblue;

  margin-bottom: 70px;
  margin-top: 40px;

}

/*.output-code,.output-text {*/
/*  outline: none;*/
/*  opacity: 0;*/

/*}*/

button:hover {
  background-color: #b6daef;

}

#counter{
  width: 20%;
  margin-top: 100px;

}

h1{
  text-align: center;
  font-family: "Arial Black";
}


@media screen and (max-width: 938px) {
  .wrapper{
    flex-direction: column;
    align-items: center;
    justify-content: center;
  }
}
</style>
