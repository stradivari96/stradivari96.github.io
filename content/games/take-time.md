---
title: "⏱ Take Time"
date: 2026-06-19
searchHidden: true
robotsNoIndex: true
showToc: false
ShowReadingTime: false
ShowShareButtons: false
comments: false
---

Juego cooperativo para 3–4 jugadores. Un anfitrión crea la sala y comparte el enlace.

- [Reglamento](https://cdn.svc.asmodee.net/production-libellud/uploads/2025/09/TT_RULES_PRINT_MASTER_EN.pdf)

<style>
#tt-app{--bg:#0d1117;--panel:#161b27;--text:#e2e6f0;--mut:#6b7590;--border:#252d42;
  --blue:#3b82f6;--gold:#fbbf24;--solar:#f59e0b;--lunar:#818cf8;
  background:var(--bg);color:var(--text);border-radius:12px;padding:16px;
  font-family:system-ui,sans-serif;font-size:15px;line-height:1.4;min-height:200px}
#tt-app h3{margin:10px 0 6px;color:var(--text)}
#tt-app input{background:#0a0d14;color:var(--text);border:1px solid var(--border);
  border-radius:8px;padding:8px 10px;font-size:15px;max-width:160px}
#tt-app button{background:var(--blue);color:#fff;border:0;border-radius:8px;
  padding:7px 13px;font-size:14px;cursor:pointer;margin:2px}
