---
title: "🤫 Just One"
date: 2026-07-12
searchHidden: true
robotsNoIndex: true
showToc: false
ShowReadingTime: false
ShowShareButtons: false
comments: false
---

Juego cooperativo de palabras para 3-7 jugadores. Uno crea la sala y comparte el código (o el enlace) con los demás. La partida vive en el navegador del anfitrión: si lo cierra, se acaba la fiesta.

<style>
#justone-app{--jbg:#1b1f27;--jpanel:#252b36;--jtext:#e8eaf0;--jmut:#9aa3b2;
  background:var(--jbg);color:var(--jtext);border-radius:12px;padding:16px;
  font-family:system-ui,sans-serif;font-size:15px;line-height:1.4}
#justone-app h3{margin:10px 0 6px;color:var(--jtext)}
#justone-app input{background:#11141a;color:var(--jtext);border:1px solid #3a4252;
  border-radius:8px;padding:8px 10px;font-size:15px;max-width:180px}
#justone-app button{background:#3b82f6;color:#fff;border:0;border-radius:8px;
  padding:8px 14px;font-size:14px;cursor:pointer;margin:2px}
#justone-app button:disabled{background:#3a4252;color:var(--jmut);cursor:not-allowed}
#justone-app button.j-red{background:#dc2626}
#justone-app button.j-green{background:#16a34a}
#justone-app .j-row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin:8px 0}
#justone-app .j-panel{background:var(--jpanel);border-radius:10px;padding:10px 12px;margin:8px 0}
#justone-app .j-word{font-size:28px;font-weight:800;letter-spacing:1px;color:#fbbf24;
  text-align:center;padding:8px;text-transform:uppercase}
#justone-app .j-clue{background:#11141a;border:2px solid #3a4252;border-radius:10px;
  padding:8px 14px;font-size:20px;font-weight:700;display:inline-flex;flex-direction:column;
  align-items:center;gap:2px;min-width:80px;position:relative}
#justone-app .j-clue.j-struck{opacity:.45;border-color:#dc2626}
#justone-app .j-clue.j-struck .j-cluetext{text-decoration:line-through}
#justone-app .j-clue.j-clickable{cursor:pointer}
#justone-app .j-clue.j-clickable:hover{border-color:#fbbf24}
#justone-app .j-cluewho{font-size:11px;font-weight:400;color:var(--jmut)}
#justone-app .j-cluewhy{position:absolute;top:-9px;right:-7px;font-size:15px;line-height:1}
#justone-app .j-numbtn{width:52px;height:70px;font-size:26px;font-weight:800;border-radius:10px}
#justone-app .j-mini{font-size:12px;color:var(--jmut)}
#justone-app .j-turn{box-shadow:0 0 0 2px #fbbf24 inset;border-radius:10px}
#justone-app .j-log{max-height:140px;overflow-y:auto;font-size:13px;color:var(--jmut)}
#justone-app .j-log div{padding:1px 0}
#justone-app .j-banner{background:#3b2f12;border:1px solid #fbbf24;border-radius:10px;
  padding:10px 14px;margin:8px 0;font-weight:600}
#justone-app .j-toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);
  background:#dc2626;color:#fff;padding:10px 18px;border-radius:10px;z-index:99;
  box-shadow:0 4px 14px rgba(0,0,0,.4)}
#justone-app .j-code{font-size:22px;font-weight:800;letter-spacing:4px;color:#fbbf24}
#justone-app .j-mut{color:var(--jmut);font-size:13px}
#justone-app .j-dot{display:inline-block;width:10px;height:10px;border-radius:50%;
  background:#3a4252;margin-right:4px}
