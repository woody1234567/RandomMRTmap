<script setup lang="ts">
import { stations, routes } from "@/constants";
import type { LineKey, Station } from "@/constants";
const lineMeta: Record<LineKey, { name: string; color: string }> = {
  red: { name: "淡水信義線", color: "#e9485d" },
  blue: { name: "板南線", color: "#2f79bd" },
  green: { name: "松山新店線", color: "#69a94b" },
  orange: { name: "中和新蘆線", color: "#f29a3f" },
  brown: { name: "文湖線", color: "#a27655" },
  yellow: { name: "環狀線", color: "#f1c632" },
};

const selectedStation = ref(
  stations.find((station) => station.id === "taipei-main")!,
);
const hoveredStation = ref<Station | null>(null);
const isPicking = ref(false);
const hasPicked = ref(false);
let pickToken = 0;
const selectedLines = computed(() =>
  selectedStation.value.lines.map((line) => lineMeta[line]),
);
const wait = (ms: number) => new Promise((resolve) => setTimeout(resolve, ms));

function setHoveredStation(station: Station | null) {
  hoveredStation.value = station;
}

function popupTransform(station: Station) {
  const xOffset = station.x > 500 ? -150 : station.x < 95 ? 12 : -70;
  const yOffset = station.y < 115 ? 16 : -90;

  return `translate(${station.x + xOffset} ${station.y + yOffset})`;
}

async function pickStation() {
  if (isPicking.value) return;
  isPicking.value = true;
  hasPicked.value = false;
  const token = ++pickToken;
  let next = selectedStation.value;
  for (let index = 0; index < 22; index++) {
    const available = stations.filter((station) => station.id !== next.id);
    next = available[Math.floor(Math.random() * available.length)]!;
    selectedStation.value = next;
    await wait(index < 11 ? 65 : 65 + (index - 10) * 23);
    if (token !== pickToken) return;
  }
  hasPicked.value = true;
  isPicking.value = false;
}
onBeforeUnmount(() => {
  pickToken++;
});
</script>

