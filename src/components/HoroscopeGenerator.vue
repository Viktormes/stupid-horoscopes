<template>
  <div class="relative flex min-h-screen flex-col overflow-hidden bg-slate-950 text-slate-100">
    <div ref="bgRef" class="pointer-events-none absolute inset-0 kosmos-bg">
      <div class="nebula-opera nebula-opera-1"></div>
      <div class="nebula-opera nebula-opera-2"></div>
      <div class="stars-opera"></div>
      <div class="absolute inset-0 vignette"></div>
    </div>

    <header class="relative z-10 border-b border-slate-800/80 bg-slate-950/70 backdrop-blur">
      <div class="mx-auto flex w-full max-w-6xl flex-col gap-3 px-4 py-4 sm:px-6 md:flex-row md:items-center md:justify-between">
        <button
            class="w-fit rounded-lg text-left transition hover:opacity-85"
            type="button"
            @click="activePage = 'home'"
        >
          <img
              alt=""
              class="h-12 w-12 rounded-lg object-cover"
              src="/horrific-horoscopes-mark.png"
          />
          <span class="sr-only">Horrific Horoscopes</span>
        </button>

        <nav class="flex flex-wrap gap-2" aria-label="Huvudnavigation">
          <button
              v-for="page in pages"
              :key="page.id"
              :class="navButtonClass(page.id)"
              type="button"
              @click="activePage = page.id"
          >
            {{ page.label }}
          </button>
        </nav>
      </div>
    </header>

    <main class="relative z-10 mx-auto flex w-full max-w-6xl flex-1 flex-col gap-5 px-4 py-5 sm:px-6 lg:py-8">
      <section v-if="activePage === 'home'">
        <div class="space-y-5">
          <section class="rounded-lg border border-slate-800 bg-slate-900/50 p-5 shadow-xl backdrop-blur">
            <div class="flex flex-wrap items-end justify-between gap-4">
              <div>
                <p class="text-sm font-medium uppercase tracking-wider text-cyan-200/80">Dagens horoskop</p>
                <h1 class="mt-1 text-3xl font-semibold tracking-tight text-slate-50 sm:text-4xl">
                  {{ valtTecken }}
                </h1>
                <p class="mt-2 text-slate-300">{{ idag }}</p>
              </div>

              <label class="flex flex-col gap-2 text-sm text-slate-300">
                Ditt tecken
                <select
                    v-model="valtTecken"
                    class="h-11 rounded-lg border border-slate-700 bg-slate-950/70 px-3 text-slate-100 outline-none hover:ring-1 hover:ring-slate-400"
                >
                  <option v-for="t in stjarntecken" :key="t" :value="t">{{ t }}</option>
                </select>
              </label>
            </div>

            <div v-if="horoskop" class="mt-6 space-y-5">
              <p class="max-w-3xl text-lg leading-relaxed text-slate-100">{{ horoskop.text }}</p>

              <div class="grid grid-cols-1 gap-3 sm:grid-cols-3">
                <div class="rounded-lg border border-slate-800 bg-slate-950/45 p-3">
                  <div class="text-xs text-slate-400">Tur-färg</div>
                  <div class="mt-1 font-semibold">{{ horoskop.turFarg }}</div>
                </div>

                <div class="rounded-lg border border-slate-800 bg-slate-950/45 p-3">
                  <div class="text-xs text-slate-400">Tur-nummer</div>
                  <div class="mt-1 font-semibold">{{ horoskop.turNummer }}</div>
                </div>

                <div class="rounded-lg border border-slate-800 bg-slate-950/45 p-3">
                  <div class="text-xs text-slate-400">Tur-tid</div>
                  <div class="mt-1 font-semibold">{{ horoskop.turTid }}</div>
                </div>
              </div>
            </div>
          </section>

          <p class="text-center text-xs text-slate-500">
            Disclaimer: Kosmos är inte en medicinteknisk produkt.
          </p>
        </div>
      </section>

      <section v-else-if="activePage === 'signs'" class="rounded-lg border border-slate-800 bg-slate-900/50 p-5 shadow-xl backdrop-blur">
        <p class="text-sm font-medium uppercase tracking-wider text-cyan-200/80">Välj stjärntecken</p>
        <h1 class="mt-1 text-3xl font-semibold tracking-tight text-slate-50">Ditt dagliga kosmiska grundfel</h1>
        <div class="mt-6 grid grid-cols-2 gap-3 sm:grid-cols-3 lg:grid-cols-4">
          <button
              v-for="tecken in stjarntecken"
              :key="tecken"
              :class="signButtonClass(tecken)"
              type="button"
              @click="selectSign(tecken)"
          >
            {{ tecken }}
          </button>
        </div>
      </section>

      <section v-else-if="activePage === 'about'">
        <article class="rounded-lg border border-slate-800 bg-slate-900/50 p-5 shadow-xl backdrop-blur">
          <p class="text-sm font-medium uppercase tracking-wider text-cyan-200/80">Om</p>
          <h1 class="mt-1 text-3xl font-semibold tracking-tight text-slate-50">Horoskop med tveksam precision</h1>
          <div class="mt-5 space-y-4 leading-7 text-slate-300">
            <p>
              Horrific Horoscopes gör dagliga horoskop av slumpade textbanker, sparsam astrologisk attityd och vardaglig administrativ oro.
            </p>
            <p>
              Texterna är underhållning. De ska inte användas för medicinska, juridiska, ekonomiska eller emotionellt kostsamma beslut.
            </p>
          </div>
        </article>
      </section>

      <section v-else-if="activePage === 'privacy'">
        <article class="rounded-lg border border-slate-800 bg-slate-900/50 p-5 shadow-xl backdrop-blur">
          <p class="text-sm font-medium uppercase tracking-wider text-cyan-200/80">Integritet</p>
          <h1 class="mt-1 text-3xl font-semibold tracking-tight text-slate-50">Privacy policy</h1>
          <div class="mt-5 space-y-4 leading-7 text-slate-300">
            <p>
              Horrific Horoscopes sparar ditt valda stjärntecken i webbläsarens localStorage så att samma tecken öppnas nästa gång.
            </p>
            <p>
              Just nu skickas ingen personlig information till någon server från den här funktionen. Horoskopet skapas lokalt i webbläsaren från appens textbanker.
            </p>
            <p>
              Webbplatsen har en förberedd annonsplats, men visar inga riktiga annonser än. När annonser aktiveras kan annonsleverantörer använda cookies eller liknande tekniker för att mäta annonser, begränsa upprepning och visa relevanta annonser.
            </p>
            <p>
              Om Google AdSense eller en liknande annonstjänst används kommer besökare i EU/EES, Storbritannien och Schweiz att få möjlighet att hantera samtycke via en godkänd samtyckeslösning innan personanpassade annonser visas.
            </p>
            <p>
              Den här policyn kommer att uppdateras innan riktiga annonser, analysverktyg eller andra externa tjänster aktiveras.
            </p>
          </div>
        </article>
      </section>

      <div class="ad-slot">
        <span>Annons</span>
      </div>
    </main>

    <footer class="relative z-10 border-t border-slate-800/80 bg-slate-950/70 px-4 py-5 text-center text-xs text-slate-500 backdrop-blur">
      © 2026 Horrific Horoscopes. Kosmos tar inget ansvar.
    </footer>
  </div>
