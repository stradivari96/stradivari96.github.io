---
title: "💍 LotR Trick-Taking"
date: 2026-06-13
searchHidden: true
robotsNoIndex: true
showToc: false
ShowReadingTime: false
ShowShareButtons: false
comments: false
---

<style>
#lotr-app {
  background: #0d1117;
  border-radius: 12px;
  padding: 24px;
  font-family: system-ui, sans-serif;
  color: #e6edf3;
}
#lotr-app h3 {
  margin: 24px 0 10px;
  color: #8b949e;
  font-size: 13px;
  letter-spacing: 1px;
  text-transform: uppercase;
}
.l-grid {
  display: flex;
  flex-wrap: wrap;
  gap: 10px;
  margin-bottom: 20px;
}
/* ── sprite card ── */
.lcard {
  width: 110px;
  height: 154px;
  border-radius: 10px;
  background-image: url('https://steamusercontent-a.akamaihd.net/ugc/34440677816838739/C609A55EEEB4A08A3F74F90AA4AA90CF9DEC1EF4/');
  background-size: 800% 500%;
  border: 2px solid #30363d;
  cursor: pointer;
  position: relative;
  flex: none;
  transition: transform .12s, box-shadow .12s, border-color .12s;
}
.lcard:hover {
  transform: translateY(-4px) scale(1.05);
  box-shadow: 0 8px 20px rgba(0,0,0,.6);
  z-index: 10;
  border-color: #58a6ff;
}
/* reverso del mazo principal */
.lcard-back {
  background-image: url('https://steamusercontent-a.akamaihd.net/ugc/34440677816838931/2DC6F20372D3BB0AA7A1ACB960616AE311F84ACD/');
  background-size: 100% 100%;
}
/* personajes — anverso */
.lcard-char {
  background-image: url('https://steamusercontent-a.akamaihd.net/ugc/34441311448452068/A45E85531AD7219FFD87731E1057D8628D2F0D7A/');
  background-size: 500% 500%;
}
/* personajes — reverso */
.lcard-char-back {
  background-image: url('https://steamusercontent-a.akamaihd.net/ugc/34441311448452379/E6CE202864F928F273B4926AAA1147A7FF5004AF/');
  background-size: 500% 500%;
}

/* ── modal de preview ── */
#lcard-modal {
  display: none;
  position: fixed;
  inset: 0;
  z-index: 9999;
  align-items: center;
  justify-content: center;
  background: rgba(0,0,0,.75);
  backdrop-filter: blur(4px);
  pointer-events: none;   /* el ratón lo atraviesa → no rompe mouseleave */
}
#lcard-modal.open { display: flex; }
#lcard-modal-inner {
  display: flex;
  flex-direction: column;
  align-items: center;
  gap: 14px;
}
#lcard-modal-img {
  width: 330px;
  height: 462px;
  border-radius: 16px;
  border: 3px solid #58a6ff;
  box-shadow: 0 20px 60px rgba(0,0,0,.9);
}
#lcard-modal-label {
  font-family: system-ui, sans-serif;
  font-size: 18px;
  color: #e6edf3;
  text-shadow: 0 2px 8px rgba(0,0,0,.8);
}
</style>

<!-- modal overlay -->
<div id="lcard-modal">
  <div id="lcard-modal-inner">
    <div id="lcard-modal-img"></div>
    <div id="lcard-modal-label"></div>
  </div>
</div>

<div id="lotr-app">
  <div id="lotr-grid"></div>
</div>

