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
      
      <v-container>
      <span v-if="currentQuiz.text.length > 0">
        
        <v-card v-if="currentQuiz.text.length > 0">
<!--v-btn @click="refreshPage" prepend-icon="mdi-reload">Reload</v-btn-->                      <v-btn prepend-icon="mdi-home" to="/" text="Zurück"></v-btn>

          <v-card-title class="text-center">{{ currentQuiz.type }}</v-card-title>
          <v-card-text class="text-center">
            <p>{{ currentQuiz.text }}</p>
              <br>
              <v-btn v-for="(choice, index) in choices" :key="index" @click="selectChoice(choice, currentQuiz.section)">
                {{ choice.text }}
              </v-btn>           


          </v-card-text>
          <v-progress-linear :model-value="(currentIndex/quizzes.length)*100"></v-progress-linear>

          
        </v-card>

        </span>
        <span v-else>
        <v-card class="py-4 overflow-visible" rounded="lg"
                variant="tonal">
                <v-btn prepend-icon="mdi-home" to="/" text="Zurück"></v-btn>
          <v-card-title class="text-center">Fertig!</v-card-title>
          <v-card-text class="text-center">»Strebt nach der Liebe!  Bemüht euch um die Gaben des Geistes, (...) so trachtet danach, dass ihr die Gemeinde erbaut und alles reichlich habt«
          </v-card-text>
          <v-card-text class="text-center">1. Korinther 14,1+12</v-card-text>
        </v-card>
        <br/>
        <v-card class="py-4 overflow-visible" rounded="lg"
                variant="tonal">
          <v-card-title class="text-center">Ergebnis</v-card-title>
          <v-sparkline
                auto-line-width
                :labels="label"
                :model-value="data"
                type="bar"
                auto-draw
              ></v-sparkline>
          <v-card-text class="text-center">{{ score }}</v-card-text>
          <v-card-text class="text-center">mach bitte einen Screenshot</v-card-text>
        </v-card>
        <br/>
        <v-expansion-panels>
          <v-expansion-panel
            v-for="i in detailed"
            :key="i.section[0]"
            :title="i.section.join('/')"
            :text="i.text"
          ><v-expansion-panel-text>Bibel: {{ i.bible }}</v-expansion-panel-text>
          <v-expansion-panel-text>Teams: {{ i.team }}</v-expansion-panel-text>
          </v-expansion-panel>
        </v-expansion-panels>
        <br/>
          <v-btn prepend-icon="mdi-home" to="/" text="Zurück"></v-btn>
        
        </span>
      </v-container>
            </div>
  </v-container>
</template>

<script setup lang="ts">
import { ref } from 'vue';

import type { Ref } from 'vue';



