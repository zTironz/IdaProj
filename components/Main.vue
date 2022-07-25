/* eslint-disable no-console */
<template>
  <div class="main">
    <div class="fist-column">
      <h1 class="fist-column__header">
        Добавление товара
      </h1>
      <div class="form">
        <h4 class="form__title">
          Наименование товара
        </h4>
        <div class="input__container">
          <input
            v-model="title"
            class="form__input"
            type="text"
            placeholder="Введите наименование товара"
            pattern="[а-яё]{2,100}"
            min="1"
          >
          <span class="form__error">Поле является обязательным</span>
        </div>

        <h4 class="form__title">
          Описание товара
        </h4>
        <textarea
          v-model="descr"
          class="form__area"
          type="text"
          placeholder="Введите описание товара"
        />
        <h4 class="form__title">
          Ссылка на изображение товара
        </h4>
        <div class="input__container">
          <input v-model="link" class="form__input" placeholder="Введите ссылку" pattern="https?://.+" type="text">
          <span class="form__error">Поле является обязательным</span>
        </div>

        <h4 class="form__title">
          Цена товара
        </h4>
        <div class="input__container">
          <input
            v-model="price"
            class="form__input"
            placeholder="Введите цену"
            type="text"
            pattern="[0-9]"
            min="1"
          >
          <span class="form__error">Поле является обязательным</span>
        </div>

        <button :disabled="!title||!descr||!link||!price" class="form__button" @click="createCard()">
          Добавить товар
        </button>
      </div>
    </div>
    <div class="second-column">
      <!-- <button class="button-sort">
        <p class="button-sort__text">По умолчанию</p>
        <img src="" />
      </button> -->
      <select class="button-sort" @change="onChange">
        <option value="default">
          По умолчанию
        </option>
        <option value="min">
          Цена по возрастанию
        </option>
        <option value="max">
          Цена по убыванию
        </option>
        <option value="name">
          По наименованию
        </option>
      </select>
      <div class="cards">
        <div class="card">
          <img class="card__image" src="./photo.png">
          <div>
            <h2 class="card__title">
              Наименование товара
            </h2>
            <p class="card__descr">
              Довольно-таки интересное описание товара в несколько строк.
              Довольно-таки интересное описание товара в несколько строк
            </p>
            <p class="card__price">
              10 000
            </p>
          </div>

          <div class="card__del">
            <img>
          </div>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
