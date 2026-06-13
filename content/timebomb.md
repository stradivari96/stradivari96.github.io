---
title: "💣 Time Bomb"
date: 2026-06-13
searchHidden: true
robotsNoIndex: true
showToc: false
ShowReadingTime: false
ShowShareButtons: false
comments: false
---

Juego de deducción social para 4-8 jugadores.

<style>
#tb-app{--tbg:#0f1117;--tpanel:#1a1f2c;--ttext:#e8eaf0;--tmut:#9aa3b2;
  background:var(--tbg);color:var(--ttext);border-radius:12px;padding:16px;
  font-family:system-ui,sans-serif;font-size:15px;line-height:1.4}
#tb-app h3{margin:8px 0 6px;color:var(--ttext)}
#tb-app input{background:#11141a;color:var(--ttext);border:1px solid #3a4252;
  border-radius:8px;padding:8px 10px;font-size:15px;max-width:160px}
#tb-app button{background:#3b82f6;color:#fff;border:0;border-radius:8px;
  padding:8px 14px;font-size:14px;cursor:pointer;margin:2px}
#tb-app button:disabled{background:#3a4252;color:var(--tmut);cursor:not-allowed}
#tb-app .tb-row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin:8px 0}
#tb-app .tb-panel{background:var(--tpanel);border-radius:10px;padding:10px 12px;margin:8px 0}
#tb-app .tb-turn{box-shadow:0 0 0 2px #fbbf24 inset;border-radius:10px}
#tb-app .tb-code{font-size:22px;font-weight:800;letter-spacing:4px;color:#fbbf24}
#tb-app .tb-mut{color:var(--tmut);font-size:13px;margin:4px 0}
#tb-app .tb-cable{width:42px;height:58px;border-radius:8px;display:inline-flex;
  align-items:center;justify-content:center;font-size:20px;font-weight:700;
  border:2px solid rgba(0,0,0,.35);user-select:none;flex:none;
  text-shadow:0 1px 2px rgba(0,0,0,.5);margin:2px}
#tb-app .tb-cable-hidden{background:#2a3040;border:2px dashed #4a5366;color:#6a7488;font-size:16px}
#tb-app .tb-cable-safe{background:#64748b;color:#fff}
#tb-app .tb-cable-success{background:#16a34a;color:#fff}
#tb-app .tb-cable-exp{background:#dc2626;color:#fff}
#tb-app .tb-cables-row{display:flex;flex-wrap:wrap;gap:4px;margin:6px 0}
#tb-app .tb-pname{font-weight:600;margin-bottom:4px}
#tb-app .tb-identity{border-radius:10px;padding:10px 14px;margin:8px 0;font-weight:600;font-size:14px}
#tb-app .tb-agent{background:#1a2f4a;border:1px solid #3b82f6}
#tb-app .tb-terrorist{background:#3b1515;border:1px solid #dc2626}
#tb-app .tb-banner{background:#3b2f12;border:1px solid #fbbf24;border-radius:10px;
  padding:10px 14px;margin:8px 0;font-weight:600}
#tb-app .tb-log{max-height:140px;overflow-y:auto;font-size:13px;color:var(--tmut)}
#tb-app .tb-log div{padding:1px 0}
#tb-app .tb-toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);
  background:#dc2626;color:#fff;padding:10px 18px;border-radius:10px;z-index:99;
  box-shadow:0 4px 14px rgba(0,0,0,.4)}
</style>

<div id="tb-app">
  <div id="tb-setup">
    <div class="tb-row"><label>Tu nombre: <input id="tb-name" maxlength="14" placeholder="Nombre"></label></div>
    <div class="tb-row"><button id="tb-create">💣 Crear sala</button></div>
    <div class="tb-row"><input id="tb-code" maxlength="4" placeholder="CÓDIGO" style="text-transform:uppercase;width:110px"><button id="tb-join">Unirse</button></div>
    <div id="tb-setupmsg" class="tb-mut"></div>
  </div>
  <div id="tb-game" style="display:none"></div>
</div>

<script src="https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js"></script>

