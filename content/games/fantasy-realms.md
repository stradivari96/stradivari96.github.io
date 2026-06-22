---
title: "🏰 Fantasy Realms"
date: 2026-06-13
searchHidden: true
robotsNoIndex: true
showToc: false
ShowReadingTime: false
ShowShareButtons: false
comments: false
---

Juego de cartas para 2–6 jugadores. Uno crea la sala y comparte el enlace. La partida vive en el navegador del anfitrión.

- [Reglamento](https://wizkids.com/posters/repository/wizkids/FR_Rulebook-WEB.pdf)


<style>
#fr-app{--bg:#1b1f27;--panel:#252b36;--text:#e8eaf0;--mut:#9aa3b2;--border:#3a4252;--blue:#3b82f6;
  background:var(--bg);color:var(--text);border-radius:12px;padding:16px;
  font-family:system-ui,sans-serif;font-size:15px;line-height:1.4}
#fr-app h3{margin:10px 0 6px;color:var(--text)}
#fr-app input{background:#11141a;color:var(--text);border:1px solid var(--border);
  border-radius:8px;padding:8px 10px;font-size:15px;max-width:160px}
#fr-app button{background:var(--blue);color:#fff;border:0;border-radius:8px;
  padding:8px 14px;font-size:14px;cursor:pointer;margin:2px}
