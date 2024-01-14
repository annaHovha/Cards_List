<template>
<v-card @click="isAddCard ? addCard() : editCard()" width="300px" class="rounded-xl">
  <v-img v-if="!isAddCard" :src="imageUrl || cardData.imageUrl" height="200px" width="300px"></v-img>
  <v-card-title v-if="!isAddCard">{{ cardData.name }}</v-card-title>
  <v-card-text v-if="!isAddCard">{{ cardData.description }}</v-card-text>
  <v-card v-if="isAddCard" height="338px" color="#afaed7" class="py-5">
  <v-row class="fill-height d-flex align-center justify-center">
    <v-col>
      <v-card-text class="text-center">
        <v-icon size="80px" color="white" overlap>mdi-plus-circle-outline</v-icon>
      </v-card-text>
    </v-col>
  </v-row>
</v-card>
  <v-btn v-if="!isAddCard" @click="editCard" color="#95c5f0" class="px-5">
    Edit
  </v-btn>
  <v-btn v-if="!isAddCard" icon @click="deleteCard">
    <v-icon>mdi-delete</v-icon>
  </v-btn>
</v-card>
</template>
    
<script>
export default {
props: {
  cardData: {
    type: Object,
    required: true,
  },
  isAddCard: {
    type: Boolean,
    default: false,
  },
  editCard: {
    type: Function,
    required: true,
  },
  deleteCard: {
    type: Function,
    required: false,
  },
  addCard: {
    type: Function,
    required: false,
  },
  imageUrl: {
    type: String,
    default: '',
  },
}
};
</script>