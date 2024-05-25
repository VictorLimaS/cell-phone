<template>
  <div class="iphone">
    <div class="notch">
      <div class="point"></div>
    </div>
    <div class="screen">
      <p class="number">{{ clickedNumber }}</p>
      <div class="list">
        <ul>
          <li v-for="contact in suggestedContacts" :key="contact.id">{{ contact.nome }}</li>
        </ul>
      </div>
    </div>
    <div class="button-container">
      <div class="button-row">
        <Button number="1" letters="" @click="handleButtonClick('1')" />
        <Button number="2" letters="ABC" @click="handleButtonClick('2')" />
        <Button number="3" letters="DEF" @click="handleButtonClick('3')" />
      </div>
      <div class="button-row">
        <Button number="4" letters="GHI" @click="handleButtonClick('4')" />
        <Button number="5" letters="JKL" @click="handleButtonClick('5')" />
        <Button number="6" letters="MNO" @click="handleButtonClick('6')" />
      </div>
      <div class="button-row">
        <Button number="7" letters="PQRS" @click="handleButtonClick('7')" />
        <Button number="8" letters="TUV" @click="handleButtonClick('8')" />
        <Button number="9" letters="WXYZ" @click="handleButtonClick('9')" />
      </div>
      <div class="button-row">
        <Button number="*" letters="" @click="handleButtonClick('*')" />
        <Button number="0" letters="+" @click="handleButtonClick('0')" />
        <Button number="#" letters="" @click="handleButtonClick('#')" />
      </div>
      <div class="button-row">
        <div class="tras"></div>
        <div class="button call" @click="toggleCallClick" :class="{ 'clicked': isCallClicked }">
          <ion-icon name="call-outline"></ion-icon>
        </div>
        <div class="button trass" @click="handleBackspace" >
          <ion-icon name="backspace-outline"></ion-icon>
        </div>
      </div>
    </div>
  </div>
</template>

<script>
import Button from './Button.vue';
import contatosData from '../json/contatos.json';

export default {
  components: {
    Button,
  },
  data() {
    return {
      clickedNumber: '',
    };
  },
  computed: {
    suggestedContacts() {
      const contacts = [];
      const numberToLetters = {
        '2': ['A', 'B', 'C'],
        '3': ['D', 'E', 'F'],
        '4': ['G', 'H', 'I'],
        '5': ['J', 'K', 'L'],
        '6': ['M', 'N', 'O'],
        '7': ['P', 'Q', 'R', 'S'],
        '8': ['T', 'U', 'V'],
        '9': ['W', 'X', 'Y', 'Z'],
      };

      const combinations = this.generateLetterCombinations(this.clickedNumber, numberToLetters);
      combinations.forEach(combination => {
        const matchedContacts = contatosData.filter(contact => contact.nome.toLowerCase().includes(combination.toLowerCase()));
        contacts.push(...matchedContacts);
      });

      return contacts;
    }
  },
  methods: {
    handleButtonClick(number) {
      const lastChar = this.clickedNumber.charAt(this.clickedNumber.length - 1);

      if (lastChar !== number) {
        this.clickedNumber += number;
      } else {
        this.clickedNumber = this.clickedNumber.slice(0, -1) + number;
      }
    },

    handleBackspace() {
      this.clickedNumber = this.clickedNumber.slice(0, -1);
    },

    generateLetterCombinations(number, numberToLetters) {
      const result = [];
      if (number.length === 0) {
        return result;
      }

      const dfs = (index, currentCombination) => {
        if (index === number.length) {
          result.push(currentCombination);
          return;
        }

        const letters = numberToLetters[number[index]];
        for (const letter of letters) {
          dfs(index + 1, currentCombination + letter);
        }
      };

      dfs(0, '');
      return result;
    }
  }
}
</script>

<style scoped>
.iphone {
  border-radius: 2.5rem;
  position: relative;
  width: 375px;
  height: 637px;
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

.screen {
  display: flex;
  flex-direction: column;
  align-items: center;
  height: 160px;
  color: white;
  margin-top: 3rem;
}

.screen .number {
  font-size: 3rem;
}

.screen .letter {
  padding: none;
  font-size: 1.5rem;
}

.screen .list {
  font-size: 12px;
  font-size: .6rem;
  overflow: hidden;
}


.screen .list ul li {
  list-style-type: none;
  text-align: center;
}

.button-container {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: .2rem;
}

.button-row {
  display: flex;
  gap: .2rem;
}

.call {
  width: 80px;
  height: 80px;
  background: #00ff26;
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

.trass {
  width: 80px;
  height: 80px;
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

.tras {
  width: 80px;
  height: 80px;
  color: white;
  border-radius: 50%;
  display: flex;
  align-items: center;
  justify-content: center;
  font-size: 84px;
  flex-direction: column;
  transition: background-color 0.1ms ease;
}

ion-icon {
  font-size: 3rem;
  cursor: pointer;
}

@media screen and (max-width: 500px) {
  .iphone {
    width: 100vw;
    height: 100vh;
    border-radius: 0;
  }

  .button-row {
    display: flex;
    gap: 10px;
  }

}
</style>
