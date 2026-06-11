---
title: "🎆 Hanabi"
date: 2026-06-11
searchHidden: true
robotsNoIndex: true
showToc: false
ShowReadingTime: false
ShowShareButtons: false
comments: false
---

Juego cooperativo de cartas para 2-5 jugadores. Uno crea la sala y comparte el código (o el enlace) con los demás. La partida vive en el navegador del anfitrión: si lo cierra, se acaba la fiesta.

<style>
#hanabi-app{--hbg:#1b1f27;--hpanel:#252b36;--htext:#e8eaf0;--hmut:#9aa3b2;
  background:var(--hbg);color:var(--htext);border-radius:12px;padding:16px;
  font-family:system-ui,sans-serif;font-size:15px;line-height:1.4}
#hanabi-app h3{margin:10px 0 6px;color:var(--htext)}
#hanabi-app input{background:#11141a;color:var(--htext);border:1px solid #3a4252;
  border-radius:8px;padding:8px 10px;font-size:15px;max-width:160px}
#hanabi-app button{background:#3b82f6;color:#fff;border:0;border-radius:8px;
  padding:8px 14px;font-size:14px;cursor:pointer;margin:2px}
#hanabi-app button:disabled{background:#3a4252;color:var(--hmut);cursor:not-allowed}
#hanabi-app button.h-red{background:#dc2626}
#hanabi-app .h-row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin:8px 0}
#hanabi-app .h-panel{background:var(--hpanel);border-radius:10px;padding:10px 12px;margin:8px 0}
#hanabi-app .hcard{width:48px;height:66px;border-radius:8px;display:inline-flex;
  align-items:center;justify-content:center;font-size:26px;font-weight:700;
  cursor:pointer;border:2px solid rgba(0,0,0,.35);user-select:none;color:#fff;
  text-shadow:0 1px 2px rgba(0,0,0,.5);position:relative;flex:none}
#hanabi-app .hcard.h-sel{outline:3px solid #fff;outline-offset:2px}
#hanabi-app .hcard.h-noclick{cursor:default}
#hanabi-app .h-cardwrap{display:flex;flex-direction:column;align-items:center;gap:3px}
#hanabi-app .h-new{animation:h-deal .45s ease}
@keyframes h-deal{from{transform:translateX(-50px) scale(.5);opacity:0}to{transform:none;opacity:1}}
#hanabi-app .h-fly{position:fixed;z-index:98;margin:0;transition:left .55s ease,top .55s ease,opacity .55s ease,transform .55s ease}
#hanabi-app .h-mini{font-size:12px;color:var(--hmut);letter-spacing:1px}
#hanabi-app .h-pile{width:44px;height:60px;border-radius:8px;display:inline-flex;
  align-items:center;justify-content:center;font-size:22px;font-weight:700;color:#fff;
  opacity:.95;border:2px dashed rgba(255,255,255,.25)}
#hanabi-app .h-turn{box-shadow:0 0 0 2px #fbbf24 inset;border-radius:10px}
#hanabi-app .h-name{font-weight:600;margin-bottom:4px}
#hanabi-app .h-log{max-height:140px;overflow-y:auto;font-size:13px;color:var(--hmut)}
#hanabi-app .h-log div{padding:1px 0}
#hanabi-app .h-banner{background:#3b2f12;border:1px solid #fbbf24;border-radius:10px;
  padding:10px 14px;margin:8px 0;font-weight:600}
#hanabi-app .h-toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);
  background:#dc2626;color:#fff;padding:10px 18px;border-radius:10px;z-index:99;
  box-shadow:0 4px 14px rgba(0,0,0,.4)}
#hanabi-app .h-code{font-size:22px;font-weight:800;letter-spacing:4px;color:#fbbf24}
#hanabi-app .h-mut{color:var(--hmut);font-size:13px}
</style>
<div id="hanabi-app">
  <div id="h-setup">
    <div class="h-row"><label>Tu nombre: <input id="h-name" maxlength="14" placeholder="Xiang"></label></div>
    <div class="h-row"><button id="h-create">🎇 Crear sala</button></div>
    <div class="h-row"><input id="h-code" maxlength="4" placeholder="CÓDIGO" style="text-transform:uppercase;width:110px"><button id="h-join">Unirse</button></div>
    <div id="h-setupmsg" class="h-mut"></div>
  </div>
  <div id="h-game" style="display:none"></div>
