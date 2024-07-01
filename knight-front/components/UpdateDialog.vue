<template>
    <div class="text-center">
      <v-dialog
        v-model="isOpen"
        width="auto"
        @click:outside="$emit('closeUpdateDialog')"
      >
        <v-card>
          <v-card-text>
            <h3 class="mb-1">Knight: {{ id }}</h3>
            <div class="pa-2">
              <v-text-field
                label="Nickname"
                variant="outlined"
                v-model="nickname"
              ></v-text-field>
            </div>
            <v-btn class='bg-primary' block @click="updateKnight">Confirm</v-btn>
          </v-card-text>
        </v-card>
      </v-dialog>
    </div>
  </template>
  
  <script setup lang="ts">
  import { defineEmits, defineProps, ref, watch, type PropType } from "vue";
  
  const props = defineProps({
    isOpen: {
      required: true,
      type: Boolean as PropType<boolean>,
    },
    id: {
      required: true,
      type: String as PropType<string>,
    },
  });
  
  const emit = defineEmits(["closeUpdateDialog", "nicknameUpdated", 'update:open']);
  
  const nickname = ref<string>("");
  
  watch(() => props.isOpen, (newVal) => {
    isOpen.value = newVal;
  });
  
  const isOpen = ref(props.isOpen);
  
  const updateKnight = async () => {
    try {
      await fetch(`http://localhost:3001/knights/${props.id}`, {
        method: "PATCH",
        headers: {
          "Content-Type": "application/json",
        },
        body: JSON.stringify({ nickname: nickname.value }),
      });
      emit("nicknameUpdated", { id: props.id, name: nickname.value });
      emit("closeUpdateDialog");
    } catch (error) {
      console.error("Error updating knight:", error);
    }
  };
  </script>
  
  <style scoped>
  </style>