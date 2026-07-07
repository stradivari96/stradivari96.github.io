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

Juego cooperativo de bazas para 3–4 jugadores. Un anfitrión crea la sala y comparte el enlace.

<style>
#lotr-app{--bg:#0d1117;--panel:#161b27;--text:#e6edf3;--mut:#7d8aa3;--border:#252d42;
  --blue:#58a6ff;--gold:#fbbf24;--ring:#d4af37;--green:#16a34a;--red:#dc2626;
  background:var(--bg);color:var(--text);border-radius:12px;padding:16px;
  font-family:system-ui,sans-serif;font-size:15px;line-height:1.4;min-height:200px}
#lotr-app h3{margin:10px 0 6px;color:var(--text)}
#lotr-app input{background:#0a0d14;color:var(--text);border:1px solid var(--border);
  border-radius:8px;padding:8px 10px;font-size:15px;max-width:160px}
#lotr-app select{background:#0a0d14;color:var(--text);border:1px solid var(--border);
  border-radius:8px;padding:7px 10px;font-size:15px}
#lotr-app button{background:var(--blue);color:#fff;border:0;border-radius:8px;
  padding:7px 13px;font-size:14px;cursor:pointer;margin:2px}
#lotr-app button.l-sm{padding:5px 10px;font-size:13px}
#lotr-app button:disabled{background:var(--border);color:var(--mut);cursor:not-allowed}
#lotr-app button.l-warn{background:#dc2626}
#lotr-app button.l-ok{background:#16a34a}
.l-row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin:6px 0}
.l-panel{background:var(--panel);border-radius:10px;padding:10px 12px;margin:6px 0}
.l-code{font-size:22px;font-weight:800;letter-spacing:4px;color:var(--gold)}
.l-mut{color:var(--mut);font-size:13px;margin:3px 0}
.l-name{font-weight:600;font-size:14px;margin-bottom:5px}
/* cards */
.lcard{width:84px;height:118px;border-radius:8px;border:2px solid #30363d;flex:none;
  position:relative;user-select:none;
  background-image:url('https://steamusercontent-a.akamaihd.net/ugc/34440677816838739/C609A55EEEB4A08A3F74F90AA4AA90CF9DEC1EF4/');
  background-size:800% 500%}
.lcard-sm{width:54px;height:76px;border-radius:6px}
.lcard-back{background-image:url('https://steamusercontent-a.akamaihd.net/ugc/34440677816838931/2DC6F20372D3BB0AA7A1ACB960616AE311F84ACD/');
  background-size:100% 100%}
.lchar{width:84px;height:118px;border-radius:8px;border:2px solid #30363d;flex:none;
  position:relative;user-select:none;
  background-image:url('https://steamusercontent-a.akamaihd.net/ugc/34441311448452068/A45E85531AD7219FFD87731E1057D8628D2F0D7A/');
  background-size:500% 500%}
.lchar-sm{width:48px;height:67px;border-radius:6px}
.lcard-legal{cursor:pointer}
.lcard-legal:hover{outline:2px solid var(--blue);outline-offset:2px}
.lcard-blocked{opacity:.4;filter:grayscale(.6);cursor:not-allowed}
.lcard-sel{outline:3px solid var(--gold)!important;outline-offset:2px}
.l-val{position:absolute;bottom:1px;right:3px;font-size:11px;font-weight:800;color:#fff;
  text-shadow:0 1px 4px rgba(0,0,0,.95)}
.l-suit{position:absolute;top:1px;left:3px;font-size:12px;line-height:1}
.l-hand{display:flex;flex-wrap:wrap;gap:6px;align-items:center}
.l-clickable{cursor:pointer}
.l-pickable{cursor:pointer}
.l-pickable:hover{outline:2px solid var(--gold);outline-offset:2px}
.l-active{box-shadow:0 0 0 2px var(--gold) inset;border-radius:10px}
/* trick area */
.l-trick{display:flex;flex-wrap:wrap;gap:10px;align-items:flex-end;min-height:130px;
  background:#11151f;border:1px dashed var(--border);border-radius:10px;padding:10px}
.l-trick-slot{display:flex;flex-direction:column;align-items:center;gap:4px}
.l-trick-winner{outline:3px solid var(--green);outline-offset:2px;border-radius:8px}
/* log */
.l-log{max-height:130px;overflow-y:auto;font-size:13px;color:var(--mut)}
.l-log div{padding:1px 0}
/* toast */
.l-toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);background:#dc2626;
  color:#fff;padding:10px 18px;border-radius:10px;z-index:99;box-shadow:0 4px 14px rgba(0,0,0,.4)}