<script>
(function(){
'use strict';
// ---------- constantes ----------
var CABLE_COUNTS={4:{safe:15,success:4,exp:1},5:{safe:19,success:5,exp:1},
  6:{safe:23,success:6,exp:1},7:{safe:27,success:7,exp:1},8:{safe:31,success:8,exp:1}};
// pool de identidades (se barajan y se reparten np de ellas; la sobrante se retira sin revelar)
var ID_POOL={4:{a:3,t:2},5:{a:3,t:2},6:{a:4,t:2},7:{a:5,t:3},8:{a:5,t:3}};
var PREFIX='timebomb-xiang-';
var MINP=4,MAXP=8;

// ---------- estado ----------
var peer=null,isHost=false,hostConn=null,roomCode='';
var G=null,V=null,myName='';

function el(id){return document.getElementById(id);}
function esc(s){return String(s).replace(/[&<>"']/g,function(c){
  return{'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];});}
function toast(msg){
  var t=document.createElement('div');t.className='tb-toast';t.textContent=msg;
  el('tb-app').appendChild(t);setTimeout(function(){t.remove();},3500);}
function setupMsg(m){el('tb-setupmsg').textContent=m;}
function shuffle(arr){
  for(var i=arr.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));
    var tmp=arr[i];arr[i]=arr[j];arr[j]=tmp;}
  return arr;}

// ---------- lógica de juego (host) ----------
function newGame(){
  G={phase:'lobby',players:[],cables:[],hands:[],round:0,cutsThisRound:0,
     turn:0,log:[],result:null,cutSeq:0,lastCutType:null};}

function startGame(){
  var np=G.players.length;
  // Roles: se barajan todas las cartas del pool y se reparte una a cada jugador;
  // la(s) sobrante(s) quedan fuera sin revelar, creando incertidumbre en mesa.
  var pool=ID_POOL[np],roles=[];
  for(var i=0;i<pool.a;i++)roles.push('agent');
  for(var i=0;i<pool.t;i++)roles.push('terrorist');
  shuffle(roles);
  roles=roles.slice(0,np); // retira la(s) carta(s) sobrante(s) al azar
  G.players.forEach(function(p,i){p.role=roles[i];});

  // Cables
  var cc=CABLE_COUNTS[np];
  G.cables=[];var id=0;
  for(var i=0;i<cc.safe;i++)G.cables.push({id:id++,type:'safe',revealed:false});
  for(var i=0;i<cc.success;i++)G.cables.push({id:id++,type:'success',revealed:false});
  G.cables.push({id:id++,type:'exp',revealed:false});

  G.round=1;G.cutsThisRound=0;G.result=null;G.cutSeq=0;G.lastCutType=null;
  G.turn=Math.floor(Math.random()*np);
  G.log=['💣 ¡Empieza la partida! Ronda 1 · '+np+' jugadores'];
  G.phase='playing';
  dealRound();broadcast();}

function dealRound(){
  var np=G.players.length;
  var uncut=G.cables.filter(function(c){return !c.revealed;});
  shuffle(uncut);
  // uncut.length siempre es divisible por np: ronda 1→5, 2→4, 3→3, 4→2 cartas por jugador
  var perP=uncut.length/np;
  G.hands=[];
  for(var i=0;i<np;i++)
    G.hands.push(uncut.slice(i*perP,(i+1)*perP).map(function(c){return c.id;}));}

function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>60)G.log.shift();}

function applyAction(p,a){
  if(!G||G.phase!=='playing')return;
  if(G.turn!==p)return sendErr(p,'No es tu turno');
  if(a.kind==='cut'){
    var tgt=a.target;
    if(tgt===p)return sendErr(p,'No puedes cortar tus propios cables');
    if(tgt<0||tgt>=G.players.length)return;
    var uncut=G.hands[tgt].filter(function(cid){return !G.cables[cid].revealed;});
    if(!uncut.length)return sendErr(p,'Sin cables disponibles');
    var cid=uncut[Math.floor(Math.random()*uncut.length)];
    var cable=G.cables[cid];
    cable.revealed=true;
    G.cutSeq++;G.lastCutType=cable.type;G.cutsThisRound++;
    G.turn=tgt; // el cortado es el siguiente en jugar

    var label=cable.type==='safe'?'✂️ A salvo':cable.type==='success'?'⚡ ¡Éxito!':'💥 ¡EXPLOSIÓN!';
    glog(pname(p)+' → '+pname(tgt)+': '+label);

    // Comprobar victoria
    if(cable.type==='exp'){
      G.phase='ended';G.result={winner:'terrorists',reason:'explosion'};
      glog('💥 ¡La bomba explota! Los terroristas ganan.');
      return broadcast();
    }
    var sc=G.cables.filter(function(c){return c.revealed&&c.type==='success';}).length;
    if(sc>=CABLE_COUNTS[G.players.length].success){
      G.phase='ended';G.result={winner:'agents',reason:'success'};
      glog('⚡ ¡Todos los cables de éxito cortados! Los agentes ganan.');
      return broadcast();
    }
    // Fin de ronda
    if(G.cutsThisRound>=G.players.length){
      if(G.round>=4){
        G.phase='ended';G.result={winner:'terrorists',reason:'timeout'};
        glog('⏰ Fin de la última ronda. Los terroristas ganan.');
      }else{
        G.round++;G.cutsThisRound=0;
        dealRound();
        glog('🔄 Ronda '+G.round+' — los cables se barajan sin revelar.');}
    }
    broadcast();
  }
}

function sendErr(p,msg){
  if(p===0)toast(msg);
  else if(G.players[p].conn)G.players[p].conn.send({t:'error',msg:msg});}

function viewFor(i){
  var np=G.players.length;
  var sc=G.phase!=='lobby'?G.cables.filter(function(c){return c.revealed&&c.type==='success';}).length:0;
  var ts=np>=MINP?CABLE_COUNTS[np].success:0;
  return{
    phase:G.phase,me:i,myRole:G.players[i].role||null,
    turn:G.turn,round:G.round,cutsThisRound:G.cutsThisRound,cutsPerRound:np,
    result:G.result,
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    roles:G.result?G.players.map(function(p){return p.role;}):null,
    hands:G.hands.length?G.hands.map(function(hand,j){
      return hand.map(function(cid){
        var c=G.cables[cid];
        if(c.revealed)return{id:cid,revealed:true,type:c.type};
        if(j===i)return{id:cid,revealed:false,type:c.type,own:true};
        return{id:cid,revealed:false,type:null};});}):
      G.players.map(function(){return[];}),
    log:G.log.slice(-30),
    successCut:sc,totalSuccess:ts,
    cutSeq:G.cutSeq,lastCutType:G.lastCutType};}

function broadcast(){
  G.players.forEach(function(p,i){
    if(i===0)return;
    if(p.conn&&p.connected)p.conn.send({t:'state',s:viewFor(i)});});
  V=viewFor(0);render();}

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
        return;}
      if(G.players.length>=MAXP)return c.send({t:'error',msg:'Sala llena (máx 8)'});
      while(G.players.some(function(p){return p.name===name;}))name+='2';
      G.players.push({name:name,conn:c,connected:true,role:null});
      c._hIdx=G.players.length-1;
      broadcast();
    }else if(msg.t==='action'&&typeof c._hIdx==='number'){
      applyAction(c._hIdx,msg.a);}});
  c.on('close',function(){
    if(typeof c._hIdx!=='number')return;
    if(G.phase==='lobby'){G.players.splice(c._hIdx,1);
      G.players.forEach(function(p,i){if(p.conn)p.conn._hIdx=i;});}
    else{G.players[c._hIdx].connected=false;glog('🔌 '+pname(c._hIdx)+' se ha desconectado');}
    broadcast();});}