export default {
  name: 'MainComponent',
  data () {
    return {
      title: '',
      link: '',
      descr: '',
      price: ''
    }
  },
  mounted () {
    window.addEventListener('message', this.deleteCard())
  },
  beforeDestroy () {
    window.addEventListener('message', this.deleteCard())
  },
  methods: {
    onChange (event) {
      // console.log(event.target.value)
      if (event.target.value === 'max') {
        // console.log('min')
        const cards = document.querySelector('.cards')
        const children = [...cards.children]
        children.sort((a, b) => {
          return Number(a.querySelector('.card__price').innerText.replace(/ /g, '')) < Number(b.querySelector('.card__price').innerText.replace(/ /g, '')) ? 1 : -1
        })
        cards.innerHTML = ''
        children.forEach(item => cards.appendChild(item))
      } else if (event.target.value === 'min') {
        // console.log('max')
        const cards = document.querySelector('.cards')
        const children = [...cards.children]
        children.sort((a, b) => {
          return Number(a.querySelector('.card__price').innerText.replace(/ /g, '')) > Number(b.querySelector('.card__price').innerText.replace(/ /g, '')) ? 1 : -1
        })
        cards.innerHTML = ''
        children.forEach(item => cards.appendChild(item))
      } else if (event.target.value === 'name') {
        // console.log('name')
        const cards = document.querySelector('.cards')
        const children = [...cards.children]
        children.sort((a, b) => {
          return a.querySelector('.card__title').innerText > b.querySelector('.card__title').innerText ? 1 : -1
        })
        cards.innerHTML = ''
        children.forEach(item => cards.appendChild(item))
      }
    },
    // min () {
    //   console.log(event.target.value)
    //   console.log('min')
    //   const cards = document.querySelector('.cards')
    //   const children = [...cards.children]
    //   children.sort((a, b) => {
    //     return Number(a.querySelector('.card__price').innerText.replace(/ /g, '')) < Number(b.querySelector('.card__price').innerText.replace(/ /g, '')) ? 1 : -1
    //   })
    //   cards.innerHTML = ''
    //   children.forEach(item => cards.appendChild(item))
    // },
    // max () {
    //   console.log('max')
    //   const cards = document.querySelector('.cards')
    //   const children = [...cards.children]
    //   children.sort((a, b) => {
    //     return Number(a.querySelector('.card__price').innerText.replace(/ /g, '')) > Number(b.querySelector('.card__price').innerText.replace(/ /g, '')) ? 1 : -1
    //   })
    //   cards.innerHTML = ''
    //   children.forEach(item => cards.appendChild(item))
    // },
    createCard () {
      const cards = document.querySelector('.cards')
      const card = document.createElement('div')
      const img = document.createElement('img')
      const title = document.createElement('h3')
      const description = document.createElement('p')
      const price = document.createElement('p')
      const del = document.createElement('div')

      card.classList.add('card')
      img.classList.add('card__image')
      title.classList.add('card__title')
      description.classList.add('card__descr')
      price.classList.add('card__price')
      del.classList.add('card__del')

      img.src = this.link
      title.textContent = this.title
      description.textContent = this.descr
      price.textContent = this.price
      // https://upload.wikimedia.org/wikipedia/commons/thumb/3/3a/Cat03.jpg/240px-Cat03.jpg

      card.appendChild(img)
      card.appendChild(title)
      card.appendChild(description)
      card.appendChild(price)
      card.appendChild(del)
      cards.appendChild(card)
      // function deleteCard () {
      //   const prices = document.querySelectorAll('.card__price')
      //   console.log(prices)
      //   for (const price of prices) {
      //     console.log(price.innerHTML)
      //   }
      //   // console.log(prices.map(el => el.innerHTML))
      //   // const del = document.querySelector('.card__del')
      //   // console.log(del, 'fgfgfgfgfgfgfg')
      //   const dels = document.querySelectorAll('.card__del')
      //   console.log(dels)
      //   for (const el of dels) {
      //     el.addEventListener('click', () => {
      //       el.parentElement.style.animation = 'shake ease .5s'
      //       setTimeout(function () {
      //         el.parentElement.remove(el)
      //       }, 500)
      //     })
      //   }
      // }

      this.deleteCard()
    },
    deleteCard () {
      // const prices = document.querySelectorAll('.card__price')
      // console.log(prices)
      // for (const price of prices) {
      //   console.log(price.innerHTML)
      // }
      // console.log(prices.map(el => el.innerHTML))
      // const del = document.querySelector('.card__del')
      // console.log(del, 'fgfgfgfgfgfgfg')
      const dels = document.querySelectorAll('.card__del')
      // console.log(dels)
      for (const el of dels) {
        el.addEventListener('click', () => {
          el.parentElement.style.animation = 'shake ease .5s'
          setTimeout(function () {
            el.parentElement.remove(el)
          }, 500)
        })
      }
    }
    // validErr () {
    //   const validationErrors = {
    //     noValue: 'Это обязательное поле',
    //     range: 'Должно быть от 2 до 30 символов',
    //     link: 'Здесь должна быть ссылка'
    //   }
    // }
  }

}
</script>

<style lang="scss">
* {
  box-sizing: border-box;
  font-family: Source Sans Pro, Arial, Helvetica, sans-serif;
  font-style: normal;
  font-weight: normal;
  margin: 0;
  padding: 0;
}

.main {
  max-width: 1440px;
  margin: auto;
  display: flex;
  background-color: $color-back;
}

.fist-column {
  min-width: 332px;
  display: flex;
  flex-direction: column;
  margin: 32px 16px 0 32px;
}

.fist-column__header {
  font-weight: 600;
  font-size: 28px;
  line-height: 35px;
  color: #3f3f3f;
}

.form {
  width: 332px;
  height: 440px;
  display: flex;
  margin-top: 16px;
  flex-direction: column;
  padding: 24px;
  background-color: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
}

.form__title {
  position: relative;
    height: 13px;
    margin-bottom: 4px;
    margin-top: 16px;
    font-size: 10px;
    line-height: 13px;
    letter-spacing: -0.02em;
    color: $color-text-form;
}

.form__title:first-child {
  margin-top: 0;
}

.form__title::after {
 content: "";
    position: absolute;
    width: 4px;
    height: 4px;
    background: $color-error;
    border-radius: 50%;
}
.form__title:nth-child(3):after {
  content: none;
}

.form__input {
  height: 36px;
  width: 100%;
    background: $color-back-div;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border: none;
    outline: none;
    border-radius: 4px;
    padding: 10px 0 11px 16px;
}

span {
  margin-top: 4px;
  font-size: 8px;
  line-height: 10px;
  letter-spacing: -0.02em;
  color: #ff8484;
}

