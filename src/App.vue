<script setup>
import { reactive, onMounted, ref } from 'vue';
import Search from './components/Search.vue';
import fakeData from './assets/MOCK_DATA.json';
import Chip from 'primevue/chip';

const users = reactive({data:[]});

const searchUser = ref('');
const searchingCount = ref(0);

onMounted(() => {
  users['data'] = fakeData;
});

function filterUsers(searched) {
  return users['data'].filter(user => user.firstName.toLocaleLowerCase().includes(searched.searching.toLocaleLowerCase()));
}

function searching(searched) {

  if (searched.searching.length == 0) {
    searchUser.value = '';
    users['data'] = fakeData;

    searchingCount.value = 0;
    return;
  }

  if (searched.type === 'search') {
    users['data'] = filterUsers(searched);

  } else {
    searchUser.value = searched.searching;

    searchingCount.value = filterUsers(searched).length;
  }

}
</script>

<template>
  <h2>My App</h2>

  <Search @searching="searching" />
  <div class="chip">
    <Chip :label="searchingCount.toString()" icon="pi pi-user" class="chip-searching" />
  </div>
  <ul>
    <li 
      v-for="user in users['data']" 
      :key="user.id"
      :class="{'searchUser':searchUser.length > 0 && user.firstName.toLocaleLowerCase().includes(searchUser.toLocaleLowerCase())}"
    >
      {{ user.firstName }} {{ user.lastName }}
    </li>
  </ul>
</template>

<style scoped>
ul {
  list-style: none;
  display: flex;
  flex-wrap: wrap;
  justify-content: space-between;
}

ul li {
  background-color: aliceblue;
  color: black;
  padding: 5px;
  margin-right: 2px;
  margin-bottom: 5px;
  border: solid 1px #333;
}
.searchUser {
  background-color: blueviolet;
  color: aliceblue;
}

.chip {
  margin-top: 10px;
}
.chip-searching {
  background-color: blue;
  color: aliceblue;
}
</style>
