<template>
  <v-container>
    <v-row>
      <v-col>
        <h1>Knight Selector</h1>
        <v-btn color="primary" :to="`/create-knight`"
          >New Knight</v-btn
        >
        <DataTable
          :items="knights"
          :totalItems="totalItems"
          :loading="loading"
          :itemsPerPage="itemsPerPage"
          :page="page"
          @updatePage="updatePage"
          @updateFilter="updateFilter"
          @deleteKnight="handleDeleteKnight"
          @changeNickname="handleChangeNickname"
        />
        <v-list>
          <v-list-item v-for="knight in knights" :key="knight._id">
            <v-list-item-content>
              <v-list-item-title>
                <nuxt-link :to="`/knights/${knight._id}`">{{ knight.name }}</nuxt-link>
              </v-list-item-title>
            </v-list-item-content>
          </v-list-item>
        </v-list>
      </v-col>
    </v-row>
  </v-container>
</template>
  
<script setup lang="ts">
const knights = ref([]);
const totalItems = ref(0);
const loading = ref(true);
const itemsPerPage = ref(10);
const page = ref(1);
const showHeroes = ref(false);

const fetchKnights = async () => {
  loading.value = true;
  try {
    const data = await $fetch("http://localhost:3001/knights", {
      query: {
        hero: showHeroes.value,
      },
      method: "GET",
    });
    console.log(data);
    knights.value = data || [];
    // totalItems.value = data.length ? data[0].total : 0; // assuming your API returns total items count in each item
  } catch (error) {
    console.error(error);
  } finally {
    loading.value = false;
  }
};

const updatePage = (newPage) => {
  page.value = newPage;
  fetchKnights();
};

const updateFilter = (isHero) => {
  showHeroes.value = isHero;
  fetchKnights();
};

const handleChangeNickname = async ({ id, name }) => {
  try {
    await $fetch(`http://localhost:3001/knights/${id}`, {
      method: "PATCH",
      body: { name },
    });
    fetchKnights();
  } catch (error) {
    console.error(error);
  }
};

const handleDeleteKnight = async (id) => {
  console.log("oi");
  //   try {
  //     await $fetch(`http://localhost:3001/knights/${id}`, {
  //       method: 'DELETE'
  //     })
  //     fetchKnights()
  //   } catch (error) {
  //     console.error(error)
  // }
};

// Fetch knights when the component is mounted
fetchKnights();

// Fetch knights when page or filter changes
watch([page, showHeroes], fetchKnights);

</script>
  
  <style scoped>
</style>