function createRoom(){
  myName=el('tb-name').value.trim();
  if(!myName)return setupMsg('Pon tu nombre primero');
  var alpha='ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
  roomCode='';for(var i=0;i<4;i++)roomCode+=alpha[Math.floor(Math.random()*alpha.length)];
  setupMsg('Creando sala...');
  peer=new Peer(PREFIX+roomCode);
  peer.on('open',function(){
    isHost=true;newGame();
    G.players.push({name:myName,conn:null,connected:true,role:null});
    history.pushState(null,'',location.pathname+'?sala='+roomCode);
    el('tb-setup').style.display='none';el('tb-game').style.display='block';
    broadcast();});
  peer.on('connection',setupHostConn);
  peer.on('error',function(e){
    if(e.type==='unavailable-id')setupMsg('Código ocupado, prueba otra vez');
    else setupMsg('Error de conexión: '+e.type);});}

function joinRoom(){
  myName=el('tb-name').value.trim();
  roomCode=el('tb-code').value.trim().toUpperCase();
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
        el('tb-setup').style.display='none';el('tb-game').style.display='block';
        render();
      }else if(msg.t==='error'){V?toast(msg.msg):setupMsg(msg.msg);}});
    hostConn.on('close',function(){toast('Se ha perdido la conexión con el anfitrión 😢');});});
  peer.on('error',function(e){
    if(e.type==='peer-unavailable')setupMsg('No existe la sala '+roomCode);
    else setupMsg('Error de conexión: '+e.type);});}

function sendAction(a){
  if(isHost)applyAction(0,a);
  else hostConn.send({t:'action',a:a});}

// ---------- sonidos ----------
var FAIL_SOUNDS=[
  'https://www.myinstants.com/media/sounds/jixaw-metal-pipe-falling-sound.mp3',
  'https://www.myinstants.com/media/sounds/fahhhh-6.mp3',
  'https://www.myinstants.com/media/sounds/wrong_5.mp3',
  'https://www.myinstants.com/media/sounds/oh-no.mp3',
  'https://www.myinstants.com/media/sounds/movie_1.mp3',
  'https://www.myinstants.com/media/sounds/el-diablo_MicOe0x.mp3'];
