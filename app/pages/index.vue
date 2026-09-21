<script setup lang="ts">
type LineKey = 'red' | 'blue' | 'green' | 'orange' | 'brown' | 'yellow'
interface Station { id: string, name: string, en: string, x: number, y: number, lines: LineKey[] }

const lineMeta: Record<LineKey, { name: string, color: string }> = {
  red: { name: '淡水信義線', color: '#e9485d' }, blue: { name: '板南線', color: '#2f79bd' },
  green: { name: '松山新店線', color: '#69a94b' }, orange: { name: '中和新蘆線', color: '#f29a3f' },
  brown: { name: '文湖線', color: '#a27655' }, yellow: { name: '環狀線', color: '#f1c632' }
}

const stations: Station[] = [
  { id: 'tamsui', name: '淡水', en: 'Tamsui', x: 42, y: 44, lines: ['red'] },
  { id: 'hongshulin', name: '紅樹林', en: 'Hongshulin', x: 68, y: 44, lines: ['red'] },
  { id: 'zhuwei', name: '竹圍', en: 'Zhuwei', x: 94, y: 44, lines: ['red'] },
  { id: 'guandu', name: '關渡', en: 'Guandu', x: 120, y: 44, lines: ['red'] },
  { id: 'zhongyi', name: '忠義', en: 'Zhongyi', x: 146, y: 44, lines: ['red'] },
  { id: 'fuxinggang', name: '復興崗', en: 'Fuxinggang', x: 172, y: 44, lines: ['red'] },
  { id: 'beitou', name: '北投', en: 'Beitou', x: 198, y: 44, lines: ['red'] },
  { id: 'xinbeitou', name: '新北投', en: 'Xinbeitou', x: 226, y: 44, lines: ['red'] },
  { id: 'qiyan', name: '奇岩', en: 'Qiyan', x: 214, y: 61, lines: ['red'] },
  { id: 'qilian', name: '唭哩岸', en: 'Qilian', x: 214, y: 82, lines: ['red'] },
  { id: 'shipai', name: '石牌', en: 'Shipai', x: 214, y: 103, lines: ['red'] },
  { id: 'mingde', name: '明德', en: 'Mingde', x: 214, y: 124, lines: ['red'] },
  { id: 'zhishan', name: '芝山', en: 'Zhishan', x: 214, y: 145, lines: ['red'] },
  { id: 'shilin', name: '士林', en: 'Shilin', x: 214, y: 166, lines: ['red'] },
  { id: 'jiantan', name: '劍潭', en: 'Jiantan', x: 214, y: 187, lines: ['red'] },
  { id: 'yuanshan', name: '圓山', en: 'Yuanshan', x: 214, y: 228, lines: ['red'] },
  { id: 'shuanglian', name: '雙連', en: 'Shuanglian', x: 214, y: 248, lines: ['red'] },
  { id: 'minquan', name: '民權西路', en: 'Minquan W. Rd.', x: 214, y: 216, lines: ['red', 'orange'] },
  { id: 'zhongshan', name: '中山', en: 'Zhongshan', x: 214, y: 272, lines: ['red', 'green'] },
  { id: 'taipei-main', name: '台北車站', en: 'Taipei Main Station', x: 214, y: 327, lines: ['red', 'blue'] },
  { id: 'ntu-hospital', name: '台大醫院', en: 'NTU Hospital', x: 214, y: 349, lines: ['red'] },
  { id: 'cks', name: '中正紀念堂', en: 'C.K.S. Memorial Hall', x: 226, y: 373, lines: ['red', 'green'] },
  { id: 'dongmen', name: '東門', en: 'Dongmen', x: 258, y: 373, lines: ['red', 'orange'] },
  { id: 'daan-park', name: '大安森林公園', en: 'Daan Park', x: 290, y: 373, lines: ['red'] },
  { id: 'daan', name: '大安', en: 'Daan', x: 322, y: 373, lines: ['red', 'brown'] },
  { id: 'xinyi-anhe', name: '信義安和', en: 'Xinyi Anhe', x: 366, y: 373, lines: ['red'] },
  { id: 'taipei101', name: '台北101／世貿', en: 'Taipei 101 / World Trade Center', x: 410, y: 373, lines: ['red'] },
  { id: 'xiangshan', name: '象山', en: 'Xiangshan', x: 454, y: 373, lines: ['red'] },
  { id: 'guangci', name: '廣慈／奉天宮', en: 'Guangci / Fengtian Temple', x: 498, y: 373, lines: ['red'] },
  { id: 'dingpu', name: '頂埔', en: 'Dingpu', x: 99, y: 531, lines: ['blue'] },
  { id: 'yongning', name: '永寧', en: 'Yongning', x: 99, y: 496, lines: ['blue'] },
  { id: 'tucheng', name: '土城', en: 'Tucheng', x: 99, y: 461, lines: ['blue'] },
  { id: 'haishan', name: '海山', en: 'Haishan', x: 99, y: 426, lines: ['blue'] },
  { id: 'far-eastern-hospital', name: '亞東醫院', en: 'Far Eastern Hospital', x: 99, y: 391, lines: ['blue'] },
  { id: 'fuzhong', name: '府中', en: 'Fuzhong', x: 99, y: 356, lines: ['blue'] },
  { id: 'banqiao', name: '板橋', en: 'Banqiao', x: 118, y: 327, lines: ['blue', 'yellow'] },
  { id: 'xinpu', name: '新埔', en: 'Xinpu', x: 142, y: 327, lines: ['blue'] },
  { id: 'jiangzicui', name: '江子翠', en: 'Jiangzicui', x: 162, y: 327, lines: ['blue'] },
  { id: 'longshan', name: '龍山寺', en: 'Longshan Temple', x: 178, y: 327, lines: ['blue'] },
  { id: 'ximen', name: '西門', en: 'Ximen', x: 184, y: 327, lines: ['blue', 'green'] },
  { id: 'shandao', name: '善導寺', en: 'Shandao Temple', x: 246, y: 327, lines: ['blue'] },
  { id: 'zhongxiao-xinsheng', name: '忠孝新生', en: 'Zhongxiao Xinsheng', x: 290, y: 327, lines: ['blue', 'orange'] },
  { id: 'zhongxiao-fuxing', name: '忠孝復興', en: 'Zhongxiao Fuxing', x: 322, y: 327, lines: ['blue', 'brown'] },
  { id: 'zhongxiao-dunhua', name: '忠孝敦化', en: 'Zhongxiao Dunhua', x: 358, y: 327, lines: ['blue'] },
  { id: 'sun-yat-sen', name: '國父紀念館', en: 'Sun Yat-Sen Memorial Hall', x: 394, y: 327, lines: ['blue'] },
  { id: 'city-hall', name: '市政府', en: 'Taipei City Hall', x: 430, y: 327, lines: ['blue'] },
  { id: 'yongchun', name: '永春', en: 'Yongchun', x: 466, y: 327, lines: ['blue'] },
  { id: 'houshanpi', name: '後山埤', en: 'Houshanpi', x: 502, y: 327, lines: ['blue'] },
  { id: 'kunyang', name: '昆陽', en: 'Kunyang', x: 538, y: 327, lines: ['blue'] },
  { id: 'nangang', name: '南港', en: 'Nangang', x: 570, y: 327, lines: ['blue'] },
  { id: 'nangang-exhibition', name: '南港展覽館', en: 'Nangang Exhibition Center', x: 570, y: 305, lines: ['blue', 'brown'] },
  { id: 'songshan', name: '松山', en: 'Songshan', x: 456, y: 272, lines: ['green'] },
  { id: 'nanjing-sanmin', name: '南京三民', en: 'Nanjing Sanmin', x: 414, y: 272, lines: ['green'] },
  { id: 'nanjing-fuxing', name: '南京復興', en: 'Nanjing Fuxing', x: 366, y: 272, lines: ['green', 'brown'] },
  { id: 'songjiang-nanjing', name: '松江南京', en: 'Songjiang Nanjing', x: 290, y: 272, lines: ['green', 'orange'] },
  { id: 'beimen', name: '北門', en: 'Beimen', x: 184, y: 272, lines: ['green'] },
  { id: 'xiaonanmen', name: '小南門', en: 'Xiaonanmen', x: 184, y: 349, lines: ['green'] },
  { id: 'guting', name: '古亭', en: 'Guting', x: 246, y: 395, lines: ['green', 'orange'] },
  { id: 'taipower-building', name: '台電大樓', en: 'Taipower Building', x: 264, y: 414, lines: ['green'] },
  { id: 'gongguan', name: '公館', en: 'Gongguan', x: 282, y: 433, lines: ['green'] },
  { id: 'wanlong', name: '萬隆', en: 'Wanlong', x: 294, y: 454, lines: ['green'] },
  { id: 'jingmei', name: '景美', en: 'Jingmei', x: 298, y: 476, lines: ['green'] },
  { id: 'dapinglin', name: '大坪林', en: 'Dapinglin', x: 305, y: 499, lines: ['green', 'yellow'] },
  { id: 'qizhang', name: '七張', en: 'Qizhang', x: 305, y: 522, lines: ['green'] },
  { id: 'xindian-district-office', name: '新店區公所', en: 'Xindian District Office', x: 305, y: 544, lines: ['green'] },
  { id: 'xindian', name: '新店', en: 'Xindian', x: 305, y: 566, lines: ['green'] },
  { id: 'xiaobitan', name: '小碧潭', en: 'Xiaobitan', x: 280, y: 550, lines: ['green'] },
  { id: 'luzhou', name: '蘆洲', en: 'Luzhou', x: 78, y: 132, lines: ['orange'] },
  { id: 'sanmin-senior-high', name: '三民高中', en: 'Sanmin Senior High School', x: 100, y: 153, lines: ['orange'] },
  { id: 'st-ignatius-high', name: '徐匯中學', en: 'St. Ignatius High School', x: 122, y: 174, lines: ['orange'] },
  { id: 'sanhe-junior-high', name: '三和國中', en: 'Sanhe Junior High School', x: 144, y: 195, lines: ['orange'] },
  { id: 'sanchong-elementary', name: '三重國小', en: 'Sanchong Elementary School', x: 166, y: 216, lines: ['orange'] },
  { id: 'huilong', name: '迴龍', en: 'Huilong', x: 35, y: 382, lines: ['orange'] },
  { id: 'danfeng', name: '丹鳳', en: 'Danfeng', x: 35, y: 360, lines: ['orange'] },
  { id: 'fu-jen-university', name: '輔大', en: 'Fu Jen University', x: 64, y: 335, lines: ['orange'] },
  { id: 'xinzhuang', name: '新莊', en: 'Xinzhuang', x: 86, y: 314, lines: ['orange'] },
  { id: 'touqianzhuang', name: '頭前庄', en: 'Touqianzhuang', x: 108, y: 293, lines: ['orange', 'yellow'] },
  { id: 'xianse-temple', name: '先嗇宮', en: 'Xianse Temple', x: 130, y: 272, lines: ['orange'] },
  { id: 'sanchong', name: '三重', en: 'Sanchong', x: 152, y: 251, lines: ['orange'] },
  { id: 'cailiao', name: '菜寮', en: 'Cailiao', x: 166, y: 237, lines: ['orange'] },
  { id: 'taipei-bridge', name: '台北橋', en: 'Taipei Bridge', x: 180, y: 223, lines: ['orange'] },
  { id: 'daqiaotou', name: '大橋頭', en: 'Daqiaotou', x: 190, y: 216, lines: ['orange'] },
  { id: 'zhongshan-elementary', name: '中山國小', en: 'Zhongshan Elementary School', x: 258, y: 230, lines: ['orange'] },
  { id: 'xingtian-temple', name: '行天宮', en: 'Xingtian Temple', x: 276, y: 250, lines: ['orange'] },
  { id: 'dingxi', name: '頂溪', en: 'Dingxi', x: 250, y: 416, lines: ['orange'] },
  { id: 'yongan-market', name: '永安市場', en: 'Yongan Market', x: 250, y: 437, lines: ['orange'] },
  { id: 'jing-an', name: '景安', en: 'Jing-an', x: 250, y: 499, lines: ['orange', 'yellow'] },
  { id: 'nanshijiao', name: '南勢角', en: 'Nanshijiao', x: 250, y: 530, lines: ['orange'] },
  { id: 'taipei-zoo', name: '動物園', en: 'Taipei Zoo', x: 472, y: 532, lines: ['brown'] },
  { id: 'muzha', name: '木柵', en: 'Muzha', x: 454, y: 511, lines: ['brown'] },
  { id: 'wanfang-community', name: '萬芳社區', en: 'Wanfang Community', x: 434, y: 490, lines: ['brown'] },
  { id: 'wanfang-hospital', name: '萬芳醫院', en: 'Wanfang Hospital', x: 414, y: 469, lines: ['brown'] },
  { id: 'xinhai', name: '辛亥', en: 'Xinhai', x: 394, y: 448, lines: ['brown'] },
  { id: 'linguang', name: '麟光', en: 'Linguang', x: 374, y: 427, lines: ['brown'] },
  { id: 'liuzhangli', name: '六張犁', en: 'Liuzhangli', x: 354, y: 406, lines: ['brown'] },
  { id: 'technology', name: '科技大樓', en: 'Technology Building', x: 334, y: 385, lines: ['brown'] },
  { id: 'zhongshan-junior-high', name: '中山國中', en: 'Zhongshan Junior High School', x: 350, y: 250, lines: ['brown'] },
  { id: 'songshan-airport', name: '松山機場', en: 'Songshan Airport', x: 366, y: 216, lines: ['brown'] },
  { id: 'dazhi', name: '大直', en: 'Dazhi', x: 382, y: 195, lines: ['brown'] },
  { id: 'jiannan-road', name: '劍南路', en: 'Jiannan Road', x: 406, y: 184, lines: ['brown'] },
  { id: 'xihu', name: '西湖', en: 'Xihu', x: 438, y: 184, lines: ['brown'] },
  { id: 'gangqian', name: '港墘', en: 'Gangqian', x: 470, y: 184, lines: ['brown'] },
  { id: 'wende', name: '文德', en: 'Wende', x: 502, y: 184, lines: ['brown'] },
  { id: 'neihu', name: '內湖', en: 'Neihu', x: 534, y: 184, lines: ['brown'] },
  { id: 'dahu-park', name: '大湖公園', en: 'Dahu Park', x: 558, y: 195, lines: ['brown'] },
  { id: 'huzhou', name: '葫洲', en: 'Huzhou', x: 570, y: 216, lines: ['brown'] },
  { id: 'donghu', name: '東湖', en: 'Donghu', x: 570, y: 238, lines: ['brown'] },
  { id: 'nangang-software-park', name: '南港軟體園區', en: 'Nangang Software Park', x: 570, y: 272, lines: ['brown'] },
  { id: 'new-taipei-industrial', name: '新北產業園區', en: 'New Taipei Industrial Park', x: 100, y: 245, lines: ['yellow'] },
  { id: 'xinpu-minsheng', name: '新埔民生', en: 'Xinpu Minsheng', x: 118, y: 306, lines: ['yellow'] },
  { id: 'banxin', name: '板新', en: 'Banxin', x: 136, y: 349, lines: ['yellow'] },
  { id: 'zhongyuan', name: '中原', en: 'Zhongyuan', x: 174, y: 412, lines: ['yellow'] },
  { id: 'qiaohe', name: '橋和', en: 'Qiaohe', x: 194, y: 433, lines: ['yellow'] },
  { id: 'zhonghe', name: '中和', en: 'Zhonghe', x: 214, y: 454, lines: ['yellow'] },
  { id: 'jingping', name: '景平', en: 'Jingping', x: 265, y: 499, lines: ['yellow'] },
  { id: 'xiulang-bridge', name: '秀朗橋', en: 'Xiulang Bridge', x: 280, y: 499, lines: ['yellow'] },
  { id: 'shisizhang', name: '十四張', en: 'Shisizhang', x: 290, y: 499, lines: ['yellow'] }
]