<script>
(function(){

// ── Modal logic ──────────────────────────────────────────────────────────────
var modal      = document.getElementById('lcard-modal');
var modalImg   = document.getElementById('lcard-modal-img');
var modalLabel = document.getElementById('lcard-modal-label');

function showModal(bgImage, bgSize, bgPos, label) {
  modalImg.style.backgroundImage    = bgImage;
  modalImg.style.backgroundSize     = bgSize;
  modalImg.style.backgroundPosition = bgPos;
  modalLabel.textContent = label;
  modal.classList.add('open');
}
function hideModal() { modal.classList.remove('open'); }

document.addEventListener('keydown', function(e){ if (e.key === 'Escape') hideModal(); });

function attachHover(el, bgImage, bgSize, bgPos, label) {
  el.addEventListener('mouseenter', function() { showModal(bgImage, bgSize, bgPos, label); });
  el.addEventListener('mouseleave', hideModal);
}

// ── Card helpers ─────────────────────────────────────────────────────────────
var MAIN_URL = "url('https://steamusercontent-a.akamaihd.net/ugc/34440677816838739/C609A55EEEB4A08A3F74F90AA4AA90CF9DEC1EF4/')";
var BACK_URL = "url('https://steamusercontent-a.akamaihd.net/ugc/34440677816838931/2DC6F20372D3BB0AA7A1ACB960616AE311F84ACD/')";
var CHAR_URL = "url('https://steamusercontent-a.akamaihd.net/ugc/34441311448452068/A45E85531AD7219FFD87731E1057D8628D2F0D7A/')";
var CHARB_URL= "url('https://steamusercontent-a.akamaihd.net/ugc/34441311448452379/E6CE202864F928F273B4926AAA1147A7FF5004AF/')";

var COLS = 8, ROWS = 5;
var CHAR_COLS = 5, CHAR_ROWS = 5;

var SUITS = [
  { name: '⛰️ Mountain', start: 0,  count: 8 },
  { name: '🏔️ Hill',     start: 8,  count: 8 },
  { name: '🌲 Forest',   start: 16, count: 8 },
  { name: '🌑 Shadow',   start: 24, count: 8 },
  { name: '💍 Ring',     start: 32, count: 5 },
];

function suitOf(pos) {
  for (var s of SUITS) if (pos >= s.start && pos < s.start + s.count) return s;
  return null;
}
function valOf(pos) {
  var s = suitOf(pos); return s ? (pos - s.start + 1) : '?';
}

function bpos(col, row, totalCols, totalRows) {
  return (col / (totalCols - 1) * 100).toFixed(2) + '% ' +
         (row / (totalRows - 1) * 100).toFixed(2) + '%';
}

// ── Build HTML ───────────────────────────────────────────────────────────────
var container = document.getElementById('lotr-grid');
var html = '';

// Main suits
SUITS.forEach(function(s) {
  html += '<h3>' + s.name + ' (1–' + s.count + ')</h3><div class="l-grid" data-section="main">';
  for (var i = s.start; i < s.start + s.count; i++) {
    var col = i % COLS, row = Math.floor(i / COLS);
    var bp = bpos(col, row, COLS, ROWS);
    var sName = s.name.split(' ')[1];
    html += '<div class="lcard" style="background-position:' + bp + '"' +
            ' data-bg="main" data-bp="' + bp + '" data-label="' + sName + ' ' + valOf(i) + '"></div>';
  }
  html += '</div>';
});

// Back sample
html += '<h3>Reverso (común a todas)</h3><div class="l-grid">' +
        '<div class="lcard lcard-back" data-bg="back" data-label="Reverso"></div></div>';

// Characters
var CHARS = [
  {pos:0,  name:'Frodo'},           {pos:1,  name:'Bilbo'},
  {pos:2,  name:'Merry'},           {pos:3,  name:'Gildor Inglorien'},
  {pos:4,  name:'Fatty Bolger'},    {pos:5,  name:'Gandalf'},
  {pos:6,  name:'Pippin'},          {pos:7,  name:'Sam'},
  {pos:8,  name:'Farmer Maggot'},   {pos:9,  name:'Tom Bombadil'},
  {pos:10, name:'Strider'},         {pos:11, name:'Goldberry'},
  {pos:12, name:'Barliman Butterbur'},{pos:13,name:'Mr. Underhill'},
  {pos:14, name:'Glorfindel'},      {pos:15, name:'Bill the Pony'},
  {pos:16, name:'Glóin'},           {pos:17, name:'Arwen'},
  {pos:18, name:'Elrond'},          {pos:19, name:'Aragorn'},
  {pos:20, name:'Bilbo Baggins'},   {pos:21, name:'Boromir'},
  {pos:22, name:'Radagast'},        {pos:23, name:'Shadowfax'},
  {pos:24, name:'Gwaihir'},
];

html += '<h3>👤 Personajes</h3><div class="l-grid">';
CHARS.forEach(function(c) {
  var col = c.pos % CHAR_COLS, row = Math.floor(c.pos / CHAR_COLS);
  var bp = bpos(col, row, CHAR_COLS, CHAR_ROWS);
  html += '<div class="lcard lcard-char" style="background-position:' + bp + '"' +
          ' data-bg="char" data-bp="' + bp + '" data-label="' + c.name + '"></div>';
});
html += '</div>';

html += '<h3>Reversos de personajes</h3><div class="l-grid">';
CHARS.forEach(function(c) {
  var col = c.pos % CHAR_COLS, row = Math.floor(c.pos / CHAR_COLS);
  var bp = bpos(col, row, CHAR_COLS, CHAR_ROWS);
  html += '<div class="lcard lcard-char-back" style="background-position:' + bp + '"' +
          ' data-bg="charback" data-bp="' + bp + '" data-label="' + c.name + ' (reverso)"></div>';
});
html += '</div>';

container.innerHTML = html;

// ── Attach modal events after render ─────────────────────────────────────────
var bgMap = {
  main:     { img: MAIN_URL,  size: '800% 500%' },
  back:     { img: BACK_URL,  size: '100% 100%' },
  char:     { img: CHAR_URL,  size: '500% 500%' },
  charback: { img: CHARB_URL, size: '500% 500%' },
};

container.querySelectorAll('.lcard').forEach(function(el) {
  var bg = el.dataset.bg;
  if (!bg || !bgMap[bg]) return;
  var m = bgMap[bg];
  var bp = el.dataset.bp || '0% 0%';
  var label = el.dataset.label || '';
  attachHover(el, m.img, m.size, bp, label);
});

})();
</script>
