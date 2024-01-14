<template>
  <v-dialog v-model="dialog" max-width="600">
    <v-card>
      <v-card-title>{{ selectedCard ? 'Edit Card' : 'Add Card' }}</v-card-title>
      <v-card-text>
        <v-form ref="form">
          <v-text-field v-model="formData.name" label="Name"></v-text-field>
          <v-textarea v-model="formData.description" label="Description"></v-textarea>
          <v-file-input :value="null" type="file" class="input" label="Upload image" hint="Add a picture of your card" outlined dense @change="onFileChange" ref="fileInputRef" />
        </v-form>
        <v-img v-if="fileInput" :src="fileInput" height="100px" width="200px"></v-img>
      </v-card-text>
      <v-card-actions>
        <v-btn @click="saveCard" color="#95c5f0">Save</v-btn>
        <v-btn @click="closeDialog">Cancel</v-btn>
      </v-card-actions>
    </v-card>
  </v-dialog>
</template>

<script>
export default {
  props: {
    dialogData: {
      type: Object,
      default: null,
    },
    saveCard: {
      type: Function,
      required: true,
    },
    closeDialog: {
      type: Function,
      required: true,
    },
    selectedCard: {
      type: Object,
      default: null,
    },
    value: {
      type: String,
      default: null,
    },
  },
  data() {
    return {
      dialog: false,
      formData: {
        name: '',
        description: '',
      },
      fileInput: this.value,
    };
  },
  watch: {
    dialogData: {
      handler(newData) {
        this.$set(this, 'formData', { ...newData });
      },
      immediate: true,
    },
    value(newValue) {
      this.fileInput = newValue;
    },
    dialog(newValue) {
      if (newValue) {
        this.$nextTick(() => {
          this.$refs.fileInputRef.reset();
        });
      }
    },
  },
  
  methods: {
    createImage(file) {
      if (!(file instanceof Blob)) {
        return;
      }
      const reader = new FileReader();
      reader.onload = (e) => {
        this.fileInput = e.target.result;
        this.$emit('update:value', this.fileInput);
      };
      reader.readAsDataURL(file);
    },
    onFileChange(file) {
      if (!file) {
        return;
      }
      this.createImage(file);
    },
  },
};
</script>
