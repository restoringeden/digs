<template>
    <br/>
    <br/>
<v-bottom-navigation >
  <v-btn v-for="link in links" :value="link.title" :href="link.href" >
    <v-icon>{{ link.icon }} }}</v-icon>
    <span>{{ link.title }}</span>
  </v-btn>
</v-bottom-navigation> 
</template>

<script setup lang="ts">
  const links = [
    {
      href: 'https://fcg-gifhorn.church.tools/grouphomepage/cntiwIK9ra074RcMC0i6pfQiL2XTYaxe',
      icon: 'mdi-star-circle-outline',
      title: 'FokusGroups',
    },
    {
      href: 'https://fcg-gifhorn.church.tools/grouphomepage/NNyxLfwas9EOICgfUvBOMgc6qNXoniVa',
      icon: 'mdi-widgets-outline',
      title: 'LifeGroups',
    },
    {
      href: 'https://kirche-im-brauhaus.de/kontakt/',
      icon: 'mdi-account-group-outline',
      title: 'KONTAKT',
    },
    {
      href: 'https://kirche-im-brauhaus.de/',
      icon: 'mdi-home',
      title: 'KIB',
    },
        {
      href: '#',
      icon: 'mdi-alert',
      title: 'Vorführseite: Nicht Offiziell',
    },
  ]
</script>