// Define the quizzes with titles and text
const quizzes = [
    {
        "type": "Gaben Entdecken",
        "text": "Mir macht es Spaß, Aufgaben und Veranstaltungen zu organisieren.",
        "section": "A"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich würde gerne Gemeinden aufbauen, an Orten, in denen es noch keine Gemeinde gibt.",
        "section": "B"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Es macht mir Freude mit Holz, Stoff, Metall, Farben, Glas oder anderen Materialien zu arbeiten.",
        "section": "C"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich kann Falschheit und Täuschung erkennen, bevor sie auch für andere offensichtlich werden.",
        "section": "D"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich sammle Gebetsanliegen und bete regelmäßig für die Verlorenen.",
        "section": "E"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich kann andere Menschen gut motivieren, ein Ziel zu verfolgen.",
        "section": "F"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Mir fällt es leicht, darauf zu vertrauen, dass meine Gebete erhört werden.",
        "section": "G"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich gebe gerne großzügig für Menschen in Not oder für Projekte, die finanzielle Unterstützung brauchen.",
        "section": "H"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich halte Ausschau nach Möglichkeiten, um für kranke Menschen zu beten.",
        "section": "I"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich mag es auch, durch kleine Dinge die Arbeit anderer zu unterstützen.",
        "section": "J"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich öffne meine Wohnung gerne für andere Menschen.",
        "section": "K"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Es macht mir Freude, lange Zeit im Gebet zu verbringen.",
        "section": "L"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Das Weitergeben von biblischem Wissen an die nächste Generation ist mir sehr wichtig.",
        "section": "M"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich kann andere Menschen motivieren, ein Projekt voranzutreiben.",
        "section": "N"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich kann mich gut in Menschen einfühlen, die Probleme oder seelische Verletzungen haben, und begleite gerne ihren Heilungsprozess.",
        "section": "O"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich bin fest davon überzeugt, dass ich mit Gottes Kraft große Dinge vollbringen werde.",
        "section": "P"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich kann Menschen, die Gott nicht kennen, das Evangelium klar mitteilen.",
        "section": "Q"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Es macht mir Spaß, meine musikalischen Fähigkeiten zu entwickeln und einzusetzen.",
        "section": "R"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich habe großes Mitleid mit Menschen, die seelisch verletzt sind.",
        "section": "S"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich fühle mich verantwortlich, andere Menschen, die sündigen, mit der Wahrheit zu konfrontieren.",
        "section": "T"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich arbeite gerne im Hintergrund.",
        "section": "U"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich nehme mir gerne feste Zeiten zum Bibel lesen und -studium, um die Aussagen der Bibel wirklich genau zu erfassen.",
        "section": "V"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Gott hat mich schon gebraucht, um das Sprachengebet auszulegen.",
        "section": "W"
    },
    {
        "type": "Gaben Entdecken",
        "text": "In der Bibel lese ich am liebsten die Sprüche Salomos.",
        "section": "X"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich bin sorgfältig und geschickt, wenn es darum geht Kleinigkeiten effektiv zu erledigen.",
        "section": "A"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Es reizt mich, wenn ich bei neuen Aufgaben Pionierarbeit leisten kann.",
        "section": "B"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich habe die Erfahrung gemacht, dass ich mit der Geschicklichkeit meiner Hände anderen Menschen helfen und sie erfreuen kann.",
        "section": "C"
    },
        {
        "type": "Gaben Entdecken",
        "text": "Ich kann schnell unterscheiden zwischen geistlicher Wahrheit und Irrtum, zwischen Gut und Böse.",
        "section": "D"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Dass so viele Menschen verloren sind, zerreißt mir das Herz.",
        "section": "E"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Es macht mir große Freude, Menschen zu helfen, denen niemand helfen will; Menschen, die am Rande unserer Gesellschaft stehen.",
        "section": "F"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Gott um menschenunmögliche Dinge zu bitten und im Glauben seinen Thron zu bestürmen, löst in mir Begeisterung aus.",
        "section": "G"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich gebe gerne mehr als meinen Zehnten (10 % meines Einkommens), um christliche und karitative Projekte zu unterstützen.",
        "section": "H"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich glaube an göttliche Heilung und bete anhaltend darum.",
        "section": "I"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Es bereitet mir große Freude, anderen Menschen weiterzuhelfen, schwierige Situationen zu bewältigen.",
        "section": "J"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Andere Menschen zu unterhalten und ein offenes Haus zu haben, liebe ich.",
        "section": "K"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich verspüre eine Last, für die Nöte dieser Welt zu beten.",
        "section": "L"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich kann so über die Bibel sprechen, dass andere Menschen angeregt werden, selbst die Bibel zu studieren und mehr aus ihr lernen zu wollen.",
        "section": "M"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Am liebsten leite ich selber.",
        "section": "N"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich kann geduldig Menschen begleiten, die durch schmerzhafte Prozesse gehen und die versuchen, ihr Leben wieder ins Lot zu bringen.",
        "section": "O"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich habe schon einige starke Glaubenswunder erlebt.",
        "section": "P"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Die Idee, als Missionar in ein anderes Land zu gehen, begeistert mich.",
        "section": "Q"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Gerne diene ich der Gemeinde durch Lobpreis und Anbetung.",
        "section": "R"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich kann neue Perspektiven so vermitteln, dass andere Menschen sie gerne aufnehmen und bereit sind, eine neue Richtung einzuschlagen.",
        "section": "S"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Menschen direkt mit ihrer Sünde zu konfrontieren, fällt mir nicht schwer.",
        "section": "T"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Wenn Leute nur herum sitzen und nichts tun, regt mich das auf.",
        "section": "U"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich teile gerne Gottes Wort mit, in der Hoffnung, dass andere Christen hierdurch persönlich wachsen.",
        "section": "V"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Täglich bete ich in neuen Sprachen.",
        "section": "W"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Der Heilige Geist schenkt mir Erkenntnisse und Einsichten in biblische Zusammenhänge.",
        "section": "X"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich genieße es, Dinge zu strukturieren und zu organisieren.",
        "section": "A"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich liebe den Gedanken, einmal selbst eine Gemeinde zu gründen.",
        "section": "B"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Dinge mit meinen Händen zu erledigen, bereitet mir Freude.",
        "section": "C"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich erkenne Probleme und Ungereimtheiten schneller als andere.",
        "section": "D"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich erzähle Menschen ganz offen, dass ich Christ bin, und es freut mich, wenn sie mir Fragen zu meinem Glauben stellen.",
        "section": "E"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich halte immer Ausschau nach einer Möglichkeit, andere zu ermutigen.",
        "section": "F"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich gehe gerne mutige Glaubensschritte auf dem Wasser mit der Gewissheit, dass Gott mich hält.",
        "section": "G"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich spende gerne großzügig. Mehr Geld haben, bedeutet für mich, mehr Geld geben können.",
        "section": "H"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Gott hat mich schon öfters gebraucht, um körperliche Heilung zu den Kranken zu bringen.",
        "section": "I"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ein Teil des Prozesses zu sein, reicht mir völlig aus.",
        "section": "J"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich vermittle auch fremden Menschen sehr schnell das Gefühl, willkommen zu sein.",
        "section": "K"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Notsituationen anderer bringen mich spontan dazu, für sie zu beten.",
        "section": "L"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Wenn ich die Bibel lese oder studiere, entdecke ich darin wichtige Aussagen, die ich anderen Christen weitergeben will.",
        "section": "M"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich delegiere Verantwortung, um Aufgaben zu erledigen.",
        "section": "N"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich bin hoch motiviert, notleidenden Menschen zu helfen.",
        "section": "O"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich habe einen tiefen Hunger in mir, die wunderwirkende Kraft Gottes in meinem Leben zu sehen.",
        "section": "P"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Mein Fokus liegt klar darauf, diese Welt mit dem Evangelium von Jesus Christus zu erreichen.",
        "section": "Q"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Es bereitet mir große Freude, andere Menschen in Lobpreis und Anbetung anzuleiten.",
        "section": "R"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Gerne begleite ich Menschen durch ihre tiefsten Nöte hindurch.",
        "section": "S"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich habe schon erlebt, dass Gott mir mit Träumen/Bildern oder durch andere Eindrücke etwas mitgeteilt hat.",
        "section": "T"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich mag es auch die kleinen Dinge zu tun, die andere lieber abgeben.",
        "section": "U"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Ich bevorzuge es, über ein bestimmtes Thema der Bibel zu reden, anstatt Vers für Vers durch die Passagen zu gehen.",
        "section": "V"
    },
    {
        "type": "Gaben Entdecken",
        "text": "In neuen Sprachen zu beten ist ermutigend und mir persönlich sehr wichtig.",
        "section": "W"
    },
    {
        "type": "Gaben Entdecken",
        "text": "Wenn ich mit Schwierigkeiten und Problemen konfrontiert werde, tendiere ich dazu, weise zu entscheiden.",
        "section": "X"
    }

];
// Define choices with associated points
const choices = [
  { text: 'niemals', point: 1 },
  { text: 'selten', point: 2 },
  { text: 'manchmal', point: 3 },
  { text: 'oft', point: 4 },
  { text: 'immer', point: 5 },
];

