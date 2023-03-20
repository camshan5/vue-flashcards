<template>
  <div id="flashcard-app" class="container">
    <div class="flashcard-form">
      <label for="front"
        >Front
        <input v-model="newFront" type="text" id="front" />
      </label>
      <label for="back"
        >Back
        <input
          v-on:keypress.enter="addNew"
          v-model="newBack"
          type="text"
          id="back"
        />
      </label>
      <button v-on:click="addNew">Add a New Card</button>
      <span v-show="error" class="error"
        >Hold on now! Don't forget to add data to the front and back!</span
      >
    </div>
    <hr class="hr-padding" />

    <ul class="flashcard-list">
      <li v-on:click="toggleCard(card)" v-for="(card, index) in cards">
        <transition name="flip">
          <p v-if="!card.flipped" key="front" class="card">
            {{ card.front }}
            <span v-on:click="cards.splice(index, 1)" class="delete-card"
              >X</span
            >
          </p>
          <p v-else key="back" class="card">
            {{ card.back }}
            <span v-on:click="cards.splice(index, 1)" class="delete-card"
              >X</span
            >
          </p>
        </transition>
      </li>
    </ul>
  </div>
</template>

<script>
import { cards } from "@/api/data";

export default {
  data() {
    return {
      cards: cards,
      newFront: "",
      newBack: "",
      error: false,
    };
  },
  methods: {
    toggleCard: function (card) {
      card.flipped = !card.flipped;
    },
    addNew: function () {
      if (!this.newFront || !this.newBack) {
        this.error = true;
      } else {
        this.cards.push({
          front: this.newFront,
          back: this.newBack,
          flipped: false,
        });
        this.newFront = "";
        this.newBack = "";
        this.error = false;
      }
    },
  },
};
</script>
