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

Juego de cartas para 3–6 jugadores. Uno crea la sala y comparte el enlace. La partida vive en el navegador del anfitrión.

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
.fr-dragging{opacity:.25!important}
.fr-drop-line{width:5px;height:203px;background:#fbbf24;border-radius:3px;flex:none;
  box-shadow:0 0 10px #fbbf24}
</style>

<div id="fr-app">
  <div id="fr-setup">
    <div class="fr-row"><label>Tu nombre: <input id="fr-name" maxlength="14" placeholder="Xiang"></label></div>
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
var MAXP=6,MINP=3,MAX_DISCARD=10,HAND_SIZE=7;

// [name, cardId, suit]
var CARDS_DEF=[
  ['Mountain',600,'Land'],['Cavern',601,'Land'],['Forest',603,'Land'],
  ['Earth Elemental',604,'Land'],['Swamp',606,'Land'],['Island',608,'Land'],
  ['Water Elemental',609,'Flood'],['Rainstorm',610,'Flood'],['Blizzard',611,'Flood'],
  ['Smoke',612,'Flood'],['Whirlwind',613,'Flood'],['Air Elemental',614,'Flood'],
  ['Wildfire',615,'Flame'],['Candle',616,'Flame'],['Forge',617,'Flame'],
  ['Lightning',618,'Flame'],['Fire Elemental',619,'Flame'],
  ['Knights',620,'Army'],['Elven Archers',621,'Army'],['Light Cavalry',622,'Army'],
  ['Dwarvish Infantry',623,'Army'],
  ['Collector',625,'Leader'],['Beastmaster',626,'Leader'],['Warlock Lord',628,'Leader'],
  ['Enchantress',629,'Leader'],['King',630,'Leader'],['Queen',631,'Leader'],
  ['Princess',632,'Leader'],['Warlord',633,'Leader'],['Empress',634,'Leader'],
  ['Unicorn',635,'Beast'],['Basilisk',636,'Beast'],['Warhorse',637,'Beast'],
  ['Dragon',638,'Beast'],['Hydra',639,'Beast'],
  ['Warship',640,'Artifact'],['Magic Wand',641,'Artifact'],['Sword of Keth',642,'Artifact'],
  ['Elven Longbow',643,'Artifact'],['War Dirigible',644,'Artifact'],
  ['Shield of Keth',645,'Artifact'],['Gem of Order',646,'Artifact'],
  ['Book of Changes',648,'Spell'],['Protection Rune',649,'Spell'],['Doppelganger',652,'Spell']
];

var SUIT_COLOR={
  Land:'#65a30d',Flood:'#0284c7',Flame:'#ef4444',Army:'#92400e',
  Leader:'#7c3aed',Beast:'#d97706',Artifact:'#6b7280',Spell:'#be185d'
};

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
function spriteStyle(cardId){
  var idx=cardId-600,col=idx%10,row=Math.floor(idx/10);
  var xp=(col/9*100).toFixed(2),yp=(row/5*100).toFixed(2);
  return 'background-image:url('+SPRITE+');background-size:1000% 600%;background-position:'+xp+'% '+yp+'%';
}

function cardHtml(card,cls,act,extra){
  var attrs='data-cid="'+card.id+'" title="'+esc(card.name)+' ('+card.suit+')"';
  if(act)attrs+=' data-act="'+act+'"';
  if(extra)attrs+=' '+extra;
  return '<div class="'+cls+'" style="'+spriteStyle(card.cardId)+'" '+attrs+'></div>';
}

// ---------- hand order ----------
var myOrder=[];
var dragId=null,dragDropId=null,dragBefore=true,isDragging=false;

function reconcileOrder(hand){
  var ids=hand.map(function(c){return c.id;});
  myOrder=myOrder.filter(function(id){return ids.indexOf(id)!==-1;});
  ids.forEach(function(id){if(myOrder.indexOf(id)===-1)myOrder.push(id);});
}

// ---------- game logic (host) ----------
function mkDeck(){
  var d=CARDS_DEF.map(function(def,i){return{id:i,name:def[0],cardId:def[1],suit:def[2]};});
  for(var i=d.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));var t=d[i];d[i]=d[j];d[j]=t;}
  return d;
}

