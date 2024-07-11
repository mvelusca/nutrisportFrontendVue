<template>
  <v-data-table
    :items-per-page="itemsPerPage"
    :headers="headers"
    :items="formattedUtilisateurs"
  ></v-data-table>
</template>

<script>

import axios from 'axios';

export default {
  name: 'DataTableUtilisateurComponent',
  data() {
    return {
      utilisateurs: [],
      headers: [
        { title: 'Id', key: 'id_user', align: 'start' },
        { title: 'nom', key: 'nom_user', align: 'end' },
        { title: 'prenom', key: 'prenom_user', align: 'end' },
        { title: 'mot de passe', key: 'hashpwd_user', align: 'end' },
        { title: 'photo de profil', key: 'photo_profil_user', align: 'end' },
        { title: 'bio', key: 'bio_user', align: 'end' },
        { title: 'email', key: 'mail_user', align: 'end' },
        { title: 'date de naissance', key: 'formattedDateNaissance', align: 'end' },
        { title: 'localisation', key: 'loc_user', align: 'end' },
      ],
      itemsPerPage: 10,
      loading: true,
      
    }
  },
  computed: {
    formattedUtilisateurs() {
      // Formater la date de naissance pour chaque utilisateur
      return this.utilisateurs.map(utilisateur => ({
        ...utilisateur,
        formattedDateNaissance: this.formatDate(utilisateur.dateNaissance_user)
      }));
    }
  },
  methods: {
    getUtilisateurs() {
      const url = 'http://localhost:8080/utilisateur/all'
      axios
      .get(url)
      .then((response) => {
        this.utilisateurs = response.data
        this.totalItems = response.data.length
        this.loading = false
        console.log(this.utilisateurs)
      })
      .catch((error) => {
        console.error('Error fetching utilisateurs:', error);
        this.loading = false; 
        console.log(this.utilisateurs)
      })
    },
    formatDate(date) {
      // Fonction pour formater la date au format "yyyy-MM-dd"
      const dateObj = new Date(date);
      const year = dateObj.getFullYear();
      const month = (dateObj.getMonth() + 1).toString().padStart(2, '0');
      const day = dateObj.getDate().toString().padStart(2, '0');
      return `${year}-${month}-${day}`;
    },
    
  },
  mounted(){
    this.getUtilisateurs()
  },

}
</script>