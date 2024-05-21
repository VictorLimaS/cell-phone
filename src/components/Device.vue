<template>
  <div class="iphone">
    <div class="notch">
      <div class="point"></div>
    </div>
    <div class="screen">
      <p class="text">{{ buttonText }}</p>
      <p class="name" v-if="nameText">{{ nameText }}</p>
      <p class="contact" v-if="contactName">{{ contactName }}</p>
    </div>
    <div class="contact-container" v-if="contactName">
      <p class="contact">{{ contactName }}</p>
    </div>
    <div class="button-container">
      <div class="button-group">
        <Button v-for="(item, index) in buttons" :key="index" :number="item.number" :letters="item.letters.join('')"
          @click="updateText(item.number, item.letters)" />
      </div>
      <div class="button-group">
        <div class="button Trans"></div>
        <div class="button call" @click="toggleCallClick" :class="{ 'clicked': isCallClicked }">
          <ion-icon name="call-outline"></ion-icon>
        </div>
        <div class="button Transs" @click="deleteLastDigit" v-show="buttonText.length > 0">
          <ion-icon name="backspace-outline"></ion-icon>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import { ref } from 'vue';
import Button from './Button.vue';
import buttonsData from '../json/buttons.json';
import contatosData from '../json/contatos.json';

export default {
  components: {
    Button,
  },
  setup() {
    const buttonText = ref('');
    const nameText = ref('');
    const contactName = ref('');
    let lastButtonClickTime = 0;
    let lastNumberClicked = null;
    const letterChangeDelay = 1000;

    const buttons = buttonsData;
    const contatos = contatosData;

    const selectedLetters = ref([]);
    const isCallClicked = ref(false);

    const toggleCallClick = () => {
      isCallClicked.value = !isCallClicked.value;
    };

    const updateText = (number, letters) => {
      const currentTime = new Date().getTime();

      if (number === 1 || number === '*' || number === '#') {
        if (lastNumberClicked !== number || currentTime - lastButtonClickTime > letterChangeDelay) {
          buttonText.value += number.toString();
          lastButtonClickTime = currentTime;
          lastNumberClicked = number;
        }
        return;
      }

      if (lastNumberClicked === number && currentTime - lastButtonClickTime <= letterChangeDelay) {
        const currentLetterIndex = selectedLetters.value.findIndex(item => item.number === number);
        if (currentLetterIndex !== -1) {
          let nextLetterIndex = (selectedLetters.value[currentLetterIndex].index + 1) % letters.length;
          selectedLetters.value[currentLetterIndex].index = nextLetterIndex;
          const nextLetter = letters[nextLetterIndex];
          nameText.value = nameText.value.slice(0, -1) + nextLetter;
        } else {
          selectedLetters.value.push({ number, index: 0 });
          nameText.value += letters[0];
        }
      } else {
        selectedLetters.value = [{ number, index: 0 }];
        buttonText.value += number.toString();
        nameText.value += letters[0];
      }
      lastButtonClickTime = currentTime;
      lastNumberClicked = number;

      const filteredContacts = contatos.filter(contato =>
        contato.nome.toLowerCase().startsWith(nameText.value.toLowerCase())
      );
      contactName.value = filteredContacts.length > 0 ? filteredContacts[0].nome : '';
    };

    const deleteLastDigit = () => {
      if (buttonText.value.length > 0) {
        const lastButton = buttons.find(btn => btn.number.toString() === buttonText.value.slice(-1));
        if (lastButton) {
          const number = lastButton.number;
          const index = selectedLetters.value.findIndex(item => item.number === number);
          if (index !== -1) {
            selectedLetters.value.splice(index, 1);
          }
        }
        buttonText.value = buttonText.value.slice(0, -1);

        if (buttonText.value.length === 0) {
          nameText.value = '';
          contactName.value = '';
        } else {
          nameText.value = nameText.value.slice(0, -1);
          const selectedNumbers = selectedLetters.value.map(item => item.number);
          nameText.value += selectedNumbers.map(number => {
            const button = buttons.find(btn => btn.number === number);
            return button.letters[selectedLetters.value.find(item => item.number === number).index];
          }).join('');
        }
      }
    };

    return {
      buttonText,
      nameText,
      contactName,
      buttons,
      updateText,
      deleteLastDigit,
      isCallClicked,
      toggleCallClick,
    };
  },
};
</script>

<style scoped>
.iphone {
  position: relative;
  width: 375px;
  height: 667px;
  background: #000000;
  border-radius: 40px;
  overflow: hidden;
  box-shadow: 0 0 30px rgba(0, 0, 0, 0.5);
}

.notch {
  position: absolute;
  display: flex;
  align-items: center;
  top: 1rem;
  left: 50%;
  transform: translateX(-50%);
  width: 100px;
  height: 22px;
  background: #0e0e0e;
  border-radius: 10px;
}

.notch .point {
  position: absolute;
  right: 5px;
  border-radius: 50%;
  width: 8px;
  height: 8px;
  background-color: rgb(25, 25, 68);
}

.device {
  position: relative;
  padding: 20px;
}

.screen {
  position: absolute;
  margin-top: 3rem;
  width: 100%;
  display: flex;
  align-items: center;
  flex-direction: column;
}

.text {
  text-align: center;
  margin-top: 20px;
  font-size: 60px;
  flex: 1;
}

.screen p {
  margin: 0;
}

.button-container {
  position: absolute;
  top: 180px;
  left: 50%;
  transform: translateX(-50%);
  display: flex;
  flex-direction: column;
  align-items: center;
  width: 100%;
}

.button-group {
  display: grid;
  grid-template-columns: repeat(3, 1fr);
  margin-top: 1rem;
  gap: 10px;
}

.button {
  width: 80px;
  height: 80px;
  background: #0e0e0e;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  cursor: pointer;
  font-size: 84px;
  flex-direction: column;
  transition: background-color 0.1ms ease;
}

.button:active {
  background: #565656;
}

.Trans {
  z-index: -100;
  cursor: none;
  background-color: transparent;
}

.Transs {
  background-color: transparent !important;
  color: #565656;
}

.Transs:active {
  color: #ffffff;
}

.call {
  background-color: #27ae60;
}

.call.clicked {
  background-color: #f90000; 
}

ion-icon {
  font-size: 3rem;
  cursor: pointer;
}
</style>