function newGame(){
  G={phase:'lobby',players:[],hands:[],deck:[],discard:[],turn:0,turnPhase:'pickup',log:[],result:null};
}

function startGame(){
  G.deck=mkDeck();G.discard=[];G.result=null;
  G.turn=Math.floor(Math.random()*G.players.length);
  G.turnPhase='pickup';
  G.log=['🏰 ¡Empieza la partida! Turno inicial: '+pname(G.turn)];
  G.hands=G.players.map(function(){return[];});
  for(var k=0;k<HAND_SIZE;k++)G.players.forEach(function(_,i){G.hands[i].push(G.deck.pop());});
  G.phase='playing';
  broadcast();
}

function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>60)G.log.shift();}

function buildStateFor(me){
  return{
    me:me,
    phase:G.phase,turn:G.turn,turnPhase:G.turnPhase,
    deckCount:G.deck.length,
    discard:G.discard.map(function(c){return{id:c.id,name:c.name,cardId:c.cardId,suit:c.suit};}),
    hands:G.hands.map(function(h,i){return h.map(function(c){
      if(i===me||G.phase==='ended')return{id:c.id,name:c.name,cardId:c.cardId,suit:c.suit};
      return{id:c.id,hidden:true};
    });}),
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    log:G.log.slice(-30),
    result:G.result
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
  if(!G||G.phase!=='playing')return;
  if(G.turn!==p)return sendErr(p,'No es tu turno');

  if(a.kind==='draw'){
    if(G.turnPhase!=='pickup')return sendErr(p,'Ya has cogido carta, ahora descarta');
    if(!G.deck.length)return sendErr(p,'El mazo está vacío');
    G.hands[p].push(G.deck.pop());
    glog('🂠 '+pname(p)+' roba una carta del mazo');
    G.turnPhase='discard';
  }else if(a.kind==='take'){
    if(G.turnPhase!=='pickup')return sendErr(p,'Ya has cogido carta, ahora descarta');
    if(!G.discard.length)return sendErr(p,'El descarte está vacío');
    var dIdx=G.discard.findIndex(function(c){return c.id===a.cid;});
    if(dIdx===-1)return sendErr(p,'Carta no encontrada en el descarte');
    var taken=G.discard.splice(dIdx,1)[0];
    G.hands[p].push(taken);
    glog('✋ '+pname(p)+' toma '+taken.name+' del descarte');
    G.turnPhase='discard';
  }else if(a.kind==='discard'){
    if(G.turnPhase!=='discard')return sendErr(p,'Primero roba o toma una carta');
    var hIdx=G.hands[p].findIndex(function(c){return c.id===a.cid;});
    if(hIdx===-1)return sendErr(p,'Carta no encontrada en tu mano');
    var disc=G.hands[p].splice(hIdx,1)[0];
    G.discard.push(disc);
    glog('🗑 '+pname(p)+' descarta '+disc.name);
    if(G.discard.length>=MAX_DISCARD){
      G.phase='ended';G.result={};
      glog('🏁 ¡El descarte tiene '+G.discard.length+' cartas — fin de la partida!');
      broadcast();return;
    }
    G.turn=(G.turn+1)%G.players.length;
    G.turnPhase='pickup';
    glog('👉 Turno de '+pname(G.turn));
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

// ---------- render ----------
function render(){
  if(!V)return;

  // reconcile local hand order with new state
  if(V.hands&&V.hands[V.me])reconcileOrder(V.hands[V.me]);

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
      h+='<div class="fr-banner">🏁 ¡Fin de la partida! Contad las puntuaciones de vuestras manos.'+
         (V.me===0?' <button data-act="start">🔄 Otra partida</button>':'')+'</div>';
    }

    var myTurn=V.phase==='playing'&&V.turn===V.me;
    var canPickup=myTurn&&V.turnPhase==='pickup';
    var canDiscard=myTurn&&V.turnPhase==='discard';

    if(V.phase==='playing'){
      if(myTurn){
        if(canPickup)h+='<div class="fr-action-hint">✨ Tu turno: roba del mazo <b>o</b> toma una carta del descarte</div>';
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
         ' style="background-image:url('+BACK+');background-size:cover;background-position:center" title="Robar del mazo ('+V.deckCount+')">'+
         '<span class="fr-back-count">'+V.deckCount+'</span></div>';
    }else{
      h+='<div class="fr-card-sm fr-empty" title="Mazo vacío">∅</div>';
    }
    h+='</div>';

    // Discard
    h+='<div class="fr-zone fr-discard-zone"><div class="fr-zone-label">Descarte ('+V.discard.length+'/'+MAX_DISCARD+')</div>';
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
        // sort by myOrder, show drop lines while dragging
        var sorted=V.hands[i].slice().sort(function(a,b){
          return myOrder.indexOf(a.id)-myOrder.indexOf(b.id);});
        sorted.forEach(function(card){
          var isDraggingThis=isDragging&&dragId===card.id;
          var isDropTarget=isDragging&&dragDropId===card.id;
          if(isDropTarget&&dragBefore)h+='<div class="fr-drop-line"></div>';
          var clickable=canDiscard&&!isDragging;
          var cls='fr-card'+(clickable?' fr-clickable fr-discard-target':'')+(isDraggingThis?' fr-dragging':'');
          var act=clickable?'discard':null;
          h+=cardHtml(card,cls,act,'draggable="true" data-hand="1"');
          if(isDropTarget&&!dragBefore)h+='<div class="fr-drop-line"></div>';
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

  // FLIP animation (skip during drag to avoid jank)
  if(isDragging)return;
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
      sendAction({kind:'take',cid:+cid});
  }else if(act==='discard'){
    if(V&&V.turn===V.me&&V.turnPhase==='discard'&&cid!==null)
      sendAction({kind:'discard',cid:+cid});
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

// ---------- drag to reorder ----------
el('fr-game').addEventListener('dragstart',function(e){
  var card=e.target.closest('[draggable="true"][data-cid]');
  if(!card)return;
  dragId=+card.getAttribute('data-cid');
  isDragging=true;
  e.dataTransfer.effectAllowed='move';
  e.dataTransfer.setData('text/plain',String(dragId));
  tip.style.display='none';
  setTimeout(render,0);
});

el('fr-game').addEventListener('dragend',function(){
  isDragging=false;dragId=null;dragDropId=null;
  render();
});

el('fr-game').addEventListener('dragover',function(e){
  var card=e.target.closest('[draggable="true"][data-cid]');
  if(!card){e.preventDefault();return;}
  e.preventDefault();
  var overId=+card.getAttribute('data-cid');
  var rect=card.getBoundingClientRect();
  var before=e.clientX<rect.left+rect.width/2;
  if(overId!==dragDropId||before!==dragBefore){
    dragDropId=overId;dragBefore=before;
    render();
  }
});

el('fr-game').addEventListener('drop',function(e){
  e.preventDefault();
  if(dragId===null||dragDropId===null||dragId===dragDropId){
    isDragging=false;dragId=null;dragDropId=null;render();return;
  }
  var fromIdx=myOrder.indexOf(dragId);
  myOrder.splice(fromIdx,1);
  var toIdx=myOrder.indexOf(dragDropId);
  if(!dragBefore)toIdx++;
  myOrder.splice(toIdx,0,dragId);
  isDragging=false;dragId=null;dragDropId=null;
  render();
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

**Fin de partida:** La partida termina cuando hay **10 cartas** en el descarte. Todos los jugadores revelan sus manos y calculan su puntuación.

Las cartas están agrupadas por suit (color del nombre): **Land** · **Flood** · **Flame** · **Army** · **Leader** · **Beast** · **Artifact** · **Spell**. La puntuación depende de las sinergias entre cartas — consultad el reglamento para los efectos exactos.
