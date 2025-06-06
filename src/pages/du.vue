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
                      <v-btn @click="refreshPage" prepend-icon="mdi-reload">Neu laden</v-btn>
                      <v-btn prepend-icon="mdi-home" to="/" text="Zurück"></v-btn>

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
          <v-card-text class="text-center">»Ich danke dir dafür, dass ich so wunderbar erschaffen bin, es erfüllt mich mit Ehrfurcht. Ja, das habe ich erkannt: Deine Werke sind wunderbar!«
          </v-card-text>
          <v-card-text class="text-center">Psalm 139,14</v-card-text>
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
        <v-row justify="center">
          <v-col cols="12" md="6" v-for="typ in people">
            <v-card class="mx-auto">
              <v-card-item>
                <div>
                  <div class="text-overline mb-1">
                    Wenn dich <b>»{{ typ.type}}«-Tendenzen</b> motivieren...
                  </div>
                  <div class="text-h6 mb-1">
                    {{ typ.name}}
                  </div>
                  <div class="text-caption">({{ typ.char}})</div>
                  <v-list tag='ul'>
                    <span v-for="how in typ.list">
                      <v-list-item tag='li'>
                        * {{ how }}
                      </v-list-item>
                    </span>
                  </v-list>
                </div>
              </v-card-item>
            </v-card>
          </v-col>
        </v-row>
        <br/>
        <v-expansion-panels>
          <v-expansion-panel
            v-for="i in detailed"
            :key="i.section[0]"
            :title="i.section.join('/')"
            :text="i.text"
          >
          </v-expansion-panel>
        </v-expansion-panels>
        <br/>
          <v-btn prepend-icon="mdi-home" to="/" text="Zurück"></v-btn>
        
        </span>
      </v-container>
            </div>
  </v-container>
  <Nav></Nav>
</template>

<script setup lang="ts">
import { ref } from 'vue';