#fr-app button:disabled{background:var(--border);color:var(--mut);cursor:not-allowed}
.fr-row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin:8px 0}
.fr-panel{background:var(--panel);border-radius:10px;padding:10px 12px;margin:8px 0}
.fr-turn{box-shadow:0 0 0 3px #fbbf24 inset;border-radius:10px}
.fr-code{font-size:22px;font-weight:800;letter-spacing:4px;color:#fbbf24}
.fr-mut{color:var(--mut);font-size:13px;margin:4px 0}
.fr-name{font-weight:600;margin-bottom:6px;font-size:14px}
/* own hand: single scrollable row of full-size cards */
.fr-hand{display:flex;flex-wrap:wrap;gap:8px;margin:4px 0;align-items:flex-start;
  align-content:flex-start;padding-bottom:4px}
/* large card (own hand) */
.fr-card{width:145px;height:203px;border-radius:8px;border:2px solid rgba(0,0,0,.45);
  flex:none;position:relative;user-select:none;background-color:#2a3040}
/* small card (opponent hands) */
.fr-card-opp{width:80px;height:112px;border-radius:6px;border:2px solid rgba(0,0,0,.45);
  flex:none;position:relative;user-select:none;background-color:#2a3040}
/* small card (deck + discard pile) */
.fr-card-sm{width:65px;height:91px;border-radius:6px;border:2px solid rgba(0,0,0,.45);
  flex:none;position:relative;user-select:none;background-color:#2a3040}
.fr-back{display:flex;align-items:center;justify-content:center}
.fr-back-count{font-size:42px;font-weight:800;color:#fff;text-shadow:0 2px 8px rgba(0,0,0,.9);
  position:absolute;bottom:10px;right:12px}
.fr-card-sm .fr-back-count{font-size:18px;bottom:4px;right:5px}
.fr-empty{display:flex;align-items:center;justify-content:center;font-size:28px;
  border:2px dashed var(--border)!important;background:transparent!important;color:var(--mut)}
.fr-card-sm.fr-empty{font-size:18px}
.fr-clickable{cursor:pointer;transition:transform .12s}
.fr-card.fr-clickable:hover{transform:translateY(-6px)}
.fr-card-sm.fr-clickable:hover{transform:translateY(-4px)}
.fr-pickup-target{outline:4px solid #fbbf24;outline-offset:3px}
.fr-discard-target{outline:4px solid #f87171;outline-offset:3px}
.fr-zone{display:flex;flex-direction:column;align-items:center;gap:5px;flex:none}
.fr-zone-label{font-size:10px;color:var(--mut);letter-spacing:1px;font-weight:700;text-transform:uppercase}
.fr-board-row{display:flex;gap:14px;align-items:flex-start;flex-wrap:wrap;padding-bottom:4px}
.fr-discard-zone{flex:1;min-width:200px}
.fr-discard-fan{display:flex;flex-wrap:wrap;gap:5px;padding:2px}
.fr-log{max-height:140px;overflow-y:auto;font-size:13px;color:var(--mut)}
.fr-log div{padding:1px 0}
.fr-banner{background:#3b2f12;border:1px solid #fbbf24;border-radius:10px;
  padding:10px 14px;margin:8px 0;font-weight:600}
.fr-action-hint{background:#1e3a5f;border:1px solid #3b82f6;border-radius:8px;
  padding:8px 12px;margin:6px 0;font-size:14px}
.fr-toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);
  background:#dc2626;color:#fff;padding:10px 18px;border-radius:10px;z-index:99;
  box-shadow:0 4px 14px rgba(0,0,0,.4)}
#fr-tip{width:260px;height:364px;border-radius:10px;border:2px solid rgba(255,255,255,.22);
  box-shadow:0 10px 40px rgba(0,0,0,.85);background-size:1000% 600%;
  position:fixed;z-index:200;pointer-events:none;display:none}
@keyframes fr-fadein{from{opacity:0;transform:scale(.6)}to{opacity:1;transform:scale(1)}}
.fr-new{animation:fr-fadein .4s ease}
</style>

<div id="fr-app">
  <div id="fr-setup">
    <div class="fr-row"><label>Tu nombre: <input id="fr-name" maxlength="14" placeholder="Nombre"></label></div>
    <div class="fr-row"><button id="fr-create">🏰 Crear sala</button></div>
    <div class="fr-row"><input id="fr-code" maxlength="4" placeholder="CÓDIGO" style="text-transform:uppercase;width:110px"><button id="fr-join">Unirse</button></div>
    <div id="fr-setupmsg" class="fr-mut"></div>
  </div>
  <div id="fr-game" style="display:none"></div>
</div>

<script src="https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js"></script>

<script>
(function(){
'use strict';

var SPRITE='https://steamusercontent-a.akamaihd.net/ugc/1812114214287641959/29522BD4EC40B09E56541D728732C149E9819365/';
var BACK='https://steamusercontent-a.akamaihd.net/ugc/1786218626612222249/734934D88FAE55BC72C969D3B1E649A45015B405/';
var PREFIX='fantasy-realms-xiang-';
var MAXP=6,MINP=2,MAX_DISCARD=10,HAND_SIZE=7,DUEL_MIN_DISCARD=12;

// El mazo (las 53 cartas) se construye a partir de FRDEF; cada carta se
// identifica por su id FRxx, que también determina su posición en el sprite.

// ================= SCORING ENGINE (Fantasy Realms, base set FR01–FR53) =================
// Adaptado de las definiciones oficiales. Los suits van en minúscula aquí.
var PHOENIX='FR55',PHOENIX_PROMO='FR55P'; // no están en este mazo, pero las defs los referencian
function isPhoenix(c){return c.id===PHOENIX||c.id===PHOENIX_PROMO;}
function isArmyClearedFromPenalty(card,hand){
  // FR25 Rangers: limpia "Ejército" de todas las Penalizaciones.
  // FR41 Warship: limpia "Ejército" de las Penalizaciones de las Inundaciones.
  return hand.containsId('FR25',true)||(card.suit==='flood'&&hand.containsId('FR41',true));
}

var FRDEF={
  'FR01':{id:'FR01',suit:'land',name:'Mountain',strength:9,
    bonusScore:function(hand){return hand.contains('Smoke')&&hand.contains('Wildfire')?50:0;},
    clearsPenalty:function(card){return card.suit==='flood';}},
  'FR02':{id:'FR02',suit:'land',name:'Cavern',strength:6,
    bonusScore:function(hand){return hand.contains('Dwarvish Infantry')||hand.contains('Dragon')?25:0;},
    clearsPenalty:function(card){return card.suit==='weather'||isPhoenix(card);}},
  'FR03':{id:'FR03',suit:'land',name:'Bell Tower',strength:8,
    bonusScore:function(hand){return hand.containsSuit('wizard')?15:0;}},
  'FR04':{id:'FR04',suit:'land',name:'Forest',strength:7,
    bonusScore:function(hand){return 12*hand.countSuit('beast')+(hand.contains('Elven Archers')?12:0);}},
  'FR05':{id:'FR05',suit:'land',name:'Earth Elemental',strength:4,
    bonusScore:function(hand){return 15*hand.countSuitExcluding('land',this.id);}},
  'FR06':{id:'FR06',suit:'flood',name:'Fountain of Life',strength:1,
    bonusScore:function(hand){var max=0;for(const card of hand.nonBlankedCards()){if(card.suit==='weapon'||card.suit==='flood'||card.suit==='flame'||card.suit==='land'||card.suit==='weather'||isPhoenix(card)){if(card.strength>max)max=card.strength;}}return max;}},
  'FR07':{id:'FR07',suit:'flood',name:'Swamp',strength:18,
    penaltyScore:function(hand){var pc=hand.countSuit('flame');if(!isArmyClearedFromPenalty(this,hand))pc+=hand.countSuit('army');return -3*pc;}},
  'FR08':{id:'FR08',suit:'flood',name:'Great Flood',strength:32,
    blanks:function(card,hand){return (card.suit==='army'&&!isArmyClearedFromPenalty(this,hand))||(card.suit==='land'&&card.name!=='Mountain')||(card.suit==='flame'&&card.name!=='Lightning')||card.id===PHOENIX_PROMO;}},
  'FR09':{id:'FR09',suit:'flood',name:'Island',strength:14},
  'FR10':{id:'FR10',suit:'flood',name:'Water Elemental',strength:4,
    bonusScore:function(hand){return 15*hand.countSuitExcluding('flood',this.id);}},
  'FR11':{id:'FR11',suit:'weather',name:'Rainstorm',strength:8,
    bonusScore:function(hand){return 10*hand.countSuit('flood');},
    blanks:function(card,hand){return (card.suit==='flame'&&card.name!=='Lightning')||card.id===PHOENIX_PROMO;}},
  'FR12':{id:'FR12',suit:'weather',name:'Blizzard',strength:30,
    penaltyScore:function(hand){var pc=hand.countSuit('leader')+hand.countSuit('beast')+hand.countSuit('flame');if(!isArmyClearedFromPenalty(this,hand))pc+=hand.countSuit('army');return -5*pc;},
    blanks:function(card,hand){return card.suit==='flood';}},
  'FR13':{id:'FR13',suit:'weather',name:'Smoke',strength:27,
    blankedIf:function(hand){return !hand.containsSuit('flame');}},
  'FR14':{id:'FR14',suit:'weather',name:'Whirlwind',strength:13,
    bonusScore:function(hand){return hand.contains('Rainstorm')&&(hand.contains('Blizzard')||hand.contains('Great Flood'))?40:0;}},
  'FR15':{id:'FR15',suit:'weather',name:'Air Elemental',strength:4,
    bonusScore:function(hand){return 15*hand.countSuitExcluding('weather',this.id);}},
  'FR16':{id:'FR16',suit:'flame',name:'Wildfire',strength:40,
    blanks:function(card,hand){return !(card.suit==='flame'||card.suit==='wizard'||card.suit==='weather'||card.suit==='weapon'||card.suit==='artifact'||card.suit==='wild'||card.name==='Mountain'||card.name==='Great Flood'||card.name==='Island'||card.name==='Unicorn'||card.name==='Dragon'||isPhoenix(card));}},
  'FR17':{id:'FR17',suit:'flame',name:'Candle',strength:2,
    bonusScore:function(hand){return hand.contains('Book of Changes')&&hand.contains('Bell Tower')&&hand.containsSuit('wizard')?100:0;}},
  'FR18':{id:'FR18',suit:'flame',name:'Forge',strength:9,
    bonusScore:function(hand){return 9*(hand.countSuit('weapon')+hand.countSuit('artifact'));}},
  'FR19':{id:'FR19',suit:'flame',name:'Lightning',strength:11,
    bonusScore:function(hand){return hand.contains('Rainstorm')?30:0;}},
  'FR20':{id:'FR20',suit:'flame',name:'Fire Elemental',strength:4,
    bonusScore:function(hand){return 15*hand.countSuitExcluding('flame',this.id);}},
  'FR21':{id:'FR21',suit:'army',name:'Knights',strength:20,
    penaltyScore:function(hand){return hand.containsSuit('leader')?0:-8;}},
  'FR22':{id:'FR22',suit:'army',name:'Elven Archers',strength:10,
    bonusScore:function(hand){return hand.containsSuit('weather')?0:5;}},
  'FR23':{id:'FR23',suit:'army',name:'Light Cavalry',strength:17,
    penaltyScore:function(hand){return -2*hand.countSuit('land');}},
  'FR24':{id:'FR24',suit:'army',name:'Dwarvish Infantry',strength:15,
    penaltyScore:function(hand){if(!isArmyClearedFromPenalty(this,hand))return -2*hand.countSuitExcluding('army',this.id);return 0;}},
  'FR25':{id:'FR25',suit:'army',name:'Rangers',strength:5,
    bonusScore:function(hand){return 10*hand.countSuit('land');}},
  'FR26':{id:'FR26',suit:'wizard',name:'Collector',strength:7,
    bonusScore:function(hand){var bySuit={};if(hand.containsId(PHOENIX_PROMO,true)){var phoenix=hand.getCardById(PHOENIX_PROMO);bySuit['flame']={};bySuit['flame'][phoenix.name]=phoenix;bySuit['weather']={};bySuit['weather'][phoenix.name]=phoenix;bySuit[phoenix.suit]={};bySuit[phoenix.suit][phoenix.name]=phoenix;}for(const card of hand.nonBlankedCards()){if(card.id!==PHOENIX_PROMO){if(card.id===PHOENIX){if(bySuit['flame']===undefined)bySuit['flame']={};bySuit['flame'][card.name]=card;if(bySuit['weather']===undefined)bySuit['weather']={};bySuit['weather'][card.name]=card;}var suit=card.suit;if(bySuit[suit]===undefined)bySuit[suit]={};bySuit[suit][card.name]=card;}}var bonus=0;for(const suit of Object.values(bySuit)){var count=Object.keys(suit).length;if(count===3)bonus+=10;else if(count===4)bonus+=40;else if(count>=5)bonus+=100;}return bonus;}},
  'FR27':{id:'FR27',suit:'wizard',name:'Beastmaster',strength:9,
    bonusScore:function(hand){return 9*hand.countSuit('beast');},
    clearsPenalty:function(card){return card.suit==='beast';}},
  'FR28':{id:'FR28',suit:'wizard',name:'Necromancer',strength:3},
  'FR29':{id:'FR29',suit:'wizard',name:'Warlock Lord',strength:25,
    penaltyScore:function(hand){return -10*(hand.countSuit('leader')+hand.countSuitExcluding('wizard',this.id));}},
  'FR30':{id:'FR30',suit:'wizard',name:'Enchantress',strength:5,
    bonusScore:function(hand){return 5*(hand.countSuit('land')+hand.countSuit('weather')+hand.countSuit('flood')+hand.countSuit('flame'));}},
  'FR31':{id:'FR31',suit:'leader',name:'King',strength:8,
    bonusScore:function(hand){return (hand.contains('Queen')?20:5)*hand.countSuit('army');}},
  'FR32':{id:'FR32',suit:'leader',name:'Queen',strength:6,
    bonusScore:function(hand){return (hand.contains('King')?20:5)*hand.countSuit('army');}},
  'FR33':{id:'FR33',suit:'leader',name:'Princess',strength:2,
    bonusScore:function(hand){return 8*(hand.countSuit('army')+hand.countSuit('wizard')+hand.countSuitExcluding('leader',this.id));}},
  'FR34':{id:'FR34',suit:'leader',name:'Warlord',strength:4,
    bonusScore:function(hand){var total=0;for(const card of hand.nonBlankedCards()){if(card.suit==='army')total+=card.strength;}return total;}},
  'FR35':{id:'FR35',suit:'leader',name:'Empress',strength:15,
    bonusScore:function(hand){return 10*hand.countSuit('army');},
    penaltyScore:function(hand){return -5*hand.countSuitExcluding('leader',this.id);}},
  'FR36':{id:'FR36',suit:'beast',name:'Unicorn',strength:9,
    bonusScore:function(hand){return hand.contains('Princess')?30:(hand.contains('Empress')||hand.contains('Queen')||hand.contains('Enchantress'))?15:0;}},
  'FR37':{id:'FR37',suit:'beast',name:'Basilisk',strength:35,
    blanks:function(card,hand){return (card.suit==='army'&&!isArmyClearedFromPenalty(this,hand))||card.suit==='leader'||(card.suit==='beast'&&card.id!==this.id&&card.id!==PHOENIX);}},
  'FR38':{id:'FR38',suit:'beast',name:'Warhorse',strength:6,
    bonusScore:function(hand){return hand.containsSuit('leader')||hand.containsSuit('wizard')?14:0;}},
  'FR39':{id:'FR39',suit:'beast',name:'Dragon',strength:30,
    penaltyScore:function(hand){return hand.containsSuit('wizard')?0:-40;}},
  'FR40':{id:'FR40',suit:'beast',name:'Hydra',strength:12,
    bonusScore:function(hand){return hand.contains('Swamp')?28:0;}},
  'FR41':{id:'FR41',suit:'weapon',name:'Warship',strength:23,
    blankedIf:function(hand){return !hand.containsSuit('flood');}},
  'FR42':{id:'FR42',suit:'weapon',name:'Magic Wand',strength:1,
    bonusScore:function(hand){return hand.containsSuit('wizard')?25:0;}},
  'FR43':{id:'FR43',suit:'weapon',name:'Sword of Keth',strength:7,
    bonusScore:function(hand){return hand.containsSuit('leader')?(hand.contains('Shield of Keth')?40:10):0;}},
  'FR44':{id:'FR44',suit:'weapon',name:'Elven Longbow',strength:3,
    bonusScore:function(hand){return hand.contains('Elven Archers')||hand.contains('Warlord')||hand.contains('Beastmaster')?30:0;}},
  'FR45':{id:'FR45',suit:'weapon',name:'War Dirigible',strength:35,
    blankedIf:function(hand){return (!hand.containsSuit('army')&&!isArmyClearedFromPenalty(this,hand))||hand.containsSuitExcluding('weather',PHOENIX);}},
  'FR46':{id:'FR46',suit:'artifact',name:'Shield of Keth',strength:4,
    bonusScore:function(hand){return hand.containsSuit('leader')?(hand.contains('Sword of Keth')?40:15):0;}},
  'FR47':{id:'FR47',suit:'artifact',name:'Gem of Order',strength:5,
    bonusScore:function(hand){var strengths=hand.nonBlankedCards().map(function(card){return card.strength;}).sort(function(a,b){return a-b;});var bonus=0,runFound=false;do{var run=[];for(var i=0;i<strengths.length;i++){var strength=strengths[i];if(run.length!==0&&(strength===run[run.length-1]+1)){run.push(strength);}else if(run.length<3&&!run.includes(strength)){run=[strength];}}if(run.length<3){runFound=false;}else{runFound=true;for(var i=0;i<run.length;i++){strengths.splice(strengths.indexOf(run[i]),1);}if(run.length===3)bonus+=10;else if(run.length===4)bonus+=30;else if(run.length===5)bonus+=60;else if(run.length===6)bonus+=100;else if(run.length>=7)bonus+=150;}}while(runFound);return bonus;}},
  'FR48':{id:'FR48',suit:'artifact',name:'World Tree',strength:2,
    bonusScore:function(hand){var suits=[];for(const card of hand.nonBlankedCards()){if(isPhoenix(card)){if(suits.includes('weather')||suits.includes('flame'))return 0;suits.push('weather');suits.push('flame');}if(suits.includes(card.suit))return 0;suits.push(card.suit);}return 50;}},
  'FR49':{id:'FR49',suit:'artifact',name:'Book of Changes',strength:3},
  'FR50':{id:'FR50',suit:'artifact',name:'Protection Rune',strength:1,
    clearsPenalty:function(card){return true;}},
  'FR51':{id:'FR51',suit:'wild',name:'Shapeshifter',strength:0},
  'FR52':{id:'FR52',suit:'wild',name:'Mirage',strength:0},
  'FR53':{id:'FR53',suit:'wild',name:'Doppelganger',strength:0}
};

// Cartas que requieren decisión del jugador al puntuar (por id FRxx)
var IMPERSONATE_SUITS={FR51:['artifact','leader','wizard','weapon','beast'],FR52:['army','land','weather','flood','flame']};
function isActionCard(id){return id==='FR09'||id==='FR28'||id==='FR49'||id==='FR51'||id==='FR52'||id==='FR53';}
var NECRO_SUITS=['army','leader','wizard','beast'];

// Mano de puntuación: opera siempre sobre cartas NO anuladas.
function makeScoringHand(scs){
  var blanked=new Set();
  return {
    setBlanked:function(s){blanked=s;},
    nonBlankedCards:function(){return scs.filter(function(c){return !blanked.has(c.id);});},
    size:function(){return this.nonBlankedCards().length;},
    contains:function(n){return this.nonBlankedCards().some(function(c){return c.name===n;});},
    containsSuit:function(s){return this.nonBlankedCards().some(function(c){return c.suit===s;});},
    countSuit:function(s){return this.nonBlankedCards().filter(function(c){return c.suit===s;}).length;},
    countSuitExcluding:function(s,id){return this.nonBlankedCards().filter(function(c){return c.suit===s&&c.id!==id;}).length;},
    containsSuitExcluding:function(s,id){return this.nonBlankedCards().some(function(c){return c.suit===s&&c.id!==id;});},
    containsId:function(id,nb){var list=nb?this.nonBlankedCards():scs;return list.some(function(c){return c.id===id;});},
    getCardById:function(id){return scs.find(function(c){return c.id===id;});}
  };
}

function setEq(a,b){if(a.size!==b.size)return false;for(const v of a)if(!b.has(v))return false;return true;}

// Punto fijo: una carta anulada deja de anular a otras.
function computeBlanked(H,scs){
  var blanked=new Set();
  for(var it=0;it<30;it++){
    H.setBlanked(blanked);
    var next=new Set();
    scs.forEach(function(x){
      if(x.blankedIf&&x.blankedIf(H)){next.add(x.id);return;}
      for(var j=0;j<scs.length;j++){
        var y=scs[j];
        if(y.id===x.id||blanked.has(y.id))continue;
        if(y.blanks&&y.blanks(x,H)){next.add(x.id);return;}
      }
    });
    if(setEq(next,blanked)){H.setBlanked(next);return next;}
    blanked=next;
  }
  H.setBlanked(blanked);return blanked;
}

// Puntúa la mano de un jugador. handCards: [{id(FRxx),name,suit}], assign: decisiones.
function computeHandScore(handCards,assign){
  assign=assign||{};
  var scs=handCards.map(function(gc){return Object.assign({},FRDEF[gc.id]);});
  // 0) Necromancer: añade una carta extra (Army/Leader/Wizard/Beast) recuperada del descarte.
  if(assign['FR28']){
    var ex=FRDEF[assign['FR28']];
    if(ex&&NECRO_SUITS.indexOf(ex.suit)>=0&&!scs.some(function(s){return s.id===ex.id;}))
      scs.push(Object.assign({},ex));
  }
  // 1) Comodines (Doppelgänger, Mirage, Shapeshifter): copian nombre/fuerza/suit/bonus.
  scs.forEach(function(sc){
    if(sc.id==='FR51'||sc.id==='FR52'||sc.id==='FR53'){
      var t=assign[sc.id]&&FRDEF[assign[sc.id]];
      if(t){sc.name=t.name;sc.suit=t.suit;sc.strength=t.strength;sc.bonusScore=t.bonusScore;}
    }
  });
  // 2) Book of Changes: cambia el suit de otra carta.
  scs.forEach(function(sc){
    if(sc.id==='FR49'){
      var ch=assign[sc.id];
      if(ch&&ch.target&&ch.suit){
        var tgt=scs.find(function(s){return s.id===ch.target&&s.id!==sc.id;});
        if(tgt)tgt.suit=ch.suit;
      }
    }
  });
  // 3) Island: limpia la penalización de una Flood/Flame y suma su fuerza.
  scs.forEach(function(sc){
    if(sc.id==='FR09'){
      var ch=assign[sc.id];
      if(ch){
        var tgt=scs.find(function(s){return s.id===ch&&s.id!==sc.id;});
        if(tgt&&(tgt.suit==='flood'||tgt.suit==='flame')){tgt._penaltyCleared=true;var ab=tgt.strength;sc.bonusScore=function(){return ab;};}
      }
    }
  });
  var H=makeScoringHand(scs);
  var blanked=computeBlanked(H,scs);
  H.setBlanked(blanked);
  var total=0,breakdown=[];
  scs.forEach(function(sc){
    if(blanked.has(sc.id)){breakdown.push({name:sc.name,suit:sc.suit,blanked:true,base:sc.strength,bonus:0,penalty:0,sub:0});return;}
    var bonus=sc.bonusScore?sc.bonusScore(H):0;
    var pen=0;
    if(sc.penaltyScore&&!sc._penaltyCleared){
      var cleared=H.nonBlankedCards().some(function(y){return y.id!==sc.id&&y.clearsPenalty&&y.clearsPenalty(sc);});
      if(!cleared)pen=sc.penaltyScore(H);
    }
    var sub=sc.strength+bonus+pen;
    total+=sub;
    breakdown.push({name:sc.name,suit:sc.suit,blanked:false,base:sc.strength,bonus:bonus,penalty:pen,sub:sub});
  });
  breakdown.sort(function(a,b){return b.sub-a.sub;});
  return {total:total,breakdown:breakdown};
}

function computeAllScores(){
  return G.hands.map(function(hand,p){return computeHandScore(hand,G.assign&&G.assign[p]);});
}

var SUIT_ES={land:'Tierra',flood:'Inundación',weather:'Clima',flame:'Fuego',army:'Ejército',wizard:'Mago',leader:'Líder',beast:'Bestia',weapon:'Arma',artifact:'Artefacto',wild:'Comodín'};
function suitEs(s){return SUIT_ES[s]||SUIT_ES[String(s).toLowerCase()]||s;}

// ---------- state ----------
var peer=null,isHost=false,hostConn=null,roomCode='';
var G=null; // full state (host)
var V=null; // view state (rendered)
var myName='';

function el(id){return document.getElementById(id);}
function esc(s){return String(s).replace(/[&<>"']/g,function(c){
  return{'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];});}
function toast(msg){var t=document.createElement('div');t.className='fr-toast';
  t.textContent=msg;el('fr-app').appendChild(t);setTimeout(function(){t.remove();},3500);}

// ---------- sprite ----------
function spriteStyle(id){
  var idx=parseInt(id.slice(2),10)-1,col=idx%10,row=Math.floor(idx/10);
  var xp=(col/9*100).toFixed(2),yp=(row/5*100).toFixed(2);
  return 'background-image:url('+SPRITE+');background-size:1000% 600%;background-position:'+xp+'% '+yp+'%';
}

function cardHtml(card,cls,act,extra){
  var attrs='data-cid="'+card.id+'" title="'+esc(card.name)+' ('+card.suit+')"';
  if(act)attrs+=' data-act="'+act+'"';
  if(extra)attrs+=' '+extra;
  return '<div class="'+cls+'" style="'+spriteStyle(card.id)+'" '+attrs+'></div>';
}

// ---------- hand order ----------
// La mano se ordena por tipo (suit) y, dentro del tipo, por id de carta.
var SUIT_ORDER=['land','flood','weather','flame','army','wizard','leader','beast','weapon','artifact','wild'];
function sortByType(hand){
  return hand.slice().sort(function(a,b){
    var d=SUIT_ORDER.indexOf(a.suit)-SUIT_ORDER.indexOf(b.suit);
    return d!==0?d:(a.id<b.id?-1:1); // FRxx va con ceros, así que el orden lexicográfico = numérico
  });
}

// ---------- game logic (host) ----------
function mkDeck(){
  var d=Object.keys(FRDEF).map(function(id){var c=FRDEF[id];return{id:id,name:c.name,suit:c.suit};});
  for(var i=d.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));var t=d[i];d[i]=d[j];d[j]=t;}
  return d;
}

function newGame(){
  G={phase:'lobby',players:[],hands:[],deck:[],discard:[],turn:0,turnPhase:'pickup',log:[],result:null,assign:{}};
}

function startGame(){
  G.deck=mkDeck();G.discard=[];G.result=null;G.assign={};
  G.turn=Math.floor(Math.random()*G.players.length);
  G.turnPhase='pickup';
  G.hands=G.players.map(function(){return[];});
  if(isDuel()){
    // Variante duelo (2 jugadores): se empieza sin cartas.
    G.log=['⚔️ ¡Duelo a 2! Empezáis sin cartas. Turno inicial: '+pname(G.turn)];
  }else{
    for(var k=0;k<HAND_SIZE;k++)G.players.forEach(function(_,i){G.hands[i].push(G.deck.pop());});
    G.log=['🏰 ¡Empieza la partida! Turno inicial: '+pname(G.turn)];
  }
  G.phase='playing';
  broadcast();
}

function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>60)G.log.shift();}

// ---------- reglas según variante ----------
// Con 2 jugadores se juega la variante "duelo" (manos vacías + robar 2/descartar 1).
function isDuel(){return G.players.length===2;}

// ¿Sigue este jugador formando su mano inicial? (solo aplica en duelo)
function isBuilding(p){return isDuel()&&G.hands[p].length<HAND_SIZE;}

function endGame(msg){G.phase='ended';G.result={};glog(msg);}

// Condición de fin según la variante. Devuelve true si la partida ha terminado.
function checkEnd(){
  if(isDuel()){
    if(G.hands[0].length===HAND_SIZE&&G.hands[1].length===HAND_SIZE&&
       G.discard.length>=DUEL_MIN_DISCARD){
      endGame('🏁 ¡Ambos tenéis 7 cartas y hay '+G.discard.length+
        ' en el descarte — fin de la partida!');
      return true;
    }
    return false;
  }
  if(G.discard.length>=MAX_DISCARD){
    endGame('🏁 ¡El descarte tiene '+G.discard.length+' cartas — fin de la partida!');
    return true;
  }
  return false;
}

function nextTurn(){
  G.turn=(G.turn+1)%G.players.length;
  G.turnPhase='pickup';
  glog('👉 Turno de '+pname(G.turn));
}

// Cierra el turno actual: comprueba el fin y, si no, pasa al siguiente jugador.
function finishTurn(){if(!checkEnd())nextTurn();}

function buildStateFor(me){
  return{
    me:me,
    duel:isDuel(),
    phase:G.phase,turn:G.turn,turnPhase:G.turnPhase,
    deckCount:G.deck.length,
    discard:G.discard.map(function(c){return{id:c.id,name:c.name,suit:c.suit};}),
    hands:G.hands.map(function(h,i){return h.map(function(c){
      if(i===me||G.phase==='ended')return{id:c.id,name:c.name,suit:c.suit};
      return{id:c.id,hidden:true};
    });}),
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    log:G.log.slice(-30),
    result:G.result,
    myAssign:(G.assign&&G.assign[me])?G.assign[me]:{},
    scores:G.phase==='ended'?computeAllScores():null
  };
}

function broadcast(){
  G.players.forEach(function(p,i){
    if(i===0)return;
    if(p.conn&&p.connected)p.conn.send({t:'state',s:buildStateFor(i)});
  });
  V=buildStateFor(0);
  render();
}

function sendErr(p,msg){
  if(p===0)toast(msg);
  else if(G.players[p].conn)G.players[p].conn.send({t:'error',msg:msg});
}

function applyAction(p,a){
  if(!G)return;
  if(a.kind==='assign'){
    if(G.phase!=='ended')return;
    if(!G.assign[p])G.assign[p]={};
    var owns=G.hands[p].some(function(c){return c.id===a.card&&isActionCard(c.id);});
    if(!owns)return;
    // Necromancer: la carta elegida debe estar realmente en el descarte.
    if(a.card==='FR28'&&a.choice!=null&&!G.discard.some(function(d){return d.id===a.choice;}))return;
    if(a.choice==null)delete G.assign[p][a.card];
    else G.assign[p][a.card]=a.choice;
    broadcast();return;
  }
  if(G.phase!=='playing')return;
  if(G.turn!==p)return sendErr(p,'No es tu turno');
  var building=isBuilding(p);

  if(a.kind==='draw'){
    if(G.turnPhase!=='pickup')return sendErr(p,'Ya has cogido carta, ahora descarta');
    if(building){
      // Duelo, formando mano: roba 2 y luego descarta 1 (neto +1).
      if(G.deck.length<2)return sendErr(p,'No quedan 2 cartas en el mazo');
      G.hands[p].push(G.deck.pop());
      G.hands[p].push(G.deck.pop());
      glog('🂠🂠 '+pname(p)+' roba 2 cartas del mazo');
    }else{
      if(!G.deck.length)return sendErr(p,'El mazo está vacío');
      G.hands[p].push(G.deck.pop());
      glog('🂠 '+pname(p)+' roba una carta del mazo');
    }
    G.turnPhase='discard';
  }else if(a.kind==='take'){
    if(G.turnPhase!=='pickup')return sendErr(p,'Ya has cogido carta, ahora descarta');
    if(!G.discard.length)return sendErr(p,'El descarte está vacío');
    var dIdx=G.discard.findIndex(function(c){return c.id===a.cid;});
    if(dIdx===-1)return sendErr(p,'Carta no encontrada en el descarte');
    var taken=G.discard.splice(dIdx,1)[0];
    G.hands[p].push(taken);
    glog('✋ '+pname(p)+' toma '+taken.name+' del descarte');
    if(building){
      // En duelo, tomar del descarte completa el turno sin descartar.
      finishTurn();broadcast();return;
    }
    G.turnPhase='discard';
  }else if(a.kind==='discard'){
    if(G.turnPhase!=='discard')return sendErr(p,'Primero roba o toma una carta');
    var hIdx=G.hands[p].findIndex(function(c){return c.id===a.cid;});
    if(hIdx===-1)return sendErr(p,'Carta no encontrada en tu mano');
    var disc=G.hands[p].splice(hIdx,1)[0];
    G.discard.push(disc);
    glog('🗑 '+pname(p)+' descarta '+disc.name);
    finishTurn();broadcast();return;
  }else return;
  broadcast();
}

// ---------- network ----------
function setupHostConn(c){
  c.on('data',function(msg){
    if(!msg||typeof msg!=='object')return;
    if(msg.t==='join'){
      var name=String(msg.name||'???').slice(0,14);
      if(G.phase!=='lobby'){
        var old=G.players.findIndex(function(p){return!p.connected&&p.name===name;});
        if(old>=0){G.players[old].conn=c;G.players[old].connected=true;
          c._hIdx=old;glog('🔌 '+name+' ha vuelto');broadcast();}
        else c.send({t:'error',msg:'Partida en curso, no puedes entrar'});
        return;
      }
      if(G.players.length>=MAXP)return c.send({t:'error',msg:'Sala llena (máx '+MAXP+')'});
      while(G.players.some(function(p){return p.name===name;}))name+='2';
      G.players.push({name:name,conn:c,connected:true});
      c._hIdx=G.players.length-1;
      broadcast();
    }else if(msg.t==='action'&&typeof c._hIdx==='number'){
      applyAction(c._hIdx,msg.a);
    }
  });
  c.on('close',function(){
    if(typeof c._hIdx!=='number')return;
    if(G.phase==='lobby'){G.players.splice(c._hIdx,1);
      G.players.forEach(function(p,i){if(p.conn)p.conn._hIdx=i;});}
    else{G.players[c._hIdx].connected=false;
      glog('🔌 '+pname(c._hIdx)+' se ha desconectado');}
    broadcast();
  });
}

function createRoom(){
  myName=el('fr-name').value.trim();
  if(!myName)return setupMsg('Pon tu nombre primero');
  var alpha='ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
  roomCode='';for(var i=0;i<4;i++)roomCode+=alpha[Math.floor(Math.random()*alpha.length)];
  setupMsg('Creando sala...');
  peer=new Peer(PREFIX+roomCode);
  peer.on('open',function(){
    isHost=true;newGame();
    G.players.push({name:myName,conn:null,connected:true});
    el('fr-setup').style.display='none';el('fr-game').style.display='block';
    broadcast();
  });
  peer.on('connection',setupHostConn);
  peer.on('error',function(e){
    if(e.type==='unavailable-id')setupMsg('Código ocupado, prueba otra vez');
    else setupMsg('Error: '+e.type);
  });
}

function joinRoom(){
  myName=el('fr-name').value.trim();
  roomCode=el('fr-code').value.trim().toUpperCase();
  if(!myName)return setupMsg('Pon tu nombre primero');
  if(roomCode.length!==4)return setupMsg('El código tiene 4 letras');
  setupMsg('Conectando...');
  peer=new Peer();
  peer.on('open',function(){
    hostConn=peer.connect(PREFIX+roomCode,{reliable:true});
    hostConn.on('open',function(){hostConn.send({t:'join',name:myName});});
    hostConn.on('data',function(msg){
      if(!msg)return;
      if(msg.t==='state'){
        V=msg.s;
        el('fr-setup').style.display='none';el('fr-game').style.display='block';
        render();
      }else if(msg.t==='error'){V?toast(msg.msg):setupMsg(msg.msg);}
    });
    hostConn.on('close',function(){toast('Se ha perdido la conexión con el anfitrión 😢');});
  });
  peer.on('error',function(e){
    if(e.type==='peer-unavailable')setupMsg('No existe la sala '+roomCode);
    else setupMsg('Error: '+e.type);
  });
}

function sendAction(a){
  if(isHost)applyAction(0,a);
  else hostConn.send({t:'action',a:a});
}

function setupMsg(m){el('fr-setupmsg').textContent=m;}

// ---------- end-of-game scoring UI ----------
function opt(val,label,sel){
  return '<option value="'+esc(String(val))+'"'+(String(sel)===String(val)?' selected':'')+'>'+esc(label)+'</option>';
}

function renderEnded(V){
  var h='';
  var myHand=V.hands[V.me]||[];
  var assign=V.myAssign||{};
  var actionCards=myHand.filter(function(c){return !c.hidden&&isActionCard(c.id);});

  // ---- assignment controls (my own action/wild cards) ----
  if(actionCards.length){
    h+='<div class="fr-panel"><h3>🎴 Asigna tus cartas especiales</h3>'+
       '<div class="fr-mut">Estas cartas dependen de tu elección. Las puntuaciones se actualizan al instante.</div>';
    actionCards.forEach(function(c){
      var gid=c.id,sel=assign[gid];
      h+='<div class="fr-action-hint"><b>'+esc(c.name)+'</b> ';
      if(c.id==='FR09'){ // Island
        var floods=myHand.filter(function(x){return !x.hidden&&x.id!==gid&&(x.suit==='flood'||x.suit==='flame');});
        h+='absorbe una Inundación/Fuego (limpia su penalización y suma su fuerza): ';
        h+='<select data-assign="single" data-card="'+gid+'"><option value="">— ninguna —</option>';
        floods.forEach(function(x){h+=opt(x.id,x.name+' ('+suitEs(x.suit)+')',sel);});
        h+='</select>';
      }else if(c.id==='FR28'){ // Necromancer
        var elig=(V.discard||[]).filter(function(x){return NECRO_SUITS.indexOf(x.suit)>=0;});
        h+='recupera del descarte un Ejército/Líder/Mago/Bestia y lo puntúa como tu 8ª carta: ';
        h+='<select data-assign="single" data-card="'+gid+'"><option value="">— ninguna —</option>';
        elig.forEach(function(x){h+=opt(x.id,x.name+' ('+suitEs(x.suit)+')',sel);});
        h+='</select>';
      }else if(c.id==='FR49'){ // Book of Changes
        var others=myHand.filter(function(x){return !x.hidden&&x.id!==gid;});
        var suits={};others.forEach(function(x){suits[x.suit]=1;});
        var t=sel?sel.target:'',su=sel?sel.suit:'';
        h+='cambia el tipo de '+
           '<select data-assign="boc-target" data-card="'+gid+'"><option value="">— carta —</option>';
        others.forEach(function(x){h+=opt(x.id,x.name,t);});
        h+='</select> a <select data-assign="boc-suit" data-card="'+gid+'"><option value="">— tipo —</option>';
        Object.keys(suits).forEach(function(s){h+=opt(s,suitEs(s),su);});
        h+='</select>';
      }else if(c.id==='FR51'||c.id==='FR52'){ // Shapeshifter / Mirage
        var suits=IMPERSONATE_SUITS[c.id];
        h+='imita a: <select data-assign="single" data-card="'+gid+'"><option value="">— elige carta —</option>';
        suits.forEach(function(su){
          Object.keys(FRDEF).forEach(function(frid){
            var d=FRDEF[frid];
            if(d.suit===su)h+=opt(frid,suitEs(su)+': '+d.name+' ('+d.strength+')',sel);
          });
        });
        h+='</select>';
      }else if(c.id==='FR53'){ // Doppelganger
        var others=myHand.filter(function(x){return !x.hidden&&x.id!==gid&&x.suit!=='wild';});
        h+='duplica una carta de tu mano: <select data-assign="single" data-card="'+gid+'"><option value="">— ninguna —</option>';
        others.forEach(function(x){h+=opt(x.id,x.name,sel);});
        h+='</select>';
      }
      h+='</div>';
    });
    h+='</div>';
  }

  // ---- standings ----
  var scores=V.scores;
  if(scores){
    var order=V.names.map(function(_,i){return i;}).sort(function(a,b){return scores[b].total-scores[a].total;});
    var best=scores[order[0]].total;
    h+='<div class="fr-panel"><h3>🏆 Puntuaciones</h3>';
    order.forEach(function(i,rank){
      var s=scores[i],win=s.total===best;
      h+='<div style="margin:8px 0;padding:8px 10px;border-radius:8px;background:'+(win?'#3b2f12':'#1f2530')+
         (win?';border:1px solid #fbbf24':'')+'">';
      h+='<div style="font-weight:700;font-size:16px">'+(win?'👑 ':'')+(rank+1)+'. '+esc(V.names[i])+
         (i===V.me?' (tú)':'')+' — <span style="color:#fbbf24">'+s.total+'</span> pts</div>';
      h+='<div class="fr-mut" style="margin-top:4px;line-height:1.5">';
      s.breakdown.forEach(function(b){
        if(b.blanked){
          h+='<div style="opacity:.45;text-decoration:line-through">'+esc(b.name)+' — anulada</div>';
        }else{
          var extra='';
          if(b.bonus)extra+=' <span style="color:#4ade80">+'+b.bonus+'</span>';
          if(b.penalty)extra+=' <span style="color:#f87171">'+b.penalty+'</span>';
          h+='<div><b>'+b.sub+'</b> · '+esc(b.name)+' <span style="opacity:.6">('+b.base+extra+')</span></div>';
        }
      });
      h+='</div></div>';
    });
    h+='</div>';
  }
  return h;
}

// ---------- render ----------
function render(){
  if(!V)return;

  // snapshot card positions and deck rect before DOM wipe
  var prev={};
  el('fr-game').querySelectorAll('[data-cid]').forEach(function(e){
    prev[e.getAttribute('data-cid')]=e.getBoundingClientRect();
  });
  var deckEl=el('fr-game').querySelector('[data-zone="deck"]');
  var deckRect=deckEl?deckEl.getBoundingClientRect():null;

  var h='';
  h+='<div class="fr-row">Sala: <span class="fr-code">'+roomCode+'</span> '+
     '<button data-act="copy">📋 Copiar enlace</button></div>';

  if(V.phase==='lobby'){
    h+='<div class="fr-panel"><h3>Jugadores ('+V.names.length+'/'+MAXP+')</h3>';
    V.names.forEach(function(n,i){
      h+='<div>'+esc(n)+(i===0?' 👑':'')+(i===V.me?' (tú)':'')+'</div>';});
    h+='</div>';
    if(V.me===0){
      h+='<button data-act="start"'+(V.names.length<MINP?' disabled':'')+'>🚀 Empezar ('+
        (V.names.length<MINP?'mínimo '+MINP:V.names.length+' jugadores')+')</button>';
    }else{
      h+='<div class="fr-mut">Esperando a que '+esc(V.names[0])+' empiece...</div>';
    }
  }else{
    if(V.phase==='ended'){
      h+='<div class="fr-banner">🏁 ¡Fin de la partida!'+
         (V.me===0?' <button data-act="start">🔄 Otra partida</button>':'')+'</div>';
      h+=renderEnded(V);
    }

    var myTurn=V.phase==='playing'&&V.turn===V.me;
    var canPickup=myTurn&&V.turnPhase==='pickup';
    var canDiscard=myTurn&&V.turnPhase==='discard';
    // Duelo: ¿estoy aún formando mi mano inicial (al inicio del turno, antes de robar)?
    var iAmBuilding=V.duel&&V.hands[V.me]&&V.hands[V.me].length<HAND_SIZE;
    var drawTitle=iAmBuilding?'Robar 2 del mazo':'Robar del mazo';
    var discMax=V.duel?(DUEL_MIN_DISCARD+'+'):MAX_DISCARD;

    if(V.phase==='playing'){
      if(myTurn){
        if(canPickup){
          if(iAmBuilding)h+='<div class="fr-action-hint">✨ Tu turno ('+V.hands[V.me].length+'/'+HAND_SIZE+' cartas): roba <b>2</b> del mazo (descartarás 1) <b>o</b> toma 1 del descarte (sin descartar)</div>';
          else h+='<div class="fr-action-hint">✨ Tu turno: roba del mazo <b>o</b> toma una carta del descarte</div>';
        }
        else h+='<div class="fr-action-hint">🗑 Ahora elige una carta de tu mano para descartar</div>';
      }else{
        h+='<div class="fr-mut">Turno de <b>'+esc(V.names[V.turn])+'</b>'+
           (V.turnPhase==='discard'?' (descartando...)':'')+'...</div>';
      }
    }

    // Board: deck + discard
    h+='<div class="fr-panel"><div class="fr-board-row">';

    // Deck
    h+='<div class="fr-zone"><div class="fr-zone-label">Mazo</div>';
    if(V.deckCount>0){
      var deckCls='fr-card-sm fr-back'+(canPickup?' fr-clickable fr-pickup-target':'');
      h+='<div class="'+deckCls+'" data-zone="deck"'+(canPickup?' data-act="draw"':'')+
         ' style="background-image:url('+BACK+');background-size:cover;background-position:center" title="'+drawTitle+' ('+V.deckCount+')">'+
         '<span class="fr-back-count">'+V.deckCount+'</span></div>';
    }else{
      h+='<div class="fr-card-sm fr-empty" title="Mazo vacío">∅</div>';
    }
    h+='</div>';

    // Discard
    h+='<div class="fr-zone fr-discard-zone"><div class="fr-zone-label">Descarte ('+V.discard.length+'/'+discMax+')</div>';
    h+='<div class="fr-discard-fan">';
    if(!V.discard.length){
      h+='<div class="fr-card-sm fr-empty" title="Descarte vacío">🗑</div>';
    }else{
      V.discard.forEach(function(card){
        var cls='fr-card-sm'+(canPickup?' fr-clickable fr-pickup-target':'');
        var act=canPickup?'take':null;
        h+=cardHtml(card,cls,act);
      });
    }
    h+='</div></div>';
    h+='</div></div>';

    // Hands — current player always first
    var nPlayers=V.names.length;
    Array.from({length:nPlayers},function(_,k){return(V.me+k)%nPlayers;}).forEach(function(i){
      var name=V.names[i];
      var isTurn=V.phase==='playing'&&V.turn===i;
      var isMe=i===V.me;
      h+='<div class="fr-panel'+(isTurn?' fr-turn':'')+'">'+
         '<div class="fr-name">'+(isTurn?'▶ ':'')+esc(name)+(isMe?' (tú)':'')+(V.connected[i]?'':' 🔌❌')+'</div>';
      h+='<div class="fr-hand" data-pl="'+i+'">';
      if(isMe){
        sortByType(V.hands[i]).forEach(function(card){
          var clickable=canDiscard;
          var cls='fr-card'+(clickable?' fr-clickable fr-discard-target':'');
          var act=clickable?'discard':null;
          h+=cardHtml(card,cls,act,null);
        });
      }else{
        V.hands[i].forEach(function(card){
          if(card.hidden){
            h+='<div class="fr-card-opp fr-back" style="background-image:url('+BACK+');background-size:cover;background-position:center" title="Carta oculta"></div>';
          }else{
            h+=cardHtml(card,'fr-card-opp','',null);
          }
        });
      }
      h+='</div>';
      h+='</div>';
    });

    // Log
    h+='<div class="fr-panel fr-log">'+V.log.slice().reverse().map(function(l){
      return'<div>'+esc(l)+'</div>';}).join('')+'</div>';
  }

  el('fr-game').innerHTML=h;

  // FLIP animation
  el('fr-game').querySelectorAll('[data-cid]').forEach(function(e){
    var cid=e.getAttribute('data-cid');
    var r0=prev[cid];
    var r1=e.getBoundingClientRect();
    var dx,dy;
    if(r0){
      dx=r0.left-r1.left;dy=r0.top-r1.top;
      if(!dx&&!dy)return;
    }else{
      // card is new to the DOM
      var inMyHand=e.closest('[data-pl="'+V.me+'"]');
      if(inMyHand&&deckRect){
        dx=deckRect.left-r1.left;dy=deckRect.top-r1.top;
      }else{
        e.classList.add('fr-new');return;
      }
    }
    e.style.zIndex='50';
    e.style.transition='none';
    e.style.transform='translate('+dx+'px,'+dy+'px)';
    e.getBoundingClientRect(); // force reflow
    e.style.transition='transform .45s ease';
    e.style.transform='';
    e.addEventListener('transitionend',function(){
      e.style.transition='';e.style.zIndex='';
    },{once:true});
  });
}

// ---------- events ----------
el('fr-create').addEventListener('click',createRoom);
el('fr-join').addEventListener('click',joinRoom);
el('fr-game').addEventListener('click',function(e){
  var b=e.target.closest('[data-act]');
  if(!b)return;
  var act=b.getAttribute('data-act');
  var cid=b.getAttribute('data-cid');

  if(act==='copy'){
    navigator.clipboard.writeText(location.origin+location.pathname+'?sala='+roomCode)
      .then(function(){toast('Enlace copiado 📋');});
  }else if(act==='start'&&isHost){
    startGame();
  }else if(act==='draw'){
    if(V&&V.turn===V.me&&V.turnPhase==='pickup'&&V.deckCount>0)
      sendAction({kind:'draw'});
  }else if(act==='take'){
    if(V&&V.turn===V.me&&V.turnPhase==='pickup'&&cid!==null)
      sendAction({kind:'take',cid:cid});
  }else if(act==='discard'){
    if(V&&V.turn===V.me&&V.turnPhase==='discard'&&cid!==null)
      sendAction({kind:'discard',cid:cid});
  }
});

// ---------- assignment selects (end of game) ----------
el('fr-game').addEventListener('change',function(e){
  var s=e.target.closest('[data-assign]');
  if(!s)return;
  var type=s.getAttribute('data-assign');
  var card=s.getAttribute('data-card');
  if(type==='boc-target'||type==='boc-suit'){
    var t=el('fr-game').querySelector('[data-assign="boc-target"][data-card="'+card+'"]');
    var su=el('fr-game').querySelector('[data-assign="boc-suit"][data-card="'+card+'"]');
    var choice=(t.value!==''&&su.value!=='')?{target:t.value,suit:su.value}:null;
    sendAction({kind:'assign',card:card,choice:choice});
  }else{
    sendAction({kind:'assign',card:card,choice:s.value===''?null:s.value});
  }
});

// ---------- tooltip (discard only) ----------
var tip=document.createElement('div');tip.id='fr-tip';document.body.appendChild(tip);

el('fr-game').addEventListener('mouseover',function(e){
  var card=e.target.closest('.fr-discard-fan [data-cid]');
  if(!card){tip.style.display='none';return;}
  tip.style.backgroundImage=card.style.backgroundImage;
  tip.style.backgroundSize=card.style.backgroundSize;
  tip.style.backgroundPosition=card.style.backgroundPosition;
  tip.style.display='block';
});
el('fr-game').addEventListener('mousemove',function(e){
  if(tip.style.display==='none')return;
  var x=e.clientX+18,y=e.clientY-182;
  if(x+260>window.innerWidth)x=e.clientX-278;
  if(y<8)y=8;
  if(y+364>window.innerHeight)y=window.innerHeight-372;
  tip.style.left=x+'px';tip.style.top=y+'px';
});
el('fr-game').addEventListener('mouseleave',function(){tip.style.display='none';});
el('fr-game').addEventListener('mouseout',function(e){
  if(!e.target.closest('.fr-discard-fan'))tip.style.display='none';
});

var m=location.search.match(/sala=([A-Za-z0-9]{4})/);
if(m)el('fr-code').value=m[1].toUpperCase();
})();
</script>

## ¿Cómo se juega?

Fantasy Realms es un juego de **colección de sets**: cada jugador construye una mano de **7 cartas** que combinen bien entre sí. Las cartas tienen efectos sinérgicos que aumentan o reducen la puntuación final.

**Preparación:** Se reparten 7 cartas a cada jugador. Se elige un jugador inicial al azar.

**En tu turno:**

1. **Roba o toma:** Coge la carta de encima del mazo, *o bien* toma cualquier carta visible del descarte. (El primer jugador debe robar del mazo.)
2. **Descarta:** Pon una carta de tu mano (ahora de 8) en el descarte. Todas las cartas del descarte quedan visibles.

**Fin de partida:** La partida termina cuando hay **10 cartas** en el descarte. Todos los jugadores revelan sus manos y la app **calcula la puntuación automáticamente** (bonus, penalizaciones y cartas anuladas). Si tienes cartas especiales (**Island**, **Necromancer**, **Book of Changes**, **Shapeshifter**, **Mirage** o **Doppelgänger**) eliges a qué imitan, qué modifican o qué recuperan, y el marcador se actualiza al instante.

### Variante a 2 jugadores (duelo)

Con **2 jugadores** la app activa automáticamente la variante en duelo, con reglas distintas:

- Empezáis **sin cartas** en la mano.
- **En tu turno**, elige una de dos:
  1. **Roba 2** del mazo y **descarta 1** (tu mano crece en 1), *o bien*
  2. **Toma 1 carta del descarte** (sin descartar nada).
- Cuando un jugador llega a **7 cartas**, sigue jugando turnos normales (roba 1 del mazo o del descarte y descarta 1, manteniéndose en 7).
- **Fin:** cuando **ambos** jugadores tenéis 7 cartas **y** hay **al menos 12** cartas en el descarte.

Las cartas están agrupadas por suit: **Land** · **Flood** · **Weather** · **Flame** · **Army** · **Wizard** · **Leader** · **Beast** · **Weapon** · **Artifact** · **Wild**. La puntuación depende de las sinergias entre cartas y la calcula la app al terminar; consultad el reglamento para entender cada efecto.

> ℹ️ **Necromancer**: al final eliges una carta de Ejército, Líder, Mago o Bestia del descarte y se puntúa como tu 8ª carta (con todos sus bonus, penalizaciones y anulaciones).
