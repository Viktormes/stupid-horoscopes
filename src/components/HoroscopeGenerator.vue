<template>
  <div class="relative min-h-screen overflow-hidden bg-slate-950 text-slate-100">

    <div ref="bgRef" class="pointer-events-none absolute inset-0 kosmos-bg">
      <!-- Deep nebula light -->
      <div class="nebula-opera nebula-opera-1"></div>
      <div class="nebula-opera nebula-opera-2"></div>

      <!-- Bright sparse stars -->
      <div class="stars-opera"></div>

      <!-- Cinematic vignette -->
      <div class="absolute inset-0 vignette"></div>
    </div>

    <!-- Innehåll -->
    <div class="relative grid min-h-screen place-items-center p-6">
      <div class="w-full max-w-3xl rounded-2xl border border-slate-800 bg-slate-900/40 p-5 shadow-xl backdrop-blur">
        <div class="flex flex-wrap items-end justify-between gap-4">
          <div>
            <h1 class="m-0 text-2xl font-semibold tracking-tight">Dagens horoskop</h1>
            <p class="mt-1 text-slate-300">Se vad Kosmos har på gång.</p>
          </div>

          <div class="flex gap-2">
            <select
                v-model="valtTecken"
                class="h-11 rounded-xl bg-slate-950/60 border border-slate-700 px-3 text-slate-100 outline-none hover:ring-1 hover:ring-slate-400 cursor-pointer"
            >
              <option v-for="t in stjarntecken" :key="t" :value="t">{{ t }}</option>
            </select>
          </div>
        </div>

        <div class="mt-5 rounded-2xl border border-slate-800 bg-slate-950/40 p-4">
          <div v-if="horoskop" class="flex items-center justify-between gap-3 text-sm text-slate-300">
            <div><strong class="text-slate-100">{{ valtTecken }}</strong> • {{ idag }}</div>
          </div>

          <div v-if="horoskop" class="mt-3 space-y-4">
            <p class="text-lg leading-relaxed">{{ horoskop.text }}</p>

            <div class="grid grid-cols-1 gap-3 sm:grid-cols-3">
              <div class="rounded-xl border border-slate-800 bg-slate-900/40 p-3">
                <div class="text-xs text-slate-400">Tur-färg</div>
                <div class="mt-1 font-semibold">{{ horoskop.turFarg }}</div>
              </div>

              <div class="rounded-xl border border-slate-800 bg-slate-900/40 p-3">
                <div class="text-xs text-slate-400">Tur-nummer</div>
                <div class="mt-1 font-semibold">{{ horoskop.turNummer }}</div>
              </div>

              <div class="rounded-xl border border-slate-800 bg-slate-900/40 p-3">
                <div class="text-xs text-slate-400">Tur-tid</div>
                <div class="mt-1 font-semibold">{{ horoskop.turTid }}</div>
              </div>
            </div>
          </div>
        </div>

        <div class="mt-3 text-xs text-slate-500 text-center">
          Disclaimer: Kosmos är inte en medicinteknisk produkt.
        </div>
      </div>
    </div>
  </div>
</template>

<script setup>


import { onBeforeUnmount, onMounted, ref, computed, watch } from "vue";

const bgRef = ref(null);

function clamp(n, min, max) {
  return Math.max(min, Math.min(max, n));
}

function onMove(e) {
  if (!bgRef.value) return;

  const x = (e.clientX / window.innerWidth) * 2 - 1; // -1..1
  const y = (e.clientY / window.innerHeight) * 2 - 1; // -1..1

  // Small, subtle parallax
  const dx = clamp(x * 10, -10, 10);
  const dy = clamp(y * 8, -8, 8);

  bgRef.value.style.setProperty("--px", `${dx}px`);
  bgRef.value.style.setProperty("--py", `${dy}px`);
}

onMounted(() => window.addEventListener("pointermove", onMove, { passive: true }));
onBeforeUnmount(() => window.removeEventListener("pointermove", onMove));

/* =========================
   DATA
========================= */

const stjarntecken = [
  "Fiskarna",
  "Jungfrun",
  "Kräftan",
  "Lejonet",
  "Oxen",
  "Skorpionen",
  "Skytten",
  "Stenbocken",
  "Tvillingarna",
  "Vattumannen",
  "Vågen",
  "Väduren",
];

const valtTecken = ref("Fiskarna");
const horoskop = ref(null);

/* =========================
   TEXTBANKER (stor variation)
========================= */