</div>

<script src="https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js"></script>

<script>
(function(){
'use strict';
// ---------- constantes ----------
var COLORS=['R','G','B','Y','W'];
var CINFO={R:{name:'Rojo',bg:'#dc2626',em:'🔴'},G:{name:'Verde',bg:'#16a34a',em:'🟢'},
  B:{name:'Azul',bg:'#2563eb',em:'🔵'},Y:{name:'Amarillo',bg:'#ca8a04',em:'🟡'},
  W:{name:'Blanco',bg:'#64748b',em:'⚪'}};
var PREFIX='hanabi-xiang-';
var MAXP=5;

// ---------- estado ----------
var peer=null,isHost=false,hostConn=null,roomCode='';
var G=null;   // estado completo (solo host)
var V=null;   // vista personalizada (lo que se renderiza)
var sel=null; // carta seleccionada {pl, idx}
var myName='';

function el(id){return document.getElementById(id);}
function esc(s){return String(s).replace(/[&<>"']/g,function(c){
  return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];});}
function toast(msg){
  var t=document.createElement('div');t.className='h-toast';t.textContent=msg;
  el('hanabi-app').appendChild(t);setTimeout(function(){t.remove();},3500);
}
function cardTxt(c){return CINFO[c.c].em+c.n;}

// ---------- lógica de juego (host) ----------
function mkDeck(){
  var d=[],counts=[[1,3],[2,2],[3,2],[4,2],[5,1]];
  COLORS.forEach(function(c){counts.forEach(function(p){
    for(var i=0;i<p[1];i++)d.push({c:c,n:p[0],kc:false,kn:false});});});
  for(var i=d.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));
    var t=d[i];d[i]=d[j];d[j]=t;}
  // ids tras barajar: que el id no delate la carta
  d.forEach(function(c,k){c.id=k;});
  return d;
}
function newGame(){
  G={phase:'lobby',players:[],hands:[],deck:[],piles:{},discard:[],
     hints:8,fuses:3,turn:0,turnsLeft:null,justEmptied:false,log:[],result:null,
     lastMove:null,moveSeq:0};
}
function startGame(){
  G.deck=mkDeck();G.discard=[];G.hints=8;G.fuses=3;G.turnsLeft=null;
  G.justEmptied=false;G.result=null;G.turn=0;G.log=['🎆 Empieza la partida'];
  G.lastMove=null;G.moveSeq=0;
  COLORS.forEach(function(c){G.piles[c]=0;});
  var hs=G.players.length<=3?5:4;
  G.hands=G.players.map(function(){return [];});
  for(var k=0;k<hs;k++)G.players.forEach(function(_,i){G.hands[i].push(G.deck.pop());});
  G.phase='playing';
  broadcast();
}
function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>60)G.log.shift();}
function draw(p){
  if(!G.deck.length)return;
  G.hands[p].unshift(G.deck.pop()); // la carta nueva entra por la izquierda
  if(!G.deck.length){G.turnsLeft=G.players.length;G.justEmptied=true;
    glog('🂠 Se ha robado la última carta: una ronda final');}
}
function finish(reason){
  G.phase='ended';
  var score=COLORS.reduce(function(s,c){return s+G.piles[c];},0);
  if(reason==='boom')score=0;
  G.result={reason:reason,score:score};
  glog(reason==='boom'?'💥 ¡Tercer fallo! Los fuegos artificiales explotan':
       reason==='win'?'🎆 ¡Espectáculo perfecto!':'🏁 Fin de la partida');
}
function sendErr(p,msg){
  if(p===0)toast(msg);
  else if(G.players[p].conn)G.players[p].conn.send({t:'error',msg:msg});
}
function endTurn(){
  if(G.turnsLeft!==null&&!G.justEmptied){
    G.turnsLeft--;
    if(G.turnsLeft<=0){finish('deck');return;}
  }
  G.justEmptied=false;
  G.turn=(G.turn+1)%G.players.length;
}
function applyAction(p,a){
  if(!G||G.phase!=='playing')return;
  if(G.turn!==p)return sendErr(p,'No es tu turno');
  var hand=G.hands[p],card;
  G.lastMove=null;
  if(a.kind==='play'){
    if(!hand[a.idx])return;
    card=hand.splice(a.idx,1)[0];
    G.moveSeq++;
    G.lastMove={cid:card.id,c:card.c,n:card.n,kind:G.piles[card.c]===card.n-1?'play':'fail'};
    if(G.piles[card.c]===card.n-1){
      G.piles[card.c]=card.n;
      if(card.n===5&&G.hints<8)G.hints++;
      glog(pname(p)+' juega '+cardTxt(card)+' ✔');
      if(COLORS.every(function(c){return G.piles[c]===5;})){finish('win');return broadcast();}
    }else{
      G.fuses--;G.discard.push(card);
      glog('💥 '+pname(p)+' intenta jugar '+cardTxt(card)+' y falla ('+G.fuses+' mechas)');
      if(G.fuses===0){finish('boom');return broadcast();}
    }
    draw(p);
  }else if(a.kind==='discard'){
    if(G.hints>=8)return sendErr(p,'Ya tenéis las 8 fichas de pista');
    if(!hand[a.idx])return;
    card=hand.splice(a.idx,1)[0];
    G.moveSeq++;
    G.lastMove={cid:card.id,c:card.c,n:card.n,kind:'discard'};
    G.discard.push(card);G.hints++;
    glog(pname(p)+' descarta '+cardTxt(card));
    draw(p);
  }else if(a.kind==='hint'){
    if(G.hints<=0)return sendErr(p,'No quedan fichas de pista');
    if(a.target===p||!G.hands[a.target])return;
    var matches=G.hands[a.target].filter(function(c){
      return a.htype==='color'?c.c===a.value:c.n===a.value;});
    if(!matches.length)return sendErr(p,'La pista debe señalar al menos una carta');
    matches.forEach(function(c){if(a.htype==='color')c.kc=true;else c.kn=true;});
    G.hints--;
    var what=a.htype==='color'?CINFO[a.value].em+' '+CINFO[a.value].name:'número '+a.value;
    glog(pname(p)+' → '+pname(a.target)+': '+what+' ('+matches.length+' carta'+(matches.length>1?'s':'')+')');
  }else return;
  endTurn();
  broadcast();
}
function viewFor(i){
  return {
    phase:G.phase,me:i,turn:G.turn,hints:G.hints,fuses:G.fuses,
    deckCount:G.deck.length,turnsLeft:G.turnsLeft,result:G.result,
    lastMove:G.lastMove,moveSeq:G.moveSeq,
    piles:JSON.parse(JSON.stringify(G.piles)),
    discard:G.discard.map(function(c){return {c:c.c,n:c.n};}),
    log:G.log.slice(-30),
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    hands:G.hands.map(function(h,j){return h.map(function(c){
      if(j===i)return {id:c.id,c:c.kc?c.c:null,n:c.kn?c.n:null,kc:c.kc,kn:c.kn};
      return {id:c.id,c:c.c,n:c.n,kc:c.kc,kn:c.kn};});})
  };
}
function broadcast(){
  G.players.forEach(function(p,i){
    if(i===0)return;
    if(p.conn&&p.connected)p.conn.send({t:'state',s:viewFor(i)});
  });
  V=viewFor(0);sel=null;render();
}