const routes: { id: string, key: LineKey, points: string }[] = [
  { id: 'red-main', key: 'red', points: '42,44 68,44 94,44 120,44 146,44 172,44 198,44 214,61 214,82 214,103 214,124 214,145 214,166 214,187 214,208 214,248 214,272 214,294 214,327 214,349 226,373 258,373 290,373 322,373 366,373 410,373 454,373 498,373' },
  { id: 'red-xinbeitou', key: 'red', points: '198,44 226,44' },
  { id: 'blue', key: 'blue', points: '99,531 99,496 99,461 99,426 99,391 99,356 118,327 142,327 162,327 178,327 184,327 214,327 246,327 290,327 322,327 358,327 394,327 430,327 466,327 502,327 538,327 570,327 570,305' },
  { id: 'green-main', key: 'green', points: '456,272 414,272 366,272 290,272 214,272 184,272 184,327 184,349 226,373 246,395 264,414 282,433 294,454 298,476 305,499 305,522 305,544 305,566' },
  { id: 'green-xiaobitan', key: 'green', points: '305,522 280,550' },
  { id: 'orange-luzhou', key: 'orange', points: '78,132 100,153 122,174 144,195 166,216 190,216' },
  { id: 'orange-huilong', key: 'orange', points: '35,382 35,360 64,335 86,314 108,293 130,272 152,251 166,237 180,223 190,216' },
  { id: 'orange-main', key: 'orange', points: '190,216 214,216 258,230 276,250 290,272 290,327 258,373 246,395 250,416 250,437 250,499 250,530' },
  { id: 'brown', key: 'brown', points: '472,532 454,511 434,490 414,469 394,448 374,427 354,406 334,385 322,373 322,327 350,250 366,216 382,195 406,184 438,184 470,184 502,184 534,184 558,195 570,216 570,238 570,272 570,305' },
  { id: 'yellow', key: 'yellow', points: '100,245 108,293 118,306 118,327 136,349 174,412 194,433 214,454 250,499 265,499 280,499 295,499' }
]