const inledningar = [
  "Månen rör sig genom ett känsligt fält och påverkar din perception.",
  "Dagens planetkonstellation skapar ett märkligt lugn runt dig.",
  "Kosmos pekar inte rakt fram idag, utan snarare lite snett.",
  "Ett långsamt skifte i energierna gör dig mer observant än vanligt.",
  "Universum verkar ha läst dina senaste tankar, men feltolkat dem något.",
  "Ett stillsamt planetärt brus påverkar hur du tolkar omgivningen.",
  "Dagens kosmiska läge uppmuntrar till eftertanke snarare än handling.",
  "Energierna rör sig långsammare än vanligt, vilket gynnar reflektion.",
  "En subtil spänning i horoskopet gör dig ovanligt mottaglig för detaljer.",
  "Universum signalerar försiktigt, men utan att vara särskilt tydligt.",
  "Astrologiska mönster skapar ett behov av struktur som inte riktigt infrias.",
  "Månen drar upp gamla tankar till ytan, utan att erbjuda nya svar.",
  "Ett svagt men ihållande kosmiskt inflytande färgar hela dagen.",
  "Planeternas positioner pekar mer på stämning än på riktning.",
  "Det astrologiska klimatet är neutralt, men känslomässigt laddat.",
  "En försiktig kosmisk rörelse gör att du tolkar allt lite långsammare.",
  "Dagens stjärnbild lägger sig som ett mjukt filter över dina beslut.",
  "Ett oväntat lugn i omloppsbanorna gör dig mer selektiv.",
  "Kosmos ger inga svar, men ställer bättre frågor än vanligt.",
  "Ett diskret drag i energin gör att du ser mönster du annars missar.",
  "Planeternas tonläge är lågmält men ihärdigt.",
  "En märklig tydlighet uppstår när du sänker tempot.",
  "Dagens atmosfär känns mer vägledande än vanligt.",
  "Ett lätt kosmiskt tryck gör dig mer uppmärksam på sammanhang.",
  "Stjärnorna viskar snarare än ropar, och det hjälper.",
  "En långsam rotation i ditt inre ger ny vinkel på det gamla.",
  "Dagens horisont är mjuk men bestämd.",
  "Kosmos verkar vilja ha kvalitet före kvantitet idag.",
  "Ett lågintensivt skifte gör att du omvärderar det bekanta.",
  "Energierna är balanserade, men inte särskilt entusiastiska.",
  "En liten skevhet i stjärnläget gör allt lite mer nyanserat.",
  "Månen ger mer tålamod än tydlighet.",
  "Dagens kosmiska signal är diskret men konsekvent.",
  "Det astrologiska klimatet känns återhållsamt, men stabilt.",
  "En stilla drift i rymden gör att du väljer med större omsorg.",
];
// ... existing code ...
const fokus = [
  "Arbetsrelaterade frågor kräver tålamod snarare än snabbhet.",
  "Relationer präglas av små signaler som betyder mer än de borde.",
  "Din kropp kommunicerar tydligare än ditt schema tillåter.",
  "Praktiska beslut känns oväntat filosofiska.",
  "Din roll i ett sammanhang omvärderas, eventuellt av dig själv.",
  "Vardagsrutiner får oväntad betydelse.",
  "Kommunikation kräver extra lyhördhet.",
  "Ansvar känns tyngre än vanligt, men också mer meningsfullt.",
  "Små praktiska detaljer påverkar helheten.",
  "Sociala sammanhang kräver mer energi än de ger.",
  "Ett pågående åtagande behöver omvärderas.",
  "Gränsdragningar blir tydligare, även om de är obekväma.",
  "Tid upplevs ojämnt fördelad under dagen.",
  "Din roll i ett samarbete förändras subtilt.",
  "Förväntningar, både dina och andras, hamnar i fokus.",
  "Ditt tempo krockar lätt med andras, men det går att justera.",
  "Prioriteringar blir synliga när du säger nej till det lilla.",
  "Det som verkar praktiskt visar sig vara emotionellt viktigt.",
  "Ett samtal kräver mer struktur än känsla.",
  "Vardagens logistik blir en spegel för dina gränser.",
  "Dina resurser behöver fördelas mer medvetet.",
  "En gammal uppgift blir plötsligt relevant igen.",
  "Små löften får större vikt än vanligt.",
  "Din uppmärksamhet dras till det som saknar avslut.",
  "Du märker tydligare vad som faktiskt är ditt ansvar.",
  "En rutin känns mer förhandlingsbar än du trodde.",
  "Tidsplanering blir oväntat kreativ.",
  "Ett samarbete kräver tydligare ramar än tidigare.",
  "Du behöver be om hjälp i en fråga som annars glider.",
  "Små val får större konsekvenser i gruppsammanhang.",
  "Ditt fokus skiftar från resultat till process.",
  "En vardaglig fråga får oväntad tyngd.",
  "Du söker stabilitet i det som annars känns flytande.",
  "Ett mejl eller meddelande kräver extra precision.",
  "Du märker tydligare var dina gränser går i praktiken.",
];
// ... existing code ...
const rad = [
  "Fokusera på en sak i taget, även om allt verkar akut.",
  "Undvik att förklara mer än nödvändigt.",
  "Ta pauser innan du tror att du behöver dem.",
  "Lita mer på det som känns stabilt än det som känns spännande.",
  "Gör något långsamt medvetet ineffektivt.",
  "Välj tydlighet framför artighet.",
  "Avvakta innan du reagerar.",
  "Prioritera det som redan fungerar.",
  "Sänk tempot snarare än ambitionen.",
  "Undvik att fatta beslut av ren vana.",
  "Tillåt viss osäkerhet utan att försöka lösa den.",
  "Lyssna mer än du förklarar.",
  "Avsluta något litet innan du börjar något nytt.",
  "Acceptera att allt inte behöver optimeras.",
  "Gör plats för pauser utan att rättfärdiga dem.",
  "Skriv ner det viktiga istället för att hålla det i huvudet.",
  "Välj den enklaste vägen som fortfarande känns rätt.",
  "Skala bort detaljer tills riktningen blir tydlig.",
  "Gör en sak helt färdigt innan du rör nästa.",
  "Låt det som är brådskande passera en minut extra.",
  "Be om tid om du behöver tänka klart.",
  "Gör en liten justering istället för en stor omstart.",
  "Svara kort när frågan är kort.",
  "Lägg fokus på det som är mätbart idag.",
  "Börja där friktionen är minst.",
  "Håll löften du redan gett, inte nya.",
  "Acceptera hjälp även om du kunde gjort det själv.",
  "Välj stabilt före spektakulärt.",
  "Gör något på rutin och se vad det lär dig.",
  "Sänk volymen, inte ambitionen.",
  "Låt ett beslut mogna innan du offentliggör det.",
  "Bygg en reservplan utan att förklara den.",
  "Lita på det du redan vet är sant.",
  "Ta en kort paus varje gång du byter uppgift.",
  "Välj konsekvens före variation.",
];
// ... existing code ...
const konsekvens = [
  "Detta leder till färre missförstånd än väntat.",
  "Resultatet blir subtilt men påtagligt.",
  "Effekten märks först senare under dagen.",
  "Någon annan kommer dra större slutsatser än du av detta.",
  "Du slipper åtminstone ett onödigt problem.",
  "Stämningen blir lugnare än förväntat.",
  "Du märker en subtil lättnad.",
  "Ett missförstånd undviks precis i tid.",
  "Dagen får en mer förutsägbar struktur.",
  "Du behåller kontrollen utan att anstränga dig.",
  "Någon tolkar ditt agerande positivt.",
  "Resultatet känns mer stabilt än spännande.",
  "En lågintensiv tillfredsställelse infinner sig.",
  "Du slipper ta ansvar för något som annars kunde blivit ditt.",
  "En mindre justering ger större effekt än väntat.",
  "Du får mer tid än du trodde var möjligt.",
  "En konflikt rinner ut i sanden utan dramatik.",
  "Du vinner förtroende utan att försöka.",
  "En oväntad förenkling uppstår.",
  "Dagen blir lättare att förutse.",
  "Du får utrymme att omprioritera i lugn och ro.",
  "En liten insats ger oproportionerlig effekt.",
  "Du slipper förklara dig i onödan.",
  "En praktisk vinst visar sig först i efterhand.",
  "Du märker att du orkar mer när allt är tydligt.",
  "Du får en chans att korrigera i tid.",
  "Ett beslut visar sig vara mer hållbart än du trodde.",
  "Du sparar energi utan att tumma på kvalitet.",
  "Någon annan tar initiativ som hjälper dig.",
  "Stämningen blir mer saklig och mindre laddad.",
  "Du lyckas undvika en sidoväg.",
  "Ett svar blir mer definitivt än väntat.",
  "Du får en lugnare rytm resten av dagen.",
  "En kedjereaktion av små förbättringar startar.",
  "Det känns enklare att hålla fokus.",
];
// ... existing code ...
const absurditeter = [
  "Undvik viktiga samtal nära automatiska dörrar.",
  "Ett möte med ett kvitto kan kännas symboliskt.",
  "Teknik kommer att testa din emotionella mognad.",
  "Någon kommer använda ordet “process” felaktigt.",
  "Universum avråder från plastbestick efter kl 15.",
  "En stol kommer kännas mer dominant än vanligt.",
  "Du vinner inget på att rätta någon i onödan, men du kan ändå göra det.",
  "Ett mejl kommer kännas mer personligt än avsett.",
  "En hissresa kan trigga oväntade existentiella tankar.",
  "En kaffekopp får oproportionerlig symbolisk betydelse.",
  "Någon kommer tala länge utan att egentligen säga något.",
  "Ett lösenord kommer kännas emotionellt laddat.",
  "Du ifrågasätter plötsligt placeringen av en möbel.",
  "En kalenderpåminnelse väcker starkare känslor än rimligt.",
  "Universum rekommenderar låg förväntansnivå på möten.",
  "Ett dokument kommer kännas mer definitivt än det är.",
  "En rutin bryts utan att det märks direkt.",
  "Ett paraply blir symboliskt fastän det inte regnar.",
  "Du kommer märka en skylt du aldrig sett förut.",
  "En kortare kö känns misstänkt välorganiserad.",
  "Någon uttalar ditt namn med oväntad tyngd.",
  "Ett tangentbord låter mer än vanligt.",
  "En penna känns plötsligt mer pålitlig än datorn.",
  "Du blir ovanligt investerad i en stapel i ett diagram.",
  "En dörr öppnas med mer auktoritet än nödvändigt.",
  "Ett kvitto känns som en sammanfattning av hela dagen.",
  "En väderapp kommer kännas personligt riktad.",
  "Någon nämner en färg som påverkar ditt humör.",
  "En papperskorg känns ovanligt begriplig.",
  "Du kommer att ifrågasätta en skylt med pilar.",
  "En väntetid får filosofisk tyngd.",
  "En kopp står fel men ingen säger något.",
  "Ett gem känns laddat med oväntad betydelse.",
  "En autokorrigering uppfattas som ett råd.",
  "Universum har en åsikt om din stapling av tallrikar.",
  "En rulltrappa känns som en metafor, tyvärr.",
  "En lampa blinkar som om den försöker säga något.",
];
// ... existing code ...
const avslutningar = [
  "Kvällen känns mer sammanhängande än morgonen.",
  "Dagen avslutas utan dramatik, vilket är positivt.",
  "Ett svar uteblir, men det var inte det viktiga.",
  "Du kommer förstå mer imorgon än idag.",
  "Det räcker att dagen fungerar, den behöver inte imponera.",
  "Kvällen ger mer ro än klarhet.",
  "Dagen rundas av utan tydliga slutsatser.",
  "Ett lugn infinner sig först när kraven försvinner.",
  "Du lämnar dagen med fler observationer än svar.",
  "Det är okej att inget avgörs idag.",
  "Imorgon känns mindre komplicerad i jämförelse.",
  "Du släpper något mentalt, även om du inte vet vad.",
  "Dagens intryck sjunker in långsamt.",
  "Kvällen blir funktionell snarare än minnesvärd.",
  "Det som inte hanns med visar sig vara oviktigt.",
  "Dagen avslutas i ett tempo du själv väljer.",
  "En stillhet infinner sig när du släpper kontrollen.",
  "Kvällen blir mer återställning än reflektion.",
  "Du går till vila med färre frågetecken.",
  "Det som var rörigt tidigare känns plötsligt enklare.",
  "Du hittar en rimlig punkt att stanna vid.",
  "Kvällens lugn är mer praktiskt än poetiskt.",
  "Dagen försvinner utan att göra väsen av sig.",
  "Du märker att du faktiskt hann tillräckligt.",
  "En avslutande detalj ger dagen mening.",
  "Imorgon får bära resten.",
  "Du lämnar något öppet utan att det stör dig.",
  "Kvällen blir tydligare än eftermiddagen.",
  "Ett stilla avslut ger plats för nästa steg.",
  "Dagen blir bättre i efterhand.",
  "Du känner dig mindre splittrad än i morse.",
  "Du går vidare utan att titta bakåt.",
  "Det känns okej att dagens svar är halvfärdiga.",
  "Kvällen känns stabil, om än lågmäld.",
  "Du vaknar lättare av att du inte pressade allt.",
];
// ... existing code ...
const turFarger = [
  "Byrå-beige",
  "Existensgrå",
  "Misstänkt turkos",
  "Passiv pastell",
  "Admin-lila",
  "Obestämd blå",
  "Halvmint",
  "Känslolila",
  "Kommungrön",
  "Försiktig gul",
  "Irriterad korall",
  "Trött terrakotta",
  "Konfliktlavendel",
  "Övermagenta",
  "Dömande marin",
  "Ansvarsgreige",
  "Otydlig röd",
  "Lagom orange",
  "Dammrosa",
  "Godkänd vit",
  "Störigt gul",
  "Knäpp rosa",
  "Skiftande gråblå",
  "Fördröjd beige",
  "Lätt stressad lime",
  "Nästan-svart blå",
  "Mellanmjuk oliv",
  "Vardags-oker",
  "Tvekande teal",
  "Balanserad bärnsten",
  "Pragmatisk plommon",
  "Reservgrön",
  "Något dammig lavendel",
  "Strukturerad sand",
  "Diskret cyan",
  "Halvvarm mauve",
  "Uttänkt petrol",
  "Tyst persika",
  "Robust grå",
  "Saklig salvia",
  "Prövande saffran",
  "Nerjusterad rubin",
];
function slumpTid() {
  const h = Math.floor(Math.random() * 24);
  const m = Math.floor(Math.random() * 60);
  return `${String(h).padStart(2, "0")}:${String(m).padStart(2, "0")}`;
}

