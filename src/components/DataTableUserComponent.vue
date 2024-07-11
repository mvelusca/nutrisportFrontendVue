<template>
  <v-data-table-server
    :headers="headers"
    :items="users"
    :items-length="totalItems"
    :loading="loading"
    @update:options="loadItems"
  ></v-data-table-server>
</template>

<script>

import axios from 'axios';

export default {
  name: 'DataTableUserComponent',
  data() {
    return {
      users: [],
      headers: [
        { title: 'Id', key: 'id', align: 'end' },
        { title: 'Firstname', key: 'firstname', align: 'end' },
        { title: 'Lastname', key: 'lastname', align: 'end' },
        { title: 'Email', key: 'email', align: 'end' }
      ],
      itemsPerPage: 5,
      loading: true,
      totalItems: 0
    }
  },
  methods: {
    getUsers() {
      const url = 'http://localhost:8080/users'
      axios
      .get(url)
      .then((response) => {
        this.users = response.data
        this.totalItems = response.data.length
        this.loading = false
        console.log(this.users)
      })
      .catch((error) => {
        console.error('Error fetching users:', error);
        this.loading = false; 
      })
    },

  },
  mounted(){
    this.getUsers()
  },

}
</script>
