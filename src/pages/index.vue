<template>
  <v-container max-width="900">
    <div>
      <v-img
        class="mb-2"
        height="150"
        src="@/assets/logo.png"
      />

      <div class="mb-8 text-center">
        <div class="text-body-2 font-weight-light mb-n1">KIRCHE IM BRAUHAUS</div>
      </div>
      <div class="mb-8 text-center">
        <h1 class="text-h2 font-weight-bold">Next Steps</h1>
      </div>

      <v-row>
        <v-col cols="12">
          <v-card
            class="py-4 overflow-visible"
            color="surface-variant"
            image="https://kirche-im-brauhaus.de/wp-content/uploads/2023/11/du-und-dein-design-300x200.jpg"
            prepend-icon="mdi-stairs"
            rounded="lg"
            variant="tonal"
          >
            <template #image>
              <v-img position="top right" />
            </template>

            <template #title>
              <h2 class="text-h5 font-weight-bold">
                Step 3: Du & dein Design 
              </h2>
            </template>

            <template #text>
            Lerne uns kennen und erfahre, wie du Teil der KiB werden kannst. Hier kannst du alle Fragen stellen, die du zu unserer Kirche hast.
            </template>

            <v-card-actions>
              <v-row>
                <v-col cols="12" sm="auto">
                  <v-btn prepend-icon="mdi-shield-crown" variant="outlined" to="/du" text="Step 3: Du"></v-btn>
                </v-col>
                <v-col cols="12" sm="auto">
                  <v-btn prepend-icon="mdi-shield-crown" variant="outlined" to="/wir" text="Step 3: Dein Design"></v-btn>
                </v-col>
              </v-row>
            </v-card-actions>
          </v-card>
        </v-col>

        <v-col v-for="link in links" :key="link.href" cols="6" md="3">
          <v-card
            append-icon="mdi-open-in-new"
            class="py-4 overflow-visible"
            color="surface-variant"
            :href="link.href"
            :prepend-icon="link.icon"
            rel="noopener noreferrer"
            rounded="lg"
            :text="link.text"
            target="_blank"
            :title="link.title"
            variant="tonal"
          />
        </v-col>
      </v-row>
    </div>
  </v-container>
  <Nav></Nav>
</template>