/* =========================
   SEEDED RANDOM (kärnan)
========================= */

function hashString(str) {
  let h = 2166136261;
  for (let i = 0; i < str.length; i++) {
    h ^= str.charCodeAt(i);
    h = Math.imul(h, 16777619);
  }
  return h >>> 0;
}

function seededRandom(seed) {
  let s = seed % 2147483647;
  if (s <= 0) s += 2147483646;
  return () => (s = (s * 48271) % 2147483647) / 2147483647;
}

function pick(arr, rnd) {
  return arr[Math.floor(rnd() * arr.length)];
}

/* =========================
   GENERERA DAGENS HOROSKOP
========================= */

function getDagensData() {
  const idag = new Date().toISOString().slice(0, 10);
  const seed = hashString(idag);
  const rnd = seededRandom(seed);

  // 1. Create a shuffled copy of colors
  const farger = [...turFarger];
  for (let i = farger.length - 1; i > 0; i--) {
    const j = Math.floor(rnd() * (i + 1));
    [farger[i], farger[j]] = [farger[j], farger[i]];
  }

  // 2. Create 12 unique times
  const tider = [];
  while (tider.length < 12) {
    const tid = slumpTid(rnd);
    if (!tider.includes(tid)) tider.push(tid);
  }

  return { farger, tider, rnd };
}

function genereraDagshoroskop(tecken) {
  const { farger, tider } = getDagensData();
  const teckenIndex = stjarntecken.indexOf(tecken);

  // We still want the text to be somewhat unique per sign,
  // so we use a sub-seed for the text specifically
  const idagStr = new Date().toISOString().slice(0, 10);
  const textRnd = seededRandom(hashString(`${idagStr}-${tecken}`));

  return {
    text: [
      pick(inledningar, textRnd),
      pick(fokus, textRnd),
      pick(rad, textRnd),
      pick(konsekvens, textRnd),
      pick(absurditeter, textRnd),
      pick(avslutningar, textRnd),
    ].join(" "),
    turFarg: farger[teckenIndex], // Unique color per sign
    turNummer: 1 + Math.floor(textRnd() * 99),
    turTid: tider[teckenIndex],   // Unique time per sign
  };
}

/* =========================
   REAKTIVT BETEENDE
========================= */

watch(
    () => valtTecken.value,
    (t) => {
      horoskop.value = genereraDagshoroskop(t);
    },
    { immediate: true }
);

const idag = computed(() =>
    new Intl.DateTimeFormat("sv-SE", {
      year: "numeric",
      month: "long",
      day: "numeric",
    }).format(new Date())
);
</script>
