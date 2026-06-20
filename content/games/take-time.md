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
#tt-app h3{margin:10px 0 6px}
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
.tt-card{width:72px;height:101px;border-radius:7px;border:2px solid rgba(0,0,0,.5);
  flex:none;position:relative;user-select:none;background-color:#1e2535;cursor:default}
.tt-card-sm{width:54px;height:76px;border-radius:6px;border:2px solid rgba(0,0,0,.5);
  flex:none;position:relative;user-select:none;background-color:#1e2535}
.tt-card-xs{width:40px;height:56px;border-radius:5px;border:1px solid rgba(0,0,0,.4);
  flex:none;position:relative;user-select:none;background-color:#1e2535}
.tt-back-img{background-size:cover!important;background-position:center!important}
.tt-empty{display:flex;align-items:center;justify-content:center;
  border:2px dashed var(--border)!important;background:transparent!important;color:var(--mut)}
.tt-suit-badge{position:absolute;top:2px;left:3px;font-size:13px;line-height:1}
.tt-val-badge{position:absolute;bottom:2px;right:4px;font-size:11px;font-weight:800;
  color:#fff;text-shadow:0 1px 4px rgba(0,0,0,.9)}
.tt-hour{position:absolute;inset:0;display:flex;align-items:center;justify-content:center;
  font-size:13px;color:var(--mut);font-weight:700}
/* Clock */
.tt-clock-wrap{position:relative;width:min(360px,90vw);aspect-ratio:1;margin:8px auto}
.tt-clock-bg{position:absolute;inset:0;border-radius:50%;border:2px solid var(--border);
  background:radial-gradient(circle,#1a2035 60%,#0d1117 100%)}
.tt-clock-slot{position:absolute;transform:translate(-50%,-50%)}
/* Slot highlight */
.tt-slot-target{cursor:pointer;outline:3px solid var(--gold);outline-offset:2px;border-radius:7px}
.tt-slot-target:hover{outline-color:#fff}
/* Hand */
.tt-hand{display:flex;flex-wrap:wrap;gap:7px;padding:4px 0}
.tt-selected{outline:3px solid var(--gold)!important;outline-offset:2px;border-radius:7px;cursor:pointer}
.tt-clickable{cursor:pointer}
.tt-card.tt-selectable{cursor:pointer}
.tt-card.tt-selectable:hover{outline:2px solid var(--blue);outline-offset:2px;border-radius:7px}
/* Phases */
.tt-phase-disc{background:#1a2a1a;border:1px solid #16a34a;border-radius:10px;padding:10px 14px;margin:6px 0}
.tt-phase-place{background:#1a1a2e;border:1px solid var(--blue);border-radius:10px;padding:10px 14px;margin:6px 0}
.tt-banner-win{background:#14391c;border:2px solid #22c55e;border-radius:10px;padding:12px 16px;margin:6px 0;font-weight:700;font-size:16px}
.tt-banner-lose{background:#3b1212;border:2px solid #ef4444;border-radius:10px;padding:12px 16px;margin:6px 0;font-weight:700;font-size:16px}
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
/* resolution highlight */
.tt-ok{outline:3px solid #22c55e!important;outline-offset:2px;border-radius:7px}
.tt-fail{outline:3px solid #ef4444!important;outline-offset:2px;border-radius:7px}
</style>

<div id="tt-app">
  <div id="tt-setup">
    <div class="tt-row"><label>Tu nombre: <input id="tt-name" maxlength="14" placeholder="Xiang"></label></div>
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
var BACK='https://steamusercontent-a.akamaihd.net/ugc/10900776384674959786/3BEEBA5CE8F56A8B2D7041793909E528AF64565A/';
var PREFIX='take-time-xiang-';
var MAXP=4,MINP=3,CLOCK_SIZE=12;

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
  var col=cardId%6,row=Math.floor(cardId/6);
  return 'background-image:url('+BACK+');background-size:600% 400%;background-position:'+(col/5*100).toFixed(1)+'% '+(row/3*100).toFixed(1)+'%';
}

// ---- deck ----
function mkDeck(){
  var d=CARDS_DEF.map(function(def,i){return{id:i,name:def.name,value:def.value,suit:def.suit,cardId:def.cardId};});
  for(var i=d.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));var t=d[i];d[i]=d[j];d[j]=t;}
  return d;
}

// ---- game logic (host) ----
function newGame(){G={phase:'lobby',players:[],hands:[],aside:[],clock:[],turn:0,log:[],result:null};}

function startGame(){
  var full=mkDeck();
  var toDeal=full.slice(0,CLOCK_SIZE);
  G.aside=full.slice(CLOCK_SIZE);
  G.clock=[];for(var i=0;i<CLOCK_SIZE;i++)G.clock.push(null);
  G.hands=G.players.map(function(){return[];});
  toDeal.forEach(function(card,k){G.hands[k%G.players.length].push(card);});
  G.placeTurn=0;
  G.phase='discussion';
  G.result=null;
  G.log=['⏱ ¡Partida iniciada! Fase de discusión — planificad sin mirar vuestras cartas.'];
  broadcast();
}

function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>80)G.log.shift();}

function checkWin(){
  var vals=G.clock.map(function(s){return s?s.value:0;});
  for(var i=1;i<CLOCK_SIZE;i++){if(vals[i]<vals[i-1])return false;}
  return true;
}

function buildStateFor(me){
  return{
    me:me,phase:G.phase,placeTurn:G.placeTurn,
    aside:G.aside.map(function(c){return{id:c.id,name:c.name,cardId:c.cardId,suit:c.suit,value:c.value};}),
    clock:G.clock.map(function(slot){
      if(!slot)return null;
      if(G.phase==='resolution'||slot.faceUp)return{id:slot.id,value:slot.value,suit:slot.suit,cardId:slot.cardId,owner:slot.owner,faceUp:slot.faceUp};
      return{id:slot.id,cardId:slot.cardId,owner:slot.owner}; // face-down: back for everyone, no value
    }),
    hands:G.hands.map(function(h,i){
      if(i===me)return h.map(function(c){return{id:c.id,name:c.name,cardId:c.cardId,suit:c.suit,value:c.value};});
      return h.map(function(c){return{id:c.id,cardId:c.cardId,hidden:true};});
    }),
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    log:G.log.slice(-30),result:G.result
  };
}

function broadcast(){
  G.players.forEach(function(p,i){if(i===0)return;if(p.conn&&p.connected)p.conn.send({t:'state',s:buildStateFor(i)});});
  V=buildStateFor(0);selectedCardId=null;pendingSlot=null;render();
}
function sendErr(p,msg){if(p===0)toast(msg);else if(G.players[p].conn)G.players[p].conn.send({t:'error',msg:msg});}

function applyAction(p,a){
  if(!G)return;
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
    var si=a.slot;
    if(si<0||si>=CLOCK_SIZE||G.clock[si]!==null)return sendErr(p,'Posición no válida');
    var hIdx=G.hands[p].findIndex(function(c){return c.id===a.cid;});
    if(hIdx===-1)return sendErr(p,'Carta no encontrada');
    var card=G.hands[p].splice(hIdx,1)[0];
    var fu=!!a.faceUp;
    if(fu){
      var fuCount=G.clock.filter(function(s){return s&&s.faceUp;}).length;
      if(fuCount>=G.players.length)return sendErr(p,'Máximo '+G.players.length+' cartas boca arriba');
    }
    G.clock[si]={id:card.id,value:card.value,suit:card.suit,cardId:card.cardId,owner:p,faceUp:fu};
    glog('🎴 '+pname(p)+' coloca en posición '+(si+1)+' ('+(fu?'boca arriba':'boca abajo')+')');
    var allEmpty=G.hands.every(function(h){return h.length===0;});
    if(allEmpty){
      G.phase='resolution';
      G.result={win:checkWin()};
      glog(G.result.win?'✅ ¡Victoria! El orden del reloj es correcto.':'❌ Derrota. El orden no es ascendente.');
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
  peer.on('open',function(){isHost=true;newGame();G.players.push({name:myName,conn:null,connected:true});el('tt-setup').style.display='none';el('tt-game').style.display='block';broadcast();});
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

function clockSlotHtml(slot,si,canPlace){
  var angle=(si*30-90)*Math.PI/180;
  var x=(50+40*Math.cos(angle)).toFixed(2);
  var y=(50+40*Math.sin(angle)).toFixed(2);
  var pos='left:'+x+'%;top:'+y+'%';

  if(!slot){
    var cls='tt-clock-slot tt-card-sm tt-empty'+(canPlace?' tt-slot-target':'');
    return '<div class="'+cls+'" style="'+pos+'" data-slot="'+si+'" data-act="'+(canPlace?'placeCard':'')+'">'
      +'<span class="tt-hour">'+(si+1)+'</span></div>';
  }

  // Occupied: show front if value is known (own card, face-up, or resolution)
  var hasFront=slot.value!==undefined;
  var isOk=V.result&&V.result.win; // checked in resolution
  var isFail=V.result&&!V.result.win;
  var failSlot=isFail&&si>0&&V.clock[si-1]&&slot.value<V.clock[si-1].value;

  var cls='tt-clock-slot tt-card-sm'+(failSlot?' tt-fail':'');
  var style=pos+';'+(hasFront?spriteStyle(slot.cardId):backStyle(slot.cardId));
  return '<div class="'+cls+'" style="'+style+'" title="'+(hasFront?esc(slot.suit)+' '+slot.value:esc(slot.suit))+'"></div>';
}

function render(){
  if(!V)return;
  var h='';
  h+='<div class="tt-row">Sala: <span class="tt-code">'+roomCode+'</span> <button data-act="copy" class="tt-btn-sm">📋 Copiar enlace</button></div>';

  if(V.phase==='lobby'){
    h+='<div class="tt-panel"><h3>Jugadores ('+V.names.length+'/'+MAXP+')</h3>';
    V.names.forEach(function(n,i){h+='<div>'+esc(n)+(i===0?' 👑':'')+(i===V.me?' (tú)':'')+(V.connected[i]?'':' 🔌❌')+'</div>';});
    h+='</div>';
    if(V.me===0){
      h+='<button data-act="start"'+(V.names.length<MINP?' disabled':'')+'>🚀 Empezar ('+(V.names.length<MINP?'mínimo '+MINP:V.names.length+' jugadores')+')</button>';
    }else{h+='<div class="tt-mut">Esperando a que '+esc(V.names[0])+' empiece...</div>';}
  }else{
    // Result banner
    if(V.result){
      if(V.result.win){h+='<div class="tt-banner-win">✅ ¡Victoria! Las cartas están en orden ascendente.'+(V.me===0?' <button data-act="restart">🔄 Repetir</button>':'')+'</div>';}
      else{h+='<div class="tt-banner-lose">❌ Derrota. El orden del reloj no es ascendente.'+(V.me===0?' <button data-act="restart">🔄 Repetir</button>':'')+'</div>';}
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
    }else if(V.phase==='placement'){
      if(V.placeTurn===V.me){
        if(pendingSlot!==null){
          var fuNow=V.clock.filter(function(s){return s&&s.faceUp;}).length;
          var fuMax=V.names.length;
          var canFaceUp=fuNow<fuMax;
          h+='<div class="tt-phase-place">📍 Posición '+(pendingSlot.slot+1)+' — ¿Cómo colocarla?'
            +' <button data-act="placeFace" data-fu="1" class="tt-btn-sm tt-btn-ok"'+(canFaceUp?'':' disabled')+'>Boca arriba ('+fuNow+'/'+fuMax+')</button>'
            +' <button data-act="placeFace" data-fu="0" class="tt-btn-sm tt-btn-warn">Boca abajo</button>'
            +' <button data-act="cancelPlace" class="tt-btn-sm">Cancelar</button></div>';
        }else{
          h+='<div class="tt-phase-place">🎴 <b>Tu turno</b> — '+(selectedCardId!==null?'Elige posición en el reloj':'Selecciona una carta')+'</div>';
        }
      }else{
        h+='<div class="tt-phase-place">⏳ Turno de <b>'+esc(V.names[V.placeTurn])+'</b> — coloca su carta en silencio</div>';
      }
    }else if(V.phase==='resolution'){
      h+='<div class="tt-mut">🔍 Resolución — Orden: '+V.clock.map(function(s){return s?s.value:'?';}).join(' → ')+'</div>';
    }

    // Clock
    h+='<div class="tt-clock-wrap"><div class="tt-clock-bg"></div>';
    var myTurn=V.phase==='placement'&&V.placeTurn===V.me;
    var canPlace=myTurn&&selectedCardId!==null;
    for(var si=0;si<CLOCK_SIZE;si++)h+=clockSlotHtml(V.clock[si],si,canPlace);
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
          var selectable=myTurn;
          var cls='tt-card'+(isSel?' tt-selected':selectable?' tt-selectable':'');
          h+='<div class="'+cls+'" style="'+spriteStyle(card.cardId)+'" data-cid="'+card.id+'" data-act="selectCard" title="'+esc(card.name)+'"></div>';
        });
        if(V.hands[i].length===0)h+='<div class="tt-mut" style="padding:4px">Sin cartas en mano</div>';
      }else{
        V.hands[i].forEach(function(card){
          h+='<div class="tt-card" style="'+backStyle(card.cardId)+'" title="Carta oculta"></div>';
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
      pendingSlot={cid:selectedCardId,slot:+b.getAttribute('data-slot')};
      selectedCardId=null;
      render();
    }
  }else if(act==='placeFace'){
    if(V&&V.phase==='placement'&&V.placeTurn===V.me&&pendingSlot!==null){
      sendAction({kind:'place',cid:pendingSlot.cid,slot:pendingSlot.slot,faceUp:+b.getAttribute('data-fu')===1});
      pendingSlot=null;
    }
  }else if(act==='cancelPlace'){
    selectedCardId=null;pendingSlot=null;render();
  }else if(act==='restart'){
    sendAction({kind:'restart'});
  }
});

// ---- URL auto-fill ----
var m=location.search.match(/sala=([A-Za-z0-9]{4})/);
if(m)el('tt-code').value=m[1].toUpperCase();
})();
</script>

## ¿Cómo se juega?

Take Time es un juego **cooperativo**: todos ganan o pierden juntos. El objetivo es colocar las **12 cartas repartidas** en los 12 segmentos del reloj siguiendo un orden ascendente, sin poder comunicarse durante la colocación.

**Regla universal:** El valor de cada segmento debe ser **mayor o igual** al del segmento anterior (en sentido horario). Ningún segmento puede sumar más de 24.

Las cartas son ☀️ **Solar** (1–12) y 🌙 **Lunar** (1–12). El dorso revela el tipo pero no el número.

**Fases de cada prueba:**

1. **Discusión** — Los jugadores planifican la estrategia *sin mirar sus cartas*. Podéis hablar de posiciones, preferencias, etc.
2. **Colocación** — En silencio y por turnos, cada jugador coloca una carta boca abajo en el reloj. Los demás ven el dorso (Solar/Lunar) pero no el número.
3. **Resolución** — Se revelan todas las cartas. Si el orden es ascendente, ¡victoria!