.l-obj{background:#1a2233;border:1px solid var(--ring);border-radius:8px;padding:8px 10px;margin:5px 0}
</style>

<div id="lotr-app">
  <div id="l-setup">
    <div class="l-row"><label>Tu nombre: <input id="l-name" maxlength="14" placeholder="Nombre"></label></div>
    <div class="l-row"><button id="l-create">💍 Crear sala</button></div>
    <div class="l-row">
      <input id="l-codein" maxlength="4" placeholder="CÓDIGO" style="text-transform:uppercase;width:100px">
      <button id="l-join">Unirse</button>
    </div>
    <div id="l-setupmsg" class="l-mut"></div>
  </div>
  <div id="l-game" style="display:none"></div>
</div>

<script src="https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js"></script>
<script>
(function(){
'use strict';

var MAIN_URL='https://steamusercontent-a.akamaihd.net/ugc/34440677816838739/C609A55EEEB4A08A3F74F90AA4AA90CF9DEC1EF4/';
var BACK_URL='https://steamusercontent-a.akamaihd.net/ugc/34440677816838931/2DC6F20372D3BB0AA7A1ACB960616AE311F84ACD/';
var CHAR_URL='https://steamusercontent-a.akamaihd.net/ugc/34441311448452068/A45E85531AD7219FFD87731E1057D8628D2F0D7A/';
var CHARB_URL='https://steamusercontent-a.akamaihd.net/ugc/34441311448452379/E6CE202864F928F273B4926AAA1147A7FF5004AF/'; // cara alternativa (forma de 3 jugadores)
var RINGLOCK_URL='https://steamusercontent-a.akamaihd.net/ugc/34440677816844739/BF68B4B3D2BCB3A98F6F0EAFBF63E8060F77042F/'; // Ring no roto: no se puede salir con Ring
var RINGOK_URL='https://steamusercontent-a.akamaihd.net/ugc/34440677816844933/7F778D3AACAFA19E686D9B6A9957DDEE181EF110/'; // Ring roto: ya se puede salir con Ring
var PREFIX='lotr-xiang-';
var MAXP=4,MINP=3;
var RING1=32; // global card index of 💍 Ring 1

// ---- suits ----
// 5 palos: 4 normales (1-8) + Ring (triunfo, 1-5). Índice global 0..36.
var SUITS=[
  {key:'mountain',name:'Mountain',emoji:'⛰️',start:0,count:8},
  {key:'hill',    name:'Hill',    emoji:'🏔️',start:8,count:8},
  {key:'forest',  name:'Forest',  emoji:'🌲',start:16,count:8},
  {key:'shadow',  name:'Shadow',  emoji:'🌑',start:24,count:8},
  {key:'ring',    name:'Ring',    emoji:'💍',start:32,count:5}
];
var SUITRANK={mountain:0,hill:1,forest:2,shadow:3,ring:4};
function suitOf(pos){for(var i=0;i<SUITS.length;i++){var s=SUITS[i];if(pos>=s.start&&pos<s.start+s.count)return s;}return null;}
function suitKey(pos){return suitOf(pos).key;}
function valOf(pos){var s=suitOf(pos);return pos-s.start+1;}
function isRing(pos){return suitKey(pos)==='ring';}
function cardName(pos){var s=suitOf(pos);return s.emoji+' '+s.name+' '+valOf(pos);}

// ---- characters (sprite 5x5) ----
var CHARS=[
  'Frodo','Bilbo','Merry','Gildor Inglorien','Fatty Bolger',
  'Gandalf','Pippin','Sam','Farmer Maggot','Tom Bombadil',
  'Strider','Goldberry','Barliman Butterbur','Mr. Underhill','Glorfindel',
  'Bill the Pony','Glóin','Arwen','Elrond','Aragorn',
  'Bilbo Baggins','Boromir','Radagast','Shadowfax','Gwaihir'
];
// Personajes con forma alternativa para 3 jugadores: se muestran con el sprite trasero.
var CHAR_3P_BACK={0:true}; // Frodo
function charUsesBack(ci,nP){return!!CHAR_3P_BACK[ci]&&nP<=3;}

// ---- CAPÍTULOS ----
// Cada capítulo:
//   mandatory[] -> índices de CHARS obligatorios (siempre en juego)
//   optional[]  -> índices opcionales; se añaden EN ORDEN hasta tener tantos
//                  personajes como jugadores (cada jugador draftea uno)
//   obj[charIdx]-> texto del objetivo de ese personaje en este capítulo
// Los objetivos por ahora son solo descriptivos (los jugadores deciden si se cumplen),
// igual que en take-time. Más adelante se pueden auto-evaluar.
var CHAPTERS=[
  {n:'1 — Una reunión muy esperada',
    mandatory:[0,1],      // Frodo*, Bilbo*
    optional:[5,6],       // Gandalf, Pippin
    obj:{
      0:{ // Frodo: 3j → 4+ anillos · 4j → 2+ anillos
        text:function(nP){return 'Gana '+(nP>=4?2:4)+' o más cartas de Anillo 💍';},
        check:function(st,i,nP){return ringsWon(st,i)>=(nP>=4?2:4);},
        prog:function(st,i,nP){return ringsWon(st,i)+'/'+(nP>=4?2:4)+' 💍';}
      },
      1:'(objetivo de Bilbo — rellenar)',
      5:'(objetivo de Gandalf — rellenar)',
      6:'(objetivo de Pippin — rellenar)'
    },
    // Fase de preparación (antes de jugar bazas). Por personaje (índice de CHARS):
    //   takeLost     -> añade la carta perdida a su mano
    //   exchangeWith -> intercambia una carta con ese personaje (da carta oculta;
    //                   el receptor la ve y devuelve una, que puede ser la misma)
    prep:{
      5:{takeLost:true,exchangeWith:0} // Gandalf: coge la carta perdida e intercambia con Frodo
    }}
];
function chapterChars(ci,nP){
  var ch=CHAPTERS[ci];
  if(!ch){var d=[];for(var k=0;k<nP;k++)d.push(k);return d;}
  var out=(ch.mandatory||[]).slice();
  var opt=ch.optional||[];
  for(var i=0;i<opt.length&&out.length<nP;i++)out.push(opt[i]);
  return out;
}

var peer=null,isHost=false,hostConn=null,roomCode='';
var G=null,V=null,myName='';
var selectedPos=null,pendingRing1=null;

function el(id){return document.getElementById(id);}
function esc(s){return String(s).replace(/[&<>"']/g,function(c){return{'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];});}
function toast(msg){var t=document.createElement('div');t.className='l-toast';t.textContent=msg;el('lotr-app').appendChild(t);setTimeout(function(){t.remove();},3000);}
function setupMsg(m){el('l-setupmsg').textContent=m;}

// ---- sprite styles ----
function cardStyle(pos){var col=pos%8,row=Math.floor(pos/8);
  return "background-image:url('"+MAIN_URL+"');background-size:800% 500%;background-position:"+(col/7*100).toFixed(2)+"% "+(row/4*100).toFixed(2)+"%";}
function charStyle(ci,back){var col=ci%5,row=Math.floor(ci/5);
  return "background-image:url('"+(back?CHARB_URL:CHAR_URL)+"');background-size:500% 500%;background-position:"+(col/4*100).toFixed(2)+"% "+(row/4*100).toFixed(2)+"%";}

// ---- deck ----
function mkDeck(){var d=[];for(var i=0;i<37;i++)d.push(i);
  for(var k=d.length-1;k>0;k--){var j=Math.floor(Math.random()*(k+1));var t=d[k];d[k]=d[j];d[j]=t;}return d;}
function sortHand(h){return h.slice().sort(function(a,b){
  return SUITRANK[suitKey(a)]-SUITRANK[suitKey(b)]||valOf(a)-valOf(b);});}

// ---- game logic (host) ----
function newGame(){G={phase:'lobby',players:[],chapter:0,log:[]};}

function startGame(){
  var n=G.players.length;
  var deck=mkDeck();
  var per=Math.floor(37/n);          // 3p->12, 4p->9
  G.hands=[];for(var i=0;i<n;i++)G.hands.push(deck.slice(i*per,(i+1)*per));
  G.lost=deck[per*n];                // 1 carta perdida boca arriba
  G.ring1Holder=-1;
  for(var p=0;p<n;p++)if(G.hands[p].indexOf(RING1)>=0)G.ring1Holder=p;
  G.chars=chapterChars(G.chapter,n);
  G.assigned=G.players.map(function(){return null;});
  G.won=G.players.map(function(){return[];});
  G.trick=[];G.lastTrick=null;G.ringsBroken=false;
  if(G.ring1Holder<0){G.ring1Holder=0;G.draftTurn=0;}
  else G.draftTurn=G.ring1Holder;
  G.turn=G.ring1Holder;G.trickLeader=G.ring1Holder;
  G.phase='draft';
  G.log=['💍 Capítulo '+CHAPTERS[G.chapter].n+'. Repartidas las cartas y 1 carta perdida.'];
  if(deck[per*n]===RING1)glog('⚠️ El Ring 1 es la carta perdida: empieza '+pname(0));
  else glog('💍 '+pname(G.ring1Holder)+' tiene el Ring 1 — elige personaje primero.');
  broadcast();
}

function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>80)G.log.shift();}
function nextWithCards(start){var n=G.players.length;for(var k=0;k<n;k++){var q=(start+k)%n;if(G.hands[q].length>0)return q;}return start;}

// ---- fase de preparación (habilidades de personaje antes de las bazas) ----
function prepDesc(ci,def){
  var parts=[];
  if(def.takeLost)parts.push('añade la carta perdida a su mano');
  if(def.exchangeWith!==undefined)parts.push('intercambia una carta con '+CHARS[def.exchangeWith]);
  return CHARS[ci]+': '+parts.join(' y ');
}
function curPrep(){
  if(G.phase!=='prep')return null;
  var pl=G.prepQueue[G.prepIdx];if(pl===undefined)return null;
  var ci=G.assigned[pl];
  return{player:pl,ci:ci,def:CHAPTERS[G.chapter].prep[ci]};
}
function startPrep(){
  var pr=CHAPTERS[G.chapter].prep;
  G.prepQueue=[];G.prepIdx=0;G.exchange=null;
  if(pr)G.chars.forEach(function(ci){var pl=G.assigned.indexOf(ci);if(pl>=0&&pr[ci])G.prepQueue.push(pl);});
  if(!G.prepQueue.length){startPlay();return;}
  G.phase='prep';
  beginPrepEntry();
}
function beginPrepEntry(){
  var e=curPrep();if(!e){startPlay();return;}
  var d=e.def;
  if(d.takeLost&&G.lost!=null){
    G.hands[e.player].push(G.lost);
    glog('🪄 '+pname(e.player)+' ('+CHARS[e.ci]+') añade la carta perdida a su mano');
    G.lost=null;
  }
  if(d.exchangeWith!==undefined){
    var to=G.assigned.indexOf(d.exchangeWith);
    if(to>=0&&to!==e.player){
      G.exchange={from:e.player,to:to,toCi:d.exchangeWith,stage:'send',sent:null};
      glog('🔄 '+pname(e.player)+' intercambiará una carta con '+pname(to)+' ('+CHARS[d.exchangeWith]+')');
      return;
    }
  }
  advancePrep();
}
function advancePrep(){
  G.exchange=null;G.prepIdx++;
  if(G.prepIdx>=G.prepQueue.length){startPlay();return;}
  beginPrepEntry();
}
function startPlay(){
  G.phase='play';G.exchange=null;
  G.turn=nextWithCards(G.ring1Holder);G.trickLeader=G.turn;
  glog('🃏 ¡A jugar! Sale '+pname(G.turn));
}

// ---- trick rules ----
// Legal: hay que seguir el palo de salida si puedes. Si no puedes seguir (fallo),
// puedes jugar cualquier carta, incluido un Ring (triunfo). No puedes SALIR con un
// Ring hasta que el triunfo esté "roto" (alguien jugó un Ring por no poder seguir),
// salvo que solo te queden Rings.
function legalCards(hand,trick,broken){
  if(!trick.length){ // salida
    var hasNonRing=hand.some(function(c){return!isRing(c);});
    if(broken||!hasNonRing)return hand.slice();      // triunfo roto / solo Rings → cualquier carta
    return hand.filter(function(c){return!isRing(c);}); // si no, no puedes salir con Ring
  }
  var led=suitKey(trick[0].pos);
  var follow=hand.filter(function(c){return suitKey(c)===led;});
  return follow.length?follow:hand.slice(); // fallo: cualquier carta (incl. Ring)
}
function playError(hand,trick,pos,broken){
  if(legalCards(hand,trick,broken).indexOf(pos)<0){
    if(!trick.length&&isRing(pos))return 'No puedes salir con un Ring hasta que alguien haya fallado con uno';
    return 'Debes seguir el palo de salida';
  }
  return null;
}
// Ganador de la baza. Ring triunfa. Ring 1: su dueño eligió al jugarlo (win) si gana
// la baza o si deja que un Ring mayor le gane.
function trickWinner(trick){
  var led=suitKey(trick[0].pos);
  var r1=trick.filter(function(t){return t.pos===RING1;})[0];
  if(r1&&r1.win)return r1.player;                       // Ring 1 decide ganar: vence a todo
  var rings=trick.filter(function(t){return isRing(t.pos)&&t.pos!==RING1;});
  if(rings.length){rings.sort(function(a,b){return valOf(b.pos)-valOf(a.pos);});return rings[0].player;}
  var ledCards=trick.filter(function(t){return suitKey(t.pos)===led;});
  ledCards.sort(function(a,b){return valOf(b.pos)-valOf(a.pos);});
  return ledCards[0].player;
}

// ---- objetivos ----
// Cartas ganadas (acumuladas) por el jugador i. Funciona con G (host) o V (cliente):
// ambos llevan `won` = lista de bazas, cada baza = lista de índices de carta.
function wonCards(st,i){return(st.won[i]||[]).reduce(function(a,t){return a.concat(t);},[]);}
function ringsWon(st,i){return wonCards(st,i).filter(isRing).length;}
function tricksWon(st,i){return(st.won[i]||[]).length;}
// Devuelve {text, met, prog} para el personaje `ci` del jugador `i`.
// obj puede ser: string (solo descriptivo) u objeto {text(nP), check(st,i,nP), prog(st,i,nP)}.
function evalObjective(chapterIdx,ci,st,i,nP){
  var ch=CHAPTERS[chapterIdx];var entry=ch&&ch.obj&&ch.obj[ci];
  if(entry==null)return{text:'(objetivo pendiente de definir)',met:null,prog:''};
  if(typeof entry==='string')return{text:entry,met:null,prog:''};
  return{
    text:typeof entry.text==='function'?entry.text(nP):entry.text,
    met:entry.check?entry.check(st,i,nP):null,
    prog:entry.prog?entry.prog(st,i,nP):''
  };
}

function buildStateFor(me){
  var base={
    me:me,phase:G.phase,chapter:G.chapter,
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    log:G.log.slice(-30)
  };
  if(G.phase==='lobby')return base;
  return{
    me:me,phase:G.phase,chapter:G.chapter,
    names:base.names,connected:base.connected,
    ring1Holder:G.ring1Holder,lost:G.lost,
    chars:G.chars,assigned:G.assigned,
    draftTurn:G.draftTurn,turn:G.turn,trickLeader:G.trickLeader,ringsBroken:G.ringsBroken,
    myHand:sortHand(G.hands[me]),
    handCounts:G.hands.map(function(h){return h.length;}),
    trick:G.trick.map(function(t){return{player:t.player,pos:t.pos,win:t.win};}),
    won:G.won.map(function(w){return w.slice();}),
    prep:(G.phase==='prep'&&curPrep())?{player:curPrep().player,text:prepDesc(curPrep().ci,curPrep().def)}:null,
    exchange:G.exchange?{from:G.exchange.from,to:G.exchange.to,toCi:G.exchange.toCi,stage:G.exchange.stage}:null,
    receivedCard:(G.exchange&&G.exchange.stage==='return'&&me===G.exchange.to)?G.exchange.sent:null,
    lastTrick:G.lastTrick,
    log:G.log.slice(-30)
  };
}

function broadcast(){
  G.players.forEach(function(p,i){if(i===0)return;if(p.conn&&p.connected)p.conn.send({t:'state',s:buildStateFor(i)});});
  V=buildStateFor(0);selectedPos=null;pendingRing1=null;render();
}
function sendErr(p,msg){if(p===0)toast(msg);else if(G.players[p].conn)G.players[p].conn.send({t:'error',msg:msg});}

function applyAction(p,a){
  if(!G)return;
  if(a.kind==='setChapter'){
    if(!isHost||G.phase!=='lobby')return;
    var ch=+a.ch;if(ch<0||ch>=CHAPTERS.length)return;
    G.chapter=ch;broadcast();return;
  }
  if(a.kind==='start'){if(!isHost||G.phase!=='lobby')return;startGame();return;}
  if(a.kind==='restart'){if(!isHost)return;G.phase='lobby';glog('🔄 Nueva partida');broadcast();return;}
  if(a.kind==='pickChar'){
    if(G.phase!=='draft')return;
    if(G.draftTurn!==p)return sendErr(p,'No es tu turno de elegir personaje');
    var c=+a.char;
    if(G.chars.indexOf(c)<0||G.assigned.indexOf(c)>=0)return sendErr(p,'Personaje no disponible');
    G.assigned[p]=c;
    glog('👤 '+pname(p)+' elige a '+CHARS[c]);
    var n=G.players.length,next=(p+1)%n,guard=0;
    while(G.assigned[next]!==null&&guard++<n)next=(next+1)%n;
    if(G.assigned.every(function(x){return x!==null;}))startPrep();
    else G.draftTurn=next;
    broadcast();return;
  }
  if(a.kind==='prepGive'){
    if(G.phase!=='prep'||!G.exchange||G.exchange.stage!=='send')return;
    if(G.exchange.from!==p)return sendErr(p,'No te toca dar una carta');
    var gpos=+a.pos,gidx=G.hands[p].indexOf(gpos);
    if(gidx<0)return sendErr(p,'No tienes esa carta');
    G.hands[p].splice(gidx,1);
    G.hands[G.exchange.to].push(gpos);
    G.exchange.sent=gpos;G.exchange.stage='return';
    glog('🔄 '+pname(p)+' pasa una carta (oculta) a '+pname(G.exchange.to));
    broadcast();return;
  }
  if(a.kind==='prepReturn'){
    if(G.phase!=='prep'||!G.exchange||G.exchange.stage!=='return')return;
    if(G.exchange.to!==p)return sendErr(p,'No te toca devolver una carta');
    var rpos=+a.pos,ridx=G.hands[p].indexOf(rpos);
    if(ridx<0)return sendErr(p,'No tienes esa carta');
    G.hands[p].splice(ridx,1);
    G.hands[G.exchange.from].push(rpos);
    glog('🔄 '+pname(p)+' devuelve una carta (oculta) a '+pname(G.exchange.from));
    advancePrep();
    broadcast();return;
  }
  if(a.kind==='play'){
    if(G.phase!=='play')return;
    if(G.turn!==p)return sendErr(p,'No es tu turno');
    var pos=+a.pos,hIdx=G.hands[p].indexOf(pos);
    if(hIdx<0)return sendErr(p,'No tienes esa carta');
    var er=playError(G.hands[p],G.trick,pos,G.ringsBroken);
    if(er)return sendErr(p,er);
    G.hands[p].splice(hIdx,1);
    if(isRing(pos)&&!G.ringsBroken){G.ringsBroken=true;glog('💍 ¡El triunfo (Ring) queda roto! Ya se puede salir con Ring.');}
    G.trick.push({player:p,pos:pos,win:pos===RING1?!!a.win:false});
    glog('🎴 '+pname(p)+' juega '+cardName(pos)+(pos===RING1?(a.win?' (decide GANAR)':' (deja pasar)'):''));
    var n=G.players.length;
    // baza completa: todo jugador o ya jugó esta baza o se quedó sin cartas
    var complete=G.players.every(function(_,idx){return G.hands[idx].length===0||G.trick.some(function(t){return t.player===idx;});});
    if(complete){
      var w=trickWinner(G.trick);
      G.won[w].push(G.trick.map(function(t){return t.pos;}));
      G.lastTrick={cards:G.trick.slice(),winner:w};
      glog('🏆 '+pname(w)+' gana la baza ('+G.won[w].length+')');
      G.trick=[];
      if(G.hands.every(function(h){return h.length===0;})){
        G.phase='end';glog('🔚 Fin. Comprobad los objetivos de cada personaje.');
      }else{var lead=G.hands[w].length?w:nextWithCards((w+1)%n);G.turn=lead;G.trickLeader=lead;}
    }else G.turn=nextWithCards((p+1)%n);
    broadcast();return;
  }
}

// ---- network ----
function setupHostConn(c){
  c.on('data',function(msg){
    if(!msg||typeof msg!=='object')return;
    if(msg.t==='join'){
      var name=String(msg.name||'???').slice(0,14);
      if(G.phase!=='lobby'){
        var old=G.players.findIndex(function(p){return!p.connected&&p.name===name;});
        if(old>=0){G.players[old].conn=c;G.players[old].connected=true;c._hIdx=old;glog('🔌 '+name+' ha vuelto');broadcast();}
        else c.send({t:'error',msg:'Partida en curso'});
        return;
      }
      if(G.players.length>=MAXP)return c.send({t:'error',msg:'Sala llena (máx '+MAXP+')'});
      while(G.players.some(function(p){return p.name===name;}))name+='2';
      G.players.push({name:name,conn:c,connected:true});
      c._hIdx=G.players.length-1;broadcast();
    }else if(msg.t==='action'&&typeof c._hIdx==='number'){applyAction(c._hIdx,msg.a);}
  });
  c.on('close',function(){
    if(typeof c._hIdx!=='number')return;
    if(G.phase==='lobby'){G.players.splice(c._hIdx,1);G.players.forEach(function(p,i){if(p.conn)p.conn._hIdx=i;});}
    else{G.players[c._hIdx].connected=false;glog('🔌 '+pname(c._hIdx)+' se ha desconectado');}
    broadcast();
  });
}

function createRoom(){
  myName=el('l-name').value.trim();
  if(!myName)return setupMsg('Pon tu nombre primero');
  var alpha='ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
  roomCode='';for(var i=0;i<4;i++)roomCode+=alpha[Math.floor(Math.random()*alpha.length)];
  setupMsg('Creando sala...');
  peer=new Peer(PREFIX+roomCode);
  peer.on('open',function(){isHost=true;newGame();G.players.push({name:myName,conn:null,connected:true});history.pushState(null,'',location.pathname+'?sala='+roomCode);el('l-setup').style.display='none';el('l-game').style.display='block';broadcast();});
  peer.on('connection',setupHostConn);
  peer.on('error',function(e){setupMsg(e.type==='unavailable-id'?'Código ocupado, prueba otra vez':'Error: '+e.type);});
}

function joinRoom(){
  myName=el('l-name').value.trim();roomCode=el('l-codein').value.trim().toUpperCase();
  if(!myName)return setupMsg('Pon tu nombre primero');
  if(roomCode.length!==4)return setupMsg('El código tiene 4 letras');
  setupMsg('Conectando...');peer=new Peer();
  peer.on('open',function(){
    hostConn=peer.connect(PREFIX+roomCode,{reliable:true});
    hostConn.on('open',function(){hostConn.send({t:'join',name:myName});});
    hostConn.on('data',function(msg){
      if(!msg)return;
      if(msg.t==='state'){V=msg.s;selectedPos=null;pendingRing1=null;el('l-setup').style.display='none';el('l-game').style.display='block';render();}
      else if(msg.t==='error'){V?toast(msg.msg):setupMsg(msg.msg);}
    });
    hostConn.on('close',function(){toast('Conexión perdida con el anfitrión 😢');});
  });
  peer.on('error',function(e){setupMsg(e.type==='peer-unavailable'?'No existe la sala '+roomCode:'Error: '+e.type);});
}

function sendAction(a){if(isHost)applyAction(0,a);else hostConn.send({t:'action',a:a});}

// ---- render helpers ----
function cardHtml(pos,extraCls,attrs){
  var s=suitOf(pos);
  return '<div class="lcard'+(extraCls?' '+extraCls:'')+'" style="'+cardStyle(pos)+'" title="'+esc(cardName(pos))+'"'+(attrs||'')+'>'+
    '<span class="l-suit">'+s.emoji+'</span><span class="l-val">'+valOf(pos)+'</span></div>';
}
function backHtml(cls){return '<div class="lcard lcard-back'+(cls?' '+cls:'')+'"></div>';}
function charHtml(ci,cls,attrs){
  var back=V&&charUsesBack(ci,V.names.length);
  return '<div class="lchar'+(cls?' '+cls:'')+'" style="'+charStyle(ci,back)+'" title="'+esc(CHARS[ci])+'"'+(attrs||'')+'></div>';
}

function render(){
  if(!V)return;
  var h='';
  h+='<div class="l-row">Sala: <span class="l-code">'+roomCode+'</span> <button data-act="copy" class="l-sm">📋 Copiar enlace</button></div>';

  if(V.phase==='lobby'){
    h+='<div class="l-panel"><h3>Jugadores ('+V.names.length+'/'+MAXP+')</h3>';
    V.names.forEach(function(n,i){h+='<div>'+esc(n)+(i===0?' 👑':'')+(i===V.me?' (tú)':'')+(V.connected[i]?'':' 🔌❌')+'</div>';});
    h+='</div>';
    if(V.me===0){
      h+='<div class="l-panel"><h3>Capítulo</h3><select id="l-ch" data-act="pickChapter">';
      CHAPTERS.forEach(function(c,i){h+='<option value="'+i+'"'+(i===V.chapter?' selected':'')+'>Capítulo '+esc(c.n)+'</option>';});
      h+='</select></div>';
      h+='<button data-act="start"'+(V.names.length<MINP?' disabled':'')+'>🚀 Empezar ('+(V.names.length<MINP?'mínimo '+MINP:V.names.length+' jugadores')+')</button>';
    }else{
      h+='<div class="l-mut">Capítulo <b>'+esc(CHAPTERS[V.chapter].n)+'</b> · Esperando a que '+esc(V.names[0])+' empiece...</div>';
    }
    el('l-game').innerHTML=h;return;
  }

  // chapter + lost card header
  h+='<div class="l-panel"><div class="l-row" style="justify-content:space-between">';
  h+='<div><b>Capítulo '+esc(CHAPTERS[V.chapter].n)+'</b></div>';
  h+='<div class="l-row" style="gap:6px"><span class="l-mut">Carta perdida:</span>'+(V.lost!=null?cardHtml(V.lost,'lcard-sm'):'<span class="l-mut">(tomada)</span>')+'</div>';
  h+='</div></div>';

  // ring1 holder
  h+='<div class="l-mut">💍 '+esc(V.names[V.ring1Holder])+' tiene el Ring 1'+(V.phase==='draft'?' — elige personaje primero':'')+'</div>';

  // ---- DRAFT ----
  if(V.phase==='draft'){
    var avail=V.chars.filter(function(c){return V.assigned.indexOf(c)<0;});
    h+='<div class="l-panel"><div class="l-name">👤 Elección de personajes (horario desde el Ring 1)</div>';
    V.names.forEach(function(n,i){
      var a=V.assigned[i];
      h+='<div class="l-row'+(V.draftTurn===i?' l-active':'')+'" style="gap:8px">';
      h+='<b>'+(V.draftTurn===i?'▶ ':'')+esc(n)+(i===V.me?' (tú)':'')+'</b>';
      h+=a!==null?charHtml(a,'lchar-sm')+'<span>'+esc(CHARS[a])+'</span>':'<span class="l-mut">'+(V.draftTurn===i?'eligiendo...':'esperando')+'</span>';
      h+='</div>';
    });
    if(V.draftTurn===V.me){
      h+='<div class="l-mut" style="margin-top:6px">Tu turno — elige personaje:</div><div class="l-hand">';
      avail.forEach(function(c){h+='<div style="text-align:center"><div class="l-pickable" data-act="pickChar" data-char="'+c+'">'+charHtml(c)+'</div><div class="l-mut">'+esc(CHARS[c])+'</div></div>';});
      h+='</div>';
    }
    h+='</div>';
  }

  // ---- PREP ----
  if(V.phase==='prep'){
    h+='<div class="l-panel"><div class="l-name">🪄 Fase de preparación</div>';
    if(V.prep)h+='<div class="l-mut">'+esc(V.prep.text)+'</div>';
    var ex=V.exchange;
    if(ex){
      if(ex.stage==='send'){
        if(ex.from===V.me)h+='<div class="l-row" style="margin-top:4px">🔄 Elige una carta de tu mano (abajo) para pasar <b>oculta</b> a <b>'+esc(V.names[ex.to])+'</b>.</div>';
        else h+='<div class="l-mut" style="margin-top:4px">'+esc(V.names[ex.from])+' está eligiendo una carta para '+esc(V.names[ex.to])+'...</div>';
      }else{
        if(ex.to===V.me)h+='<div class="l-row" style="margin-top:4px">🔄 Has recibido una carta (resaltada en tu mano). Elige una carta para devolver <b>oculta</b> a <b>'+esc(V.names[ex.from])+'</b> (puede ser la misma).</div>';
        else h+='<div class="l-mut" style="margin-top:4px">'+esc(V.names[ex.to])+' está decidiendo qué carta devolver a '+esc(V.names[ex.from])+'...</div>';
      }
    }
    h+='</div>';
  }

  // ---- objectives panel (draft + play + end) ----
  if(V.phase!=='lobby'){
    var nPo=V.names.length;
    h+='<div class="l-panel"><div class="l-name">🎯 Objetivos</div>';
    var allMet=true,anyUnchecked=false,anyChar=false;
    V.names.forEach(function(n,i){
      var a=V.assigned[i];
      if(a===null){h+='<div class="l-obj"><b>'+esc(n)+'</b>: <span class="l-mut">sin personaje</span></div>';return;}
      anyChar=true;
      var o=evalObjective(V.chapter,a,V,i,nPo);
      var mark=o.met===null?'':(o.met?' ✅':' ⬜');
      if(o.met===null)anyUnchecked=true;else if(!o.met)allMet=false;
      h+='<div class="l-obj"><b>'+esc(n)+'</b> — '+esc(CHARS[a])+': '+esc(o.text)+(o.prog?' <span class="l-mut">['+esc(o.prog)+']</span>':'')+mark+'</div>';
    });
    if(V.phase==='end'&&anyChar){
      if(anyUnchecked)h+='<div class="l-mut" style="margin-top:4px">Algunos objetivos no se verifican automáticamente — comprobadlos vosotros.</div>';
      else h+='<div style="margin-top:6px;font-weight:700;font-size:16px">'+(allMet?'🎉 ¡Victoria! Todos los objetivos cumplidos':'💀 Derrota — algún objetivo no se cumplió')+'</div>';
    }
    h+='</div>';
  }

  // ---- PLAY / END: trick area ----
  if(V.phase==='play'||V.phase==='end'){
    var led=V.trick.length?suitOf(V.trick[0].pos):null;
    h+='<div class="l-panel"><div class="l-row" style="justify-content:space-between"><div class="l-name" style="margin:0">🃏 Baza actual'+(led?' · palo de salida: '+led.emoji+' '+led.name:'')+'</div>';
    h+='<img src="'+(V.ringsBroken?RINGOK_URL:RINGLOCK_URL)+'" style="height:34px;border-radius:6px" title="'+(V.ringsBroken?'Triunfo roto: ya se puede salir con Ring':'No se puede salir con Ring todavía')+'"></div>';
    h+='<div class="l-trick">';
    if(V.trick.length){
      V.trick.forEach(function(t){
        h+='<div class="l-trick-slot">'+cardHtml(t.pos)+'<span class="l-mut">'+esc(V.names[t.player])+(t.pos===RING1?(t.win?' 💍✔':' 💍✖'):'')+'</span></div>';
      });
    }else if(V.lastTrick){
      h+='<div class="l-mut" style="align-self:center">Última baza ganada por <b>'+esc(V.names[V.lastTrick.winner])+'</b>:</div>';
      V.lastTrick.cards.forEach(function(t){
        h+='<div class="l-trick-slot">'+cardHtml(t.pos,t.player===V.lastTrick.winner?'lcard-sm l-trick-winner':'lcard-sm')+'<span class="l-mut">'+esc(V.names[t.player])+'</span></div>';
      });
    }else h+='<span class="l-mut" style="align-self:center">Esperando la primera carta...</span>';
    h+='</div></div>';
  }

  // ---- players + hands ----
  var nP=V.names.length;
  Array.from({length:nP},function(_,k){return(V.me+k)%nP;}).forEach(function(i){
    var isMe=i===V.me;
    var isTurn=V.phase==='play'&&V.turn===i;
    var a=V.assigned[i];
    h+='<div class="l-panel'+(isTurn?' l-active':'')+'"><div class="l-row" style="gap:8px">';
    h+='<div class="l-name" style="margin:0">'+(isTurn?'▶ ':'')+esc(V.names[i])+(isMe?' (tú)':'')+(V.connected[i]?'':' 🔌❌')+'</div>';
    if(a!==null)h+=charHtml(a,'lchar-sm');
    h+='<span class="l-mut">bazas: '+V.won[i].length+'</span>';
    h+='</div>';
    h+='<div class="l-hand">';
    if(isMe){
      var myTurn=V.phase==='play'&&V.turn===V.me;
      var legal=myTurn?legalCards(V.myHand,V.trick,V.ringsBroken):[];
      var pGive=V.phase==='prep'&&V.exchange&&V.exchange.stage==='send'&&V.exchange.from===V.me;
      var pRet=V.phase==='prep'&&V.exchange&&V.exchange.stage==='return'&&V.exchange.to===V.me;
      V.myHand.forEach(function(pos){
        var cls='',attrs='';
        if(myTurn){
          var ok=legal.indexOf(pos)>=0;
          cls=(ok?'lcard-legal':'lcard-blocked')+(selectedPos===pos?' lcard-sel':'');
          if(ok)attrs=' data-act="selPlay" data-pos="'+pos+'"';
        }else if(pGive){cls='lcard-legal';attrs=' data-act="prepGive" data-pos="'+pos+'"';}
        else if(pRet){cls='lcard-legal'+(pos===V.receivedCard?' lcard-sel':'');attrs=' data-act="prepReturn" data-pos="'+pos+'"';}
        h+=cardHtml(pos,cls,attrs);
      });
      if(!V.myHand.length)h+='<span class="l-mut">Sin cartas</span>';
    }else{
      for(var k=0;k<V.handCounts[i];k++)h+=backHtml('lcard-sm');
      if(!V.handCounts[i])h+='<span class="l-mut">Sin cartas</span>';
    }
    h+='</div>';
    // ring1 choice controls (only my panel)
    if(isMe&&pendingRing1!==null){
      h+='<div class="l-row" style="margin-top:6px"><span class="l-mut">Juegas el 💍 Ring 1 — ¿qué haces?</span>';
      h+='<button data-act="ring1" data-win="1" class="l-sm l-ok">Ganar la baza</button>';
      h+='<button data-act="ring1" data-win="0" class="l-sm l-warn">Dejar que gane un Ring mayor</button>';
      h+='<button data-act="cancelRing1" class="l-sm">✖ Cancelar</button></div>';
    }
    h+='</div>';
  });

  // ---- END summary ----
  if(V.phase==='end'){
    h+='<div class="l-panel"><div class="l-name">🔚 Resumen</div>';
    V.names.forEach(function(n,i){
      var a=V.assigned[i];
      var cards=(V.won[i]||[]).reduce(function(acc,t){return acc.concat(t);},[]);
      h+='<div class="l-obj"><b>'+esc(n)+'</b>'+(a!==null?' — '+esc(CHARS[a]):'')+' · '+V.won[i].length+' bazas'+(cards.length?', cartas: '+cards.map(cardName).join(', '):'')+'</div>';
    });
    h+='<div class="l-mut" style="margin-top:6px">Comprobad si cada personaje cumplió su objetivo.</div>';
    if(V.me===0)h+='<div class="l-row" style="margin-top:6px"><button data-act="restart" class="l-sm l-ok">🔄 Nueva partida</button></div>';
    h+='</div>';
  }

  // ---- log ----
  h+='<div class="l-panel l-log">'+V.log.slice().reverse().map(function(l){return'<div>'+esc(l)+'</div>';}).join('')+'</div>';

  el('l-game').innerHTML=h;
}

// ---- events ----
el('l-create').addEventListener('click',createRoom);
el('l-join').addEventListener('click',joinRoom);
el('l-game').addEventListener('click',function(e){
  var b=e.target.closest('[data-act]');if(!b)return;
  var act=b.getAttribute('data-act');
  if(act==='copy'){
    navigator.clipboard.writeText(location.origin+location.pathname+'?sala='+roomCode).then(function(){toast('Enlace copiado 📋');});
  }else if(act==='start'){sendAction({kind:'start'});
  }else if(act==='restart'){sendAction({kind:'restart'});
  }else if(act==='pickChar'){sendAction({kind:'pickChar',char:+b.getAttribute('data-char')});
  }else if(act==='prepGive'){sendAction({kind:'prepGive',pos:+b.getAttribute('data-pos')});
  }else if(act==='prepReturn'){sendAction({kind:'prepReturn',pos:+b.getAttribute('data-pos')});
  }else if(act==='selPlay'){
    if(!(V&&V.phase==='play'&&V.turn===V.me))return;
    var pos=+b.getAttribute('data-pos');
    if(pos===RING1){pendingRing1=pos;selectedPos=pos;render();}
    else{sendAction({kind:'play',pos:pos});}
  }else if(act==='ring1'){
    if(pendingRing1===null)return;
    sendAction({kind:'play',pos:pendingRing1,win:+b.getAttribute('data-win')===1});
    pendingRing1=null;selectedPos=null;
  }else if(act==='cancelRing1'){pendingRing1=null;selectedPos=null;render();}
});
el('l-game').addEventListener('change',function(e){
  var s=e.target.closest('[data-act="pickChapter"]');
  if(s&&isHost)sendAction({kind:'setChapter',ch:+s.value});
});

// ---- URL auto-fill ----
var m=location.search.match(/sala=([A-Za-z0-9]{4})/);
if(m)el('l-codein').value=m[1].toUpperCase();
})();
</script>

## ¿Cómo se juega?

Juego **cooperativo** de bazas para 3–4 jugadores. Las **37 cartas** son de 5 palos: ⛰️ Mountain, 🏔️ Hill, 🌲 Forest y 🌑 Shadow (1–8 cada uno) más 💍 **Ring** (1–5), que es el **triunfo**.

**Preparación:** se reparten las cartas por igual y queda **1 carta perdida** boca arriba. Es público quién tiene el **💍 Ring 1**: ese jugador **elige personaje primero** y luego el resto en sentido horario. Cada personaje tiene un **objetivo** que el grupo debe cumplir.

**Reglas de la baza:**

- Hay que **seguir el palo** de salida si puedes.
- Si **no puedes seguir el palo**, puedes jugar cualquier carta, incluido un **Ring** (triunfo).
- No puedes **salir** liderando con un Ring hasta que el triunfo esté **roto** (alguien jugó un Ring por no poder seguir el palo). El indicador junto a la baza muestra si ya está roto.
- Gana la baza el **Ring más alto**; si no hay Rings, la carta más alta del palo de salida.
- **Ring 1** es especial: quien lo juega decide en ese momento si **gana** la baza (vence a cualquier Ring) o si **deja que un Ring mayor le gane**.

Ganáis todos juntos si se cumplen los objetivos de los personajes del capítulo.