var SUCCESS_SOUNDS=[
  'https://www.myinstants.com/media/sounds/answer-correct.mp3',
  'https://www.myinstants.com/media/sounds/kids-saying-yay-sound-effect_3.mp3',
  'https://www.myinstants.com/media/sounds/anime-wow-sound-effect.mp3',
  'https://www.myinstants.com/media/sounds/yes-yes-yes-yes-yes.mp3',
  'https://www.myinstants.com/media/sounds/yippeeeeeeeeeeeeee.mp3'];
function playAt(arr,idx){if(!arr.length)return;
  try{var a=new Audio(arr[idx%arr.length]);a.volume=0.3;a.play().catch(function(){});}catch(e){}}
var lastCutSeq=-1;
function checkSounds(){
  if(!V||typeof V.cutSeq!=='number')return;
  if(lastCutSeq>=0&&V.cutSeq>lastCutSeq){
    if(V.lastCutType==='exp')playAt(FAIL_SOUNDS,Math.floor(Math.random()*FAIL_SOUNDS.length));
    else if(V.lastCutType==='success')playAt(SUCCESS_SOUNDS,Math.floor(Math.random()*SUCCESS_SOUNDS.length));
  }
  lastCutSeq=V.cutSeq;}

// ---------- render ----------
function cableHtml(c){
  if(!c.revealed&&!c.own)return '<div class="tb-cable tb-cable-hidden">?</div>';
  var cls=c.type==='safe'?'tb-cable-safe':c.type==='success'?'tb-cable-success':'tb-cable-exp';
  var lbl=c.type==='safe'?'✓':c.type==='success'?'⚡':'💥';
  var title=c.type==='safe'?'A salvo':c.type==='success'?'Exito':'Explosion';
  var extra=(!c.revealed&&c.own)?'opacity:.45':'';
  return '<div class="tb-cable '+cls+'" style="'+extra+'" title="'+title+'">'+lbl+'</div>';}

function render(){
  if(!V)return;
  var np=V.names.length;
  var h='';
  h+='<div class="tb-row">Sala: <span class="tb-code">'+roomCode+'</span> '+
     '<button data-act="copy">📋 Copiar enlace</button></div>';

  if(V.phase==='lobby'){
    h+='<div class="tb-panel"><h3>Jugadores ('+np+'/8, mín 4)</h3>';
    V.names.forEach(function(n,i){
      h+='<div>'+esc(n)+(i===0?' 👑':'')+(i===V.me?' (tú)':'')+'</div>';});
    h+='</div>';
    if(V.me===0)
      h+='<button data-act="start"'+(np<MINP?' disabled':'')+'>💣 Empezar ('+
         (np<MINP?'mínimo 4':np+' jugadores')+')</button>';
    else h+='<div class="tb-mut">Esperando a que '+esc(V.names[0])+' empiece...</div>';
  }else{
    // Banner de identidad
    if(V.myRole){
      var rc=V.myRole==='agent'?'tb-agent':'tb-terrorist';
      var re=V.myRole==='agent'?'🕵️':'💣';
      var rn=V.myRole==='agent'?'Agente':'Terrorista';
      var hint=V.myRole==='agent'
        ?' — Cortad todos los cables ⚡ antes de que acaben las rondas'
        :' — Haz que corten la explosión 💥 o sobrevive hasta el final';
      h+='<div class="tb-identity '+rc+'">'+re+' Eres <b>'+rn+'</b>'+hint+'</div>';
    }
    // Banner de fin
    if(V.phase==='ended'){
      var r=V.result;
      var endMsg=r.winner==='agents'?'⚡ ¡Los agentes ganan! Todos los éxitos cortados.':
                 r.reason==='explosion'?'💥 ¡BOOM! Los terroristas ganan.':
                 '⏰ Se acaban las rondas. Los terroristas ganan.';
      h+='<div class="tb-banner">'+endMsg;
      if(V.roles){
        h+='<div style="margin-top:8px;font-size:13px">';
        V.names.forEach(function(n,i){
          var badge=V.roles[i]==='agent'?'🕵️ Agente':'💣 Terrorista';
          h+='<span style="margin-right:12px">'+esc(n)+': '+badge+'</span>';});
        h+='</div>';}
      if(V.me===0)h+=' <button data-act="start" style="margin-top:6px">🔄 Otra partida</button>';
      h+='</div>';
    }
    // Estado
    h+='<div class="tb-panel"><div class="tb-row">⚡ Éxitos: <b>'+V.successCut+'/'+V.totalSuccess+
       '</b> &nbsp; 🔄 Ronda: <b>'+V.round+'/4</b>'+
       ' &nbsp; ✂️ Cortes: <b>'+V.cutsThisRound+'/'+V.cutsPerRound+'</b></div></div>';
    // Manos de los jugadores
    V.names.forEach(function(n,i){
      var isTurn=V.phase==='playing'&&V.turn===i;
      var canCut=V.phase==='playing'&&V.turn===V.me&&i!==V.me;
      var hand=V.hands[i]||[];
      var hasUncut=hand.some(function(c){return !c.revealed;});
      h+='<div class="tb-panel'+(isTurn?' tb-turn':'')+'">'+
         '<div class="tb-pname">'+(isTurn?'▶ ':'')+esc(n)+(i===V.me?' (tú)':'')+
         (!V.connected[i]?' 🔌❌':'')+'</div>'+
         '<div class="tb-cables-row">';
      hand.forEach(function(c){h+=cableHtml(c);});
      if(!hand.length)h+='<span class="tb-mut">Sin cables esta ronda</span>';
      h+='</div>';
      if(canCut&&hasUncut)
        h+='<button data-act="cut" data-target="'+i+'">✂️ Cortar cable aleatorio</button>';
      h+='</div>';
    });
    // Indicación de turno
    if(V.phase==='playing')
      h+='<div class="tb-mut">'+(V.turn===V.me
        ?'✂️ Tu turno: elige a quién cortarle un cable'
        :'Turno de '+esc(V.names[V.turn])+'...')+'</div>';
    // Log
    h+='<div class="tb-panel tb-log">'+V.log.slice().reverse().map(function(l){
      return'<div>'+esc(l)+'</div>';}).join('')+'</div>';
  }
  el('tb-game').innerHTML=h;
  checkSounds();}

