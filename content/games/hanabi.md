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
#hanabi-app .h-hintglow{animation:h-halo 1.6s ease-out}
@keyframes h-halo{0%,100%{box-shadow:0 0 0 0 rgba(251,191,36,0)}
  25%,75%{box-shadow:0 0 16px 7px rgba(251,191,36,.95)}
  50%{box-shadow:0 0 6px 2px rgba(251,191,36,.4)}}
#hanabi-app .h-fan{display:flex;align-items:center;flex-wrap:wrap;margin-left:14px;
  padding-left:10px;border-left:1px solid #3a4252;min-height:60px}
#hanabi-app .h-disc{width:32px;height:44px;border-radius:6px;display:inline-flex;
  align-items:center;justify-content:center;font-size:16px;font-weight:700;color:#fff;
  border:1.5px solid rgba(0,0,0,.35);margin-left:-13px;flex:none;
  text-shadow:0 1px 2px rgba(0,0,0,.5)}
#hanabi-app .h-disc:first-child{margin-left:0}
#hanabi-app .h-discempty{background:transparent;border:1.5px dashed #4a5366;color:#6a7488;font-size:18px}
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
#hanabi-app .h-noob{display:flex;flex-direction:column;align-items:center;gap:2px;margin-top:3px}
#hanabi-app .h-noob-colors{display:flex;gap:3px}
#hanabi-app .h-noob-dot{width:9px;height:9px;border-radius:50%;flex:none}
#hanabi-app .h-noob-nums{font-size:11px;color:var(--hmut);letter-spacing:1px;line-height:1}
#hanabi-app button.h-noob-toggle{background:#2d3748;font-size:13px;padding:5px 10px}
#hanabi-app button.h-noob-toggle.h-active{background:#4a3728;border:1px solid #f59e0b}
#hanabi-app .h-hinthover{box-shadow:0 0 16px 6px rgba(251,191,36,.8);transition:box-shadow .15s}
#hanabi-app .h-intent{position:absolute;top:-9px;right:-7px;font-size:13px;line-height:1;pointer-events:none}
</style>
<div id="hanabi-app">
  <div id="h-setup">
    <div class="h-row"><label>Tu nombre: <input id="h-name" maxlength="14" placeholder="Nombre"></label></div>
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
var TOTALS={1:3,2:2,3:2,4:2,5:1};

