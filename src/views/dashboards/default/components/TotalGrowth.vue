<script setup lang="ts">
import { ref, onMounted } from 'vue';

// Retrieve posted items from localStorage
const postedItems = ref<any[]>([]);

onMounted(() => {
  postedItems.value = JSON.parse(localStorage.getItem('postedItems') || '[]');
});
</script>

<template>
  <v-container>
    <h2>Posted Items</h2>
    <v-row>
      <v-col v-for="(item, index) in postedItems" :key="index" cols="12" sm="4">
        <v-card class="mx-auto my-4" max-width="500">
          <v-card-title>{{ item.name }}</v-card-title>
          <v-img :src="item.preview" height="200px" v-if="item.preview" />
          <v-card-subtitle>{{ item.description }}</v-card-subtitle>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
