<template>
    <v-container>
      <v-toolbar class="bg-accent" flat>
        <v-toolbar-title>Knights</v-toolbar-title>
        <v-divider class="mx-4" inset vertical></v-divider>
        <v-spacer></v-spacer>
        <v-switch
          v-model="showHeroes"
          label="Access Hall of Heroes"
          class="mt-2 mr-4"
          @change="updateFilter"
        ></v-switch>
      </v-toolbar>
  
      <v-data-table
        :headers="headers.text"
        :items="items"
        :items-per-page="itemsPerPage"
        :server-items-length="totalItems"
        :loading="loading"
        :page.sync="page"
        class="elevation-1 bg-secundary"
        @update:page="updatePage"
      >
        <template #item="{ item }">
          <tr style="width: 100%">
            <td v-for="header in headers" :key="header.text">
              <p>{{ item[header.value] }}</p>
              <v-btn
                v-if="header.text === 'Actions'"
                class="bg-primary"
                icon="mdi-pencil"
                @click="openUpdateDialog(item._id)"
              >
                <v-tooltip activator="parent" location="top">Edit nickname</v-tooltip>
                <v-icon>mdi-pencil</v-icon>
              </v-btn>
              <v-btn
                v-if="header.text === 'Actions' && !item.hero"
                class="bg-primary"
                icon="mdi-coffin"
                style="margin-left: 10px;"
                @click="sendToHallOfHeroes(item._id)"
              >
                <v-tooltip activator="parent" location="top">Send to the Hall of heroes</v-tooltip>
                <v-icon>mdi-coffin</v-icon>
              </v-btn>
            </td>
          </tr>
        </template>
      </v-data-table>
  
      <UpdateDialog
        v-if="selectedKnightId"
        :isOpen.sync="isUpdateDialogOpen"
        :id="selectedKnightId"
        @closeUpdateDialog="closeUpdateDialog"
        @nicknameUpdated="handleNicknameUpdated"
      />
    </v-container>
  </template>
  
  <script setup>
  import UpdateDialog from "./UpdateDialog.vue";
  import { ref, defineProps, defineEmits } from "vue";
  
  const headers = ref([
    { text: "Id", value: "_id" },
    { text: "Name", value: "name" },
    { text: "Birthday", value: "birthday" },
    { text: "Weapons", value: "weapons" },
    { text: "Key Attribute", value: "keyAttribute" },
    { text: "Attack", value: "attack" },
    { text: "Experience", value: "exp" },
    { text: "Hero", value: "hero" },
    { text: "Actions", value: "actions", sortable: false },
  ]);
  
  const props = defineProps({
    items: {
      type: Array,
      required: true,
    },
    totalItems: {
      type: Number,
      required: true,
    },
    loading: {
      type: Boolean,
      required: true,
    },
    itemsPerPage: {
      type: Number,
      default: 10,
    },
    page: {
      type: Number,
      default: 1,
    },
  });
  
  const emit = defineEmits(['updatePage', 'updateFilter', 'deleteKnight', 'changeNickname']);
  
  const showHeroes = ref(false);
  const isUpdateDialogOpen = ref(false);
  const selectedKnightId = ref(null);
  
  const updatePage = (newPage) => {
    emit('updatePage', newPage);
  }
  
  const updateFilter = () => {
    emit('updateFilter', showHeroes.value);
  }
  
  const openUpdateDialog = (id) => {
    selectedKnightId.value = id;
    isUpdateDialogOpen.value = true;
  }
  
  const closeUpdateDialog = () => {
    isUpdateDialogOpen.value = false;
    selectedKnightId.value = null;
  }
  
  const handleNicknameUpdated = ({ id, name }) => {
    const knight = props.items.find(knight => knight._id === id);
    if (knight) {
      knight.name = name;
    }
    emit('changeNickname', { id, name });
    closeUpdateDialog();
  };
  
  const sendToHallOfHeroes = async (id) => {
    const knight = props.items.find(knight => knight._id === id);
    if (knight) {
      console.log(knight)
      try {
          await fetch(`http:///knights/${knight._id}/hall`, {
              method: "PATCH",
              headers: {
                  "Content-Type": "application/json",
                },
            });
        } catch (error) {
            console.error("Error sending knight to the hall:", error);
        } 
    } else {
        console.error("Knight not found")
    }
 };
 watch[sendToHallOfHeroes]
  </script>
  
  <style scoped>
  </style>