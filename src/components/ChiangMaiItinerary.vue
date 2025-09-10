<template>
  <div class="app">
    <header class="topbar">
      <nav class="nav">
        <strong>九月清邁旅程 9/13-9/20</strong>
        <a href="#flights">航班</a>
        <a href="#hotel">飯店</a>
        <a href="#plan">每日行程</a>
        <a href="#spots">景點</a>
        <a href="#transport">交通</a>
        <a href="#maps">地圖</a>
        <span class="chip">一鍵開地圖</span>
      </nav>
    </header>
    <main class="container">
      <h1 id="flights">航班與日期</h1>
      <section class="card">
        <div class="flight">
          <div class="box">
            <div class="k">去程｜台北 (TPE) → 清邁 (CNX)</div>
            <div>9/13（六） 13:20 → 16:20（JX751）</div>
          </div>
          <div class="box">
            <div class="k">回程｜清邁 (CNX) → 台北 (TPE)</div>
            <div>9/20（六） 17:20 → 22:10（JX752）</div>
          </div>
          <div class="box">
            <div class="k">旅宿</div>
            <div>9/13–9/20，共 7 晚</div>
          </div>
        </div>
        <p class="muted">抵達後於機場搭乘接駁車前往飯店（預計約 20–30 分）。</p>
      </section>
      <h2 id="hotel">飯店資訊</h2>
      <section class="card">
        <div class="grid">
          <div>
            <h3>清邁寧曼麥設計飯店（Nimman Mai Design Hotel）</h3>
            <p class="muted">房型：<b>蘭納族</b>｜禁菸｜位於 <b>Nimman 寧曼路</b> 商圈。</p>
            <ul class="list">
              <li>地址：Nimmanhaemin Rd. Soi 3, Chiang Mai</li>
              <li>入住：9/13（六） 17:30 左右</li>
              <li>退房：9/20（六） 12:00 前</li>
              <li>設施：泳池、早餐餐廳、代叫車服務</li>
            </ul>
          </div>
          <div>
            <iframe class="map" title="飯店地圖" loading="lazy" referrerpolicy="no-referrer-when-downgrade" :src="maps.hotel"></iframe>
            <div class="muted mt6">地圖：飯店周邊（可縮放）</div>
          </div>
        </div>
      </section>
      <h2 id="plan">建議每日行程</h2>
      <section class="card">
        <div class="day" v-for="d in days" :key="d.id">
          <h3>{{ d.title }}</h3>
          <ul class="list">
            <li v-for="(it, i) in d.items" :key="i" v-html="it"></li>
          </ul>
        </div>
      </section>
      <h2 id="maps">地圖總覽</h2>
      <section class="card">
        <p class="muted">點擊按鈕切換地圖：</p>
        <div class="btnrow">
          <button class="btn" @click="switchMap(maps.city)">清邁總覽</button>
          <button class="btn" @click="switchMap(maps.hotel)">飯店</button>
          <button class="btn" @click="switchMap(maps.suthep)">雙龍寺</button>
          <button class="btn" @click="switchMap(maps.chedi)">契迪龍寺</button>
          <button class="btn" @click="switchMap(maps.onenimman)">One Nimman</button>
          <button class="btn" @click="switchMap(maps.thaPhae)">塔佩門</button>
        </div>
        <div class="btnrow mt6">
          <button class="btn secondary" @click="openDirections('Nimman Mai? Design Hotel Chiang Mai')">導航到飯店</button>
          <button class="btn secondary" @click="openDirections('Wat Phra That Doi Suthep')">導航到雙龍寺</button>
          <button class="btn secondary" @click="openDirections('Wat Chedi Luang')">導航到契迪龍寺</button>
          <button class="btn secondary" @click="openDirections('One Nimman')">導航到 One Nimman</button>
          <button class="btn secondary" @click="openDirections('Tha Phae Gate')">導航到塔佩門</button>
        </div>
        <div class="mapwrap">
          <iframe class="map" title="主要地圖" :src="activeMap" loading="lazy"></iframe>
        </div>
      </section>
    </main>
    <div class="floatbar">
      <button class="btn" @click="printPage">列印/匯出 PDF</button>
      <button class="btn secondary" @click="toTop">回到頂部</button>
    </div>
  </div>