#justone-app .j-dot.j-ok{background:#16a34a}
#justone-app .j-new{animation:j-pop .4s ease}
@keyframes j-pop{from{transform:scale(.6);opacity:0}to{transform:none;opacity:1}}
</style>
<div id="justone-app">
  <div id="j-setup">
    <div class="j-row"><label>Tu nombre: <input id="j-name" maxlength="14" placeholder="Nombre"></label></div>
    <div class="j-row"><button id="j-create">🤫 Crear sala</button></div>
    <div class="j-row"><input id="j-codein" maxlength="4" placeholder="CÓDIGO" style="text-transform:uppercase;width:110px"><button id="j-join">Unirse</button></div>
    <div id="j-setupmsg" class="j-mut"></div>
  </div>
  <div id="j-game" style="display:none"></div>
</div>

<script src="https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js"></script>

<script>
(function(){
'use strict';
// ---------- constantes ----------
var PREFIX='justone-xiang-';
var MINP=3,MAXP=7,NCARDS=13;
var WORDS=[
'elefante','jirafa','pingüino','tiburón','mariposa','camello','búho','pulpo','canguro','tortuga',
'lobo','hormiga','delfín','murciélago','gallina','erizo','ballena','cocodrilo','panda','flamenco',
'paella','chocolate','croqueta','aceituna','sandía','queso','churro','tortilla','gazpacho','pizza',
'hamburguesa','miel','limón','ajo','tarta','helado','café','palomitas','sushi','picante',
'paraguas','tijeras','espejo','almohada','martillo','brújula','linterna','escalera','botella','guitarra',
'reloj','gafas','llave','cuchara','mochila','sombrero','vela','imán','globo','semáforo',
'ancla','telescopio','cohete','satélite','paracaídas','columpio','tobogán','disfraz','peluca','dado',
'playa','desierto','castillo','museo','hospital','biblioteca','volcán','isla','catedral','faro',
'mercado','gimnasio','circo','granja','selva','cueva','pirámide','laberinto','oasis','iglú',
'amor','miedo','suerte','sueño','música','silencio','gravedad','eco','sombra','arcoíris',
'tormenta','nieve','fuego','viento','marea','eclipse','cometa','galaxia','milagro','karma',
'Drácula','Tarzán','Cleopatra','Einstein','Picasso','Superman','Cenicienta','Pinocho','Sherlock','Mozart',
'Napoleón','Batman','Frankenstein','Messi','Shakira','Goku','Mario','Shrek','Rapunzel','Hércules',
'bombero','astronauta','payaso','dentista','espía','pirata','fontanero','peluquero','árbitro','torero',
'ninja','vikingo','samurái','detective','malabarista','ajedrez','fútbol','yoga','kárate','parchís',
'dominó','póker','maratón','esquí','surf','billar','dardos','petanca','rayuela','escondite',
'wifi','emoji','dron','robot','karaoke','tatuaje','selfi','youtuber','bitcoin','zombi',
'vampiro','unicornio','dragón','sirena','fantasma','esqueleto','bruja','ovni','meteorito','tesoro',
'huracán','terremoto','góndola','kimono','bumerán','trineo','submarino','helicóptero','tractor','ambulancia',
'ascensor','telepatía','hipnosis','alergia','hipo','bostezo','cosquillas','siesta','insomnio','horóscopo',
'boda','cumpleaños','Navidad','carnaval','lotería','casino','ruleta','imperio','corona','trono',
'espada','escudo','flecha','cañón','dinamita','veneno','vacuna','microbio','cerebro','bigote',
'momia','dinosaurio','fósil','glaciar','avalancha','géiser','ventisca','relámpago','madrugada','resaca',
'propina','atasco','mudanza','huelga','rebajas','chanclas','flotador','sombrilla','chiringuito','karateca'];

// ---------- estado ----------
var peer=null,isHost=false,hostConn=null,roomCode='';
var G=null;   // estado completo (solo host)
var V=null;   // vista personalizada (lo que se renderiza)
var myName='';

function el(id){return document.getElementById(id);}
function esc(s){return String(s).replace(/[&<>"']/g,function(c){
  return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];});}
function toast(msg){
  var t=document.createElement('div');t.className='j-toast';t.textContent=msg;
  el('justone-app').appendChild(t);setTimeout(function(){t.remove();},3500);
}
// normaliza para comparar: minúsculas, sin acentos, solo letras/números
function norm(s){
  return String(s).toLowerCase().normalize('NFD').replace(/[\u0300-\u036f]/g,'')
    .replace(/[^a-z0-9]/g,'');
}
// quita una 's' final para pillar plurales simples
function stem(s){var n=norm(s);return n.length>3&&n.slice(-1)==='s'?n.slice(0,-1):n;}
function sameWord(a,b){return norm(a)===norm(b)||stem(a)===stem(b);}

// ---------- lógica de juego (host) ----------
function mkDeck(){
  var w=WORDS.slice();
  for(var i=w.length-1;i>0;i--){var j=Math.floor(Math.random()*(i+1));
    var t=w[i];w[i]=w[j];w[j]=t;}
  var d=[];
  for(var k=0;k<NCARDS;k++)d.push(w.slice(k*5,k*5+5));
  return d;
}
function newGame(){
  G={phase:'lobby',players:[],deck:[],score:0,lost:0,round:null,log:[],
     successSeq:0,successIdx:0,failSeq:0,failIdx:0};
}
function startGame(){
  G.deck=mkDeck();G.score=0;G.lost=0;G.log=['🤫 Empieza la partida'];
  G.successSeq=0;G.failSeq=0;
  G.round={num:0,guesser:-1};
  G.phase='playing';
  nextRound();
  broadcast();
}
function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>60)G.log.shift();}
function finish(){
  G.phase='ended';
  glog('🏁 Fin de la partida: '+G.score+' aciertos de '+NCARDS);
}
function nextRound(){
  if(!G.deck.length){finish();return;}
  var g=G.round.guesser,tries=0;
  do{g=(g+1)%G.players.length;tries++;}
  while(!G.players[g].connected&&tries<=G.players.length);
  G.round={num:G.round.num+1,guesser:g,phase:'pick',card:G.deck.pop(),
    wordIdx:-1,word:null,clues:{},guess:null,outcome:null};
  glog('📇 Carta '+G.round.num+' — adivina '+pname(g));
}
function writers(){
  return G.players.map(function(p,i){return i;})
    .filter(function(i){return i!==G.round.guesser&&G.players[i].connected;});
}
function checkAllClues(){
  var ws=writers();
  if(!ws.length)return;
  if(ws.every(function(i){return G.round.clues[i];})){
    G.round.phase='compare';
    glog('✍️ Todas las pistas escritas, comparando...');
  }
}
function resolveRound(outcome){
  var R=G.round;
  R.outcome=outcome;R.phase='result';
  if(outcome==='correct'){
    G.score++;
    G.successSeq++;G.successIdx=Math.floor(Math.random()*Math.max(1,SUCCESS_SOUNDS.length));
    glog('🎉 ¡'+pname(R.guesser)+' acierta «'+R.word+'»! ('+G.score+' puntos)');
  }else if(outcome==='pass'){
    G.lost++;
    glog('🏳 '+pname(R.guesser)+' pasa. La palabra era «'+R.word+'»');
  }else{
    G.lost++;
    G.failSeq++;G.failIdx=Math.floor(Math.random()*Math.max(1,FAIL_SOUNDS.length));
    glog('❌ '+pname(R.guesser)+' dice «'+R.guess+'» pero era «'+R.word+'»');
    if(G.deck.length){G.deck.pop();G.lost++;
      glog('🗑 Se descarta otra carta de castigo');}
  }
}
function sendErr(p,msg){
  if(p===0)toast(msg);
  else if(G.players[p].conn)G.players[p].conn.send({t:'error',msg:msg});
}
function applyAction(p,a){
  if(!G||G.phase!=='playing'||!G.round)return;
  var R=G.round;
  if(a.kind==='pick'){
    if(R.phase!=='pick'||p!==R.guesser)return;
    var n=Math.floor(+a.n);
    if(!(n>=1&&n<=5))return;
    R.wordIdx=n-1;R.word=R.card[n-1];R.phase='clues';
    glog(pname(p)+' elige el número '+n);
  }else if(a.kind==='clue'){
    if(R.phase!=='clues'||p===R.guesser)return;
    var t=String(a.text||'').trim().slice(0,24);
    if(!t)return;
    if(/\s/.test(t))return sendErr(p,'La pista debe ser UNA sola palabra');
    R.clues[p]={text:t,struck:false,why:''};
    checkAllClues();
  }else if(a.kind==='strike'){
    if(R.phase!=='compare'||p===R.guesser)return;
    var c=R.clues[a.who];
    if(!c)return;
    c.struck=!c.struck;c.why=c.struck?'✋':'';
    glog(pname(p)+(c.struck?' anula':' restaura')+' la pista de '+pname(+a.who));
  }else if(a.kind==='reveal'){
    if(R.phase!=='compare'||p===R.guesser)return;
    R.phase='guess';
    glog('👁 Pistas reveladas a '+pname(R.guesser));
  }else if(a.kind==='guess'){
    if(R.phase!=='guess'||p!==R.guesser)return;
    var g=String(a.text||'').trim().slice(0,30);
    if(!g)return;
    R.guess=g;
    if(sameWord(g,R.word))resolveRound('correct');
    else{R.phase='judge';glog(pname(p)+' responde: «'+g+'»');}
  }else if(a.kind==='pass'){
    if(R.phase!=='guess'||p!==R.guesser)return;
    resolveRound('pass');
  }else if(a.kind==='judge'){
    if(R.phase!=='judge'||p===R.guesser)return;
    resolveRound(a.ok?'correct':'wrong');
  }else if(a.kind==='next'){
    if(R.phase!=='result')return;
    nextRound();
  }else return;
  broadcast();
}
function viewFor(i){
  var R=G.round,r=null;
  if(R&&R.guesser>=0){
    var isGuesser=i===R.guesser;
    var clues=[];
    Object.keys(R.clues).forEach(function(k){
      var c=R.clues[k],who=+k;
      var item={who:who,text:c.text,struck:c.struck,why:c.why};
      if(R.phase==='clues'){if(who===i)clues.push(item);}
      else if(R.phase==='compare'){if(!isGuesser)clues.push(item);}
      else if(R.phase==='guess'||R.phase==='judge'){
        if(!isGuesser||!c.struck)clues.push(item);}
      else clues.push(item); // result: todo visible
    });
    r={num:R.num,guesser:R.guesser,phase:R.phase,
       card:(R.phase==='pick'&&!isGuesser)?R.card.slice():null,
       word:(isGuesser&&R.phase!=='result')?null:R.word,
       guess:R.guess,outcome:R.outcome,
       submitted:G.players.map(function(_,j){return !!R.clues[j];}),
       clues:clues};
  }
  return {
    phase:G.phase,me:i,score:G.score,lost:G.lost,deckCount:G.deck.length,
    total:NCARDS,round:r,log:G.log.slice(-30),
    successSeq:G.successSeq,successIdx:G.successIdx,failSeq:G.failSeq,failIdx:G.failIdx,
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;})
  };
}
function broadcast(){
  G.players.forEach(function(p,i){
    if(i===0)return;
    if(p.conn&&p.connected)p.conn.send({t:'state',s:viewFor(i)});
  });
  V=viewFor(0);render();
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
          c._jIdx=old;glog('🔌 '+name+' ha vuelto');broadcast();}
        else c.send({t:'error',msg:'Partida en curso, no puedes entrar'});
        return;
      }
      if(G.players.length>=MAXP)return c.send({t:'error',msg:'Sala llena (máx '+MAXP+')'});
      while(G.players.some(function(p){return p.name===name;}))name+='2';
      G.players.push({name:name,conn:c,connected:true});
      c._jIdx=G.players.length-1;
      broadcast();
    }else if(msg.t==='action'&&typeof c._jIdx==='number'){
      applyAction(c._jIdx,msg.a);
    }
  });
  c.on('close',function(){
    if(typeof c._jIdx!=='number')return;
    if(G.phase==='lobby'){G.players.splice(c._jIdx,1);
      G.players.forEach(function(p,i){if(p.conn)p.conn._jIdx=i;});}
    else{G.players[c._jIdx].connected=false;
      glog('🔌 '+pname(c._jIdx)+' se ha desconectado');
      if(G.round&&G.round.phase==='clues')checkAllClues();}
    broadcast();
  });
}
function createRoom(){
  myName=el('j-name').value.trim();
  if(!myName)return setupMsg('Pon tu nombre primero');
  var alpha='ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
  roomCode='';for(var i=0;i<4;i++)roomCode+=alpha[Math.floor(Math.random()*alpha.length)];
  setupMsg('Creando sala...');
  peer=new Peer(PREFIX+roomCode);
  peer.on('open',function(){
    isHost=true;newGame();
    G.players.push({name:myName,conn:null,connected:true});
    history.pushState(null,'',location.pathname+'?sala='+roomCode);
    el('j-setup').style.display='none';el('j-game').style.display='block';
    broadcast();
  });
  peer.on('connection',setupHostConn);
  peer.on('error',function(e){
    if(e.type==='unavailable-id')setupMsg('Código ocupado, prueba otra vez');
    else setupMsg('Error de conexión: '+e.type);
  });
}
function joinRoom(){
  myName=el('j-name').value.trim();
  roomCode=el('j-codein').value.trim().toUpperCase();
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
        el('j-setup').style.display='none';el('j-game').style.display='block';
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
}
function setupMsg(m){el('j-setupmsg').textContent=m;}

