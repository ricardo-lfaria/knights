<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Knight Details</h1>
        <v-card v-if="knight">
          <v-card-text>
            <h3>Name: {{ knight.name }}</h3>
            <p>Nickname: {{ knight.nickname }}</p>
            <p>Birthday: {{ knight.birthday }}</p>
            <p>Key Attribute: {{ knight.keyAttribute }}</p>
            <p>Hero: {{ knight.hero ? "Yes" : "No" }}</p>
            <h4>Weapons:</h4>
            <v-list>
              <v-list-item
                v-for="(weapon, index) in knight.weapons"
                :key="index"
              >
                <v-list-item-content>
                  <v-list-item-title>{{ weapon.name }}</v-list-item-title>
                  <v-list-item-subtitle
                    >Modifier: {{ weapon.mod }}</v-list-item-subtitle
                  >
                  <v-list-item-subtitle
                    >Attribute: {{ weapon.attr }}</v-list-item-subtitle
                  >
                  <v-list-item-subtitle
                    >Equipped:
                    {{ weapon.equipped ? "Yes" : "No" }}</v-list-item-subtitle
                  >
                </v-list-item-content>
              </v-list-item>
            </v-list>
            <h4>Attributes:</h4>
            <p>Strength: {{ knight.attributes.strength }}</p>
            <p>Dexterity: {{ knight.attributes.dexterity }}</p>
            <p>Constitution: {{ knight.attributes.constitution }}</p>
            <p>Intelligence: {{ knight.attributes.intelligence }}</p>
            <p>Wisdom: {{ knight.attributes.wisdom }}</p>
            <p>Charisma: {{ knight.attributes.charisma }}</p>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>
  
  <script setup>
import { ref, onMounted } from "vue";
import { useRoute } from "vue-router";

const knight = ref(null);
const route = useRoute();

const fetchKnight = async (id) => {
  try {
    const data = await $fetch(`http://localhost:3001/knights/${id}`, {
      method: "GET",
    });
    knight.value = data;
  } catch (error) {
    console.error("Error fetching knight data:", error);
  }
};

onMounted(() => {
  const knightId = route.params.id;
  fetchKnight(knightId);
});
</script>
  
  <style scoped>
</style>