// ---------- red ----------
function setupHostConn(c){
  c.on('data',function(msg){
    if(!msg||typeof msg!=='object')return;
    if(msg.t==='join'){
      var name=String(msg.name||'???').slice(0,14);
      if(G.phase!=='lobby'){
        var old=G.players.findIndex(function(p){return !p.connected&&p.name===name;});
        if(old>=0){G.players[old].conn=c;G.players[old].connected=true;
          c._hIdx=old;glog('🔌 '+name+' ha vuelto');broadcast();}
        else c.send({t:'error',msg:'Partida en curso, no puedes entrar'});
        return;
      }
      if(G.players.length>=MAXP)return c.send({t:'error',msg:'Sala llena (máx 5)'});
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
  myName=el('h-name').value.trim();
  if(!myName)return setupMsg('Pon tu nombre primero');
  var alpha='ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
  roomCode='';for(var i=0;i<4;i++)roomCode+=alpha[Math.floor(Math.random()*alpha.length)];
  setupMsg('Creando sala...');
  peer=new Peer(PREFIX+roomCode);
  peer.on('open',function(){
    isHost=true;newGame();
    G.players.push({name:myName,conn:null,connected:true});
    el('h-setup').style.display='none';el('h-game').style.display='block';
    broadcast();
  });
  peer.on('connection',setupHostConn);
  peer.on('error',function(e){
    if(e.type==='unavailable-id')setupMsg('Código ocupado, prueba otra vez');
    else setupMsg('Error de conexión: '+e.type);
  });
}
function joinRoom(){
  myName=el('h-name').value.trim();
  roomCode=el('h-code').value.trim().toUpperCase();
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
        V=msg.s;sel=null;
        el('h-setup').style.display='none';el('h-game').style.display='block';
        render();
      }else if(msg.t==='error'){V?toast(msg.msg):setupMsg(msg.msg);}
    });
    hostConn.on('close',function(){toast('Se ha perdido la conexión con el anfitrión 😢');});
  });
  peer.on('error',function(e){
    if(e.type==='peer-unavailable')setupMsg('No existe la sala '+roomCode);
    else setupMsg('Error de conexión: '+e.type);
  });
}
function sendAction(a){
  if(isHost)applyAction(0,a);
  else hostConn.send({t:'action',a:a});
  sel=null;render();
}
function setupMsg(m){el('h-setupmsg').textContent=m;}

