<script setup lang="ts">
import { ref } from 'vue';

const itemName = ref('');
const itemDescription = ref('');
const selectedFile = ref<File | null>(null);
const previewUrl = ref<string | null>(null);
const dialog = ref(false); // Control the modal visibility

// Central store for shared data (example using reactive store)
const postedItems = ref<any[]>(JSON.parse(localStorage.getItem('postedItems') || '[]'));

// Handle file selection
const onFileSelected = (event: Event) => {
  const target = event.target as HTMLInputElement;
  const file = target.files?.[0];
  if (file) {
    selectedFile.value = file;
    previewUrl.value = URL.createObjectURL(file); // Generate preview URL
  }
};

// Post item and close the modal
const postItem = () => {
  if (itemName.value && itemDescription.value) {
    postedItems.value.push({
      name: itemName.value,
      description: itemDescription.value,
      image: selectedFile.value,
      preview: previewUrl.value,
    });
    localStorage.setItem('postedItems', JSON.stringify(postedItems.value));

    // Reset form and close the modal
    itemName.value = '';
    itemDescription.value = '';
    selectedFile.value = null;
    previewUrl.value = null;
    dialog.value = false;
  } else {
    alert('Please fill out all fields.');
  }
};
</script>

<template>
  <v-container>
    <!-- Button to Open Modal -->
    <v-btn color="primary" @click="dialog = true">Post an Item</v-btn>

    <!-- Modal for Posting an Item -->
    <v-dialog v-model="dialog" max-width="500px">
      <v-card>
        <v-card-title>
          <span class="text-h5">Post an Item</span>
        </v-card-title>
        <v-card-text>
          <v-text-field label="Item Name" v-model="itemName" dense outlined required />
          <v-textarea label="Description" v-model="itemDescription" dense outlined rows="3" required />
          <label for="image-upload" class="d-block">Upload Image:</label>
          <input
            type="file"
            id="image-upload"
            accept="image/*"
            class="d-none"
            @change="onFileSelected"
          />
          <v-btn class="mt-2" color="primary" @click="$refs.fileInputModal.click()">Choose File</v-btn>
          <input ref="fileInputModal" type="file" class="d-none" @change="onFileSelected" />
          <div v-if="previewUrl" class="mt-4">
            <v-img :src="previewUrl" max-height="150" max-width="150" contain />
          </div>
        </v-card-text>
        <v-card-actions>
          <v-spacer></v-spacer>
          <v-btn text @click="dialog = false">Cancel</v-btn>
          <v-btn color="primary" @click="postItem">Post</v-btn>
        </v-card-actions>
      </v-card>
    </v-dialog>
  </v-container>
</template>