<template>
  <main class="page-shell">
    <nav class="topbar" aria-label="主要導覽">
      <a class="brand" href="#" aria-label="捷運任我行首頁"
        ><span class="brand-mark"><span /></span><span>捷運任我行</span></a
      >
      <div class="nav-links">
        <a class="active" href="#picker">隨機選站</a><a href="#how">使用說明</a>
      </div>
    </nav>
    <section id="picker" class="hero">
      <div class="hero-copy">
        <p class="eyebrow">TAIPEI METRO RANDOMIZER</p>
        <h1>下一站，<span>交給緣分。</span></h1>
        <p class="intro">
          不知道該去哪裡走走？按下按鈕，<br
            class="desktop-only"
          />讓台北捷運帶你展開一場意外的小旅行。
        </p>
      </div>
      <div class="experience-grid">
        <div class="map-card" :class="{ 'is-picking': isPicking }">
          <div class="map-heading">
            <div><span class="live-dot" /> 路網即時選站</div>
            <span>{{ stations.length }} 個精選站點</span>
          </div>
          <div class="map-wrap">
            <svg
              class="metro-map"
              viewBox="35 25 610 555"
              role="img"
              aria-label="台北捷運路線示意圖"
            >
              <g class="route-lines">
                <polyline
                  v-for="route in routes"
                  :key="route.id"
                  :points="route.points"
                  :stroke="lineMeta[route.key].color"
                />
              </g>
              <g class="stations">
                <g
                  v-for="station in stations"
                  :key="station.id"
                  class="station-node"
                  :class="{
                    selected: selectedStation.id === station.id,
                    hovered: hoveredStation?.id === station.id,
                  }"
                  :transform="`translate(${station.x} ${station.y})`"
                  tabindex="0"
                  role="button"
                  :aria-label="`${station.name}，${station.en}，${station.lines.map((line) => lineMeta[line].name).join('、')}`"
                  @mouseenter="setHoveredStation(station)"
                  @mouseleave="setHoveredStation(null)"
                  @focus="setHoveredStation(station)"
                  @blur="setHoveredStation(null)"
                >
                  <circle
                    v-if="station.lines.length > 1"
                    class="transfer-ring"
                    r="7"
                  />
                  <circle
                    class="station-dot"
                    :r="selectedStation.id === station.id ? 6 : 3.3"
                  />
                  <g
                    v-if="selectedStation.id === station.id"
                    class="selection-marker"
                  >
                    <circle class="pulse-ring" r="14" />
                    <path
                      d="M0-37c-10 0-18 8-18 18 0 14 18 31 18 31s18-17 18-31c0-10-8-18-18-18Z"
                    />
                    <circle cy="-19" r="5" />
                  </g>
                  <text
                    v-if="
                      station.lines.length > 1 ||
                      selectedStation.id === station.id ||
                      [
                        'tamsui',
                        'xiangshan',
                        'xindian',
                        'taipei-zoo',
                        'songshan',
                        'dingpu',
                        'luzhou',
                        'huilong',
                      ].includes(station.id)
                    "
                    :x="
                      station.id === 'xiangshan' || station.id === 'songshan'
                        ? -10
                        : 10
                    "
                    :y="station.id === 'taipei-main' ? 19 : -9"
                    :text-anchor="
                      station.id === 'xiangshan' || station.id === 'songshan'
                        ? 'end'
                        : 'start'
                    "
                  >
                    {{ station.name }}
                  </text>
                </g>
              </g>
              <g
                v-if="hoveredStation"
                class="station-popup"
                :transform="popupTransform(hoveredStation)"
              >
                <rect width="138" height="70" rx="10" />
                <text class="popup-name" x="14" y="24">
                  {{ hoveredStation.name }}
                </text>
                <text class="popup-en" x="14" y="41">
                  {{ hoveredStation.en }}
                </text>
                <g class="popup-lines" transform="translate(14 56)">
                  <g
                    v-for="(line, index) in hoveredStation.lines"
                    :key="line"
                    :transform="`translate(${index * 18} 0)`"
                  >
                    <circle r="4" :fill="lineMeta[line].color" />
                  </g>
                  <text :x="hoveredStation.lines.length * 18 + 2" y="3">
                    {{
                      hoveredStation.lines
                        .map((line) => lineMeta[line].name)
                        .join("、")
                    }}
                  </text>
                </g>
              </g>
            </svg>
            <div class="map-compass" aria-hidden="true">N<span>↑</span></div>
          </div>
          <div class="line-legend">
            <span v-for="(meta, key) in lineMeta" :key="key"
              ><i :style="{ background: meta.color }" />{{ meta.name }}</span
            >
          </div>
        </div>
        <aside class="picker-card">
          <p class="result-label">
            {{
              isPicking
                ? "正在穿梭路網…"
                : hasPicked
                  ? "你的目的地是"
                  : "準備好出發了嗎？"
            }}
          </p>
          <div
            class="result-orbit"
            :class="{ spinning: isPicking, landed: hasPicked }"
          >
            <span class="orbit orbit-one" /><span class="orbit orbit-two" />
            <div class="station-code">
              <span>{{
                selectedStation.lines
                  .map((line) => line.charAt(0).toUpperCase())
                  .join("")
              }}</span>
            </div>
          </div>
          <Transition name="station" mode="out-in"
            ><div :key="selectedStation.id" class="station-result">
              <h2>{{ selectedStation.name }}</h2>
              <p>{{ selectedStation.en }}</p>
              <div class="line-tags">
                <span v-for="line in selectedLines" :key="line.name"
                  ><i :style="{ background: line.color }" />{{
                    line.name
                  }}</span
                >
              </div>
            </div></Transition
          >
          <button
            class="pick-button"
            :disabled="isPicking"
            @click="pickStation"
          >
            <span class="button-icon">↗</span
            ><span>{{
              isPicking ? "選站中…" : hasPicked ? "再選一次" : "開始選站"
            }}</span>
          </button>
          <p class="hint"><span>✦</span> 每一次選擇，都是一次新的相遇</p>
        </aside>
      </div>
    </section>
    <section id="how" class="how-section">
      <p>HOW IT WORKS</p>
      <h2>按一下，下一站就有答案。</h2>
      <div class="steps">
        <article><b>01</b><span>按下開始選站</span></article>
        <article><b>02</b><span>看著站點穿梭</span></article>
        <article><b>03</b><span>帶著好奇心出發</span></article>
      </div>
    </section>
  </main>
</template>
