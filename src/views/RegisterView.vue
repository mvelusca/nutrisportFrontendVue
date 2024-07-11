<template>
  <v-container>
    <v-row justify="center">
      <v-col cols="12" sm="8" md="6">
        <v-card>
          <v-card-title class="headline">Inscription</v-card-title>
          <v-card-text>
            <v-form @submit.prevent="registerUser">
              <v-text-field v-model="user.nom_user" label="Nom" required></v-text-field>
              <v-text-field v-model="user.mail_user" label="E-mail" type="email" required></v-text-field>
              <v-text-field v-model="user.hashpwd_user" label="Mot de passe" type="password" required></v-text-field>
              <v-btn type="submit" color="primary">S'inscrire</v-btn>
            </v-form>
          </v-card-text>
        </v-card>
      </v-col>
    </v-row>
  </v-container>
</template>

<script>
import axios from 'axios';

export default {
  data() {
    return {
      user: {
        nom_user: '',
        mail_user: '',
        hashpwd_user: ''
      }
    };
  },
  methods: {
    registerUser() {
      axios.post('http://localhost:8080/utilisateur/add', this.user)
        .then(response => {
          console.log('Utilisateur enregistré avec succès:', response.data);
          // Réinitialiser le formulaire après l'enregistrement
          this.user.nom_user = '';
          this.user.mail_user = '';
          this.user.hashpwd_user = '';
        })
        .catch(error => {
          console.error('Erreur lors de l\'enregistrement de l\'utilisateur:', error);
        });
    }
  }
};
</script>