// ---------- render ----------
function cardHtml(card,pl,idx,clickable){
  var bg=card.c?CINFO[card.c].bg:'#3a4252';
  var label=card.n!==null&&card.n!==undefined?card.n:'?';
  var cls='hcard'+(sel&&sel.pl===pl&&sel.idx===idx?' h-sel':'')+(clickable?'':' h-noclick');
  var mini=(card.kc&&card.c?CINFO[card.c].em:'·')+' '+(card.kn?card.n:'·');
  var full=pl!==V.me; // carta ajena: enseño también lo que sabe el dueño
  return '<div class="h-cardwrap" data-cid="'+card.id+'"><div class="'+cls+'" data-pl="'+pl+'" data-idx="'+idx+
    '" style="background:'+bg+'">'+label+'</div>'+
    (full?'<div class="h-mini" title="Lo que sabe">'+mini+'</div>':'')+'</div>';
}
var animSeq=-1;
function flyCard(rect,move){
  var d=document.createElement('div');
  d.className='hcard h-noclick h-fly';
  d.style.background=CINFO[move.c].bg;
  d.style.left=rect.left+'px';d.style.top=rect.top+'px';
  d.textContent=move.n;
  document.body.appendChild(d);
  var target=move.kind==='play'?
    document.querySelector('#h-game [data-pile="'+move.c+'"]'):el('h-discard');
  var tr=target?target.getBoundingClientRect():null;
  requestAnimationFrame(function(){requestAnimationFrame(function(){
    if(tr){d.style.left=tr.left+'px';d.style.top=tr.top+'px';}
    d.style.opacity='0';
    if(move.kind==='fail')d.style.transform='rotate(35deg) scale(1.2)';
  });});
  setTimeout(function(){d.remove();},650);
}
function animate(prev){
  // carta jugada/descartada: vuela desde su sitio hasta la pila o los descartes
  if(V.lastMove&&V.moveSeq!==animSeq&&prev[V.lastMove.cid])flyCard(prev[V.lastMove.cid],V.lastMove);
  animSeq=V.moveSeq;
  // FLIP: las cartas que siguen en mesa se deslizan de su posición vieja a la nueva
  var els=el('h-game').querySelectorAll('[data-cid]');
  for(var i=0;i<els.length;i++){
    var e=els[i],r0=prev[e.getAttribute('data-cid')];
    if(!r0){e.firstChild.classList.add('h-new');continue;}
    var r1=e.getBoundingClientRect(),dx=r0.left-r1.left,dy=r0.top-r1.top;
    if(dx||dy){
      e.style.transition='none';e.style.transform='translate('+dx+'px,'+dy+'px)';
      e.getBoundingClientRect();
      e.style.transition='transform .4s ease';e.style.transform='';
    }
  }
}
function render(){
  if(!V)return;
  var prev={},olds=el('h-game').querySelectorAll('[data-cid]');
  for(var i=0;i<olds.length;i++)
    prev[olds[i].getAttribute('data-cid')]=olds[i].getBoundingClientRect();
  var h='';
  var shareUrl=location.origin+location.pathname+'?sala='+roomCode;
  h+='<div class="h-row">Sala: <span class="h-code">'+roomCode+'</span> '+
     '<button data-act="copy">📋 Copiar enlace</button></div>';
  if(V.phase==='lobby'){
    h+='<div class="h-panel"><h3>Jugadores ('+V.names.length+'/5)</h3>';
    V.names.forEach(function(n,i){
      h+='<div>'+esc(n)+(i===0?' 👑':'')+(i===V.me?' (tú)':'')+'</div>';});
    h+='</div>';
    if(V.me===0)h+='<button data-act="start" '+(V.names.length<2?'disabled':'')+
      '>🚀 Empezar ('+(V.names.length<2?'mínimo 2':V.names.length+' jugadores')+')</button>';
    else h+='<div class="h-mut">Esperando a que '+esc(V.names[0])+' empiece...</div>';
  }else{
    if(V.phase==='ended'){
      var r=V.result,msg;
      if(r.reason==='boom')msg='💥 ¡Tres fallos! Puntuación: 0';
      else{
        var adj=r.score===25?'¡LEGENDARIA! 🎆':r.score>=21?'¡Memorable!':
          r.score>=16?'¡Muy buena!':r.score>=11?'Honorable':r.score>=6?'Mediocre...':'Horrible 💀';
        msg='🏁 Puntuación: '+r.score+'/25 — '+adj;
      }
      h+='<div class="h-banner">'+msg+(V.me===0?' <button data-act="start">🔄 Otra partida</button>':'')+'</div>';
    }
    // tablero central
    h+='<div class="h-panel"><div class="h-row">';
    COLORS.forEach(function(c){
      h+='<div class="h-pile" data-pile="'+c+'" style="background:'+CINFO[c].bg+(V.piles[c]===0?';opacity:.3':'')+
         '">'+(V.piles[c]||'·')+'</div>';});
    h+='</div><div class="h-row">💡 Pistas: <b>'+V.hints+'</b>/8 &nbsp; 🧨 Mechas: <b>'+
       V.fuses+'</b>/3 &nbsp; 🂠 Mazo: <b>'+V.deckCount+'</b>'+
       (V.turnsLeft!==null?' &nbsp; ⏳ Turnos finales: <b>'+V.turnsLeft+'</b>':'')+'</div>';
    var dByColor=COLORS.map(function(c){
      var ns=V.discard.filter(function(d){return d.c===c;}).map(function(d){return d.n;}).sort();
      return ns.length?CINFO[c].em+ns.join(''):'';
    }).filter(Boolean).join(' &nbsp;');
    h+='<div class="h-mut" id="h-discard">Descartes: '+(dByColor||'—')+'</div>';
    // en peligro: cartas aún necesarias con una sola copia viva (por descartes/fallos)
    var TOTALS={1:3,2:2,3:2,4:2,5:1},danger=[],dead=[];
    COLORS.forEach(function(c){
      for(var n=V.piles[c]+1;n<=5;n++){
        var disc=V.discard.filter(function(d){return d.c===c&&d.n===n;}).length;
        var rem=TOTALS[n]-disc;
        if(rem===0){dead.push(CINFO[c].em+n+(n<5?'↑':''));break;}
        if(rem===1&&disc>0)danger.push(CINFO[c].em+n);
      }
    });
    h+='<div class="h-mut">⚠️ En peligro (última copia): '+(danger.join(' &nbsp;')||'—')+
       (dead.length?' &nbsp;&nbsp; 💀 Perdidas: '+dead.join(' &nbsp;'):'')+'</div></div>';
    // manos
    V.names.forEach(function(n,i){
      var turn=V.phase==='playing'&&V.turn===i;
      h+='<div class="h-panel'+(turn?' h-turn':'')+'"><div class="h-name">'+
         (turn?'▶ ':'')+esc(n)+(i===V.me?' (tú)':'')+(V.connected[i]?'':' 🔌❌')+'</div><div class="h-row">';
      V.hands[i].forEach(function(c,j){h+=cardHtml(c,i,j,V.phase==='playing'&&V.turn===V.me);});
      h+='</div></div>';
    });
    // barra de acciones
    if(V.phase==='playing'&&V.turn===V.me&&sel){
      h+='<div class="h-panel"><b>Carta '+(sel.idx+1)+' de '+esc(V.names[sel.pl])+':</b> ';
      if(sel.pl===V.me){
        h+='<button data-act="play">🎇 Jugar</button>'+
           '<button data-act="discard" class="h-red" '+(V.hints>=8?'disabled':'')+'>🗑 Descartar'+
           (V.hints>=8?' (8 pistas)':'')+'</button>';
      }else{
        var sc=V.hands[sel.pl][sel.idx];
        h+='<button data-act="hintc" '+(V.hints<1?'disabled':'')+'>Pista: '+
           CINFO[sc.c].em+' '+CINFO[sc.c].name+'</button>'+
           '<button data-act="hintn" '+(V.hints<1?'disabled':'')+'>Pista: número '+sc.n+'</button>';
      }
      h+='</div>';
    }else if(V.phase==='playing'){
      h+='<div class="h-mut">'+(V.turn===V.me?
        '✨ Tu turno: toca una carta tuya (jugar/descartar) o una ajena (dar pista)':
        'Turno de '+esc(V.names[V.turn])+'...')+'</div>';
    }
    h+='<div class="h-panel h-log">'+V.log.slice().reverse().map(function(l){
      return '<div>'+esc(l)+'</div>';}).join('')+'</div>';
  }
  el('h-game').innerHTML=h;
  if(V.phase==='playing'||V.phase==='ended')animate(prev);
}