const selectedStation = ref(stations.find(station => station.id === 'taipei-main')!)
const hoveredStation = ref<Station | null>(null)
const isPicking = ref(false)
const hasPicked = ref(false)
let pickToken = 0
const selectedLines = computed(() => selectedStation.value.lines.map(line => lineMeta[line]))
const wait = (ms: number) => new Promise(resolve => setTimeout(resolve, ms))

function setHoveredStation(station: Station | null) {
  hoveredStation.value = station
}

function popupTransform(station: Station) {
  const xOffset = station.x > 500 ? -150 : station.x < 95 ? 12 : -70
  const yOffset = station.y < 115 ? 16 : -90

  return `translate(${station.x + xOffset} ${station.y + yOffset})`
}

async function pickStation() {
  if (isPicking.value) return
  isPicking.value = true
  hasPicked.value = false
  const token = ++pickToken
  let next = selectedStation.value
  for (let index = 0; index < 22; index++) {
    const available = stations.filter(station => station.id !== next.id)
    next = available[Math.floor(Math.random() * available.length)]!
    selectedStation.value = next
    await wait(index < 11 ? 65 : 65 + (index - 10) * 23)
    if (token !== pickToken) return
  }
  hasPicked.value = true
  isPicking.value = false
}
onBeforeUnmount(() => { pickToken++ })
</script>

