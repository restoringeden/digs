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
        <h3>Vier Schritte, in denen wir Gott und seine Vision für die Kirche im Brauhaus kennenlernen, um einen Unterschied zu machen.</h3>
      </div>

      <v-row>


        <v-col cols="12">
          <v-card class="py-4" rounded="lg" variant="outlined" color="grey">
            <v-card-title class="text-center text-h6">Was ist dein nächster Schritt?</v-card-title>
            <v-card-text class="text-body-2 text-left">
              NextSteps ist eine vierteilige Reise, in der du mehr darüber erfährst, wer die Kirche im Brauhaus ist, was unsere Vision, Werte und Kultur sind und wie du mit deinen Gaben und Talenten einen Unterschied machen kannst. Oder anders gesagt: NextSteps ist genau das Richtige für dich, wenn du die Kirche im Brauhaus zu deinem Zuhause machen möchtest – und zwar ganz unabhängig davon, ob du schon lange Christ bist oder Jesus gerade erst kennenlernst.
            </v-card-text>
          </v-card>
        </v-col>

        <v-col cols="12">
          <v-card class="py-4" rounded="lg" variant="outlined" color="grey">
            <v-card-title class="text-center text-h6">So kannst du deinen nächsten Schritt gehen</v-card-title>
            <v-card-text class="text-body-2 text-left">
              An den ersten 4 Sonntagen jeweils nach unseren Gotttesdiensten steht ein Team für dich bereit, um mit dir deinen nächsten Schritt zu gehen. Besuche unsere Gottesdienste – da beginnt deine Reise.
               </v-card-text>
          </v-card>
        </v-col>

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
            Finde heraus, mit welchen einzigartigen Persönlichkeitsmerkmalen und Begabungen Gott dich beschenkt hat.
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