// Define the quizzes with titles and text
const quizzes = [
        {
           "type":"Über mich ...",
           "text":"Ich bin durchsetzungsfähig, fordernd und entschlossen",
           "section":"D"
        },
        {
           "type":"Über mich ...",
           "text":"Mir gefällt es, verschiedene Aufgaben gleichzeitig zu erledigen",
           "section":"D"
        },
        {
           "type":"Über mich ...",
           "text":"In einer herausfordernden Umgebung blühe ich auf",
           "section":"D"
        },
        {
           "type":"Über mich ...",
           "text":"Ich stelle Aufgaben über andere oder mich selbst",
           "section":"D"
        },
        {
            "type": "Über mich ...",
            "text": "Die Bewältigung einer Aufgabe und Leitungsaufgaben motivieren mich",
            "section": "D"
        },

        {
            "type": "Über mich ...",
            "text": "Ich mag es, andere Leute zu beeinflussen und sie zu inspirieren",
            "section": "I"
        },
        {
            "type": "Über mich ...",
            "text": "Anderen gegenüber bin ich optimistisch",
            "section": "I"
        },
        {
            "type": "Über mich ...",
            "text": "Normalerweise bin ich der lebendige Mittelpunkt einer Party",
            "section": "I"
        },
        {
            "type": "Über mich ...",
            "text": "Ich denke darüber nach, wie ich Leute motivieren kann",
            "section": "I"
        },
        {
            "type": "Über mich ...",
            "text": "Mich motivieren Bestätigung und Akzeptanz",
            "section": "I"
        },

        {
            "type": "Über mich ...",
            "text": "In einem stimmigen, beständigen Umfeld blühe ich auf",
            "section": "S"
        },
        {
            "type": "Über mich ...",
            "text": "Fachbezogenes mag ich lieber als Allgemeines",
            "section": "S"
        },
        {
            "type": "Über mich ...",
            "text": "Ich mag (lieber) kleine Menschengruppen",
            "section": "S"
        },
        {
            "type": "Über mich ...",
            "text": "Ich ziehe es vor, Mitglied eines Teams zu sein",
            "section": "S"
        },
        {
            "type": "Über mich ...",
            "text": "Stabilität (Beständigkeit) und Unterstützung motivieren mich",
            "section": "S"
        },

        {
            "type": "Über mich ...",
            "text": "Für gewöhnlich gehe ich keine großen Risiken ein",
            "section": "G"
        },
        {
            "type": "Über mich ...",
            "text": "Ich liebe Aufgaben, Ordnung und Einzelheiten",
            "section": "G"
        },
        {
            "type": "Über mich ...",
            "text": "Meistens habe ich recht",
            "section": "G"
        },
        {
            "type": "Über mich ...",
            "text": "Ich folge klar festgelegten Regeln",
            "section": "G"
        },
        {
            "type": "Über mich ...",
            "text": "Ein hoher Qualitätsanspruch und Korrektheit motivieren mich.",
            "section": "G"
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
            "text": "D-Typen sind direkt und entscheidungsfreudig; sie gehen Risiken ein und lösen Probleme. Sie sind stärker an der Erledigung von Aufgaben interessiert, als daran, die Anerkennung anderer zu gewinnen. Auch wenn ihr innerer Antrieb sie häufig unsensibel für die Menschen in ihrer Umgebung macht, so scheuen sich D-Typen doch nicht, den Status quo infrage zu stellen, und sie blühen auf, wenn sie etwas Neues entwickeln können. Für Höchstleistungen benötigen sie Disziplin, und sie reagieren auf eine direkte Konfrontation. Die meiste Angst haben sie davor, ausgenutzt zu werden, und ungeachtet ihrer möglichen Schwächen (zu denen eine Abneigung gegen Routine gehört, eine Tendenz, ihre Befugnisse zu überschreiten, ein streitbares Wesen sowie die Gewohnheit, sich ein zu großes Pensum aufzubürden) messen sie der Zeit einen großen Stellenwert bei und nutzen ihr innovatives Denken, um schwierige Aufgaben und Herausforderungen zu bewältigen.",
            "section": ["D"]
        },
        {
            "text": "D/I-Typen sind wissbegierig und ziehen gerne Schlussfolgerungen; sie betonen das, was am Ende »unter dem Strich steht« und arbeiten hart, um ihre Ziele zu erreichen. Sie sind eher zielstrebig als inspirierend, und doch führen die hohen Erwartungen, die sie an sich selbst und andere in ihrem Umfeld stellen, in der Regel dazu, dass sie einen ziemlichen Eindruck hinterlassen und andere dazu motivieren, ihnen zu folgen. Sie sind vielseitig interessiert und werden mitunter dadurch abgelenkt, dass sie sich zu viele Projekte auf- halsen. Häufig ist es nötig, dass sie sich konzentrieren, Prioritäten festlegen und einfach mal ein bisschen langsamer treten. Da D/I-Typen aufblühen, wenn sie aktiv sind und etwas vorangeht, möchten sie Aufgaben mithilfe einer großen Zahl von Menschen bewältigen. Biblische Beispiele: Josua (Josua 1), Noah (1. Mose 69), Sara (1. Mose 16/1. Petrus 3,6)",
            "section": ["D", "I"]
        },
        {
            "text": "D/S-Typen sind Menschen, die etwas erreichen und leisten und Durchhaltevermögen besitzen. Sie sind eher aktiv als passiv, doch ihnen ist eine Art ruhiger Feinfühligkeit und Stetigkeit zu Eigen, was sie zu guten Leitern macht. Sie scheinen auf Menschen zentriert zu sein, können aber mit Blick auf Aufgaben und Projektplanung leicht dominant und bestimmend auftreten. Sie sind bestrebt, Ziele mit leidenschaftlicher Entschlossenheit zu erreichen, die auf einem starken inneren Antrieb beruht, doch sie könnten von betrachtendem und konservativem Denken profitieren, sowie von einer stärkeren Konzentration auf Beziehungen. Biblische Beispiele: Daniel (Daniel 1-6), Hiob (Hiob 1,5/Jakobus 5,11), Marta (Lukas 10,38-42)",
            "section": ["D", "S"]
        },
        {
            "text": "D/G-Typen fordern heraus und können entweder als zielstrebige Schüler oder als provokative Kritiker auftreten. Ihnen ist es wichtig, das Sagen zu haben, dabei interessiert es sie aber wenig, was andere denken, solange diese ihre Arbeit erledigen. Sie agieren mit großem Weitblick und untersuchen alle Möglichkeiten, um die beste Lösung zu finden. Am liebsten arbeiten sie alleine. Auch wenn sie Angst vor dem Scheitern sowie vor mangelndem Einfluss haben, werden sie durch Herausforderungen motiviert und können sich oft als hervorragende Sachverwalter erweisen. Sie können davon profitieren, wenn sie lernen, sich zu entspannen und Menschen mehr Aufmerksamkeit zu widmen. Biblische Beispiele: Maleachi (Maleachi), Nathan (2. Samuel 12,1-13), Nahum (Nahum 1-3)",
            "section": ["D", "G"]
        },
        {
            "text": "I-Typen sind inspirierend und beeindruckend, enthusiastisch, optimistisch, impulsiv und emotional; in der Regel lösen sie Probleme auf kreative Weise und können andere hervor- ragend motivieren. Häufig haben sie viele Freunde, doch es kann passieren, dass sie stärker um Anerkennung und Beliebtheit bemüht sind, als um Ergebnisse. Die größte Angst haben I-Typen davor, abgelehnt zu werden, doch sie blühen auf, wenn es gilt, andere zu motivieren. Ihr positiver Sinn für Humor hilft ihnen, im Konfliktfall zu vermitteln. Auch wenn sie Details bisweilen keine Aufmerksamkeit schenken und schlecht zuhören, so können sie doch hervorragende Friedensstifter und effektive Teammitglieder sein, wenn sie ihre Gefühle kontrollieren und ihren Drang eindämmen, andere unterhalten und im Zentrum der Aufmerksamkeit stehen zu müssen. Sie schätzen eine intensive menschliche Note und Beziehungen.",
            "section": ["I"]
        },
        {
            "text": "I/D-Typen sind Überzeugungskünstler, die sich kontaktfreudig und dynamisch zeigen. Sie mögen große Gruppen und nutzen ihren Einfluss, um Respekt zu gewinnen und andere davon zu überzeugen, ihrer Führung zu folgen. Bisweilen wirken sie zappelig und nervös. Doch dies liegt an ihrem Bedürfnis, Herausforderungen anzunehmen, die Abwechslung, Freiraum und Bewegungsfreiheit mit sich bringen. I/DTypen könnten davon profitieren, würden sie lernen, zunächst einmal hinzusehen, bevor sie losstürmen, und häufiger lerneifrig und still zu sein. Sie sind inspirierende Leiter und wissen, wie sie Ergebnisse von und durch Menschen erreichen. Biblische Beispiele: Johannes der Täufer (Lukas 3), Petrus (Matthäus 16 und 26, Apostelgeschichte 3), Rebekka (1. Mose 24)",
            "section": ["I", "D"]
        },
        {
            "text": "I/S-Typen sind einflussreiche Ratgeber. Sie lieben Menschen und es ist kein Wunder, dass Menschen sie lieben. Sie leben, um zu dienen und es anderen recht zu machen. Sie sind gute Zuhörer. Es ist ihnen wichtig, gut bei anderen dazustehen. Außerdem sind sie gut darin, andere zu ermutigen. Es mangelt ihnen an Disziplin und Struktur. Ihnen sind die Menschen, die an einer Aufgabe arbeiten wichtiger, als die erfolgreiche Lösung der Aufgabe. Ob auf der Bühne oder dahinter, sie können gleichermaßen effektiv sein. Ihre besonderen Stärken liegen in ihren positiven Einfluss auf andere und ihre Hilfsbereitschaft. Biblische Beispiele: Barnabas (Apg 4; 9; 11-15), Elisa (1. Könige 19 / 2. Könige 2-3), Nikodemus (Johannes 3; 7; 19)",
            "section": ["I", "S"]
        },
        {
            "text": "I/G-Typen erfüllen die Bedürfnisse anderer. Sie rechnen mit Schwierigkeit und bereiten sich dementsprechend darauf vor. Sie sind einfallsreich, bringen Dinge voran und sind gut im Improvisieren. Sie liefern plausible Gründe für eine Richtungsänderung. Sie bewerten Ergebnisse und Menschen eher kritisch. Außerdem sind sie offen für neue und be- stehende Ideen. Am ehesten kann man einen I/G-Typen als Strategen bezeichnen. Biblische Beispiele: Miriam (2. Mose 15-21), Esra (Esra 7-8), Sunamitische Frau (2. Könige 4, 8-37)",
            "section": ["I", "G"]
        },
        {
            "text": "S-Typen erweisen sich als stetig und eher zurückhaltend. Da sie gefestigt und berechenbar sind, mögen sie keine Veränderungen und blühen in Umständen auf, die Sicherheit verheißen und in denen keine Bedrohungen warten. In der Regel sind sie freundlich und verständnisvoll, gute Zuhörer sowie loyale Mitarbeiter, die glücklich damit sind, wenn sie durchweg dieselbe Arbeit verrichten. Angesichts ihrer unglaublichen Fähigkeit zu vergeben, können S-Typen mit ihrer Zuverlässigkeit und Verlässlichkeit die besten Freunde sein. Ihre größte Angst liegt jedoch im Verlust ihrer Sicherheit, und zu ihren möglichen Schwächen gehört natürlich nicht nur der Widerstand gegenüber Veränderungen, sondern auch die Schwierigkeit, sich an diese anzupassen. Bisweilen reagieren sie auch zu empfindlich auf Kritik und sind nicht in der Lage, Prioritäten festzulegen. Damit sie nicht ausgenutzt werden, müssen S-Typen stärker werden und lernen, »Nein« zu sagen. Überdies scheuen sie das Rampenlicht, doch wenn sie eine Gelegenheit erhalten, anderen aufrichtig zu helfen, wachsen sie bereitwillig über sich hinaus. Die stärkste Wertschätzung empfinden sie, wenn sie jemandem ernsthaft helfen konnten.",
            "section": ["S"]
        },
        {
            "text": "S/D-Typen sind ruhige Leiter, auf die Verlass ist, wenn etwas erledigt werden muss. Besser kommen sie in kleinen Gruppen zurecht, und sie sprechen nicht gerne vor größeren Menschenmengen. Auch wenn sie gleichzeitig weich- und hartherzig sein können, genießen sie enge Beziehungen zu Menschen und achten genau darauf, diese nicht zu dominieren. Durch Herausforderungen werden sie motiviert, insbesondere durch solche, die es ihnen gestatten, systematisch vorzugehen. Da Personen dieses Typs in der Regel zielstrebig sind und auch längere Zeit und einigen Mühen zum Trotze durchhalten können, profitieren sie von Ermutigung und positiven Beziehungen. Biblische Beispiele: Marta (Lukas 10,38-42), Hiob (Hiob 1,5/Jakobus 5,11)",
            "section": ["S", "D"]
        },
        {
            "text": "S/I-Typen sind inspirierende Ratgeber, die Wärme und Feinfühligkeit ausstrahlen. Sie sind tolerant und versöhnlich, und so haben sie viele Freunde, da sie andere annehmen und gut vertreten. Ihr soziales Wesen sowie ihr Wunsch, sympathisch und flexibel zu sein, erzeugt bei ihnen die Neigung, übermäßig tolerant aufzutreten und Konfrontationen aus dem Weg zu gehen. S/I-Typen profitieren davon, wenn sie sich stärker auf Aufgaben konzentrieren und auf Details achten. Mit ihrem freundlichen und rücksichtsvollen Wesen beziehen sie andere mit ein und inspirieren diese, ihnen zu folgen. Mit Zuspruch erreicht man bei diesem Persönlichkeitstyp viel, und mit der richtigen Motivation können S/I-Typen zu hervorragenden Teamplayern werden. Biblische Beispiele: Maria Magdalena (Lukas 7,36-47), Barnabas (Apostelgeschichte 4; 9; 11-15), Elisa (1. Könige 19 / 2. Könige 2-13)",
            "section": ["S", "I"]
        },
        {
            "text": "S/G-Typen sind diplomatisch, stetig und detailorientiert. Durch ihre gefestigte und nachdenkliche Art wägen sie gerne die vorhandenen Beweise ab und stellen die Tatsachen fest, um zu einer logischen Schlussfolgerung zu gelangen. Sie überlegen gründlich, und so nehmen sie sich lieber Zeit, insbesondere wenn Entscheidungen andere betreffen. Als mögliche Schwäche ist zu nennen, dass sie höchst sensibel sind und nicht mit Kritik umgehen können; ferner müssen sie sich auch bewusst sein, wie sie andere behandeln. Am besten agieren sie in klar umrissenen Projekten, die die Sache wert sind. S/G-Typen können Friedensstifter sein; dadurch geben sie loyale Teammitglieder und Freunde ab. Biblische Beispiele: Mose (2. Mose 3; 4; 20; 32), Johannes (Johannes 19, 26-27), Eliëser (1. Mose 24)",
            "section": ["S", "G"]
        },
        {
            "text": "G-Typen sind entgegenkommend und analytisch. Angetrieben werden sie durch umsichtiges und logisches Denken; am wichtigsten ist für sie Fehlerfreiheit. Sie wahren hohe Standards und schätzen ein systematisches Vorgehen bei der Lösung von Problemen. Zwar blühen sie auf, wenn sie die Gelegenheit erhalten, nach Lösungen zu suchen, doch sie neigen dazu, die Gefühle anderer zu ignorieren, und sind häufig zu kritisch - bisweilen übellaunig. Ihnen fällt es schwer, Gefühle auszudrücken, doch wenn sie sich nicht in Details verzetteln und klar umrissene Grenzen haben, können sie einen großen Gewinn für das Team darstellen, indem sie präzise »Realitätsprüfungen« durchführen. Am meisten fürchten G-Typen Kritik, und ihr Bedürfnis nach Perfektion zeigt sich oft als Schwäche, ebenso wie ihre Neigung, inmitten eines Streits nachzugeben. Allerdings gehen sie bei sämtlichen Aktivitäten gründlich vor und können das Team durch ein gewissenhaftes und ausgeglichenes Element bereichern, das als solides Fundament wirkt. Den größten Wert legen sie darauf, fehlerfrei zu sein.",
            "section": ["G"]
        },
        {
            "text": "G/D-Typen sind bedächtige und zielstrebige Gestalter, die konsequent aufgabenorientiert vorgehen und sich etwaigen Problemen genau bewusst sind. Bisweilen werden sie als unsensibel angesehen, doch sie kümmern sich um Einzelne, nur fällt es ihnen schwer, dies auch zu zeigen. Häufig meinen sie, sie seien die Einzigen, die imstande sind, die Arbeit so zu erledigen, wie es notwendig ist. Doch aufgrund ihrer verwaltungsbezogenen Fähigkeiten, sind sie in der Lage, Pläne mit Blick auf Veränderungen und Verbesserungen zu verwirklichen. G/D-Typen neigen zu Ernsthaftigkeit und könnten davon profitieren, optimistischer und begeisterungsfähiger zu sein. Ungeachtet ihres natürlichen Antriebs, etwas erreichen zu wollen, sollten sie sich darauf konzentrieren,, gesunde Beziehungen aufzubauen und Menschen einfach nur zu lieben. Biblische Beispiele: Bezalel (2. Mose 35, 30-35; 37,1-9), Jochebed (2. Mose 1, 22-2,4), Jitro (2. Mose 18)",
            "section" : ["G", "D"],
        },
        {
            "text": "G/I-Typen achten auf Details. Häufig beeindrucken sie andere, indem sie das richtige tun und die Lage stabilisieren. Sie werden nicht als aggressiv oder aufdringlich wahrgenommen, und so kommen sie sowohl mit großen als auch mit kleinen Menschengruppen zurecht. Obgleich sie gut mit anderen zusammenarbeiten können, reagieren sie bisweilen zu sensibel auf das, was andere über sie und ihre Arbeit denken. Profitieren könnten sie davon, bestimmter und selbstmotivierter aufzutreten. Häufig können sie den Charakter anderer gut einschätzen und sie fassen schnell vertrauen zu denen, die ihren Maßstäben entsprechen. Sie werden durch echte und begeisterte Anerkennung bewegt sowie durch präzise und logische Erklärungen. Biblische Beispiele: Miriam (2. Mose 15-21/4. Mose 12,1-15), Esra (Esra 7; 8)",
            "section" : ["G", "I"],
        },
        {
            "text": "G/S-Typen sind systematisch und gefestigt. Sie neigen dazu, eines nach dem anderen zu erledigen - und das dann richtig. Sie sind zurückhaltend und vorsichtig und arbeiten lieber hinter den Kulissen, um bei der Sache zu bleiben. Allerdings riskieren sie nur selten etwas oder probieren Neues aus, und so gefällt es ihnen nicht, wenn sich in ihrem Umfeld plötzlich etwas ändert. Sie sind präzise bis auf Punkt und Komma, brauchen penible Genauigkeit und fürchten Kritik, die sie mit Versagen gleichsetzen. Als fleißige Arbeiter schöpfen sie ihre Motivation daraus, anderen zu dienen. Biblische Beispiele: Esther (Esther 4), Zacharias (Lukas 1), Josef (Matthäus 1,1-23)",
            "section" : ["G", "S"],
        },
];

