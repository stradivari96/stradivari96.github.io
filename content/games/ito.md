---
title: "🧵 ito"
date: 2026-07-15
searchHidden: true
robotsNoIndex: true
showToc: false
ShowReadingTime: false
ShowShareButtons: false
comments: false
---

Juego cooperativo para 2-10 jugadores. Las pistas se dicen **hablando** (en persona o por llamada); la web solo lleva las cartas. Uno crea la sala y comparte el código (o el enlace) con los demás. La partida vive en el navegador del anfitrión: si lo cierra, se corta el hilo.

<style>
#ito-app{--ibg:#1b1f27;--ipanel:#252b36;--itext:#e8eaf0;--imut:#9aa3b2;
  background:var(--ibg);color:var(--itext);border-radius:12px;padding:16px;
  font-family:system-ui,sans-serif;font-size:15px;line-height:1.4}
#ito-app h3{margin:10px 0 6px;color:var(--itext)}
#ito-app input{background:#11141a;color:var(--itext);border:1px solid #3a4252;
  border-radius:8px;padding:8px 10px;font-size:15px;max-width:160px}
#ito-app button{background:#3b82f6;color:#fff;border:0;border-radius:8px;
  padding:8px 14px;font-size:14px;cursor:pointer;margin:2px}
#ito-app button:disabled{background:#3a4252;color:var(--imut);cursor:not-allowed}
#ito-app .i-row{display:flex;flex-wrap:wrap;gap:8px;align-items:center;margin:8px 0}
#ito-app .i-panel{background:var(--ipanel);border-radius:10px;padding:10px 12px;margin:8px 0}
#ito-app .icard{width:52px;height:72px;border-radius:8px;display:inline-flex;
  align-items:center;justify-content:center;font-size:22px;font-weight:700;
  border:2px solid rgba(0,0,0,.35);user-select:none;color:#fff;
  text-shadow:0 1px 2px rgba(0,0,0,.5);position:relative;flex:none;cursor:default}
#ito-app .icard.i-click{cursor:pointer}
#ito-app .icard.i-sel{outline:3px solid #fff;outline-offset:2px}
#ito-app .i-back{background:linear-gradient(135deg,#312e81,#4c1d95);font-size:26px}
#ito-app .i-face{background:#f8fafc;color:#1e293b;text-shadow:none}
#ito-app .i-zero{background:transparent;border:2px dashed #4a5366;color:#6a7488}
#ito-app .i-ok{border-color:#22c55e;box-shadow:0 0 8px 2px rgba(34,197,94,.5)}
#ito-app .i-bad{border-color:#ef4444;box-shadow:0 0 8px 2px rgba(239,68,68,.6)}
#ito-app .i-mine{outline:2px solid #fbbf24;outline-offset:2px}
#ito-app .i-cardwrap{display:flex;flex-direction:column;align-items:center;gap:3px;flex:none}
#ito-app .i-owner{font-size:11px;font-weight:600;max-width:56px;overflow:hidden;
  text-overflow:ellipsis;white-space:nowrap;line-height:1.2}
#ito-app .i-new{animation:i-deal .45s ease}
@keyframes i-deal{from{transform:translateY(-40px) scale(.5);opacity:0}to{transform:none;opacity:1}}
#ito-app .i-pop{animation:i-pop .5s ease}
@keyframes i-pop{0%{transform:scale(1)}40%{transform:scale(1.25)}100%{transform:scale(1)}}
#ito-app .i-thread{display:flex;align-items:flex-start;flex-wrap:wrap;gap:6px;min-height:100px;
  padding:8px 4px}
#ito-app .i-slot{width:26px;height:72px;border-radius:8px;border:2px dashed #fbbf24;
  background:rgba(251,191,36,.12);color:#fbbf24;font-size:18px;font-weight:700;
  cursor:pointer;flex:none;padding:0;margin:0;align-self:flex-start}
#ito-app .i-slot:hover{background:rgba(251,191,36,.3)}
#ito-app .i-cut{align-self:center;font-size:20px;flex:none}
#ito-app .i-cat{font-size:18px;font-weight:700;color:#fbbf24}
#ito-app .i-scale{font-size:12px;color:var(--imut)}
#ito-app .i-log{max-height:140px;overflow-y:auto;font-size:13px;color:var(--imut)}
#ito-app .i-log div{padding:1px 0}
#ito-app .i-banner{background:#3b2f12;border:1px solid #fbbf24;border-radius:10px;
  padding:10px 14px;margin:8px 0;font-weight:600}
#ito-app .i-toast{position:fixed;bottom:24px;left:50%;transform:translateX(-50%);
  background:#dc2626;color:#fff;padding:10px 18px;border-radius:10px;z-index:99;
  box-shadow:0 4px 14px rgba(0,0,0,.4)}
#ito-app .i-code{font-size:22px;font-weight:800;letter-spacing:4px;color:#fbbf24}
#ito-app .i-mut{color:var(--imut);font-size:13px}
#ito-app button.i-catbtn{background:#2d3748;border:1px solid #4a5366;font-size:15px;
  padding:10px 14px;max-width:100%;white-space:normal;text-align:left}