</template>
<script setup>
import { ref } from 'vue'
const days = [
  { id: 1, title: 'D1｜9/13（六）抵達清邁・寧曼散步', items: [
    '16:20 抵達；17:00 通關＋ 使用eSIM → 17:20 搭 Grab 至飯店',
    '18:00 Check-in、休息',
    '19:30 <b>清邁大學夜市</b> 逛街用餐',
    '21:30 <b>按摩</b>（Fah Lanna / Health Lanna）'] },
  { id: 2, title: 'D2｜9/14（日）古城文化日（晚：Sunday Walking Street）', items: [
    '上午：<b>契迪龍寺</b>、<b>帕邢寺</b>',
    '午後：<b>Warorot 市場</b>、Ping 河畔咖啡',
    '晚餐：<b>清邁門夜市</b> 小吃' ] },
  { id: 3, title: 'D3｜9/15（一）素帖山＋日落', items: [
    '上午：<b>雙龍寺</b>（建議 8:00 前）',
    '午後：Huay Kaew 瀑布 / 動物園水庫步道',
    '傍晚：Nimman 屋頂酒吧；或日落散步' ] },
  { id: 4, title: 'D4｜9/16（二）大象保育／泰式料理課', items: [
    '選 A：象園保育（無騎乘，餵食＋河浴）',
    '選 B：泰式料理教室（市場採買 → 上課）',
    '晚間：Night Bazaar 夜市' ] },
  { id: 5, title: 'D5｜9/17（三）手作＆咖啡日', items: [
    '上午：銀器 / 藍染 / 木刻 體驗課',
    '下午：Nimman 咖啡巡禮（Ristr8to、Graph）',
    '晚間：Jazz Bar 或 夜間按摩' ] },
  { id: 6, title: 'D6｜9/18（四）清萊一日（選配）', items: [
    '白廟、藍廟、黑屋；或 Mae Rim 咖啡花園半日' ] },
  { id: 7, title: 'D7｜9/19（五）自由購物＋Spa', items: [
    'Maya／One Nimman 選物；預約 2 小時 Spa',
    '整理行李，確認回程交通' ] },
  { id: 8, title: 'D8｜9/20（六）返台', items: [
    '退房 12:00；建議 15:00 前往機場',
    '17:20 起飛（JX752）' ] },
]
const maps = {
  city: 'https://www.google.com/maps?q=Chiang%20Mai&output=embed',
  hotel: 'https://www.google.com/maps?q=Nimman%20Mai%20Design%20Hotel%20Chiang%20Mai&output=embed',
  suthep: 'https://www.google.com/maps?q=Wat%20Phra%20That%20Doi%20Suthep&output=embed',
  chedi: 'https://www.google.com/maps?q=Wat%20Chedi%20Luang&output=embed',
  onenimman: 'https://www.google.com/maps?q=One%20Nimman&output=embed',
  thaPhae: 'https://www.google.com/maps?q=Tha%20Phae%20Gate&output=embed'
}
const activeMap = ref(maps.city)
function switchMap(url){ activeMap.value = url }
function printPage(){ window.print() }
function toTop(){ window.scrollTo({ top: 0, behavior: 'smooth' }) }
function openDirections(query){
  const url = `https://www.google.com/maps/dir/?api=1&destination=${encodeURIComponent(query)}`
  window.open(url, '_blank')
}
</script>
<style scoped>
:root{ --brand:#6b5b95; --bg:#faf7ff; --ink:#2e2a35; --muted:#6d6775; --card:#fff; --accent:#b39ddb; }
*{ box-sizing:border-box }
.app{ background:var(--bg); color:var(--ink); min-height:100vh; }
.topbar{ position:sticky; top:0; background:#fff; border-bottom:1px solid #eee; z-index:9 }
.nav{ max-width:1100px; margin:auto; display:flex; gap:16px; align-items:center; padding:12px 16px; flex-wrap:wrap }
.nav a{ color:var(--ink); text-decoration:none; padding:8px 12px; border-radius:8px }
.nav a:hover{ background:#f5f2ff }
.chip{ background:var(--brand); color:#fff; border-radius:20px; padding:6px 12px; font-size:12px; margin-left:auto }
.container{ max-width:1100px; margin:20px auto; padding:0 16px 60px }
h1{ color:var(--brand); margin:16px 0 8px }
h2{ color:var(--brand); margin:28px 0 8px }
.card{ background:var(--card); border:1px solid #eee; border-radius:16px; padding:18px; margin:12px 0; box-shadow:0 2px 12px rgba(0,0,0,.04) }
.grid{ display:grid; grid-template-columns:repeat(auto-fit,minmax(260px,1fr)); gap:16px }
.tag{ display:inline-block; background:#f1ecff; color:#555; padding:4px 8px; border-radius:999px; font-size:12px; margin:2px 6px 0 0 }
.muted{ color:#6d6775 }
.k{ font-weight:600; color:#4b3f73 }
.list>li{ margin:6px 0 }
.flight{ display:flex; gap:16px; align-items:center; flex-wrap:wrap }
.flight .box{ background:#f7f4ff; border:1px dashed var(--accent); border-radius:12px; padding:10px 12px }
.map{ width:100%; height:360px; border:0; border-radius:12px }
.mt6{ margin-top:6px }
.day{ border-left:4px solid var(--accent); padding-left:12px; margin:10px 0 }
.btnrow{ display:flex; gap:8px; flex-wrap:wrap }
.mapwrap{ margin-top:12px }
.floatbar{ position:fixed; bottom:12px; right:12px; background:#fff; border:1px solid #eee; border-radius:999px; box-shadow:0 6px 24px rgba(0,0,0,.08); padding:6px 10px; display:flex; gap:6px; align-items:center }
.btn{ background:var(--brand); color:#fff; border:none; border-radius:10px; padding:10px 14px; font-weight:600; cursor:pointer }
.btn.secondary{ background:#eee; color:#332e3a }
</style>