<template>
  <main class="page-shell">
    <nav class="topbar" aria-label="主要導覽">
      <a class="brand" href="#" aria-label="捷運任我行首頁"><span class="brand-mark"><span /></span><span>捷運任我行</span></a>
      <div class="nav-links"><a class="active" href="#picker">隨機選站</a><a href="#how">使用說明</a></div>
    </nav>
    <section id="picker" class="hero">
      <div class="hero-copy">
        <p class="eyebrow">TAIPEI METRO RANDOMIZER</p>
        <h1>下一站，<span>交給緣分。</span></h1>
        <p class="intro">不知道該去哪裡走走？按下按鈕，<br class="desktop-only">讓台北捷運帶你展開一場意外的小旅行。</p>
      </div>
      <div class="experience-grid">
        <div class="map-card" :class="{ 'is-picking': isPicking }">
          <div class="map-heading"><div><span class="live-dot" /> 路網即時選站</div><span>{{ stations.length }} 個精選站點</span></div>
          <div class="map-wrap">
            <svg class="metro-map" viewBox="35 25 610 555" role="img" aria-label="台北捷運路線示意圖">
              <g class="route-lines"><polyline v-for="route in routes" :key="route.id" :points="route.points" :stroke="lineMeta[route.key].color" /></g>
              <g class="stations">
                <g
                  v-for="station in stations"
                  :key="station.id"
                  class="station-node"
                  :class="{ selected: selectedStation.id === station.id, hovered: hoveredStation?.id === station.id }"
                  :transform="`translate(${station.x} ${station.y})`"
                  tabindex="0"
                  role="button"
                  :aria-label="`${station.name}，${station.en}，${station.lines.map(line => lineMeta[line].name).join('、')}`"
                  @mouseenter="setHoveredStation(station)"
                  @mouseleave="setHoveredStation(null)"
                  @focus="setHoveredStation(station)"
                  @blur="setHoveredStation(null)"
                >
                  <circle v-if="station.lines.length > 1" class="transfer-ring" r="7" />
                  <circle class="station-dot" :r="selectedStation.id === station.id ? 6 : 3.3" />
                  <g v-if="selectedStation.id === station.id" class="selection-marker"><circle class="pulse-ring" r="14" /><path d="M0-37c-10 0-18 8-18 18 0 14 18 31 18 31s18-17 18-31c0-10-8-18-18-18Z" /><circle cy="-19" r="5" /></g>
                  <text v-if="station.lines.length > 1 || selectedStation.id === station.id || ['tamsui', 'xiangshan', 'xindian', 'taipei-zoo', 'songshan', 'dingpu', 'luzhou', 'huilong'].includes(station.id)" :x="station.id === 'xiangshan' || station.id === 'songshan' ? -10 : 10" :y="station.id === 'taipei-main' ? 19 : -9" :text-anchor="station.id === 'xiangshan' || station.id === 'songshan' ? 'end' : 'start'">{{ station.name }}</text>
                </g>
              </g>
              <g v-if="hoveredStation" class="station-popup" :transform="popupTransform(hoveredStation)">
                <rect width="138" height="70" rx="10" />
                <text class="popup-name" x="14" y="24">{{ hoveredStation.name }}</text>
                <text class="popup-en" x="14" y="41">{{ hoveredStation.en }}</text>
                <g class="popup-lines" transform="translate(14 56)">
                  <g v-for="(line, index) in hoveredStation.lines" :key="line" :transform="`translate(${index * 18} 0)`">
                    <circle r="4" :fill="lineMeta[line].color" />
                  </g>
                  <text :x="hoveredStation.lines.length * 18 + 2" y="3">{{ hoveredStation.lines.map(line => lineMeta[line].name).join('、') }}</text>
                </g>
              </g>
            </svg>
            <div class="map-compass" aria-hidden="true">N<span>↑</span></div>
          </div>
          <div class="line-legend"><span v-for="(meta, key) in lineMeta" :key="key"><i :style="{ background: meta.color }" />{{ meta.name }}</span></div>
        </div>
        <aside class="picker-card">
          <p class="result-label">{{ isPicking ? '正在穿梭路網…' : hasPicked ? '你的目的地是' : '準備好出發了嗎？' }}</p>
          <div class="result-orbit" :class="{ spinning: isPicking, landed: hasPicked }"><span class="orbit orbit-one" /><span class="orbit orbit-two" /><div class="station-code"><span>{{ selectedStation.lines.map(line => line.charAt(0).toUpperCase()).join('') }}</span></div></div>
          <Transition name="station" mode="out-in"><div :key="selectedStation.id" class="station-result"><h2>{{ selectedStation.name }}</h2><p>{{ selectedStation.en }}</p><div class="line-tags"><span v-for="line in selectedLines" :key="line.name"><i :style="{ background: line.color }" />{{ line.name }}</span></div></div></Transition>
          <button class="pick-button" :disabled="isPicking" @click="pickStation"><span class="button-icon">↗</span><span>{{ isPicking ? '選站中…' : hasPicked ? '再選一次' : '開始選站' }}</span></button>
          <p class="hint"><span>✦</span> 每一次選擇，都是一次新的相遇</p>
        </aside>
      </div>
    </section>
    <section id="how" class="how-section"><p>HOW IT WORKS</p><h2>按一下，下一站就有答案。</h2><div class="steps"><article><b>01</b><span>按下開始選站</span></article><article><b>02</b><span>看著站點穿梭</span></article><article><b>03</b><span>帶著好奇心出發</span></article></div></section>
  </main>
</template>