</template>

<script setup>
import { computed, onBeforeUnmount, onMounted, ref, watch } from "vue";
import { horoscopeTextBanks, turFarger } from "../data/horoscopeContent";

const bgRef = ref(null);
const activePage = ref("home");

const pages = [
  { id: "home", label: "Horoskop" },
  { id: "signs", label: "Tecken" },
  { id: "about", label: "Om" },
  { id: "privacy", label: "Integritet" },
];

function navButtonClass(pageId) {
  const isActive = activePage.value === pageId;
  return [
    "rounded-lg px-3 py-2 text-sm font-medium transition",
    isActive
        ? "bg-cyan-300 text-slate-950"
        : "bg-slate-900/70 text-slate-300 hover:bg-slate-800 hover:text-slate-50",
  ];
}

function signButtonClass(tecken) {
  const isActive = valtTecken.value === tecken;
  return [
    "rounded-lg border px-3 py-4 text-left font-medium transition",
    isActive
        ? "border-cyan-300 bg-cyan-300 text-slate-950"
        : "border-slate-800 bg-slate-950/50 text-slate-100 hover:border-slate-500 hover:bg-slate-900",
  ];
}

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

const valtTeckenStorageKey = "stupid-horoscopes:valt-tecken";
const sparatTecken = window.localStorage.getItem(valtTeckenStorageKey);
const valtTecken = ref(stjarntecken.includes(sparatTecken) ? sparatTecken : "Fiskarna");
const horoskop = ref(null);

function selectSign(tecken) {
  valtTecken.value = tecken;
  activePage.value = "home";
}

function slumpTid(rnd = Math.random) {
  const h = Math.floor(rnd() * 24);
  const m = Math.floor(rnd() * 60);
  return `${String(h).padStart(2, "0")}:${String(m).padStart(2, "0")}`;
}

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

function getDagensData() {
  const idag = new Date().toISOString().slice(0, 10);
  const seed = hashString(idag);
  const rnd = seededRandom(seed);

  const farger = [...turFarger];
  for (let i = farger.length - 1; i > 0; i--) {
    const j = Math.floor(rnd() * (i + 1));
    [farger[i], farger[j]] = [farger[j], farger[i]];
  }

  const tider = [];
  while (tider.length < 12) {
    const tid = slumpTid(rnd);
    if (!tider.includes(tid)) tider.push(tid);
  }

  return { farger, tider };
}

function genereraDagshoroskop(tecken) {
  const { farger, tider } = getDagensData();
  const teckenIndex = stjarntecken.indexOf(tecken);
  const idagStr = new Date().toISOString().slice(0, 10);
  const textRnd = seededRandom(hashString(`${idagStr}-${tecken}`));

  return {
    text: [
      pick(horoscopeTextBanks.inledningar, textRnd),
      pick(horoscopeTextBanks.fokus, textRnd),
      pick(horoscopeTextBanks.rad, textRnd),
      pick(horoscopeTextBanks.konsekvens, textRnd),
      pick(horoscopeTextBanks.absurditeter, textRnd),
      pick(horoscopeTextBanks.avslutningar, textRnd),
    ].join(" "),
    turFarg: farger[teckenIndex],
    turNummer: 1 + Math.floor(textRnd() * 99),
    turTid: tider[teckenIndex],
  };
}

watch(
    () => valtTecken.value,
    (t) => {
      window.localStorage.setItem(valtTeckenStorageKey, t);
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