//Define options
const detailed = [
        {
            "text": "Die von Gott gegebene Fähigkeit zu verstehen, wie eine Organisation funktioniert, sowie Abläufe zu planen und durchzuführen, dass gesteckte Ziele erreicht werden können.",
            "bible": "Lukas 14,28-30/Apostelgeschichte 6,1-7 ",
            "team": "Special Events-Team",
            "section": ["A. ORGANISATION"]
        },
        {
            "text": "Die von Gott gegebene Gabe, neue Dienstbereiche oder Gemeinden aufzubauen und zu betreuen.",
            "section": ["B. APOSTEL"],
            "bible": "Apostelgeschichte 15,22-35/1. Korinther 12,28/2. Korinther 12,12/Galater 2,7-10/Epheser 4,11-14",
            "team": "Next Steps-Team, Kleingruppenleiter",
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, Dinge zu entwerfen, herzustellen oder zu reparieren, die praktisch einsetzbar sind.",
            "section": ["C. GREEN TEAM"],
            "bible": "2. Mose 31,3; 35,31-35/Apostelgeschichte 9,36-39/2. Könige 22,5-64",
            "team": "Technik-Team, Green-Team, Aufbau-Team, Media-Team",
        },
        {
            "text": "Die von Gott gegebene Fähigkeit,Wahrheit und Irrtum klar aufzuzeigen. Die Gabe beinhaltet die Fähigkeit, die Geister zu unterscheiden undzu sehen,wasgut undböse,richtig undfalsch ist. ",
            "section": ["D. UNTERSCHEIDUNG DER GEISTER"],
            "bible": "1. Korinther 12,10/Apostelgeschichte 5,1-4/Matthäus 16,21-23/Matthäus 4,1-11",
            "team": "Next Steps-Team, Gebets-Team",
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, Nichtchristen das Evangelium wirkungsvoll weiterzugeben, so dass diese zum Glauben an Jesus Christus kommen. ",
            "section": ["E. EVANGELISATION"],
            "bible": "Epheser 4,11/Lukas 19,1-10/Apostelgeschichte 8,26-40",
            "team": "Gebets-Team , Kleingruppenleiter, Welcome-Team , Erzählcafe -Team, Connect-Team , CYOU-Team , Cyout-Team, Schatzinsel-Team, Royal Rangers ",
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, andere Menschen zu ermutigen und durch Gottes Wahrheiten aufzubauen.",
            "section": [
                "F. ERMUTIGUNG"
            ],
            "bible": "Römer 12,8/Apostelgeschichte 11,22-24/1. Timotheus 4,6+12",
            "team": "Welcome-Team, Connect-Team, Schatzinsel-Team, Cafe-Team, Besuchs-Team, Kleingruppenleiter, Info-Team, Service-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, im Vertrauen auf Gottes Wort zu handeln und unerschütterlich daran zu glauben, dass er seine Verheißungen erfüllt.",
            "section": [
                "G. GLAUBE"
            ],
            "bible": "1. Korinther 12,9/Hebräer 11,1+6/Römer 4,18-21",
            "team": "alle Teams"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, gerne und freiwillig Geld über den Zehnten hinaus Opfer zu geben, damit Gottes Reich auf Erden sich weiten kann.",
            "section": [
                "H. GEBEN"
            ],
            "bible": "Römer 12,8/2. Korinther 8,3-5/1. Korinther 13,3/Matthäus 6,1-4",
            "team": "alle Teams"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, Gottes Werkzeug zu sein, durch Glaube und Handauflegung, damit Menschen von körperlicher und seelischer Krankheit wieder völlig gesund werden.",
            "section": [
                "I. HEILUNG"
            ],
            "bible": "1. Korinther 12,9+28+30/Apostelgeschichte 3,1-16/Markus 2,1-12/Jakobus 5,14-15",
            "team": "Gebets-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, praktische und notwendige Aufgaben zu erfüllen, die andere entlasten, unterstützen und ihren Bedürfnissen entgegenkommen.",
            "section": [
                "J. HELFEN"
            ],
            "bible": "1. Korinther 12,28/Römer 12,7/Apostelgeschichte 6,1-4/Römer 16,1-2",
            "team": "Café-Team, Besuchs-Team, Auf-und Abbau-Team, Catering-Team, Ordnungs-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, für andere Menschen zu sorgen und ihnen Freundschaft anzubieten.",
            "section": [
                "K. GASTFREUNDSCHAFT"
            ],
            "bible": "1. Petrus 4,9-10/Römer 12,13/Hebräer 13,1-2",
            "team": "Service-Team, Welcome-Team, Connect-Team, Info-Team, Cafe-Team, Kleingruppengastgeber, Erzählcafe-Team, Catering-Team, Ordnungs-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, regelmäßig für die Anliegen anderer Menschen vor Gott einzutreten und konkrete Ergebnisse zu erwarten.",
            "section": [
                "L. FÜRBITTE"
            ],
            "bible": "Römer 8,26-27/Johannes 17,9-26/1. Timotheus 2,1-2/Kolosser 1,9-12/Lukas 11,1-13",
            "team": "Gebets-Team, Kleingruppenleiter"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, anderen Christen durch biblische Erkenntnis Wissen von Gott zu vermitteln.",
            "section": [
                "M. ERKENNTNIS"
            ],
            "bible": "1. Korinther 12,8/Markus 2,6-8/Epheser 1,17-18",
            "team": "Service-Team, Gebets-Team, Connect-Team, Kleingruppenleiter"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, Perspektiven zu vermitteln, Menschen zu motivieren und so anzuleiten, dass gemeinsam Gottes Ziele erreicht werden.",
            "section": [
                "N. LEITUNG"
            ],
            "bible": "Römer 12,8/Hebräer 13,17/Lukas 22,25-26",
            "team": "alle Teams"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, gerne und ganz praktisch Menschen zu helfen, die leiden oder in Not sind.",
            "section": [
                "O. BARMHERZIGKEIT"
            ],
            "bible": "1. Korinther 12,10; 14,5+26-28",
            "team": "Kleingruppenleiter, Besuchs-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, durch übernatürliche Ereignisse Gottes Wirken sichtbar werden zu lassen.",
            "section": [
                "P. WUNDERTATEN"
            ],
            "bible": "1. Korinther 12,10/Josua 10,12-13",
            "team": "Gebets-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, andere Menschen außerhalb der eigenen Kultur und Nationalität mit dem Evangelium zu erreichen; meistens dadurch, dass man selbst in der Kultur und Nation lebt.",
            "section": [
                "Q. MISSION"
            ],
            "bible": "Apostelgeschichte 8,4; 13,2-3; 22,21/Römer 10,15",
            "team": "Kleingruppenleiter, Erzählcafe-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, durch das Singen, Tanzen oder Spielen eines Instruments, anderen dabei zu helfen, Gott anzubeten.",
            "section": [
                "R. MUSIK/ANBETUNG"
            ],
            "bible": "5. Mose 31,22/1. Samuel 16,16/1. Chronik 16,41-42/2. Chronik 5,12-13; 34,12/Psalm 150",
            "team": "Schatzinsel-Team, Musik-Team, Technik-Team, Licht-Team, Beamer-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, Menschen zu fördern, für sie zu sorgen und sie zu geistlichem Wachstum und Reife zu führen, damit sie Christus immer ähnlicher werden.",
            "section": [
                "S. PASTOR/HIRTE"
            ],
            "bible": "Epheser 4,11-12/1. Petrus 5,1-4/Johannes 10,1-8",
            "team": "Connect-Team (Kurse), Schatzinsel-Team, Kleingruppenleiter, Royal Rangers-Team, CYOU-Team, Cyouth-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, Gottes Wahrheit für eine bestimmte Situation zu offenbaren und so konkret anzusprechen, dass sie zu mehr Verständnis, zu Korrektur, Umkehr oder Ermahnung führt.",
            "section": [
                "T. PROPHETIE"
            ],
            "bible": "Römer 12,6/1. Korinther 12,10+28/1. Korinther 13,2/2. Petrus 1,19-21/1. Korinther 14,29",
            "team": "Gebets-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, praktische und notwendige Aufgaben zu erfüllen, die andere entlasten, unterstützen und ihren Bedürfnissen entgegenkommen.",
            "section": [
                "U. DIENEN"
            ],
            "bible": "1. Korinther 12,28/Römer 12,7/Apostelgeschichte 6,1-4/Römer 16,1-2",
            "team": "alle Teams"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, die Bibel zu verstehen, verständlich zu erklären und so auf das Leben anzuwenden, dass andere in ihrem Glauben und Leben einen Schritt vorwärts gehen können.",
            "section": [
                "V. LEHREN"
            ],
            "bible": "Römer 12,7/1. Korinther 12,28-29/Apostelgeschichte 2,14-41; 18,24-28/2. Timotheus 2,2/Jakobus 3,1",
            "team": "Kleingruppenleiter, Connect-Team (Kurse)"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, in einer Sprache, die der Sprecher nicht kennt, zu reden, zu beten oder Gott zu loben, oder diese auch auszulegen.",
            "section": [
                "W. SPRACHENGEBET/AUSLEGUNG DER SPRACHEN"
            ],
            "bible": "1. Korinther 12,10; 12,28-29; 14,5",
            "team": "Gebets-Team"
        },
        {
            "text": "Die von Gott gegebene Fähigkeit, geistliche Wahrheiten und Wissen auf eine konkrete Situation oder ein Bedürfnis anzuwenden.",
            "section": [
                "X. WEISHEIT"
            ],
            "bible": "1. Korinther 12,8/Jakobus 3,13-18/Jeremia 9,23",
            "team": "Musik-Team, Gebets-Team, Kleingruppenleiter, Next Steps"
        }
];