// ---------- eventos ----------
el('h-create').addEventListener('click',createRoom);
el('h-join').addEventListener('click',joinRoom);
el('h-game').addEventListener('click',function(e){
  var b=e.target.closest('[data-act]');
  if(b){
    var act=b.getAttribute('data-act');
    if(act==='copy'){
      navigator.clipboard.writeText(location.origin+location.pathname+'?sala='+roomCode)
        .then(function(){toast('Enlace copiado 📋');});
    }
    else if(act==='start'&&isHost)startGame();
    else if(act==='play')sendAction({kind:'play',idx:sel.idx});
    else if(act==='discard')sendAction({kind:'discard',idx:sel.idx});
    else if(act==='hintc')sendAction({kind:'hint',target:sel.pl,htype:'color',value:V.hands[sel.pl][sel.idx].c});
    else if(act==='hintn')sendAction({kind:'hint',target:sel.pl,htype:'number',value:V.hands[sel.pl][sel.idx].n});
    return;
  }
  var c=e.target.closest('[data-pl]');
  if(c&&V&&V.phase==='playing'&&V.turn===V.me){
    var pl=+c.getAttribute('data-pl'),idx=+c.getAttribute('data-idx');
    sel=(sel&&sel.pl===pl&&sel.idx===idx)?null:{pl:pl,idx:idx};
    render();
  }
});
// código por URL: ?sala=ABCD
var m=location.search.match(/sala=([A-Za-z0-9]{4})/);
if(m)el('h-code').value=m[1].toUpperCase();
})();
</script>

