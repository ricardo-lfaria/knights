<template>
    <v-container>
      <v-form @submit.prevent="submitForm">
        <v-text-field v-model="knight.name" label="Name" required></v-text-field>
        <v-text-field v-model="knight.nickname" label="Nickname" required></v-text-field>
        <v-text-field v-model="knight.birthday" label="Birthday (timestamp)" required type="number"></v-text-field>
        <v-select v-model="knight.keyAttribute" :items="keyAttributes" label="Key Attribute" required></v-select>
        
        <v-card outlined class="pa-4">
          <h3>Weapons</h3>
          <v-btn @click="addWeapon">Add Weapon</v-btn>
          <v-row v-for="(weapon, index) in knight.weapons" :key="index">
            <v-col>
              <v-text-field v-model="weapon.name" label="Weapon Name" required></v-text-field>
            </v-col>
            <v-col>
              <v-text-field v-model="weapon.mod" label="Modifier" type="number" required></v-text-field>
            </v-col>
            <v-col>
              <v-text-field v-model="weapon.attr" label="Attribute" required></v-text-field>
            </v-col>
            <v-col>
              <v-checkbox v-model="weapon.equipped" label="Equipped"></v-checkbox>
            </v-col>
            <v-col>
              <v-btn @click="removeWeapon(index)">Remove</v-btn>
            </v-col>
          </v-row>
        </v-card>
  
        <v-card outlined class="pa-4">
          <h3>Attributes</h3>
          <v-row>
            <v-col>
              <v-text-field v-model="knight.attributes.strenght" label="Strenght" type="number" required></v-text-field>
            </v-col>
            <v-col>
              <v-text-field v-model="knight.attributes.dexterity" label="Dexterity" type="number" required></v-text-field>
            </v-col>
            <v-col>
              <v-text-field v-model="knight.attributes.constitution" label="Constitution" type="number" required></v-text-field>
            </v-col>
            <v-col>
              <v-text-field v-model="knight.attributes.intelligence" label="Intelligence" type="number" required></v-text-field>
            </v-col>
            <v-col>
              <v-text-field v-model="knight.attributes.wisdom" label="Wisdom" type="number" required></v-text-field>
            </v-col>
            <v-col>
              <v-text-field v-model="knight.attributes.charisma" label="Charisma" type="number" required></v-text-field>
            </v-col>
          </v-row>
        </v-card>
  
        <v-checkbox v-model="knight.hero" label="Hero" required></v-checkbox>
  
        <v-btn type="submit" color="primary">Create Knight</v-btn>
      </v-form>
    </v-container>
  </template>
  
  <script setup>
  import { ref } from 'vue';
  
  const knight = ref({
    name: '',
    nickname: '',
    birthday: null,
    keyAttribute: '',
    weapons: [],
    attributes: {
      strenght: null,
      dexterity: null,
      constitution: null,
      intelligence: null,
      wisdom: null,
      charisma: null,
    },
    hero: false,
  });
  
  const keyAttributes = ['STRENGHT', 'DEXTERITY', 'CONSTITUTION', 'INTELLIGENCE', 'WISDOM', 'CHARISMA'];
  
  const addWeapon = () => {
    knight.value.weapons.push({
      name: '',
      mod: null,
      attr: '',
      equipped: false,
    });
  };
  
  const removeWeapon = (index) => {
    knight.value.weapons.splice(index, 1);
  };
  
  const submitForm = async () => {
    try {
      const response = await fetch('http://localhost:3001/knights', {
        method: 'POST',
        headers: {
          'Content-Type': 'application/json',
        },
        body: JSON.stringify(knight.value),
      });
  
      if (response.ok) {
        // Handle successful creation
        console.log('Knight created successfully');
      } else {
        // Handle error
        console.error('Error creating knight:', await response.text());
      }
    } catch (error) {
      console.error('Error:', error);
    }
  };
  </script>
  
  <style scoped>
  .pa-4 {
    padding: 16px;
  }
  .mb-2 {
    margin-bottom: 8px;
  }
  </style>