// ---------- eventos ----------
el('tb-create').addEventListener('click',createRoom);
el('tb-join').addEventListener('click',joinRoom);
el('tb-game').addEventListener('click',function(e){
  var b=e.target.closest('[data-act]');if(!b)return;
  var act=b.getAttribute('data-act');
  if(act==='copy'){
    navigator.clipboard.writeText(location.origin+location.pathname+'?sala='+roomCode)
      .then(function(){toast('Enlace copiado 📋');});
  }else if(act==='start'&&isHost){startGame();}
  else if(act==='cut'){sendAction({kind:'cut',target:+b.getAttribute('data-target')});}
});
// Código por URL: ?sala=ABCD
var m=location.search.match(/sala=([A-Za-z0-9]{4})/);
if(m)el('tb-code').value=m[1].toUpperCase();
})();
</script>

## ¿Cómo se juega?

Time Bomb es un juego de **deducción social**: los **agentes** intentan cortar todos los cables de éxito, los **terroristas** intentan sabotearlos.

**Identidades:** Cada jugador recibe en secreto una carta de identidad (🕵️ Agente o 💣 Terrorista). El número de terroristas en mesa según jugadores:
- 4 jugadores → 1 o 2 terroristas
- 5 jugadores → 2 terroristas
- 6 jugadores → 2 terroristas
- 7 jugadores → 2 o 3 terroristas
- 8 jugadores → 3 terroristas

**Cables:** Cada jugador tiene varios cables boca abajo frente a él:
- ✓ **A salvo** — no pasa nada
- ⚡ **Éxito** — los agentes avanzan hacia la victoria
- 💥 **Explosión** — ¡solo hay una! Si se corta, los terroristas ganan de inmediato

**Cada ronda** los cables se redistribuyen aleatoriamente en silencio. Cada jugador ve sus propios cables pero no los de los demás. En tu turno debes **cortar un cable de otro jugador**: se revela uno aleatorio de entre sus cables sin cortar. El jugador cuyo cable se acaba de cortar es el siguiente en actuar (esto se mantiene entre rondas).

La ronda termina cuando se han hecho tantos cortes como jugadores. Hay un máximo de **4 rondas**.

**Condiciones de victoria:**
- 🕵️ **Agentes ganan** si se cortan todos los cables de éxito ⚡ antes de que acaben las rondas
- 💣 **Terroristas ganan** si se corta la explosión 💥, o si terminan las 4 rondas sin que los agentes lo hayan conseguido

La parte táctica: los jugadores pueden hablar libremente, pero los terroristas pueden mentir. ¿A quién le cortas el cable?