#tt-app button.tt-btn-sm{padding:5px 10px;font-size:13px}
#tt-app button:disabled{background:var(--border);color:var(--mut);cursor:not-allowed}
#tt-app button.tt-btn-warn{background:#dc2626}
#tt-app button.tt-btn-ok{background:#16a34a}
.tt-row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin:6px 0}
.tt-panel{background:var(--panel);border-radius:10px;padding:10px 12px;margin:6px 0}
.tt-code{font-size:22px;font-weight:800;letter-spacing:4px;color:var(--gold)}
.tt-mut{color:var(--mut);font-size:13px;margin:3px 0}
.tt-name{font-weight:600;font-size:14px;margin-bottom:5px}
/* Cards */
.tt-card{width:108px;height:152px;border-radius:9px;border:2px solid rgba(0,0,0,.5);
  flex:none;position:relative;user-select:none;background-color:#1e2535;cursor:default}
.tt-card-sm{width:54px;height:76px;border-radius:6px;border:2px solid rgba(0,0,0,.5);
  flex:none;position:relative;user-select:none;background-color:#1e2535}
.tt-card-xs{width:60px;height:84px;border-radius:7px;border:1px solid rgba(0,0,0,.4);
  flex:none;position:relative;user-select:none;background-color:#1e2535}
.tt-back-img{background-size:cover!important;background-position:center!important}
.tt-card-blocked{opacity:.35;cursor:not-allowed;filter:grayscale(.6)}
/* card backs: soften the fine sprite detail */
.tt-cardback{filter:blur(0.4px)}
.tt-empty{display:flex;align-items:center;justify-content:center;
  border:2px dashed var(--border)!important;background:transparent!important;color:var(--mut)}
.tt-suit-badge{position:absolute;top:2px;left:3px;font-size:13px;line-height:1}
.tt-val-badge{position:absolute;bottom:2px;right:4px;font-size:11px;font-weight:800;
  color:#fff;text-shadow:0 1px 4px rgba(0,0,0,.9)}
/* Clock: chapter image as round background + 6 sectors */
.tt-clock-wrap{position:relative;width:min(560px,86vw);aspect-ratio:1;margin:70px auto}
.tt-clock-img{position:absolute;inset:0;width:100%;height:100%;border-radius:50%;
  object-fit:cover;border:2px solid var(--border)}
.tt-sector{position:absolute;transform:translate(-50%,-50%);display:flex;gap:2px;
  padding:3px;border-radius:8px;align-items:center;justify-content:center;min-width:30px;min-height:42px}
.tt-sector-num{font-size:15px;font-weight:800;color:#fff;opacity:.55;
  text-shadow:0 1px 4px rgba(0,0,0,.9)}
.tt-sector-sum{position:absolute;top:-12px;left:50%;transform:translateX(-50%);
  font-size:12px;font-weight:800;color:#fff;background:rgba(0,0,0,.7);
  border-radius:6px;padding:1px 5px;white-space:nowrap}
/* Sector highlight while placing */
.tt-sector-target{cursor:pointer;background:rgba(251,191,36,.3);outline:2px dashed var(--gold)}
.tt-sector-target:hover{background:rgba(251,191,36,.5)}
.tt-sector-blocked{cursor:not-allowed;opacity:.4;outline:2px dashed #ef4444;background:rgba(239,68,68,.12)}
/* Hand */
.tt-hand{display:flex;flex-wrap:wrap;gap:7px;padding:4px 0;align-items:center}
/* placement controls beside the hand cards */
.tt-place-ctrl{display:flex;flex-direction:column;gap:6px;justify-content:center;
  margin-left:auto;align-self:center;min-width:170px}
.tt-place-ctrl button{margin:0}
.tt-selected{outline:3px solid var(--gold)!important;outline-offset:2px;border-radius:7px;cursor:pointer}
.tt-clickable{cursor:pointer}
.tt-card.tt-selectable{cursor:pointer}
.tt-card.tt-selectable:hover{outline:2px solid var(--blue);outline-offset:2px;border-radius:7px}
/* Phases */
.tt-phase-disc{background:#1a2a1a;border:1px solid #16a34a;border-radius:10px;padding:10px 14px;margin:6px 0}
.tt-phase-place{background:#1a1a2e;border:1px solid var(--blue);border-radius:10px;padding:10px 14px;margin:6px 0}
.tt-banner-res{background:#2a2440;border:2px solid #818cf8;border-radius:10px;padding:12px 16px;margin:6px 0;font-size:15px}
/* Log */
.tt-log{max-height:120px;overflow-y:auto;font-size:13px;color:var(--mut)}
.tt-log div{padding:1px 0}
/* Toast */
.tt-toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);
  background:#dc2626;color:#fff;padding:10px 18px;border-radius:10px;z-index:99;
  box-shadow:0 4px 14px rgba(0,0,0,.4)}
/* Aside */
.tt-aside{display:flex;flex-wrap:wrap;gap:5px}
/* Solar/Lunar labels */
.tt-solar{color:var(--solar)}
.tt-lunar{color:var(--lunar)}
/* turn highlight (hanabi style) */
.tt-active{box-shadow:0 0 0 2px #fbbf24 inset;border-radius:10px}
/* chapter selector */
#tt-app select{background:#0a0d14;color:var(--text);border:1px solid var(--border);border-radius:8px;padding:7px 10px;font-size:15px}
.tt-chapter-img{width:280px;max-width:80%;aspect-ratio:1;object-fit:cover;border-radius:50%;
  border:2px solid var(--border);display:block;margin:8px 0}
/* resolution highlight */
.tt-ok{outline:3px solid #22c55e!important;outline-offset:2px;border-radius:7px}
.tt-fail{outline:3px solid #ef4444!important;outline-offset:2px;border-radius:7px}
</style>

<div id="tt-app">
  <div id="tt-setup">
    <div class="tt-row"><label>Tu nombre: <input id="tt-name" maxlength="14" placeholder="Nombre"></label></div>
    <div class="tt-row"><button id="tt-create">⏱ Crear sala</button></div>
    <div class="tt-row">
      <input id="tt-code" maxlength="4" placeholder="CÓDIGO" style="text-transform:uppercase;width:100px">
      <button id="tt-join">Unirse</button>
    </div>
    <div id="tt-setupmsg" class="tt-mut"></div>
  </div>
  <div id="tt-game" style="display:none"></div>
</div>

<script src="https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js"></script>
<script>
(function(){
'use strict';

var SPRITE='https://steamusercontent-a.akamaihd.net/ugc/12794565721592296659/D2D66AE2BD54BBD369AB95725B49A87E9E19D455/';
// Dorsos: solo hay 2 imágenes distintas (Lunar/Solar). Recortadas del sprite original
// (31MB) y servidas desde el propio dominio. El palo se deduce del cardId.
var BACK_LUNAR='/tt-back-lunar.png',BACK_SOLAR='/tt-back-solar.png';
var PREFIX='take-time-xiang-';
var MAXP=4,MINP=3,CLOCK_SIZE=12,SECTORS=6;

// Capítulos posibles (40): nombre + imagen de la prueba
var CHAPTERS=[
  {n:'1.1',img:'https://steamusercontent-a.akamaihd.net/ugc/15703307762668170147/9654F9ED69D778DEFF959297D5689E3E3E041FEA/',rules:{s:{1:{max:1,maxSuit:{lunar:0}},6:{max:3}}}},
  {n:'1.2',img:'https://steamusercontent-a.akamaihd.net/ugc/10385147287207204669/0D522E0BC3A3B3FD3693E2C10DC4AAA008C03100/',rules:{s:{4:{max:3}}}},
  {n:'1.3',img:'https://steamusercontent-a.akamaihd.net/ugc/17233917275871474503/8B2EC3D78605E829B6F4CFA507EB9BA62C59A5FE/',rules:{order:[3,2]}},
  {n:'1.4',img:'https://steamusercontent-a.akamaihd.net/ugc/15600617884164400557/BEEA32E60496751716054F74939F2A78E1D128BF/',rules:{s:{4:{max:2,maxSuit:{solar:1,lunar:1}}}}},
  {n:'2.1',img:'https://steamusercontent-a.akamaihd.net/ugc/14280661180766557701/020371840632CFF58B67C301001323B88B6ACC53/',rules:{s:{1:{noVal:[1,2,3]},2:{noVal:[1,2,3]},3:{noVal:[1,2,3]}}}},
  {n:'2.2',img:'https://steamusercontent-a.akamaihd.net/ugc/10900870861288750109/2AA06E982F30B9EADB0C45D9AB87F0C6DCEB06AC/',rules:{s:{3:{noVal:[7,8,9]},4:{noVal:[7,8,9]}}}},
  {n:'2.3',img:'https://steamusercontent-a.akamaihd.net/ugc/15145851534642029728/86B69D30E940A5F6C7AED56961E20E5F9522DE23/',rules:{s:{1:{noVal:[1,2,3]},3:{noVal:[4,5,6]},4:{noVal:[7,8,9]},6:{noVal:[10,11,12]}}}},
  {n:'2.4',img:'https://steamusercontent-a.akamaihd.net/ugc/9770768207491621603/6287A814B52A47F3292497569FDD04FD3FB34995/',rules:{faceUp:0}},
  {n:'3.1',img:'https://steamusercontent-a.akamaihd.net/ugc/11525862569977917742/B7AA2B3B1B2B9B877921534563004E51EFBCC6F7/'},
  {n:'3.2',img:'https://steamusercontent-a.akamaihd.net/ugc/12207272073620259158/866D8B28B0273D30E67C969D9E046ECBB718DEB9/'},
  {n:'3.3',img:'https://steamusercontent-a.akamaihd.net/ugc/10591514710080541563/B694A0C294C5A5B6806D5613DDA1A2B8B66B8F9F/',rules:{order:[4,4]}},
  {n:'3.4',img:'https://steamusercontent-a.akamaihd.net/ugc/13461586841218597994/FED77033A3A99785906161BC7D766CD6B643A61F/',rules:{s:{3:{max:2}}}},
  {n:'4.1',img:'https://steamusercontent-a.akamaihd.net/ugc/16444579309301025687/A3275FBFBB379EE5D4230693A13ABA118B7BD313/',rules:{order:[2],s:{5:{max:1}},desc:true}},
  {n:'4.2',img:'https://steamusercontent-a.akamaihd.net/ugc/10558589716375396552/59611CDEEA19AE22FBA98940AAE29D4209FB5928/',rules:{order:[6],asc:true}},
  {n:'4.3',img:'https://steamusercontent-a.akamaihd.net/ugc/14620381300433252122/B116D4A35A9274E1B6016D2C8D29895D86D02566/',rules:{s:{1:{noVal:[1,2,3]},3:{noVal:[1,2,3]},5:{noVal:[1,2,3]}},lr:true}},
  {n:'4.4',img:'https://steamusercontent-a.akamaihd.net/ugc/15862743864655057638/41DED24062D0242497ABD6505F684451BAE0DF1D/',rules:{lr:true}},
  {n:'5.1',img:'https://steamusercontent-a.akamaihd.net/ugc/16235257271052605664/19E86EE46284433DCD5642E95557E0D210A5CF2F/',rules:{maxAll:2}},
  {n:'5.2',img:'https://steamusercontent-a.akamaihd.net/ugc/12870573118539567545/6542CE775ECCE03B3489DDADB7AFCF60B8206150/',rules:{maxAll:2}},
  {n:'5.3',img:'https://steamusercontent-a.akamaihd.net/ugc/12025694528045916903/51462E13E6F1B5AFBCCB281EEEC1F75BC6F7858A/',rules:{maxAll:2}},
  {n:'5.4',img:'https://steamusercontent-a.akamaihd.net/ugc/12757718271765149734/87BEB2F9D07A7071DBEF88732CADABC31746C169/',rules:{maxAll:2,s:{3:{maxSuit:{solar:1,lunar:1}},4:{maxSuit:{solar:1,lunar:1}}},order:[0,5,2]}},
  {n:'6.1',img:'https://steamusercontent-a.akamaihd.net/ugc/14291589495058044977/50CA6BC751A8FB3C7735A31657ABDB54B8F4F7F2/',rules:{maxAll:2,score:'diff'}},
  {n:'6.2',img:'https://steamusercontent-a.akamaihd.net/ugc/18120938985591900711/DD56AF52EE61EC43BE6B691AE93CC7BBF1DB17DE/',rules:{maxAll:2,score:'diff',s:{5:{maxSuit:{solar:1,lunar:1}},4:{noVal:[1,2,3]}}}},
  {n:'6.3',img:'https://steamusercontent-a.akamaihd.net/ugc/12543330935849795254/7055DBEB8380F6BD43C126F82F98BF23AD46B589/',rules:{maxAll:2,score:'diff',order:[0,5,5]}},
  {n:'6.4',img:'https://steamusercontent-a.akamaihd.net/ugc/15670765100291722138/D13C58481B51CF8F7CF889F8782CD2AB3EF29CF9/',rules:{maxAll:2,score:'diff'}},
  {n:'7.1',img:'https://steamusercontent-a.akamaihd.net/ugc/12824740814417941728/C233DFDE69A9562F0EB06E26FFDBF55003EE7EF5/',rules:{s:{3:{noVal:[7,8,9]},4:{noVal:[7,8,9]}},draw:[1]}},
  {n:'7.2',img:'https://steamusercontent-a.akamaihd.net/ugc/13676744455971127771/DF88B5917F70A080939F2AB2BAF6A9C589057A2D/',rules:{draw:[6,3]}},
  {n:'7.3',img:'https://steamusercontent-a.akamaihd.net/ugc/13398618883690484413/57173EFA23834F99670F8238472EB848DB928C34/'},
  {n:'7.4',img:'https://steamusercontent-a.akamaihd.net/ugc/16939947217815271747/11331B9F4F9AB17C6BB00E4FE963AE3996FCA825/'},
  {n:'8.1',img:'https://steamusercontent-a.akamaihd.net/ugc/9611437470663852418/B216C8D23748622481772E25EBFE9B1DCE2562B4/'},
  {n:'8.2',img:'https://steamusercontent-a.akamaihd.net/ugc/9850145143649572277/80ECB0911F8E6C4ED779118D691A4E915EE06085/'},
  {n:'8.3',img:'https://steamusercontent-a.akamaihd.net/ugc/9927209626888507396/DFB0FD349042DB1EFB6B24E8855ECF4589D8F25F/'},
  {n:'8.4',img:'https://steamusercontent-a.akamaihd.net/ugc/17517382117583532714/BD3354D792A17EDAA3BE94EE4138B3E5A90809BC/'},
  {n:'9.1',img:'https://steamusercontent-a.akamaihd.net/ugc/17328735450066306026/2150C13402F22FCA1970C5E1425480BD5D8005A6/'},
  {n:'9.2',img:'https://steamusercontent-a.akamaihd.net/ugc/11675917358681693593/0C6EAB6D75BBE2E9D07FC1C6B2C66F7A4D30762F/'},
  {n:'9.3',img:'https://steamusercontent-a.akamaihd.net/ugc/10674890130604616084/58C99D7B5AFB6E1FBA9A58DD8B0DD28AA7F9C5EE/'},
  {n:'9.4',img:'https://steamusercontent-a.akamaihd.net/ugc/15592390808083619315/2CC6AA643015BE43F0834CCC6C917F691EB22BAD/'},
  {n:'10.1',img:'https://steamusercontent-a.akamaihd.net/ugc/10766547707526683105/4ADE68F08EF2A933D11234F7AF10F4DDD4A76B64/'},
  {n:'10.2',img:'https://steamusercontent-a.akamaihd.net/ugc/17188336104454836334/40FED1E0B3A9E441B3E253D3387C6984DA17BE20/'},
  {n:'10.3',img:'https://steamusercontent-a.akamaihd.net/ugc/9261135796523656428/01A548380839E0DB9ECC61217C38318F992CDB70/'},
  {n:'10.4',img:'https://steamusercontent-a.akamaihd.net/ugc/13987952581233767420/DBA069A4FBEFCE5D9271B737D44111B7ECD491BA/'}
];

// 6×4 sprite sheet (24 cards). Assumed: Lunar (negra) rows 0-1 (cardId 0-11), Solar (blanca) rows 2-3 (cardId 12-23)
var CARDS_DEF=[];
for(var _i=0;_i<12;_i++)CARDS_DEF.push({name:'Lunar '+(_i+1),value:_i+1,suit:'lunar',cardId:_i});
for(var _i=0;_i<12;_i++)CARDS_DEF.push({name:'Solar '+(_i+1),value:_i+1,suit:'solar',cardId:_i+12});

var peer=null,isHost=false,hostConn=null,roomCode='';
var G=null,V=null,myName='';
var selectedCardId=null,pendingSlot=null;

function el(id){return document.getElementById(id);}
function esc(s){return String(s).replace(/[&<>"']/g,function(c){return{'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];});}
function toast(msg){var t=document.createElement('div');t.className='tt-toast';t.textContent=msg;el('tt-app').appendChild(t);setTimeout(function(){t.remove();},3000);}
function setupMsg(m){el('tt-setupmsg').textContent=m;}

// ---- sprite ----
function spriteStyle(cardId){
  var col=cardId%6,row=Math.floor(cardId/6);
  return 'background-image:url('+SPRITE+');background-size:600% 400%;background-position:'+(col/5*100).toFixed(1)+'% '+(row/3*100).toFixed(1)+'%';
}
function backStyle(cardId){
  return 'background-image:url('+(cardId<12?BACK_LUNAR:BACK_SOLAR)+');background-size:cover;background-position:center';
}

// ---- deck ----
function mkDeck(){
  var d=CARDS_DEF.map(function(def,i){return{id:i,name:def.name,value:def.value,suit:def.suit,cardId:def.cardId};});
  for(var i=d.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));var t=d[i];d[i]=d[j];d[j]=t;}
  return d;
}

// ---- game logic (host) ----
function newGame(){G={phase:'lobby',players:[],hands:[],aside:[],sectors:[],turn:0,chapter:0,log:[],result:null};}

function startGame(){
  var full=mkDeck();
  var toDeal=full.slice(0,CLOCK_SIZE);
  G.aside=full.slice(CLOCK_SIZE);
  G.sectors=[];for(var i=0;i<SECTORS;i++)G.sectors.push([]);
  G.hands=G.players.map(function(){return[];});
  toDeal.forEach(function(card,k){G.hands[k%G.players.length].push(card);});
  G.placeTurn=0;
  G.phase='discussion';
  G.result=null;
  G.log=['⏱ Capítulo '+CHAPTERS[G.chapter].n+' — Fase de discusión. ¡Planificad sin mirar vuestras cartas!'];
  broadcast();
}

function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>80)G.log.shift();}

function sectorSum(sec){return sec.reduce(function(a,c){return a+c.value;},0);}

function buildStateFor(me){
  return{
    me:me,phase:G.phase,placeTurn:G.placeTurn,
    aside:G.aside.map(function(c){return{id:c.id,name:c.name,cardId:c.cardId,suit:c.suit,value:c.value};}),
    sectors:G.sectors.map(function(sec){return sec.map(function(slot){
      if(G.phase==='resolution'||slot.faceUp)return{id:slot.id,value:slot.value,suit:slot.suit,cardId:slot.cardId,owner:slot.owner,faceUp:slot.faceUp};
      return{id:slot.id,cardId:slot.cardId,owner:slot.owner}; // face-down: back for everyone, no value
    });}),
    hands:G.hands.map(function(h,i){
      if(i===me)return h.map(function(c){return{id:c.id,name:c.name,cardId:c.cardId,suit:c.suit,value:c.value};});
      return h.map(function(c){return{id:c.id,cardId:c.cardId,hidden:true};});
    }),
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    chapter:G.chapter,
    log:G.log.slice(-30),result:G.result
  };
}

function broadcast(){
  G.players.forEach(function(p,i){if(i===0)return;if(p.conn&&p.connected)p.conn.send({t:'state',s:buildStateFor(i)});});
  V=buildStateFor(0);selectedCardId=null;pendingSlot=null;render();
}
function sendErr(p,msg){if(p===0)toast(msg);else if(G.players[p].conn)G.players[p].conn.send({t:'error',msg:msg});}

// ---- chapter rules (placement restrictions) ----
function slotSuit(s){return s.suit||(s.cardId<12?'lunar':'solar');}
function suitES(su){return su==='solar'?'blancas (Solar)':'negras (Lunar)';}
// Max face-up cards allowed this chapter (default: one per player).
function maxFaceUp(ci,nP){var R=CHAPTERS[ci]&&CHAPTERS[ci].rules;return R&&R.faceUp!==undefined?R.faceUp:nP;}
// Returns an error string if placing `card` in sector `si` (0-indexed) breaks the
// chapter rule, else null. Pure: works with G.sectors (host) or V.sectors (client),
// since face-down slots still carry cardId (suit is derivable).
// Sector-level placement check (depends on target sector + the card).
function placementError(sectors,ci,si,card){
  var R=CHAPTERS[ci]&&CHAPTERS[ci].rules;if(!R)return null;
  var placed=sectors.reduce(function(n,s){return n+s.length;},0);
  if(R.order&&placed<R.order.length&&R.order[placed]&&si!==R.order[placed]-1)
    return 'La carta nº'+(placed+1)+' debe ir en el sector '+R.order[placed];
  if(R.maxAll!==undefined&&sectors[si].length>=R.maxAll)
    return 'Cada sector admite máx. '+R.maxAll+' carta'+(R.maxAll===1?'':'s');
  var r=R.s&&R.s[si+1];
  if(r){
    var sec=sectors[si];
    if(r.max!==undefined&&sec.length>=r.max)
      return 'El sector '+(si+1)+' admite máx. '+r.max+' carta'+(r.max===1?'':'s');
    if(r.maxSuit){
      var su=slotSuit(card),lim=r.maxSuit[su];
      if(lim!==undefined){
        var cnt=sec.filter(function(c){return slotSuit(c)===su;}).length;
        if(cnt>=lim)return lim===0
          ?'El sector '+(si+1)+' no admite cartas '+suitES(su)
          :'El sector '+(si+1)+' admite máx. '+lim+' '+suitES(su);
      }
    }
    if(r.noVal&&r.noVal.indexOf(card.value)>=0)
      return 'El sector '+(si+1)+' no admite el valor '+card.value+' (prohibidos: '+r.noVal.join(', ')+')';
  }
  return null;
}
// Card-level check: can this card be the one played now? (independent of sector)
function cardPlayError(hand,ci,card){
  var R=CHAPTERS[ci]&&CHAPTERS[ci].rules;if(!R)return null;
  if(R.desc){ // play highest-to-lowest → only the max card in hand is playable
    var mx=hand.reduce(function(m,c){return c.value>m?c.value:m;},0);
    if(card.value<mx)return 'De mayor a menor: juega antes tu carta de valor '+mx;
  }
  if(R.asc){ // play lowest-to-highest → only the min card in hand is playable
    var mn=hand.reduce(function(m,c){return c.value<m?c.value:m;},Infinity);
    if(card.value>mn)return 'De menor a mayor: juega antes tu carta de valor '+mn;
  }
  if(R.lr){ // play in dealt order, left-to-right → only the first card in hand is playable
    if(hand.length&&card.id!==hand[0].id)return 'Juega de izquierda a derecha: te toca la primera carta de tu mano';
  }
  return null;
}
function describeRules(ci){
  var R=CHAPTERS[ci]&&CHAPTERS[ci].rules;if(!R)return[];
  var out=[];
  if(R.order)R.order.forEach(function(s,i){if(s)out.push('La carta nº'+(i+1)+' va en el sector '+s);});
  if(R.maxAll!==undefined)out.push('Todos los sectores: máx. '+R.maxAll+' carta'+(R.maxAll===1?'':'s'));
  if(R.score==='diff')out.push('Puntuación de cada sector: carta más alta − más baja (en vez de la suma)');
  if(R.draw)out.push('Si juegas en el sector '+R.draw.join('/')+' robas una carta');
  if(R.s)Object.keys(R.s).forEach(function(k){
    var r=R.s[k],parts=[];
    if(r.max!==undefined)parts.push('máx. '+r.max+' carta'+(r.max===1?'':'s'));
    if(r.maxSuit)Object.keys(r.maxSuit).forEach(function(su){
      var lim=r.maxSuit[su];parts.push(lim===0?'sin '+suitES(su):'máx. '+lim+' '+suitES(su));
    });
    if(r.noVal)parts.push('sin valores '+r.noVal.join('/'));
    out.push('Sector '+k+': '+parts.join(', '));
  });
  if(R.desc)out.push('Cada jugador juega sus cartas de mayor a menor');
  if(R.asc)out.push('Cada jugador juega sus cartas de menor a mayor');
  if(R.lr)out.push('Juega las cartas de izquierda a derecha (en el orden repartido)');
  if(R.faceUp!==undefined)out.push(R.faceUp===0?'Ninguna carta boca arriba':'Máx. '+R.faceUp+' carta'+(R.faceUp===1?'':'s')+' boca arriba');
  return out;
}

function applyAction(p,a){
  if(!G)return;
  if(a.kind==='setChapter'){
    if(!isHost||(G.phase!=='lobby'&&G.phase!=='resolution'))return;
    var ch=+a.ch;if(ch<0||ch>=CHAPTERS.length)return;
    G.chapter=ch;broadcast();return;
  }
  if(a.kind==='setFirst'){
    if(!isHost||G.phase!=='discussion')return;
    var fp=+a.p;if(fp<0||fp>=G.players.length)return;
    G.placeTurn=fp;
    glog('👉 '+pname(fp)+' irá primero');
    broadcast();return;
  }
  if(a.kind==='startPlacement'){
    if(!isHost||G.phase!=='discussion')return;
    G.phase='placement';
    glog('🤫 ¡Silencio! Fase de colocación. Turno: '+pname(G.placeTurn));
    broadcast();return;
  }
  if(a.kind==='place'){
    if(G.phase!=='placement')return;
    if(G.placeTurn!==p)return sendErr(p,'No es tu turno de colocar');
    var si=a.sector;
    if(si<0||si>=SECTORS)return sendErr(p,'Sector no válido');
    var hIdx=G.hands[p].findIndex(function(c){return c.id===a.cid;});
    if(hIdx===-1)return sendErr(p,'Carta no encontrada');
    var card=G.hands[p][hIdx];
    var fu=!!a.faceUp;
    if(fu){
      var fuMax=maxFaceUp(G.chapter,G.players.length);
      var fuCount=G.sectors.reduce(function(n,sec){return n+sec.filter(function(s){return s.faceUp;}).length;},0);
      if(fuCount>=fuMax)return sendErr(p,fuMax===0?'Este capítulo no permite cartas boca arriba':'Máximo '+fuMax+' cartas boca arriba');
    }
    var cerr=cardPlayError(G.hands[p],G.chapter,card)||placementError(G.sectors,G.chapter,si,card);
    if(cerr)return sendErr(p,cerr);
    G.hands[p].splice(hIdx,1);
    G.sectors[si].push({id:card.id,value:card.value,suit:card.suit,cardId:card.cardId,owner:p,faceUp:fu});
    glog('🎴 '+pname(p)+' coloca en sector '+(si+1)+' ('+(fu?'boca arriba':'boca abajo')+')');
    var cr=CHAPTERS[G.chapter].rules;
    if(cr&&cr.draw&&cr.draw.indexOf(si+1)>=0&&G.aside.length){
      G.hands[p].push(G.aside.shift());
      glog('🃏 '+pname(p)+' roba una carta al jugar en el sector '+(si+1));
    }
    var allEmpty=G.hands.every(function(h){return h.length===0;});
    if(allEmpty){
      G.phase='resolution';
      G.result={};
      glog('🔍 Todas las cartas colocadas. Revelando — comprobad los totales por sector.');
    }else{
      var next=(p+1)%G.players.length;
      while(G.hands[next].length===0)next=(next+1)%G.players.length;
      G.placeTurn=next;
      glog('👉 Turno de '+pname(next));
    }
    broadcast();return;
  }
  if(a.kind==='restart'){if(!isHost)return;startGame();return;}
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
        else c.send({t:'error',msg:'Partida en curso'});return;
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
  myName=el('tt-name').value.trim();
  if(!myName)return setupMsg('Pon tu nombre primero');
  var alpha='ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
  roomCode='';for(var i=0;i<4;i++)roomCode+=alpha[Math.floor(Math.random()*alpha.length)];
  setupMsg('Creando sala...');
  peer=new Peer(PREFIX+roomCode);
  peer.on('open',function(){isHost=true;newGame();G.players.push({name:myName,conn:null,connected:true});history.pushState(null,'',location.pathname+'?sala='+roomCode);el('tt-setup').style.display='none';el('tt-game').style.display='block';broadcast();});
  peer.on('connection',setupHostConn);
  peer.on('error',function(e){setupMsg(e.type==='unavailable-id'?'Código ocupado, prueba otra vez':'Error: '+e.type);});
}

function joinRoom(){
  myName=el('tt-name').value.trim();roomCode=el('tt-code').value.trim().toUpperCase();
  if(!myName)return setupMsg('Pon tu nombre primero');
  if(roomCode.length!==4)return setupMsg('El código tiene 4 letras');
  setupMsg('Conectando...');peer=new Peer();
  peer.on('open',function(){
    hostConn=peer.connect(PREFIX+roomCode,{reliable:true});
    hostConn.on('open',function(){hostConn.send({t:'join',name:myName});});
    hostConn.on('data',function(msg){
      if(!msg)return;
      if(msg.t==='state'){V=msg.s;selectedCardId=null;pendingSlot=null;el('tt-setup').style.display='none';el('tt-game').style.display='block';render();}
      else if(msg.t==='error'){V?toast(msg.msg):setupMsg(msg.msg);}
    });
    hostConn.on('close',function(){toast('Conexión perdida con el anfitrión 😢');});
  });
  peer.on('error',function(e){setupMsg(e.type==='peer-unavailable'?'No existe la sala '+roomCode:'Error: '+e.type);});
}

function sendAction(a){if(isHost)applyAction(0,a);else hostConn.send({t:'action',a:a});}

// ---- render ----
function suitLabel(suit){return suit==='solar'?'<span class="tt-solar">☀️ Solar</span>':'<span class="tt-lunar">🌙 Lunar</span>';}

function viewSectorSum(sec){
  // sums only known values; if any card hidden, sum is partial.
  // score:'diff' chapters score a sector as highest − lowest instead of the sum.
  var R=V&&CHAPTERS[V.chapter]&&CHAPTERS[V.chapter].rules;
  var vals=sec.map(function(c){return c.value||0;});
  if(R&&R.score==='diff')return vals.length?Math.max.apply(null,vals)-Math.min.apply(null,vals):0;
  return vals.reduce(function(a,b){return a+b;},0);
}
// sector i centered at -90+i*60 deg → sector 0 middle at 12 o'clock.
// radius 57% (>50%) places the cards just outside the clock rim.
function sectorHtml(sec,si,canPlace,isResolution,blocked){
  var angle=(-90+si*60)*Math.PI/180;
  var x=(50+57*Math.cos(angle)).toFixed(2);
  var y=(50+57*Math.sin(angle)).toFixed(2);
  var pos='left:'+x+'%;top:'+y+'%';

  var cls='tt-sector'+(canPlace?' tt-sector-target':'')+(blocked?' tt-sector-blocked':'');
  var inner='';
  if(isResolution){
    // show neutral per-sector total; the players judge against the chapter rules
    inner+='<div class="tt-sector-sum">'+viewSectorSum(sec)+'</div>';
  }
  if(!sec.length){
    inner+='<div class="tt-sector-num">'+(si+1)+'</div>';
  }else{
    sec.forEach(function(slot){
      var hasFront=slot.value!==undefined;
      var st=hasFront?spriteStyle(slot.cardId):backStyle(slot.cardId);
      inner+='<div class="tt-card-xs'+(hasFront?'':' tt-cardback')+'" style="'+st+'" title="'+(hasFront?esc(slot.suit)+' '+slot.value:'oculta')+'"></div>';
    });
  }
  return '<div class="'+cls+'" style="'+pos+'" data-sector="'+si+'" data-act="'+(canPlace?'placeCard':'')+'"'+(blocked?' title="'+esc(blocked)+'"':'')+'>'+inner+'</div>';
}

function render(){
  if(!V)return;
  var h='';
  h+='<div class="tt-row">Sala: <span class="tt-code">'+roomCode+'</span> <button data-act="copy" class="tt-btn-sm">📋 Copiar enlace</button></div>';

  if(V.phase==='lobby'){
    h+='<div class="tt-panel"><h3>Jugadores ('+V.names.length+'/'+MAXP+')</h3>';
    V.names.forEach(function(n,i){h+='<div>'+esc(n)+(i===0?' 👑':'')+(i===V.me?' (tú)':'')+(V.connected[i]?'':' 🔌❌')+'</div>';});
    h+='</div>';
    var ch=CHAPTERS[V.chapter||0];
    if(V.me===0){
      h+='<div class="tt-panel"><h3>Capítulo</h3>';
      h+='<select id="tt-chapter-sel" data-act="pickChapter">';
      CHAPTERS.forEach(function(c,i){h+='<option value="'+i+'"'+(i===(V.chapter||0)?' selected':'')+'>Capítulo '+c.n+'</option>';});
      h+='</select>';
      h+='<img class="tt-chapter-img" src="'+ch.img+'" alt="Capítulo '+ch.n+'">';
      h+='</div>';
      h+='<button data-act="start"'+(V.names.length<MINP?' disabled':'')+'>🚀 Empezar ('+(V.names.length<MINP?'mínimo '+MINP:V.names.length+' jugadores')+')</button>';
    }else{
      h+='<div class="tt-panel"><div class="tt-mut">Capítulo <b>'+esc(ch.n)+'</b></div>';
      h+='<img class="tt-chapter-img" src="'+ch.img+'" alt="Capítulo '+ch.n+'"></div>';
      h+='<div class="tt-mut">Esperando a que '+esc(V.names[0])+' empiece...</div>';
    }
  }else{
    // Resolution banner (no automatic verdict — special chapters decide)
    if(V.phase==='resolution'){
      h+='<div class="tt-banner-res">🔍 <b>Resolución</b> — Totales por sector: '+
         V.sectors.map(viewSectorSum).join(' → ')+'<br><span class="tt-mut">Comprobad las reglas del capítulo para decidir el resultado.</span>';
      if(V.me===0){
        h+='<div class="tt-row" style="margin-top:8px;gap:6px"><span class="tt-mut" style="font-size:13px">Siguiente capítulo:</span>';
        h+='<select id="tt-chapter-sel" data-act="pickChapter">';
        CHAPTERS.forEach(function(c,i){h+='<option value="'+i+'"'+(i===(V.chapter||0)?' selected':'')+'>Capítulo '+c.n+'</option>';});
        h+='</select>';
        h+='<button data-act="restart" class="tt-btn-sm tt-btn-ok">🔄 Nueva ronda</button></div>';
      }
      h+='</div>';
    }

    // Phase banner
    if(V.phase==='discussion'){
      h+='<div class="tt-phase-disc">💬 <b>Discusión</b> — ¡Sin mirar las cartas! Planificad la estrategia.';
      if(V.me===0){
        h+='<div class="tt-row" style="margin-top:6px;gap:5px"><span class="tt-mut" style="font-size:13px">Primer jugador:</span>';
        V.names.forEach(function(n,i){
          h+='<button data-act="setFirst" data-p="'+i+'" class="tt-btn-sm'+(V.placeTurn===i?' tt-btn-ok':'')+'">'+esc(n)+'</button>';
        });
        h+='</div>';
        h+='<button data-act="startPlacement" class="tt-btn-sm tt-btn-ok" style="margin-top:4px">🤫 Iniciar colocación</button>';
      }else{
        h+='<div class="tt-mut" style="margin-top:4px">Esperando al anfitrión... (primero: <b>'+esc(V.names[V.placeTurn])+'</b>)</div>';
      }
      h+='</div>';
    }else if(V.phase==='placement'&&V.placeTurn!==V.me){
      h+='<div class="tt-phase-place">⏳ Turno de <b>'+esc(V.names[V.placeTurn])+'</b> — coloca su carta en silencio</div>';
    }

    // Chapter restrictions (auto-enforced)
    var rd=describeRules(V.chapter);
    if(rd.length){
      h+='<div class="tt-panel" style="border:1px solid var(--gold)"><div class="tt-name">📜 Restricciones del capítulo '+esc(CHAPTERS[V.chapter].n)+'</div>';
      h+='<div class="tt-mut">'+rd.map(esc).join(' · ')+'</div></div>';
    }

    // Clock: chapter image as round background + 6 sectors
    var isRes=V.phase==='resolution';
    h+='<div class="tt-clock-wrap"><img class="tt-clock-img" src="'+CHAPTERS[V.chapter||0].img+'" alt="Capítulo '+CHAPTERS[V.chapter||0].n+'">';
    var myTurn=V.phase==='placement'&&V.placeTurn===V.me;
    var canPlace=myTurn&&selectedCardId!==null;
    var selCard=canPlace?V.hands[V.me].filter(function(c){return c.id===selectedCardId;})[0]:null;
    for(var si=0;si<SECTORS;si++){
      var blk=selCard?placementError(V.sectors,V.chapter,si,selCard):null;
      h+=sectorHtml(V.sectors[si],si,canPlace&&!blk,isRes,blk);
    }
    h+='</div>';

    // Hands
    var nP=V.names.length;
    Array.from({length:nP},function(_,k){return(V.me+k)%nP;}).forEach(function(i){
      var isMe=i===V.me;
      var isTurn=V.phase==='placement'&&V.placeTurn===i;
      h+='<div class="tt-panel'+(isTurn?' tt-active':'')+'"><div class="tt-name">'+(isTurn?'▶ ':'')+esc(V.names[i])+(isMe?' (tú)':'')+(V.connected[i]?'':' 🔌❌')+'</div>';
      h+='<div class="tt-hand">';
      if(isMe){
        V.hands[i].forEach(function(card){
          var isSel=selectedCardId===card.id;
          var cpErr=myTurn?cardPlayError(V.hands[i],V.chapter,card):null;
          var selectable=myTurn&&!cpErr;
          var cls='tt-card'+(isSel?' tt-selected':selectable?' tt-selectable':'')+(cpErr?' tt-card-blocked':'');
          h+='<div class="'+cls+'" style="'+spriteStyle(card.cardId)+'" '+(cpErr?'':'data-cid="'+card.id+'" data-act="selectCard" ')+'title="'+esc(cpErr||card.name)+'"></div>';
        });
        if(V.hands[i].length===0)h+='<div class="tt-mut" style="padding:4px">Sin cartas en mano</div>';
        // placement controls — fill the empty space to the right of the cards
        if(myTurn){
          h+='<div class="tt-place-ctrl">';
          if(pendingSlot!==null){
            var fuNow=V.sectors.reduce(function(n,sec){return n+sec.filter(function(s){return s.faceUp;}).length;},0);
            var fuMax=maxFaceUp(V.chapter,V.names.length);var canFaceUp=fuNow<fuMax;
            h+='<div class="tt-mut">📍 Sector '+(pendingSlot.sector+1)+' — ¿cómo?</div>';
            h+='<button data-act="placeFace" data-fu="1" class="tt-btn-sm tt-btn-ok"'+(canFaceUp?'':' disabled')+'>⬆ Boca arriba ('+fuNow+'/'+fuMax+')</button>';
            h+='<button data-act="placeFace" data-fu="0" class="tt-btn-sm tt-btn-warn">⬇ Boca abajo</button>';
            h+='<button data-act="cancelPlace" class="tt-btn-sm">✖ Cancelar</button>';
          }else if(selectedCardId!==null){
            h+='<div class="tt-mut">🎴 Elige un sector del reloj</div>';
          }else{
            h+='<div class="tt-mut">🎴 Selecciona una carta de tu mano</div>';
          }
          h+='</div>';
        }
      }else{
        V.hands[i].forEach(function(card){
          h+='<div class="tt-card tt-cardback" style="'+backStyle(card.cardId)+'" title="Carta oculta"></div>';
        });
        if(V.hands[i].length===0)h+='<div class="tt-mut" style="padding:4px">Sin cartas</div>';
      }
      h+='</div></div>';
    });

    // Log
    h+='<div class="tt-panel tt-log">'+V.log.slice().reverse().map(function(l){return'<div>'+esc(l)+'</div>';}).join('')+'</div>';
  }

  el('tt-game').innerHTML=h;
}

// ---- events ----
el('tt-create').addEventListener('click',createRoom);
el('tt-join').addEventListener('click',joinRoom);
el('tt-game').addEventListener('click',function(e){
  var b=e.target.closest('[data-act]');
  if(!b)return;
  var act=b.getAttribute('data-act');
  var cid=b.getAttribute('data-cid');

  if(act==='copy'){
    navigator.clipboard.writeText(location.origin+location.pathname+'?sala='+roomCode).then(function(){toast('Enlace copiado 📋');});
  }else if(act==='start'&&isHost){
    startGame();
  }else if(act==='startPlacement'){
    sendAction({kind:'startPlacement'});
  }else if(act==='selectCard'){
    if(V&&V.phase==='placement'&&V.placeTurn===V.me){
      selectedCardId=(selectedCardId===+cid)?null:+cid;
      render();
    }
  }else if(act==='setFirst'){
    sendAction({kind:'setFirst',p:+b.getAttribute('data-p')});
  }else if(act==='placeCard'){
    if(V&&V.phase==='placement'&&V.placeTurn===V.me&&selectedCardId!==null){
      pendingSlot={cid:selectedCardId,sector:+b.getAttribute('data-sector')};
      selectedCardId=null;
      render();
    }
  }else if(act==='placeFace'){
    if(V&&V.phase==='placement'&&V.placeTurn===V.me&&pendingSlot!==null){
      sendAction({kind:'place',cid:pendingSlot.cid,sector:pendingSlot.sector,faceUp:+b.getAttribute('data-fu')===1});
      pendingSlot=null;
    }
  }else if(act==='cancelPlace'){
    selectedCardId=null;pendingSlot=null;render();
  }else if(act==='restart'){
    sendAction({kind:'restart'});
  }
});
el('tt-game').addEventListener('change',function(e){
  var s=e.target.closest('[data-act="pickChapter"]');
  if(s&&isHost)sendAction({kind:'setChapter',ch:+s.value});
});

// ---- URL auto-fill ----
var m=location.search.match(/sala=([A-Za-z0-9]{4})/);
if(m)el('tt-code').value=m[1].toUpperCase();
})();
</script>

## ¿Cómo se juega?

Take Time es un juego **cooperativo**: todos ganan o pierden juntos. El objetivo es colocar las **12 cartas repartidas** en los **6 sectores del reloj**, sin poder comunicarse durante la colocación.

**Regla universal:** La suma de cada sector debe ser **mayor o igual** que la del sector anterior (en sentido horario) y ningún sector debe sumar más de 24. Pero **cada capítulo añade sus propias reglas especiales** — consultad la imagen del reloj.

Las cartas son ☀️ **Solar** (1–12) y 🌙 **Lunar** (1–12). El dorso revela el tipo pero no el número.

**Fases de cada prueba:**

1. **Discusión** — Los jugadores planifican la estrategia *sin mirar sus cartas*. Podéis hablar de sectores, preferencias, etc.
2. **Colocación** — En silencio y por turnos, cada jugador coloca una carta en un sector, boca abajo o boca arriba (máx. tantas boca arriba como jugadores).
3. **Resolución** — Se revelan todas las cartas y se muestran los totales por sector. **Vosotros decidís** si habéis superado la prueba según las reglas del capítulo.