.form__area {
   position: relative;
   border: none;
       padding: 10px 16px;
    min-height: 108px;
    box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
    border-radius: 4px;
    outline: none;
    font-size: 10px;
    line-height: 13px;
    letter-spacing: -0.02em;
    background-color: $color-back-div;
}

::placeholder {
  padding: 0;
  font-size: 12px;
  line-height: 15px;
  color: #b4b4b4;
}

.form__button {
  height: 36px;
  border: none;
  margin-top: 24px;
  background-color: #eeeeee;
border-radius: 10px;
  font-size: 12px;
  line-height: 15px;
  text-align: center;
  letter-spacing: -0.02em;
  color: #b4b4b4;
  cursor: pointer;
}

.form__button_ok {
  background-color: #7bae73;
  color: #ffffff;
}

.second-column {
  width: 100%;
  margin-top: 31px;
  display: flex;
  flex-direction: column;
}

.button-sort {
  width: 121.49px;
  height: 36px;
  display: flex;
  flex-direction: row;
  background-color: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border: none;
  border-radius: 4px;
  margin-left: auto;
  margin-right: 32px;
}

.button-sort__text {
  padding: 10px 0 11px 16px;
  font-size: 12px;
  line-height: 15px;
  color: #b4b4b4;
}

.cards {
  margin-top: 16px;
    display: flex;
    flex-wrap: wrap;
    // justify-content: center;
    gap: 15px;
    margin-bottom: 16px;
}

.card {
  position: relative;
       height: 423px;
    width: 332px;
  display: flex;
  flex-direction: column;
  background-color: #fffefb;
  box-shadow: 0px 20px 30px rgba(0, 0, 0, 0.04),
    0px 6px 10px rgba(0, 0, 0, 0.02);
  border-radius: 4px;
      cursor: pointer;
}

.card__image {
  width: 100%;
  height: 200px;
  object-fit: contain;
  // background: url(photo.png);
}

.card__text {
  padding: 16px;
}

.card__title {
  margin-top: 16px;
  margin-left: 16px;
  margin-right: 16px;
    margin-bottom: 16px;
    font-weight: 600;
    font-size: 20px;
    line-height: 25px;
    color: #3f3f3f;
}

.card__descr {
font-family: 'Source Sans Pro';
font-style: normal;
font-weight: 400;
    font-size: 16px;
    line-height: 20px;
    color: #3f3f3f;
    overflow: scroll;
    margin-left: 16px;
  margin-right: 16px;
    height: 85px;
}

.card__price {
  font-weight: 600;
    font-size: 24px;
    line-height: 30px;
    color: #3f3f3f;
    margin-top: 16px;
    margin-left: 16px;
  margin-right: 16px;
}

.card__price::after {
  content: ' руб.';
}

.card__del {
  display: none;
  position: absolute;
  right: -8px;
  top: -8px;
  cursor: pointer;
  width: 32px;
  height: 32px;
  background: #ff8484;
  background-image: url('trash.svg');
  box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
  border-radius: 10px;
  background-repeat: no-repeat;
    background-position: center;
}

.card:hover .card__del{
  display: block;
}

.form__error {
  display: none;
}

.form__input:valid:not(:placeholder-shown) {
  border-color: #ffdb4d;
}

.form__input:invalid:not(:placeholder-shown) {
  border-color: #df4b41;
}

.form__input:invalid:not(:placeholder-shown) + .form__error {
  display: block;
  position: absolute;
}

.form__button:not(:disabled) {
  background: #7BAE73;
box-shadow: 0px 2px 4px rgba(0, 0, 0, 0.1);
border-radius: 10px;
color: #fff;
}

@media screen and (max-width:1024px) {
  .main {flex-direction: column;
    align-items: center;
  }

  .cards {
    margin-top: 16px;
    display: flex;
    flex-wrap: wrap;
    justify-content: center;
    gap: 10px;
  }

  .fist-column {
    margin: 32px 32px;
  }
  .second-column {
    margin: 0;
  }

  .button-sort {
  width: 121.49px;
  height: 36px;
  display: flex;
  flex-direction: row;
  background-color: #fffefb;
  box-shadow: 0px 2px 5px rgba(0, 0, 0, 0.1);
  border: none;
  border-radius: 4px;
  margin: auto;
}
}

@media screen and (max-width: 350px) {
  .fist-column {
    margin: 0;
  }
}

@keyframes shake {
  0% {
    top: 0px;

  }

  50% {
    top: 5px;

  }
  100% {
    top: 0px;

  }

}
</style>