// ---------- estado ----------
var peer=null,isHost=false,hostConn=null,roomCode='';
var G=null;   // estado completo (solo host)
var V=null;   // vista personalizada (lo que se renderiza)
var sel=null; // carta seleccionada {pl, idx}
var myName='';
var noobMode=false;

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
    for(var i=0;i<p[1];i++)d.push({c:c,n:p[0],kc:false,kn:false,notC:[],notN:[]});});});
  for(var i=d.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));
    var t=d[i];d[i]=d[j];d[j]=t;}
  // ids tras barajar: que el id no delate la carta
  d.forEach(function(c,k){c.id=k;});
  return d;
}
function newGame(){
  G={phase:'lobby',players:[],hands:[],deck:[],piles:{},pileTops:{},discard:[],
     hints:8,fuses:3,turn:0,turnsLeft:null,justEmptied:false,log:[],result:null,
     failSeq:0,failIdx:0,successSeq:0,successIdx:0,hintSeq:0,hintCids:[],intentions:{}};
}
function startGame(){
  G.deck=mkDeck();G.discard=[];G.hints=8;G.fuses=3;G.turnsLeft=null;
  G.justEmptied=false;G.result=null;G.turn=0;G.log=['🎆 Empieza la partida'];
  G.pileTops={};G.failSeq=0;G.failIdx=0;G.successSeq=0;G.successIdx=0;G.hintSeq=0;G.hintCids=[];G.intentions={};
  COLORS.forEach(function(c){G.piles[c]=0;});
  var hs=G.players.length<=3?5:4;
  G.hands=G.players.map(function(){return [];});
  for(var k=0;k<hs;k++)G.players.forEach(function(_,i){G.hands[i].push(G.deck.pop());});
  G.phase='playing';
  broadcast();
}
function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>60)G.log.shift();}
function checkLostForever(card){
  if(G.phase!=='playing')return;
  if(card.n<=G.piles[card.c])return;
  var disc=G.discard.filter(function(d){return d.c===card.c&&d.n===card.n;}).length;
  if(disc>=TOTALS[card.n]){
    G.failSeq++;G.failIdx=Math.floor(Math.random()*Math.max(1,FAIL_SOUNDS.length));
    glog('🚨 ¡'+cardTxt(card)+' perdida para siempre!');
    finish('lost');
  }
}
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
       reason==='win'?'🎆 ¡Espectáculo perfecto!':
       reason==='lost'?'💀 Carta irrecuperable — la partida es imposible':'🏁 Fin de la partida');
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
  if(a.kind==='intent'){
    if(!G.intentions[p])G.intentions[p]={};
    if(a.value===null||a.value===undefined)delete G.intentions[p][a.cid];
    else G.intentions[p][a.cid]=a.value;
    broadcast();return;
  }
  if(G.turn!==p)return sendErr(p,'No es tu turno');
  var hand=G.hands[p],card;
  if(a.kind==='play'){
    if(!hand[a.idx])return;
    card=hand.splice(a.idx,1)[0];
    if(G.intentions[p])delete G.intentions[p][card.id];
    if(G.piles[card.c]===card.n-1){
      G.piles[card.c]=card.n;G.pileTops[card.c]=card.id;
      if(card.n===5){if(G.hints<8)G.hints++;G.successSeq++;G.successIdx=Math.floor(Math.random()*Math.max(1,SUCCESS_SOUNDS.length));}
      glog(pname(p)+' juega '+cardTxt(card)+' ✔');
      if(COLORS.every(function(c){return G.piles[c]===5;})){finish('win');return broadcast();}
    }else{
      G.fuses--;G.discard.push(card);G.failSeq++;G.failIdx=Math.floor(Math.random()*Math.max(1,FAIL_SOUNDS.length));
      glog('💥 '+pname(p)+' intenta jugar '+cardTxt(card)+' y falla ('+G.fuses+' mechas)');
      checkLostForever(card);
      if(G.phase!=='playing'){return broadcast();}
      if(G.fuses===0){finish('boom');return broadcast();}
    }
    draw(p);
  }else if(a.kind==='discard'){
    if(G.hints>=8)return sendErr(p,'Ya tenéis las 8 fichas de pista');
    if(!hand[a.idx])return;
    card=hand.splice(a.idx,1)[0];
    if(G.intentions[p])delete G.intentions[p][card.id];
    G.discard.push(card);G.hints++;
    if(G.piles[card.c]===card.n-1){G.failSeq++;G.failIdx=Math.floor(Math.random()*Math.max(1,FAIL_SOUNDS.length));}
    glog(pname(p)+' descarta '+cardTxt(card));
    checkLostForever(card);
    if(G.phase!=='playing'){return broadcast();}
    draw(p);
  }else if(a.kind==='hint'){
    if(G.hints<=0)return sendErr(p,'No quedan fichas de pista');
    if(a.target===p||!G.hands[a.target])return;
    var matches=G.hands[a.target].filter(function(c){
      return a.htype==='color'?c.c===a.value:c.n===a.value;});
    if(!matches.length)return sendErr(p,'La pista debe señalar al menos una carta');
    G.hands[a.target].forEach(function(c){
      if(a.htype==='color'){
        if(c.c===a.value)c.kc=true;
        else if(c.notC.indexOf(a.value)===-1)c.notC.push(a.value);
      }else{
        if(c.n===a.value)c.kn=true;
        else if(c.notN.indexOf(a.value)===-1)c.notN.push(a.value);
      }
    });
    G.hints--;
    G.hintSeq++;G.hintCids=matches.map(function(c){return c.id;});
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
    failSeq:G.failSeq,failIdx:G.failIdx,successSeq:G.successSeq,successIdx:G.successIdx,hintSeq:G.hintSeq,hintCids:G.hintCids.slice(),
    piles:JSON.parse(JSON.stringify(G.piles)),
    pileTops:JSON.parse(JSON.stringify(G.pileTops)),
    discard:G.discard.map(function(c){return {id:c.id,c:c.c,n:c.n};}),
    log:G.log.slice(-30),
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    hands:G.hands.map(function(h,j){return h.map(function(c){
      if(j===i)return {id:c.id,c:c.kc?c.c:null,n:c.kn?c.n:null,kc:c.kc,kn:c.kn,notC:c.notC,notN:c.notN};
      return {id:c.id,c:c.c,n:c.n,kc:c.kc,kn:c.kn};}); }),
    intentions:JSON.parse(JSON.stringify(G.intentions))
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
    history.pushState(null,'',location.pathname+'?sala='+roomCode);
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
function sendIntent(idx,value){
  var cid=V.hands[V.me][idx].id;
  var a={kind:'intent',cid:cid,value:value};
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
  var isOwn=pl===V.me;
  var extra='';
  if(isOwn&&noobMode){
    var nc=card.notC||[],nn=card.notN||[];
    var posC=card.kc&&card.c?[card.c]:COLORS.filter(function(c){return nc.indexOf(c)===-1;});
    var posN=card.kn&&card.n?[card.n]:[1,2,3,4,5].filter(function(n){return nn.indexOf(n)===-1;});
    var dots=posC.map(function(c){return '<div class="h-noob-dot" style="background:'+CINFO[c].bg+'" title="'+CINFO[c].name+'"></div>';}).join('');
    extra='<div class="h-noob"><div class="h-noob-colors">'+dots+'</div>'+
          '<div class="h-noob-nums">'+posN.join('')+'</div></div>';
  }
  var intent=V.intentions&&V.intentions[pl]&&V.intentions[pl][card.id];
  var intentBadge=intent==='play'?'<span class="h-intent">🎇</span>':intent==='discard'?'<span class="h-intent">🗑️</span>':'';
  return '<div class="h-cardwrap" data-cid="'+card.id+'"><div class="'+cls+'" data-pl="'+pl+'" data-idx="'+idx+
    '" style="background:'+bg+'">'+label+intentBadge+'</div>'+
    (!isOwn?'<div class="h-mini" title="Lo que sabe">'+mini+'</div>':extra)+'</div>';
}
var FAIL_SOUNDS=[
  'https://www.myinstants.com/media/sounds/jixaw-metal-pipe-falling-sound.mp3',
  'https://www.myinstants.com/media/sounds/fahhhh-6.mp3',
  'https://www.myinstants.com/media/sounds/wrong_5.mp3',
  'https://www.myinstants.com/media/sounds/faaah.mp3',
  'https://www.myinstants.com/media/sounds/oh-no.mp3',
  'https://www.myinstants.com/media/sounds/movie_1.mp3',
  'https://www.myinstants.com/media/sounds/el-diablo_MicOe0x.mp3',
  'https://www.myinstants.com/media/sounds/lego-yoda-death-sound.mp3',
  'https://www.myinstants.com/media/sounds/dio-wryyy.mp3',
  'https://www.myinstants.com/media/sounds/shizaaaaaa.mp3',
  'https://www.myinstants.com/media/sounds/jotaro-no.mp3',
  'https://www.myinstants.com/media/sounds/preview_4.mp3',
  'https://www.myinstants.com/media/sounds/sound-fail-fallo.mp3',
  'https://www.myinstants.com/media/sounds/mission-failed-well-get-em-next-time.mp3',
  'https://www.myinstants.com/media/sounds/defeat_90KWHvE.mp3',
  'https://www.myinstants.com/media/sounds/ping_missing.mp3',
  'https://www.myinstants.com/media/sounds/among-us-role-reveal-sound.mp3',
  'https://www.myinstants.com/media/sounds/y-se-marcho_VhXz4Hd.mp3',
  'https://www.myinstants.com/media/sounds/penalti-a-favor-del-real-madrid-jajaja.mp3',
  'https://www.myinstants.com/media/sounds/no-no-no-la-policia.mp3',

];
var SUCCESS_SOUNDS=[
  'https://www.myinstants.com/media/sounds/answer-correct.mp3',
  'https://www.myinstants.com/media/sounds/click-nice.mp3',
  'https://www.myinstants.com/media/sounds/kids-saying-yay-sound-effect_3.mp3',
  'https://www.myinstants.com/media/sounds/anime-wow-sound-effect.mp3',
  'https://www.myinstants.com/media/sounds/789-audio-extractor.mp3',
  'https://www.myinstants.com/media/sounds/fairy-dust-sound-effect.mp3',
  'https://www.myinstants.com/media/sounds/yes-yes-yes-yes-yes.mp3',
  'https://www.myinstants.com/media/sounds/michael-jackson-hee-hee.mp3',
  'https://www.myinstants.com/media/sounds/mercadona.mp3',
  'https://www.myinstants.com/media/sounds/yippeeeeeeeeeeeeee.mp3',
  'https://www.myinstants.com/media/sounds/tuturu_1.mp3',
  'https://www.myinstants.com/media/sounds/e33-monoco-owowow.mp3',
  'https://www.myinstants.com/media/sounds/rajoy-japones.mp3',
  'https://www.myinstants.com/media/sounds/mission-success.mp3',
  'https://www.myinstants.com/media/sounds/129-received-an-item.mp3',
  'https://www.myinstants.com/media/sounds/reeeee-2.mp3',
  'https://www.myinstants.com/media/sounds/es-la-hora-de-la-paja-video-original-audiotrimmer.mp3',
];
function playAt(arr,idx){
  if(!arr.length)return;
  try{var a=new Audio(arr[idx%arr.length]);a.volume=0.2;a.play().catch(function(){});}catch(e){}
}
var failHeard=-1,successHeard=-1;
function checkSounds(){
  if(typeof V.failSeq==='number'){
    if(failHeard>=0&&V.failSeq>failHeard)playAt(FAIL_SOUNDS,V.failIdx||0);
    failHeard=V.failSeq;
  }
  if(typeof V.successSeq==='number'){
    if(successHeard>=0&&V.successSeq>successHeard)playAt(SUCCESS_SOUNDS,V.successIdx||0);
    successHeard=V.successSeq;
  }
}
var hintSeen=-1;
function checkHintGlow(){
  if(typeof V.hintSeq!=='number')return;
  if(hintSeen>=0&&V.hintSeq>hintSeen&&V.hintCids){
    V.hintCids.forEach(function(id){
      var w=el('h-game').querySelector('[data-cid="'+id+'"]');
      if(w&&w.firstChild&&w.firstChild.classList)w.firstChild.classList.add('h-hintglow');
    });
  }
  hintSeen=V.hintSeq;
}
function animate(prev){
  // FLIP: toda carta con data-cid (mano, pila, descartes) se desliza
  // desde su posición del render anterior hasta la nueva
  var els=el('h-game').querySelectorAll('[data-cid]');
  for(var i=0;i<els.length;i++){
    var e=els[i],r0=prev[e.getAttribute('data-cid')];
    if(!r0){
      if(e.classList.contains('h-cardwrap'))e.firstChild.classList.add('h-new');
      continue;
    }
    var r1=e.getBoundingClientRect(),dx=r0.left-r1.left,dy=r0.top-r1.top;
    if(dx||dy){
      var base=e.style.transform||''; // conserva la rotación del abanico
      e.style.transition='none';e.style.transform='translate('+dx+'px,'+dy+'px) '+base;
      e.getBoundingClientRect();
      e.style.transition='transform .45s ease';e.style.transform=base;
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
     '<button data-act="copy">📋 Copiar enlace</button>'+
     '<button data-act="noob" class="h-noob-toggle'+(noobMode?' h-active':'')+'">🔰 Modo noob'+(noobMode?' ✓':'')+'</button></div>';
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
      else if(r.reason==='lost')msg='💀 Carta perdida irrecuperablemente. Puntuación: '+r.score;
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
      h+='<div class="h-pile"'+(V.piles[c]?' data-cid="'+V.pileTops[c]+'"':'')+
         ' style="background:'+CINFO[c].bg+(V.piles[c]===0?';opacity:.3':'')+
         '">'+(V.piles[c]||'·')+'</div>';});
    // abanico de descartes, agrupado por color
    var ds=V.discard.slice().sort(function(a,b){
      return COLORS.indexOf(a.c)-COLORS.indexOf(b.c)||a.n-b.n;});
    h+='<div class="h-fan" title="Descartes"><div class="h-disc h-discempty">🗑</div>';
    ds.forEach(function(d){
      h+='<div class="h-disc" data-cid="'+d.id+'" style="background:'+CINFO[d.c].bg+
         ';transform:rotate('+((d.id%5)-2)*4+'deg)">'+d.n+'</div>';});
    h+='</div>';
    h+='</div><div class="h-row">💡 Pistas: <b>'+V.hints+'</b>/8 &nbsp; 🧨 Mechas: <b>'+
       V.fuses+'</b>/3 &nbsp; 🂠 Mazo: <b>'+V.deckCount+'</b>'+
       (V.turnsLeft!==null?' &nbsp; ⏳ Turnos finales: <b>'+V.turnsLeft+'</b>':'')+'</div>';
    // en peligro: cartas aún necesarias con una sola copia viva (por descartes/fallos)
    var danger=[],dead=[];
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
    var playerOrder=[];
    for(var oi=0;oi<V.names.length;oi++)playerOrder.push((V.me+oi)%V.names.length);
    var actionRendered=false;
    playerOrder.forEach(function(i){
      var n=V.names[i];
      var turn=V.phase==='playing'&&V.turn===i;
      h+='<div class="h-panel'+(turn?' h-turn':'')+'"><div class="h-name">'+
         (turn?'▶ ':'')+esc(n)+(i===V.me?' (tú)':'')+(V.connected[i]?'':' 🔌❌')+'</div><div class="h-row">';
      V.hands[i].forEach(function(c,j){h+=cardHtml(c,i,j,V.phase==='playing'&&(V.turn===V.me||i===V.me));});
      h+='</div></div>';
      // intención: aparece debajo de tus propias cartas cuando no es tu turno
      if(V.phase==='playing'&&V.turn!==V.me&&sel&&sel.pl===V.me&&i===V.me){
        var intentCard=V.hands[V.me][sel.idx];
        var curIntent=intentCard&&V.intentions&&V.intentions[V.me]&&V.intentions[V.me][intentCard.id];
        h+='<div class="h-panel"><b>Carta '+(sel.idx+1)+' tuya — marcar intención:</b> '+
           '<button data-act="intent-play"'+(curIntent==='play'?' style="outline:2px solid #fbbf24;outline-offset:1px"':'')+'>🎇 Jugar</button>'+
           '<button data-act="intent-discard"'+(curIntent==='discard'?' style="outline:2px solid #fbbf24;outline-offset:1px"':'')+'>🗑️ Descartar</button>'+
           '</div>';
        actionRendered=true;
      }
      // barra de acciones: aparece justo debajo del jugador seleccionado
      if(V.phase==='playing'&&V.turn===V.me&&sel&&sel.pl===i){
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
        actionRendered=true;
      }
    });
    if(V.phase==='playing'&&!actionRendered){
      h+='<div class="h-mut">'+(V.turn===V.me?
        '✨ Tu turno: toca una carta tuya (jugar/descartar) o una ajena (dar pista)':
        'Turno de '+esc(V.names[V.turn])+'...')+'</div>';
    }
    h+='<div class="h-panel h-log">'+V.log.slice().reverse().map(function(l){
      return '<div>'+esc(l)+'</div>';}).join('')+'</div>';
  }
  el('h-game').innerHTML=h;
  if(V.phase==='playing'||V.phase==='ended')animate(prev);
  checkSounds();
  checkHintGlow();
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
    else if(act==='noob'){noobMode=!noobMode;render();}
    else if(act==='start'&&isHost)startGame();
    else if(act==='play')sendAction({kind:'play',idx:sel.idx});
    else if(act==='discard')sendAction({kind:'discard',idx:sel.idx});
    else if(act==='hintc')sendAction({kind:'hint',target:sel.pl,htype:'color',value:V.hands[sel.pl][sel.idx].c});
    else if(act==='hintn')sendAction({kind:'hint',target:sel.pl,htype:'number',value:V.hands[sel.pl][sel.idx].n});
    else if(act==='intent-play'){var ci=V.hands[V.me][sel.idx].id;sendIntent(sel.idx,V.intentions&&V.intentions[V.me]&&V.intentions[V.me][ci]==='play'?null:'play');}
    else if(act==='intent-discard'){var ci=V.hands[V.me][sel.idx].id;sendIntent(sel.idx,V.intentions&&V.intentions[V.me]&&V.intentions[V.me][ci]==='discard'?null:'discard');}
    return;
  }
  var c=e.target.closest('[data-pl]');
  if(c&&V&&V.phase==='playing'){
    var pl=+c.getAttribute('data-pl'),idx=+c.getAttribute('data-idx');
    if(V.turn===V.me||pl===V.me){
      sel=(sel&&sel.pl===pl&&sel.idx===idx)?null:{pl:pl,idx:idx};
      render();
    }
  }
});
// hover preview de pista
el('h-game').addEventListener('mouseover',function(e){
  var b=e.target.closest('[data-act="hintc"],[data-act="hintn"]');
  if(!b||!sel||sel.pl===V.me)return;
  var act=b.getAttribute('data-act'),sc=V.hands[sel.pl][sel.idx];
  var val=act==='hintc'?sc.c:sc.n;
  V.hands[sel.pl].forEach(function(c,j){
    if((act==='hintc'?c.c:c.n)===val){
      var ce=el('h-game').querySelector('[data-pl="'+sel.pl+'"][data-idx="'+j+'"]');
      if(ce)ce.classList.add('h-hinthover');
    }
  });
});
el('h-game').addEventListener('mouseout',function(e){
  var b=e.target.closest('[data-act="hintc"],[data-act="hintn"]');
  if(!b||b.contains(e.relatedTarget))return;
  el('h-game').querySelectorAll('.h-hinthover').forEach(function(c){c.classList.remove('h-hinthover');});
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