// ---------- sonidos ----------
var FAIL_SOUNDS=[
  'https://www.myinstants.com/media/sounds/jixaw-metal-pipe-falling-sound.mp3',
  'https://www.myinstants.com/media/sounds/wrong_5.mp3',
  'https://www.myinstants.com/media/sounds/oh-no.mp3',
  'https://www.myinstants.com/media/sounds/lego-yoda-death-sound.mp3',
  'https://www.myinstants.com/media/sounds/sound-fail-fallo.mp3',
  'https://www.myinstants.com/media/sounds/mission-failed-well-get-em-next-time.mp3',
  'https://www.myinstants.com/media/sounds/no-no-no-la-policia.mp3',
  'https://www.myinstants.com/media/sounds/y-se-marcho_VhXz4Hd.mp3',
];
var SUCCESS_SOUNDS=[
  'https://www.myinstants.com/media/sounds/answer-correct.mp3',
  'https://www.myinstants.com/media/sounds/kids-saying-yay-sound-effect_3.mp3',
  'https://www.myinstants.com/media/sounds/anime-wow-sound-effect.mp3',
  'https://www.myinstants.com/media/sounds/yes-yes-yes-yes-yes.mp3',
  'https://www.myinstants.com/media/sounds/michael-jackson-hee-hee.mp3',
  'https://www.myinstants.com/media/sounds/mercadona.mp3',
  'https://www.myinstants.com/media/sounds/yippeeeeeeeeeeeeee.mp3',
  'https://www.myinstants.com/media/sounds/tuturu_1.mp3',
  'https://www.myinstants.com/media/sounds/mission-success.mp3',
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

// ---------- render ----------
function clueHtml(c,clickable){
  var cls='j-clue'+(c.struck?' j-struck':'')+(clickable?' j-clickable':'');
  return '<div class="'+cls+'" data-who="'+c.who+'">'+
    (c.why?'<span class="j-cluewhy">'+c.why+'</span>':'')+
    '<span class="j-cluetext">'+esc(c.text)+'</span>'+
    '<span class="j-cluewho">'+esc(V.names[c.who])+'</span></div>';
}
function render(){
  if(!V)return;
  // conserva lo que estés escribiendo aunque llegue un re-render
  var keep={};
  ['j-clueinput','j-guessinput'].forEach(function(id){
    var n=el(id);
    if(n)keep[id]={v:n.value,f:document.activeElement===n};
  });
  var h='';
  h+='<div class="j-row">Sala: <span class="j-code">'+roomCode+'</span> '+
     '<button data-act="copy">📋 Copiar enlace</button></div>';
  if(V.phase==='lobby'){
    h+='<div class="j-panel"><h3>Jugadores ('+V.names.length+'/'+MAXP+')</h3>';
    V.names.forEach(function(n,i){
      h+='<div>'+esc(n)+(i===0?' 👑':'')+(i===V.me?' (tú)':'')+'</div>';});
    h+='</div>';
    if(V.me===0)h+='<button data-act="start" '+(V.names.length<MINP?'disabled':'')+
      '>🚀 Empezar ('+(V.names.length<MINP?'mínimo '+MINP:V.names.length+' jugadores')+')</button>';
    else h+='<div class="j-mut">Esperando a que '+esc(V.names[0])+' empiece...</div>';
  }else{
    if(V.phase==='ended'){
      var s=V.score,msg;
      if(s>=13)msg='🏆 ¡Puntuación perfecta! ¿Podréis repetirlo?';
      else if(s===12)msg='🤩 ¡Increíble! Vuestros amigos deben estar impresionados';
      else if(s===11)msg='🎉 ¡Genial! Una puntuación digna de mención';
      else if(s>=9)msg='😮 ¡Guau, no está nada mal!';
      else if(s>=7)msg='🙂 Estáis en la media. ¿Otra partida?';
      else if(s>=4)msg='😅 Es un comienzo... Intentadlo de nuevo';
      else msg='💀 Intentadlo otra vez...';
      h+='<div class="j-banner">🏁 '+s+'/'+V.total+' aciertos — '+msg+
        (V.me===0?' <button data-act="start">🔄 Otra partida</button>':'')+'</div>';
    }
    h+='<div class="j-row">📇 Carta: <b>'+(V.round?V.round.num:'-')+'</b>/'+V.total+
       ' &nbsp; ⭐ Aciertos: <b>'+V.score+'</b> &nbsp; 🗑 Perdidas: <b>'+V.lost+
       '</b> &nbsp; 🂠 Quedan: <b>'+V.deckCount+'</b></div>';
    var R=V.round;
    if(R&&V.phase==='playing'){
      var iAmGuesser=V.me===R.guesser;
      var gname=esc(V.names[R.guesser]);
      h+='<div class="j-panel j-turn"><b>🕵️ Adivina: '+gname+(iAmGuesser?' (tú)':'')+'</b></div>';
      // la palabra, solo para los que dan pistas
      if(R.word&&!iAmGuesser&&R.phase!=='result'){
        h+='<div class="j-panel"><div class="j-mut" style="text-align:center">🤫 La palabra secreta (¡no la digas!)</div>'+
           '<div class="j-word">'+esc(R.word)+'</div></div>';
      }
      if(R.phase==='pick'){
        if(iAmGuesser){
          h+='<div class="j-panel"><b>Elige un número de la carta:</b><div class="j-row">';
          for(var n=1;n<=5;n++)h+='<button class="j-numbtn" data-act="pick" data-n="'+n+'">'+n+'</button>';
          h+='</div></div>';
        }else{
          h+='<div class="j-panel"><b>La carta — '+gname+' está eligiendo un número a ciegas:</b>'+
             '<div class="j-row">'+R.card.map(function(w,k){
               return '<div class="j-clue"><span class="j-cluetext">'+esc(w)+
                 '</span><span class="j-cluewho">'+(k+1)+'</span></div>';}).join('')+'</div></div>';
        }
      }else if(R.phase==='clues'){
        var dots=V.names.map(function(n,i){
          if(i===R.guesser)return '';
          return '<span><span class="j-dot'+(R.submitted[i]?' j-ok':'')+'"></span>'+esc(n)+'</span>';
        }).join(' &nbsp;');
        if(iAmGuesser){
          h+='<div class="j-panel">✍️ Los demás están escribiendo sus pistas...<div class="j-row">'+dots+'</div></div>';
        }else{
          var mine=R.clues.filter(function(c){return c.who===V.me;})[0];
          h+='<div class="j-panel"><b>Escribe UNA palabra como pista:</b>'+
             '<div class="j-row"><input id="j-clueinput" maxlength="24" placeholder="Tu pista">'+
             '<button data-act="clue" class="j-green">✍️ Enviar</button></div>'+
             (mine?'<div class="j-mut">Pista enviada: <b>'+esc(mine.text)+'</b> (puedes cambiarla hasta que estén todas)</div>':'')+
             '<div class="j-row">'+dots+'</div></div>';
        }
      }else if(R.phase==='compare'){
        if(iAmGuesser){
          h+='<div class="j-panel">🤐 Están comparando las pistas y quitando las repetidas...</div>';
        }else{
          h+='<div class="j-panel"><b>Pistas (toca una para anularla/restaurarla):</b>'+
             '<div class="j-mut">Anulad (✋) las repetidas, las derivadas de la palabra o las demasiado parecidas. Sed honestos 😇</div>'+
             '<div class="j-row">'+R.clues.map(function(c){return clueHtml(c,true);}).join('')+'</div>'+
             '<button data-act="reveal">👁 Revelar al adivinador</button></div>';
        }
      }else if(R.phase==='guess'){
        var visibles=R.clues.filter(function(c){return !c.struck;});
        if(iAmGuesser){
          h+='<div class="j-panel"><b>Tus pistas:</b><div class="j-row">'+
             (visibles.length?visibles.map(function(c){return clueHtml(c,false);}).join(''):
              '<span class="j-mut">😱 ¡Todas las pistas se anularon entre sí!</span>')+'</div>'+
             '<div class="j-row"><input id="j-guessinput" maxlength="30" placeholder="La palabra es...">'+
             '<button data-act="guess" class="j-green">🎯 Adivinar</button>'+
             '<button data-act="pass" class="j-red">🏳 Pasar</button></div>'+
             '<div class="j-mut">Pasar pierde esta carta; fallar pierde esta y otra más.</div></div>';
        }else{
          h+='<div class="j-panel"><b>Pistas reveladas:</b><div class="j-row">'+
             R.clues.map(function(c){return clueHtml(c,false);}).join('')+'</div>'+
             '<div class="j-mut">Esperando la respuesta de '+gname+'...</div></div>';
        }
      }else if(R.phase==='judge'){
        if(iAmGuesser){
          h+='<div class="j-panel">⚖️ Has dicho «<b>'+esc(R.guess)+'</b>». Los demás deciden si vale...</div>';
        }else{
          h+='<div class="j-panel"><b>⚖️ '+gname+' ha dicho:</b> «<b>'+esc(R.guess)+
             '</b>» (la palabra era «<b>'+esc(R.word)+'</b>»)'+
             '<div class="j-row"><button data-act="jok" class="j-green">✔ Darla por buena</button>'+
             '<button data-act="jno" class="j-red">✘ Rechazar</button></div></div>';
        }
      }else if(R.phase==='result'){
        var om=R.outcome==='correct'?'🎉 ¡Correcto!':
          R.outcome==='pass'?'🏳 Pasada — se pierde esta carta':
          '❌ Fallo — se pierde esta carta y otra de castigo';
        h+='<div class="j-banner j-new">'+om+'<div class="j-word">'+esc(R.word)+'</div>'+
           (R.guess?'<div>Respuesta: «'+esc(R.guess)+'»</div>':'')+'</div>';
        h+='<div class="j-panel"><b>Las pistas eran:</b><div class="j-row">'+
           R.clues.map(function(c){return clueHtml(c,false);}).join('')+'</div>'+
           '<button data-act="next">➡ '+(V.deckCount?'Siguiente carta':'Ver resultado final')+'</button></div>';
      }
    }
    h+='<div class="j-panel"><h3>Jugadores</h3>'+V.names.map(function(n,i){
      return '<div>'+(R&&i===R.guesser?'🕵️ ':'')+esc(n)+(i===V.me?' (tú)':'')+
        (V.connected[i]?'':' 🔌❌')+'</div>';}).join('')+'</div>';
    h+='<div class="j-panel j-log">'+V.log.slice().reverse().map(function(l){
      return '<div>'+esc(l)+'</div>';}).join('')+'</div>';
  }
  el('j-game').innerHTML=h;
  Object.keys(keep).forEach(function(id){
    var n=el(id);
    if(n){n.value=keep[id].v;if(keep[id].f)n.focus();}
  });
  checkSounds();
}

// ---------- eventos ----------
el('j-create').addEventListener('click',createRoom);
el('j-join').addEventListener('click',joinRoom);
el('j-game').addEventListener('click',function(e){
  var b=e.target.closest('[data-act]');
  if(b){
    var act=b.getAttribute('data-act');
    if(act==='copy'){
      navigator.clipboard.writeText(location.origin+location.pathname+'?sala='+roomCode)
        .then(function(){toast('Enlace copiado 📋');});
    }
    else if(act==='start'&&isHost)startGame();
    else if(act==='pick')sendAction({kind:'pick',n:+b.getAttribute('data-n')});
    else if(act==='clue'){
      var t=el('j-clueinput').value.trim();
      if(!t)return toast('Escribe una pista');
      if(/\s/.test(t))return toast('Solo UNA palabra');
      el('j-clueinput').value='';
      sendAction({kind:'clue',text:t});
    }
    else if(act==='reveal')sendAction({kind:'reveal'});
    else if(act==='guess'){
      var g=el('j-guessinput').value.trim();
      if(!g)return toast('Escribe tu respuesta');
      sendAction({kind:'guess',text:g});
    }
    else if(act==='pass')sendAction({kind:'pass'});
    else if(act==='jok')sendAction({kind:'judge',ok:true});
    else if(act==='jno')sendAction({kind:'judge',ok:false});
    else if(act==='next')sendAction({kind:'next'});
    return;
  }
  // anular/restaurar pista en fase de comparación
  var c=e.target.closest('.j-clue.j-clickable');
  if(c&&V&&V.round&&V.round.phase==='compare'&&V.me!==V.round.guesser){
    sendAction({kind:'strike',who:+c.getAttribute('data-who')});
  }
});
el('j-game').addEventListener('keydown',function(e){
  if(e.key!=='Enter')return;
  if(e.target.id==='j-clueinput'){
    var b=el('j-game').querySelector('[data-act="clue"]');if(b)b.click();
  }else if(e.target.id==='j-guessinput'){
    var b2=el('j-game').querySelector('[data-act="guess"]');if(b2)b2.click();
  }
});
// código por URL: ?sala=ABCD
var m=location.search.match(/sala=([A-Za-z0-9]{4})/);
if(m)el('j-codein').value=m[1].toUpperCase();
})();
</script>

## ¿Cómo se juega?

Just One es **cooperativo**: entre todos intentáis acertar **13 palabras misteriosas**. En cada carta uno de vosotros es el **adivinador** 🕵️ y los demás le dan pistas... con trampa.

Cada carta funciona así:

1. El adivinador **elige un número del 1 al 5** sin ver la carta (los demás sí ven sus 5 palabras): esa es la palabra secreta, que ven todos menos él.
2. Cada uno de los demás escribe **una sola palabra** como pista, en secreto.
3. Se comparan las pistas: **anulad a mano** las repetidas, las derivadas de la palabra secreta o las demasiado parecidas — el adivinador nunca las verá. Sed honestos 😇.
4. El adivinador ve las pistas supervivientes y tiene **un único intento** (o puede pasar).

**Puntuación**: acierto = 1 punto ⭐. Pasar = se pierde esa carta. Fallar = se pierde esa carta **y otra más** de castigo. La partida acaba cuando se agotan las cartas: la puntuación máxima es 13.

El truco está en las pistas: si es demasiado obvia, alguien más la habrá escrito y se anularán entre sí. Si es demasiado rara, no servirá de nada. 🤹