// State management
const currentIndex = ref(0);

const scores: Record<string, Ref<number>> = {}
'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').slice(0, 24).forEach(letter => { scores[`score${letter}`] = ref(0)
})

var score = "";

let data =  [] as any;
const label = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').slice(0, 24);
const isAnswered = ref(false);

// Computed property to get the current quiz
const currentQuiz = ref(quizzes[currentIndex.value]);

// Method to handle choice selection

const selectChoice = (choice: { text: string; point: number }, section: string) => {
 const key = `score${section}`
 if (scores[key]) {
  scores[key].value += choice.point
 }
 isAnswered.value = true // Mark as answered
 nextQuiz()
}

// Method to go to the next quiz
const nextQuiz = () => {
  isAnswered.value = false; // Reset answer status
  currentIndex.value++;
  if (currentIndex.value < quizzes.length) {
    currentQuiz.value = quizzes[currentIndex.value];
  } else {
    //currentQuiz.value = null; // No more quizzes
  currentQuiz.value = {
    type: '',
    text: '',
    section: ''
        };
    calculateHighestSection(); // Determine highest scoring section
    
    
  }
};

// Method to determine which section has the highest score
const calculateHighestSection = () => {
  const values = Object.entries(scores).map(([key, refValue]) => ({
    section: key.replace('score', ''),
    value: refValue.value
  }))


  // Create a readable score summary
  const scoreSummary = values
    .map(({ section, value }) => `${section}: ${value}`)
    .join('\n  ')
  console.log('Score Summary:\n', scoreSummary)
  score = scoreSummary;
  data = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ'.split('').slice(0, 24).map(letter => scores[`score${letter}`].value);

}



const refreshPage = () => {
  window.location.reload(); // Reloads the current page
};

</script>
