<template>
  <div>
    <v-row>
      <Header height="100px"></Header>
    </v-row>
    <v-row>
      <v-col v-for="(card, index) in cards" :key="index" :cols="4">
        <Card 
          :cardData="card" 
          :editCard="editCard(card)" 
          :deleteCard="deleteCard(index)"
          :imageUrl="cardImageUrls[card.id]" />
      </v-col>
      <v-col :cols="4">
        <Card :isAddCard="true" :addCard="openDialog" :cardData="{}" :editCard="() => {}" />
      </v-col>
    </v-row>
    <CardDialog
      :dialogData="selectedCard"
      :saveCard="saveCard"
      :closeDialog="closeDialog"
      :selectedCard="selectedCard"
      :value="selectedCard ? cardImageUrls[selectedCard.id] : null"
      ref="cardDialog"
    />
  </div>
</template>

<script>
import Card from '~/components/Card.vue';
import CardDialog from '~/components/CardDialog.vue';

export default {
  components: {
    Card,
    CardDialog,
  },
  data() {
    return {
      cards: [],
      selectedCard: null,
      cardImageUrls: {},
    };
  },
  methods: {
    editCard(card) {
      return () => {
        this.selectedCard = card;
        this.$refs.cardDialog.dialog = true;
      };
    },
    deleteCard(index) {
      return () => {
        this.cards.splice(index, 1);
      };
    },
    openDialog() {
      this.$set(this, 'selectedCard', null);
      this.$refs.cardDialog.fileInput = null;
      this.$refs.cardDialog.dialog = true;
      this.$refs.cardDialog.formData.name = '';
      this.$refs.cardDialog.formData.description = '';
    },
    saveCard() {
      if (!this.$refs.cardDialog.$refs.form.validate()) {
        return;
      }

      if (this.selectedCard === null) {
        const newCard = { ...this.$refs.cardDialog.formData, id: Date.now() };
        this.cards.push(newCard);
        this.$set(this.cardImageUrls, newCard.id, this.$refs.cardDialog.fileInput);
      } else {
        const index = this.cards.findIndex((card) => card.id === this.selectedCard.id);
        if (index !== -1) {
          this.$set(this.cards, index, { ...this.$refs.cardDialog.formData, id: this.selectedCard.id });
          this.$set(this.cardImageUrls, this.selectedCard.id, this.$refs.cardDialog.fileInput);
        }
      }

      this.$refs.cardDialog.fileInput = null;
      this.$refs.cardDialog.dialog = false;
    },
    closeDialog() {
      this.$refs.cardDialog.fileInput = null;
      this.$refs.cardDialog.dialog = false;
    },
  },
};
</script>