## ¿Cómo se juega?

Hanabi es **cooperativo**: montáis juntos 5 fuegos artificiales (del 1 al 5 en cada color). El truco: **ves las cartas de los demás, pero no las tuyas**.

En tu turno haces **una** de estas tres cosas:

1. **Dar una pista** (gasta 1 ficha 💡): toca una carta de otro jugador y elige decirle su color o su número. La pista marca *todas* sus cartas de ese color/número.
2. **Descartar** una carta tuya: recuperáis 1 ficha 💡 y robas otra.
3. **Jugar** una carta tuya: si es la siguiente de su pila, ¡fuego artificial! (jugar un 5 devuelve una ficha 💡). Si no encaja, perdéis una mecha 🧨. A la tercera mecha, todo explota.

La partida acaba con las 3 mechas gastadas (derrota), los 5 fuegos completos (25 puntos, perfecto) o cuando se agota el mazo (una última ronda y se cuentan puntos).

Debajo de cada carta ajena se muestra en pequeño **lo que su dueño sabe** de ella (`🔴 ·` = sabe el color, `· 3` = sabe el número).

La línea **⚠️ En peligro** avisa de las cartas necesarias de las que solo queda una copia viva (si se descarta, ese fuego ya no se puede completar). Los 5 no se listan porque siempre son copia única. **💀 Perdidas** marca desde qué número un color ya es imposible (`🔴4↑` = el rojo ya solo puede llegar a 3).