const people = [
  {
      "list": ["zuhören, ohne zu unterbrechen", "Einfühlungsvermögen und Verständnis zeigen", "Einwände anderer als Chancen nutzen", "sich nicht auf Kriegspfad begeben, um andere zu überzeugen", "andere mehr als hilfsbereite anstatt als gefügige Partner einbinden", "mit Menschen zusammenarbeiten, die besser im Team kooperieren können", "investiere mehr in persönliche Beziehungen und Freundschaften"],
      "type": "D",
      "name": "Dominant",
      "char": "dominant, direkt, aufgabenorientiert, sich durchsetzend, organisiert, aufgeschlossen, gerade heraus"
  },
  {
      "list": ["sich an der Aufgabe orientieren ","Termine einhalten ","energisch und direkt sprechen ","bei der Entscheidungsfindung Objektivität walten lassen ","Einwänden direkt begegnen","mit Menschen zusammenarbeiten, die besser organisiert sind","rede weniger und höre besser zu","habe mehr Selbstkontrolle über deine Emotionen, Wörter und Handlungen" ],
     "type":"I",
     "name": "Initiativ",
     "char":"initiativ, menschenorientiert, selbstbewusst, aufgeschlossen, eher extrovertiert"
  },
  {
     "list": ["auch unter Druck die Kontrolle bewahren ","verantwortungslose Menschen sofort tadeln ","die Initiative ergreifen anstatt auf andere Menschen oder Ereignisse zu reagieren ","persönliche Fähigkeiten nicht herunerspielen und sich selbst nicht unterschätzen ","sich bewusst für Vielfalt öffnen ","sich bewusst machen, wie Veränderung auch gesund sein kann, versuche auch  Veränderungen schneller anzunehmen"],
     "type":"S",
     "name": "Stetig",
     "char":"stetig, beständig, analysierend, menschenorientiert, eher introvertiert"
  },
  {
     "list": ["neue Verbindungen zu anderen aufbauen ","Konflikte ertragen lernen ","Entscheidungsfindung beschleunigen ","erkennen, dass nicht alle Probleme kompliziert sind ","üben in weniger wichtigen Bereichen schnelle Entscheidungen zu treffen ","mit Menschen zusammenarbeiten, die besser persönliche Kontakte knüpfen können ","lerne auch spontan Pläne zu ändern"],
     "type":"G",
     "name": "Gewissenhaft",
     "char":"gewissenhaft, kompetent, aufgabenorientiert, zielorientiert, eher introvertiert"
  },
];

// State management
const currentIndex = ref(0);
const scoreD = ref(0);
const scoreI = ref(0);
const scoreG = ref(0);
const scoreS = ref(0);
var score ="";

let data =  [] as any;
const label = ref([
    'Dominant',
    'Initiativ',
    'Gewissenhaft',
    'Stetig',
  ])
const isAnswered = ref(false);

// Computed property to get the current quiz
const currentQuiz = ref(quizzes[currentIndex.value]);

// Method to handle choice selection
const selectChoice = (choice: { text: string; point: number }, section: string) => {
    if( section === 'D')
      scoreD.value += choice.point;
    if( section === 'I')
      scoreI.value += choice.point;
    if( section === 'G')
      scoreG.value += choice.point;
    if( section === 'S')
      scoreS.value += choice.point;
    isAnswered.value = true; // Mark as answered
  nextQuiz()
};

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
  data = [scoreD.value, scoreI.value, scoreG.value, scoreS.value ]
  score ="D:"+  scoreD.value +    "\n  I:" +  scoreI.value +"\n  G:" +  scoreG.value +"\n  S:" +  scoreS.value;
};


const refreshPage = () => {
  window.location.reload(); // Reloads the current page
};

</script>