#ito-app button.i-catbtn:hover{border-color:#fbbf24}
</style>
<div id="ito-app">
  <div id="i-setup">
    <div class="i-row"><label>Tu nombre: <input id="i-name" maxlength="14" placeholder="Nombre"></label></div>
    <div class="i-row"><button id="i-create">🧵 Crear sala</button></div>
    <div class="i-row"><input id="i-code" maxlength="4" placeholder="CÓDIGO" style="text-transform:uppercase;width:110px"><button id="i-join">Unirse</button></div>
    <div id="i-setupmsg" class="i-mut"></div>
  </div>
  <div id="i-game" style="display:none"></div>
</div>

<script src="https://unpkg.com/peerjs@1.5.4/dist/peerjs.min.js"></script>

<script>
(function(){
'use strict';
// ---------- constantes ----------
var PREFIX='ito-xiang-';
var MAXP=10;
var PCOLORS=['#ef4444','#3b82f6','#22c55e','#eab308','#a855f7','#ec4899','#14b8a6','#f97316','#64748b','#84cc16'];
var CATS=[ // [tema, 1 = ..., 100 = ...]
  // --- Entretenimiento y ocio ---
  ['Películas','Pésima / Aburrida','Obra maestra'],
  ['Juegos de mesa','Aburridísimo','Tremendamente adictivo'],
  ['Canciones para cantar','Deprimentes','Eufóricas'],
  ['Libros','Incomprensible','Fascinante'],
  ['Series de televisión','Pérdida de tiempo','Imposible dejar de ver'],
  ['Videojuegos','Frustrante / Roto','Experiencia perfecta'],
  ['Deportes para ver','Soporífero','Pura adrenalina'],
  ['Canales de YouTube','Vergüenza ajena','Contenido de altísima calidad'],
  ['Conciertos','Suplicio sonoro','Experiencia extrasensorial'],
  ['Hobbies','Completamente inútil','Muy productivo y gratificante'],
  ['Programas de televisión','Telebasura insoportable','Cultura y entretenimiento puro'],
  ['Obras de teatro','Para dormirse','Emocionante hasta las lágrimas'],
  ['Festivales','Desorganización total','El evento de tu vida'],
  ['Espectáculos de magia','Trucos baratos y evidentes','Ilusionismo inexplicable'],
  ['Chistes','Sin ninguna gracia','Llorar de la risa'],
  ['Atracciones de feria','Para bebés','Terroríficamente extremas'],
  ['Museos','Aburrimiento mortal','Interesantísimo'],
  ['Documentales','Monótono','Te cambia la forma de ver el mundo'],
  ['Podcasts','Ruido de fondo inútil','Aprendizaje indispensable'],
  ['Fiestas','Incómoda y aburrida','La mejor noche de tu vida'],
  // --- Comida y bebida ---
  ['Comida rápida','Asquerosa','Deliciosa'],
  ['Sabores de helado','Repugnante','Manjar de dioses'],
  ['Desayunos','Indigesto','Nutritivo y sabroso'],
  ['Bebidas alcohólicas','Sabe a colonia','Elixir exquisito'],
  ['Comida picante','Totalmente insípido','Fuego insoportable'],
  ['Ingredientes de pizza','Un sacrilegio','El toque perfecto'],
  ['Snacks / Aperitivos','Sabe a cartón','Imposible comer solo uno'],
  ['Frutas','Agua sin sabor','Dulzor y frescura perfecta'],
  ['Verduras','Textura horrible','Sabrosa y crujiente'],
  ['Tipos de carne','Dura como la suela de un zapato','Se deshace en la boca'],
  ['Pescados y mariscos','Olor nauseabundo','Sabor a mar fresco'],
  ['Tipos de queso','Sabor a plástico','Sabor intenso y gourmet'],
  ['Salsas','Arruina el plato','Mejora cualquier comida'],
  ['Bebidas calientes','Agua sucia','Reconfortante'],
  ['Comida callejera','Intoxicación segura','Auténtico manjar'],
  ['Platos tradicionales','Sobrevalorados','Patrimonio de la humanidad'],
  ['Tipos de pan','Duro y seco','Crujiente y esponjoso'],
  // --- Vida cotidiana y objetos ---
  ['Regalos de cumpleaños','Decepcionantes','El mejor regalo posible'],
  ['Tareas domésticas','Rápidas y fáciles','Agotadoras y odiosas'],
  ['Ropa de invierno','Inútil contra el frío','Aislamiento térmico perfecto'],
  ['Muebles','Incomodísimos','Confort absoluto'],
  ['Herramientas','Se rompen a la primera','Reparan cualquier cosa'],
  ['Calzado','Destroza los pies','Como caminar sobre nubes'],
  ['Electrodomésticos','Totalmente prescindibles','Imprescindibles para vivir'],
  ['Aplicaciones del móvil','Solo ocupan espacio','Las uso todos los días'],
  ['Regalos de boda','Tacaños','Tremendamente generosos'],
  ['Accesorios de moda','Ridículos','Dan muchísimo estilo'],
  ['Cosas que llevar en el bolso/bolsillo','Peso inútil','Te salvan de apuros'],
  ['Juguetes infantiles','Peligrosos y frágiles','Educativos y duraderos'],
  ['Objetos de escritorio','Estorbo','Aumentan la productividad'],
  ['Cosas que compras por Internet','Estafa total','La mejor compra del año'],
  ['Instrumentos musicales','Ruido insoportable','Sonido celestial'],
  ['Coches','Chatarra que te deja tirado','Lujo, velocidad y fiabilidad'],
  ['Inventos cotidianos','Absurdos','Revolucionarios'],
  ['Materiales de construcción','Endebles','Indestructibles'],
  ['Productos de higiene','Irritantes','Sensación de limpieza pura'],
  // --- Situaciones sociales y relaciones ---
  ['Excusas para llegar tarde','Poco creíbles / Patéticas','Totalmente justificadas'],
  ['Lugares para una primera cita','Incómodos y desastrosos','Románticos e ideales'],
  ['Mentiras piadosas','Se notan a kilómetros','Indetectables y necesarias'],
  ['Formas de romper con alguien','Cobardes y crueles','Maduras y empáticas'],
  ['Formas de saludar','Muy frías e incómodas','Cálidas y amigables'],
  ['Temas de conversación','Generan silencios incómodos','Fascinantes para todos'],
  ['Maneras de pedir perdón','Falsas y forzadas','Sinceras y redentoras'],
  ['Sitios para conocer gente','Pésimo ambiente','Lugar ideal para socializar'],
  ['Cosas que dan vergüenza ajena','Ligeramente incómodo','Trágame tierra'],
  ['Bromas','Pesadas y de mal gusto','Ingeniosas e inofensivas'],
  ['Gestos románticos','Cursis y empalagosos','Conmovedores'],
  ['Comportamientos en público','Mala educación extrema','Civismo ejemplar'],
  ['Formas de llamar la atención','Patéticas','Carismáticas'],
  ['Consejos de vida','Tópicos inútiles','Sabiduría profunda'],
  ['Formas de vengarse','Infantiles','Maquiavélicas y perfectas'],
  ['Secretos','Tonterías sin importancia','Pueden destruir vidas'],
  ['Compañeros de piso','Pesadilla de convivencia','Amigos para toda la vida'],
  ['Preguntas incómodas','Fáciles de evadir','Te dejan sin palabras'],
  ['Cumplidos','Falsos y genéricos','Suben la autoestima al máximo'],
  ['Discusiones','Peleas por tonterías','Debates intelectuales profundos'],
  // --- Sensaciones y emociones ---
  ['Miedos irracionales','Inofensivos / Dan risa','Aterradores y paralizantes'],
  ['Nivel de dolor físico','Imperceptible','Insoportable'],
  ['Cosas que huelen mal','Soportable','Te provoca náuseas'],
  ['Ruidos molestos','Fáciles de ignorar','Desquiciantes'],
  ['Manías personales','Peculiaridad curiosa','Condición insoportable'],
  ['Cosas que te hacen llorar','Un poquito de pena','Tragedia devastadora'],
  ['Razones para no dormir','Ligero desvelo','Ansiedad máxima'],
  ['Nivel de estrés','Relajación total','Ataque de nervios'],
  ['Sensación de alivio','Casi imperceptible','Te quitas un peso de encima enorme'],
  ['Cosas que dan buena suerte','Superstición tonta','Te cambian el destino'],
  ['Sensaciones placenteras','Agradables sin más','Éxtasis puro'],
  ['Cosas que te hacen reír','Sonrisa ligera','Carcajada sin poder respirar'],
  ['Nivel de cansancio','Ligeramente fatigado','Exhausto, sin poder moverte'],
  ['Sensación de frío','Fresco agradable','Hipotermia dolorosa'],
  ['Sensación de calor','Calorcito reconfortante','Asfixiante e insoportable'],
  ['Cosas que te sorprenden','Predecible','Te deja boquiabierto'],
  ['Cosas que dan asco','Desagradable a la vista','Revuelta de estómago inmediata'],
  // --- Naturaleza, lugares y viajes ---
  ['Animales como mascota','Peligrosos e indomables','Compañeros fieles e ideales'],
  ['Formas de viajar','Incómodas y tortuosas','Lujosas y placenteras'],
  ['Tipos de clima','Horrible para salir de casa','Día perfecto y soleado'],
  ['Sitios para ir de vacaciones','Decepcionantes y feos','Paradisíacos'],
  ['Insectos','Inofensivos','Peligrosos y aterradores'],
  ['Criaturas del océano','Diminutas e inofensivas','Monstruos de las profundidades'],
  ['Cosas que hacer en la playa','Aburridas e incómodas','Relajantes y divertidas'],
  ['Sonidos de la naturaleza','Estridentes','Relajantes (ASMR puro)'],
  ['Sitios para vivir','Cuchitril inhabitable','Mansión de ensueño'],
  ['Paisajes','Desoladores y feos','Impresionantes y majestuosos'],
  ['Fenómenos meteorológicos','Imperceptibles','Catastróficos'],
  ['Cosas que hacer en la nieve','Frías y aburridas','Pura diversión invernal'],
  ['Parques naturales','Secos y sin vida','Llenos de biodiversidad'],
  ['Tipos de bosque','Escasos y aburridos','Mágicos y frondosos'],
  ['Cosas que te encuentras en la calle','Basura asquerosa','Tesoros valiosos'],
  ['Sitios para dormir','Duros e incómodos','Descanso profundo asegurado'],
  ['Rutas de senderismo','Paseo aburrido por asfalto','Aventura espectacular'],
  ['Ciudades del mundo','Caóticas y sucias','Llenas de cultura y belleza'],
  ['Transporte público','Lento y agobiante','Rápido y eficientísimo'],
  ['Sitios para esconderse','Muy evidentes','Imposibles de encontrar'],
  // --- Habilidades, profesiones y vida ---
  ['Profesiones','Mal pagadas y estresantes','El trabajo de tus sueños'],
  ['Habilidades inútiles','No sirven absolutamente para nada','Impresionan a todo el mundo'],
  ['Asignaturas del colegio','Inútiles y aburridas','Apasionantes y prácticas'],
  ['Formas de ganar dinero','Explotadoras y miserables','Negocio redondo y ético'],
  ['Deportes para practicar','Lesivos y aburridos','Saludables y divertidos'],
  ['Cosas que hacer un domingo','Deprimentes','Planazos inolvidables'],
  ['Idiomas para aprender','Inútiles globalmente','Te abren todas las puertas'],
  ['Carreras universitarias','Sin salida laboral','Garantizan tu futuro'],
  ['Cosas que hacer con 1000 euros','Gasto estúpido y efímero','Inversión muy inteligente'],
  ['Cosas que hacer antes de morir','Irrelevantes','Logros vitales épicos'],
  ['Hábitos diarios','Destructivos para la salud','Mejoran tu vida drásticamente'],
  ['Formas de hacer ejercicio','Tortura absoluta','Altamente motivadoras'],
  ['Trabajos físicos','Te destrozan el cuerpo','Te mantienen muy en forma'],
  ['Talentos ocultos','Decepcionantes','Dignos de un programa de talentos'],
  ['Formas de ahorrar','Vivir en la miseria','Eficiencia económica perfecta'],
  ['Cosas que te hacen parecer inteligente','Pedantes y falsas','Demuestran genialidad'],
  ['Estilos de liderazgo','Tóxicos y dictatoriales','Inspiradores y justos'],
  ['Formas de estudiar','Pérdida de tiempo','Retención de memoria perfecta'],
  ['Cursos online','Vendehúmos','Te cambian la carrera profesional'],
  // --- Ficción, fantasía y situaciones extremas ---
  ['Superpoderes','Completamente inútiles','Te hacen invencible'],
  ['Cosas que llevar a una isla desierta','No sirven para nada','Garantizan tu supervivencia'],
  ['Armas contra zombies','Muerte segura','Destrucción masiva eficaz'],
  ['Formas de morir','Ridículas y patéticas','Épicas y heroicas'],
  ['Monstruos míticos','Débiles e inofensivos','Pesadillas destructoras'],
  ['Objetos mágicos','Trucos de feria','Poder cósmico absoluto'],
  ['Vehículos para una persecución','Lentos y fáciles de atrapar','Escape garantizado y veloz'],
  ['Villanos de películas','Patéticos y sin motivos','Mentes maestras aterradoras'],
  ['Cosas que pasan en el espacio','Flotar aburrido','Peligros cósmicos mortales'],
  ['Mascotas exóticas','Aburridas o invisibles','Fascinantes y majestuosas'],
  ['Hechizos de magia','Efectos ridículos','Alteran la realidad'],
  ['Cosas que pasan en películas de terror','Clichés absurdos','Giros de guion brillantes'],
  ['Sitios para esconder un cadáver (ficticio)','Te pillan en un minuto','El crimen perfecto'],
  ['Naves espaciales','Chatarra voladora','Tecnología punta intergaláctica'],
  ['Alienígenas','Bacterias inofensivas','Civilización superior conquistadora'],
  ['Cosas que hacer en un apocalipsis','Rendirse el primer día','Reconstruir la sociedad'],
  ['Armaduras de combate','Pesadas e inútiles','Protección impenetrable'],
  ['Clases de personajes de rol (RPG)','Injugables y débiles','Daño masivo y versátiles'],
  // --- Miscelánea ---
  ['Cosas que encuentras en el sofá','Miseria y suciedad','Tesoros perdidos'],
  ['Cosas que hacer en un atasco','Aumentan tu furia','Te relajan muchísimo'],
  ['Cosas que olvidarías al salir de casa','Sin importancia','Te arruinan el día'],
  ['Fragilidad de las cosas','Extremadamente duraderas','Se rompen solo con mirarlas'],
  ['Cosas que se pueden coleccionar','Acumulación de basura','Inversiones millonarias'],
  ['Palabras para insultar','Infantiles y flojas','Destrucción psicológica total'],
  ['Palabras bonitas','Cursis y vacías','Poesía pura'],
  ['Sitios web','Llenos de virus y estafas','Imprescindibles para el día a día'],
  ['Cosas que hacer en un avión','Agobiantes y aburridas','Hacen que el tiempo vuele'],
  ['Cosas que puedes comprar por 1 euro','Timada absoluta','Una auténtica ganga'],
  ['Sitios para hacer una foto','Fondos horribles','Dignos de ganar premios'],
  ['Objetos de los años 90','Trastos obsoletos','Pura nostalgia invaluable'],
  ['Cosas que hacer con cajas de cartón','Simplemente tirarlas','Construir cosas increíbles'],
  ['Sitios para estar en silencio','Imposible concentrarse','Paz absoluta y espiritual'],
  ['Cosas que hacer con hielo','Mojarlo todo inútilmente','Obras de arte efímeras'],
  ['Cosas que compras pero nunca usas','Derroche de dinero total','Te salvan en emergencias'],
  ['Formas de caminar','Torpes y ridículas','Elegantes y seguras']
];

// ---------- estado ----------
var peer=null,isHost=false,hostConn=null,roomCode='';
var G=null;   // estado completo (solo host)
var V=null;   // vista personalizada (lo que se renderiza)
var sel=null; // id de mi carta seleccionada
var myName='';

function el(id){return document.getElementById(id);}
function esc(s){return String(s).replace(/[&<>"']/g,function(c){
  return {'&':'&amp;','<':'&lt;','>':'&gt;','"':'&quot;',"'":'&#39;'}[c];});}
function toast(msg){
  var t=document.createElement('div');t.className='i-toast';t.textContent=msg;
  el('ito-app').appendChild(t);setTimeout(function(){t.remove();},3500);
}

// ---------- lógica de juego (host) ----------
function newGame(){
  G={phase:'lobby',players:[],hands:[],thread:[],cat:null,catPair:null,
     level:0,gameNum:0,fails:0,revealTimer:null,result:null,log:[],
     failSeq:0,failIdx:0,successSeq:0,successIdx:0};
}
function drawCat(){
  var a=Math.floor(Math.random()*CATS.length),b;
  do{b=Math.floor(Math.random()*CATS.length);}while(b===a);
  G.catPair=[CATS[a],CATS[b]];
}
function startGame(){
  if(G.revealTimer){clearInterval(G.revealTimer);G.revealTimer=null;}
  var P=G.players.length;
  var extra=Math.min(G.level,P),total=P+extra;
  // números únicos del 1 al 100 barajados
  var nums=[];for(var i=1;i<=100;i++)nums.push(i);
  for(var j=nums.length-1;j>0;j--){var k=Math.floor(Math.random()*(j+1));
    var t=nums[j];nums[j]=nums[k];nums[k]=t;}
  G.thread=[];G.result=null;G.fails=0;G.cat=null;drawCat();
  G.hands=G.players.map(function(){return [];});
  var deal=0;
  for(var p=0;p<P;p++)G.hands[p].push({id:deal,n:nums[deal++],owner:p,revealed:false,ok:null});
  for(var e=0;e<extra;e++){var r=(G.gameNum+e)%P;
    G.hands[r].push({id:deal,n:nums[deal++],owner:r,revealed:false,ok:null});}
  G.hands.forEach(function(h){h.sort(function(a,b){return a.n-b.n;});});
  G.phase='category';
  glog('🧵 Hilo nº '+(G.gameNum+1)+' — '+total+' cartas en juego');
  broadcast();
}
function pname(i){return G.players[i].name;}
function glog(m){G.log.push(m);if(G.log.length>60)G.log.shift();}
function placedAll(){return G.hands.every(function(h){return h.length===0;});}
function sendErr(p,msg){
  if(p===0)toast(msg);
  else if(G.players[p].conn)G.players[p].conn.send({t:'error',msg:msg});
}
function revealStep(){
  if(!G||G.phase!=='reveal'){if(G&&G.revealTimer){clearInterval(G.revealTimer);G.revealTimer=null;}return;}
  var idx=G.thread.findIndex(function(c){return !c.revealed;});
  if(idx===-1)return;
  var c=G.thread[idx];
  c.revealed=true;
  c.ok=idx===0?true:c.n>G.thread[idx-1].n;
  if(!c.ok){G.fails++;G.failSeq++;G.failIdx=Math.floor(Math.random()*Math.max(1,FAIL_SOUNDS.length));
    glog('✂️ '+c.n+' de '+pname(c.owner)+' corta el hilo (venía después de '+G.thread[idx-1].n+')');}
  if(idx===G.thread.length-1){
    clearInterval(G.revealTimer);G.revealTimer=null;
    G.phase='ended';
    var win=G.fails===0;
    G.result={win:win,fails:G.fails};
    if(win){
      G.successSeq++;G.successIdx=Math.floor(Math.random()*Math.max(1,SUCCESS_SOUNDS.length));
      if(G.level<G.players.length)G.level++;
      glog('🎆 ¡Hilo perfecto! El desafío crece');
    }else glog('💔 El hilo se ha cortado '+G.fails+(G.fails>1?' veces':' vez'));
  }
  broadcast();
}
function applyAction(p,a){
  if(!G)return;
  if(a.kind==='choosecat'){
    if(p!==0||G.phase!=='category')return;
    G.cat=G.catPair[a.which?1:0];
    G.phase='placing';
    glog('📜 Categoría: '+G.cat[0]);
  }else if(a.kind==='recat'){
    if(p!==0||G.phase!=='category')return;
    drawCat();
  }else if(a.kind==='place'){
    if(G.phase!=='placing')return;
    var card=null,hi=G.hands[p].findIndex(function(c){return c.id===a.cid;});
    if(hi>=0)card=G.hands[p].splice(hi,1)[0];
    else{
      var ti=G.thread.findIndex(function(c){return c.id===a.cid;});
      if(ti===-1||G.thread[ti].owner!==p)return sendErr(p,'Esa carta no es tuya');
      card=G.thread.splice(ti,1)[0];
      if(a.pos>ti)a.pos--;
    }
    var pos=Math.max(0,Math.min(G.thread.length,a.pos));
    G.thread.splice(pos,0,card);
  }else if(a.kind==='resolve'){
    if(p!==0||G.phase!=='placing')return;
    if(!placedAll())return sendErr(p,'Faltan cartas por colocar');
    G.phase='reveal';
    G.thread.forEach(function(c){c.revealed=false;c.ok=null;});
    revealStep();
    G.revealTimer=setInterval(revealStep,1500);
    return; // revealStep ya hace broadcast
  }else if(a.kind==='start'){
    if(p!==0)return;
    if(G.phase==='lobby'&&G.players.length<2)return;
    if(G.phase!=='lobby'&&G.phase!=='ended')return;
    if(G.phase==='ended')G.gameNum++;
    startGame();return;
  }else return;
  broadcast();
}
function viewFor(i){
  return {
    phase:G.phase,me:i,cat:G.cat,catPair:G.catPair,result:G.result,
    level:G.level,gameNum:G.gameNum,fails:G.fails,
    failSeq:G.failSeq,failIdx:G.failIdx,successSeq:G.successSeq,successIdx:G.successIdx,
    names:G.players.map(function(p){return p.name;}),
    connected:G.players.map(function(p){return p.connected;}),
    handCounts:G.hands.map(function(h){return h.length;}),
    myHand:(G.hands[i]||[]).map(function(c){return {id:c.id,n:c.n};}),
    thread:G.thread.map(function(c){
      return {id:c.id,owner:c.owner,revealed:c.revealed,ok:c.ok,
        n:(c.owner===i||c.revealed)?c.n:null};}),
    log:G.log.slice(-30)
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
      if(G.players.length>=MAXP)return c.send({t:'error',msg:'Sala llena (máx 10)'});
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
  myName=el('i-name').value.trim();
  if(!myName)return setupMsg('Pon tu nombre primero');
  var alpha='ABCDEFGHJKLMNPQRSTUVWXYZ23456789';
  roomCode='';for(var i=0;i<4;i++)roomCode+=alpha[Math.floor(Math.random()*alpha.length)];
  setupMsg('Creando sala...');
  peer=new Peer(PREFIX+roomCode);
  peer.on('open',function(){
    isHost=true;newGame();
    G.players.push({name:myName,conn:null,connected:true});
    history.pushState(null,'',location.pathname+'?sala='+roomCode);
    el('i-setup').style.display='none';el('i-game').style.display='block';
    broadcast();
  });
  peer.on('connection',setupHostConn);
  peer.on('error',function(e){
    if(e.type==='unavailable-id')setupMsg('Código ocupado, prueba otra vez');
    else setupMsg('Error de conexión: '+e.type);
  });
}
function joinRoom(){
  myName=el('i-name').value.trim();
  roomCode=el('i-code').value.trim().toUpperCase();
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
        el('i-setup').style.display='none';el('i-game').style.display='block';
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
function setupMsg(m){el('i-setupmsg').textContent=m;}

// ---------- sonidos ----------
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
  'https://www.myinstants.com/media/sounds/no-no-no-la-policia.mp3'
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
  'https://www.myinstants.com/media/sounds/es-la-hora-de-la-paja-video-original-audiotrimmer.mp3'
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
var revealSeen={};
function cardHtml(c){
  // c: {id, owner, n (o null), revealed, ok} — carta del hilo
  var mine=c.owner===V.me;
  var cls='icard '+(c.n===null?'i-back':'i-face')+
    (c.revealed?(c.ok?' i-ok':' i-bad'):'')+
    (mine&&!c.revealed?' i-mine':'')+
    (mine&&V.phase==='placing'?' i-click':'')+
    (sel===c.id?' i-sel':'')+
    (c.revealed&&!revealSeen[c.id]?' i-pop':'');
  if(c.revealed)revealSeen[c.id]=true;
  var label=c.n===null?'🧵':c.n;
  return '<div class="i-cardwrap" data-cid="'+c.id+'"><div class="'+cls+'" data-card="'+c.id+'">'+label+'</div>'+
    '<div class="i-owner" style="color:'+PCOLORS[c.owner%PCOLORS.length]+'">'+esc(V.names[c.owner])+(mine?' (tú)':'')+'</div></div>';
}
function threadHtml(){
  var canPlace=V.phase==='placing'&&sel!==null;
  var selIdx=-1;
  V.thread.forEach(function(c,k){if(c.id===sel)selIdx=k;});
  var h='<div class="i-thread"><div class="i-cardwrap"><div class="icard i-zero">0</div><div class="i-owner">&nbsp;</div></div>';
  for(var p=0;p<=V.thread.length;p++){
    if(canPlace&&p!==selIdx&&p!==selIdx+1)
      h+='<button class="i-slot" data-pos="'+p+'" title="Colocar aquí">＋</button>';
    if(p<V.thread.length){
      var c=V.thread[p];
      if(c.revealed&&c.ok===false)h+='<span class="i-cut">✂️</span>';
      h+=cardHtml(c);
    }
  }
  h+='</div>';
  return h;
}
function animate(prev){
  // FLIP: toda carta con data-cid se desliza desde su posición anterior
  var els=el('i-game').querySelectorAll('[data-cid]');
  for(var i=0;i<els.length;i++){
    var e=els[i],r0=prev[e.getAttribute('data-cid')];
    if(!r0){e.firstChild.classList.add('i-new');continue;}
    var r1=e.getBoundingClientRect(),dx=r0.left-r1.left,dy=r0.top-r1.top;
    if(dx||dy){
      e.style.transition='none';e.style.transform='translate('+dx+'px,'+dy+'px)';
      e.getBoundingClientRect();
      e.style.transition='transform .45s ease';e.style.transform='';
    }
  }
}
function render(){
  if(!V)return;
  if(V.phase==='category')revealSeen={}; // nueva partida: los ids se reutilizan
  var prev={},olds=el('i-game').querySelectorAll('[data-cid]');
  for(var i=0;i<olds.length;i++)
    prev[olds[i].getAttribute('data-cid')]=olds[i].getBoundingClientRect();
  var h='';
  h+='<div class="i-row">Sala: <span class="i-code">'+roomCode+'</span> '+
     '<button data-act="copy">📋 Copiar enlace</button></div>';
  if(V.phase==='lobby'){
    h+='<div class="i-panel"><h3>Jugadores ('+V.names.length+'/10)</h3>';
    V.names.forEach(function(n,i){
      h+='<div style="color:'+PCOLORS[i%PCOLORS.length]+'">'+esc(n)+(i===0?' 👑':'')+(i===V.me?' (tú)':'')+'</div>';});
    h+='</div>';
    if(V.me===0)h+='<button data-act="start" '+(V.names.length<2?'disabled':'')+
      '>🚀 Empezar ('+(V.names.length<2?'mínimo 2':V.names.length+' jugadores')+')</button>';
    else h+='<div class="i-mut">Esperando a que '+esc(V.names[0])+' empiece...</div>';
  }else{
    var total=V.thread.length+V.handCounts.reduce(function(s,n){return s+n;},0);
    h+='<div class="i-row">🧵 Hilo nº <b>'+(V.gameNum+1)+'</b> &nbsp; 🃏 Cartas en juego: <b>'+total+'</b></div>';
    if(V.phase==='ended'){
      var r=V.result;
      h+='<div class="i-banner">'+(r.win?
        '🎆 ¡Hilo perfecto! Todos ganáis'+(V.level<V.names.length?' — la próxima, una carta más':' — ¡nivel máximo!'):
        '💔 El hilo se ha cortado '+r.fails+(r.fails>1?' veces':' vez'))+
        (V.me===0?' <button data-act="start">🔄 '+(r.win?'Siguiente hilo':'Reintentar')+'</button>':'')+
        '</div>';
    }
    // categoría
    if(V.phase==='category'){
      var copt=function(c){return '<b>'+esc(c[0])+'</b><br><span class="i-scale">1 = '+
        esc(c[1])+' &nbsp;·&nbsp; 100 = '+esc(c[2])+'</span>';};
      h+='<div class="i-panel"><h3>📜 Elegid categoría (debatidlo hablando)</h3>';
      if(V.me===0){
        h+='<div class="i-row"><button class="i-catbtn" data-act="cat0">'+copt(V.catPair[0])+'</button>'+
           '<button class="i-catbtn" data-act="cat1">'+copt(V.catPair[1])+'</button>'+
           '<button data-act="recat">🎲 Otras</button></div>';
      }else{
        h+='<div class="i-row"><div class="i-panel">'+copt(V.catPair[0])+'</div>'+
           '<div class="i-panel">'+copt(V.catPair[1])+'</div></div>'+
           '<div class="i-mut">'+esc(V.names[0])+' confirma la elegida</div>';
      }
      h+='</div>';
    }else if(V.cat){
      h+='<div class="i-panel"><span class="i-cat">📜 '+esc(V.cat[0])+'</span><br>'+
         '<span class="i-scale">1 = '+esc(V.cat[1])+' &nbsp;·&nbsp; 100 = '+esc(V.cat[2])+
         '. Da tu pista en voz alta, sin números.</span></div>';
    }
    // el hilo
    h+='<div class="i-panel"><h3>El hilo</h3>'+threadHtml();
    var pending=[];
    V.names.forEach(function(n,i){if(V.handCounts[i]>0)pending.push(esc(n)+(V.handCounts[i]>1?' ×'+V.handCounts[i]:''));});
    if(V.phase==='placing'){
      h+='<div class="i-mut">'+(pending.length?'✋ Faltan por colocar: '+pending.join(', '):'✅ Todas las cartas colocadas')+'</div>';
      if(V.me===0)h+='<button data-act="resolve" '+(pending.length?'disabled':'')+'>🔍 Resolver: revelar el hilo</button>';
    }
    if(V.phase==='reveal')h+='<div class="i-mut">👀 Revelando...</div>';
    h+='</div>';
    // mi mano
    if(V.myHand.length){
      h+='<div class="i-panel"><h3>Tus cartas (solo las ves tú)</h3><div class="i-row">';
      V.myHand.forEach(function(c){
        h+='<div class="i-cardwrap" data-cid="'+c.id+'"><div class="icard i-face'+
           (V.phase==='placing'?' i-click':'')+(sel===c.id?' i-sel':'')+
           '" data-card="'+c.id+'">'+c.n+'</div><div class="i-owner">&nbsp;</div></div>';});
      h+='</div><div class="i-mut">'+(V.phase==='placing'?
        (sel!==null?'Toca un hueco ＋ del hilo para colocarla':'Toca una carta y luego el hueco del hilo donde va'):
        'Piensa tu pista mientras se elige categoría...')+'</div></div>';
    }else if(V.phase==='placing'&&sel===null){
      h+='<div class="i-mut">Ya has colocado tus cartas. Puedes tocar una tuya del hilo para moverla.</div>';
    }
    h+='<div class="i-panel i-log">'+V.log.slice().reverse().map(function(l){
      return '<div>'+esc(l)+'</div>';}).join('')+'</div>';
  }
  el('i-game').innerHTML=h;
  if(V.phase!=='lobby')animate(prev);
  checkSounds();
}

// ---------- eventos ----------
el('i-create').addEventListener('click',createRoom);
el('i-join').addEventListener('click',joinRoom);
el('i-game').addEventListener('click',function(e){
  var b=e.target.closest('[data-act]');
  if(b){
    var act=b.getAttribute('data-act');
    if(act==='copy'){
      navigator.clipboard.writeText(location.origin+location.pathname+'?sala='+roomCode)
        .then(function(){toast('Enlace copiado 📋');});
    }
    else if(act==='start')sendAction({kind:'start'});
    else if(act==='cat0')sendAction({kind:'choosecat',which:0});
    else if(act==='cat1')sendAction({kind:'choosecat',which:1});
    else if(act==='recat')sendAction({kind:'recat'});
    else if(act==='resolve')sendAction({kind:'resolve'});
    return;
  }
  var s=e.target.closest('[data-pos]');
  if(s&&sel!==null&&V&&V.phase==='placing'){
    sendAction({kind:'place',cid:sel,pos:+s.getAttribute('data-pos')});
    return;
  }
  var c=e.target.closest('[data-card]');
  if(c&&V&&V.phase==='placing'){
    var cid=+c.getAttribute('data-card');
    var isMine=V.myHand.some(function(x){return x.id===cid;})||
      V.thread.some(function(x){return x.id===cid&&x.owner===V.me;});
    if(!isMine)return;
    sel=sel===cid?null:cid;
    render();
  }
});
// código por URL: ?sala=ABCD
var m=location.search.match(/sala=([A-Za-z0-9]{4})/);
if(m)el('i-code').value=m[1].toUpperCase();
})();
</script>

## ¿Cómo se juega?

ito es **cooperativo**: o ganáis todos, o nada. Cada uno recibe una carta con un número del **1 al 100** que solo ve su dueño. El objetivo es colocar todas las cartas en el hilo **en orden ascendente**... sin poder decir los números.

1. Se elige una **categoría** entre las dos que salen; cada una trae su escala (ej: *Películas* — 1 = pésima, 100 = obra maestra).
2. Cada jugador da una pista **en voz alta**: un ejemplo de la categoría acorde a su número. Con un 95 en *Películas* dirías "El Padrino"; con un 7, "esa de tiburones voladores". **Prohibido decir números, valores o cantidades.**
3. Con tu pista dicha, coloca tu carta en el hilo donde creas que va (los demás la ven boca abajo; tú ves tu número). Hablad, comparad pistas, y **mueve tu carta** las veces que haga falta — la pista también se puede matizar cuando quieras.
4. Cuando todos estéis de acuerdo, el anfitrión pulsa **Resolver**: las cartas se revelan una a una desde el 0. Si todo va de menor a mayor, ¡victoria! 🎆

**Modo desafío**: cada vez que ganáis, la siguiente partida entra **una carta más en juego** (a alguien le tocará llevar 2 y dar 2 pistas), hasta un máximo de 2 cartas por cabeza. Si perdéis, se repite con las mismas cartas totales.
