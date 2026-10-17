
<!DOCTYPE html>
<html lang="pt-BR">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1">
<title>RS La Casa de Papel</title>
<style>
*{box-sizing:border-box}
body{margin:0;background:#111;color:#fff;font-family:Arial,sans-serif}
header{background:#a20d18;text-align:center;padding:30px 15px;border-bottom:4px solid #d9b66c}
header h1{margin:0;font-size:clamp(25px,6vw,42px)}
header p{margin-bottom:0}
.aviso{background:#fff0cc;color:#402800;text-align:center;padding:12px;font-weight:bold}
main{max-width:1000px;margin:auto;padding:22px 15px}
h2{margin-top:10px}
.descricao{color:#ddd;line-height:1.6}
.beneficios,.produtos{display:grid;grid-template-columns:repeat(auto-fit,minmax(210px,1fr));gap:15px}
.card{background:#202020;border:1px solid #444;border-radius:12px;padding:17px}
.card h3{color:#e3bd70}
.card p{color:#ddd;line-height:1.5}
.nota{height:105px;border:2px dashed white;border-radius:8px;display:flex;align-items:center;justify-content:center;text-align:center;font-weight:bold;background:linear-gradient(135deg,#32764e,#183425);position:relative;overflow:hidden}
.nota.azul{background:linear-gradient(135deg,#426bb2,#192c50)}
.nota.dourada{background:linear-gradient(135deg,#a36d39,#503016)}
.nota.roxa{background:linear-gradient(135deg,#8055a1,#3a204b)}
.nota span{background:#111d;padding:6px;font-size:12px}
.preco{font-size:22px;font-weight:bold;color:#e3bd70;margin:12px 0}
button{border:0;border-radius:8px;padding:12px;cursor:pointer;font-weight:bold}
.adicionar{background:#b51220;color:white;width:100%}
.secundario{background:#333;color:white}
.carrinho{margin-top:28px;background:#202020;padding:18px;border-radius:12px}
.item{display:flex;justify-content:space-between;align-items:center;gap:12px;padding:12px 0;border-bottom:1px solid #444}
.total{display:flex;justify-content:space-between;font-size:21px;font-weight:bold;margin:20px 0}
.pix{margin-top:18px;padding:16px;background:#111;border:1px solid #444;border-radius:10px;overflow-wrap:anywhere}
.chave{background:#292929;padding:12px;border-radius:6px;font-family:monospace}
.pequeno{font-size:13px;color:#bbb;line-height:1.5}
footer{text-align:center;padding:22px;color:#aaa}
</style>
</head>
<body>

<header>
<h1>RS LA CASA DE PAPEL</h1>
<p>Notas cenográficas para brincadeiras e atividades criativas</p>
</header>

<div class="aviso">
DINHEIRO FICTÍCIO — PRODUTO DE BRINCADEIRA, SEM VALOR MONETÁRIO.
</div>

<main>
<h2>QUALIDADE PARA BRINCAR</h2>
<p class="descricao">
Nossas notas cenográficas são feitas para brincadeiras, jogos de faz de conta,
teatrinhos e atividades criativas. Os modelos são fictícios e devem ser
claramente identificados como dinheiro de brincadeira, sem imitar os recursos
de segurança das cédulas verdadeiras.
</p>

<div class="beneficios">
<div class="card">
<h3>MODELOS CRIATIVOS</h3>
<p>Notas fictícias para jogos, cenários e atividades recreativas.</p>
</div>
<div class="card">
<h3>IDENTIFICAÇÃO FICTÍCIA</h3>
<p>Modelos identificados como dinheiro de brincadeira e sem valor monetário.</p>
</div>
<div class="card">
<h3>USO CENOGRÁFICO</h3>
<p>Para atividades artísticas e brincadeiras, nunca para pagamentos reais.</p>
</div>
</div>

<h2>Nosso catálogo</h2>
<p class="pequeno">Preços ilustrativos: altere-os para os preços reais dos seus produtos.</p>
<div class="produtos" id="produtos"></div>

<div class="carrinho">
<h2>🛒 Seu carrinho</h2>
<div id="itens">Seu carrinho está vazio.</div>
<div class="total"><span>Total:</span><span id="total">R$ 0,00</span></div>
<button class="adicionar" onclick="finalizar()">Continuar para pagamento Pix</button>
<button class="secundario" onclick="limpar()">Limpar carrinho</button>

<div class="pix" id="pagamento" hidden>
<h3>Pagamento via Pix</h3>
<p>Use a chave Pix informada pelo vendedor:</p>
<div class="chave" id="chave">elenildoj49@gmail.com</div>
<br>
<button class="secundario" onclick="copiarPix()">Copiar chave Pix</button>
<button class="adicionar" onclick="compartilharPedido()">Compartilhar pedido</button>
<p class="pequeno">
O site não confirma pagamentos automaticamente. Verifique o recebimento
no aplicativo do seu banco antes de liberar o pedido. Confira também se a
chave Pix está correta no seu banco.
</p>
<p id="resumo" class="pequeno"></p>
</div>
</div>
</main>

<footer>RS La Casa de Papel · Produtos cenográficos de brincadeira</footer>

<script>
const PIX = "elenildoj49@gmail.com";

const produtos = [
 {id:1,nome:"Kit Cenográfico Verde",preco:12.90,cor:""},
 {id:2,nome:"Kit Cenográfico Azul",preco:14.90,cor:"azul"},
 {id:3,nome:"Kit Cenográfico Dourado",preco:16.90,cor:"dourada"},
 {id:4,nome:"Kit Cenográfico Roxo",preco:18.90,cor:"roxa"}
];

const carrinho = {};

function dinheiro(valor){
 return valor.toLocaleString("pt-BR",{style:"currency",currency:"BRL"});
}

function montarProdutos(){
 const area=document.getElementById("produtos");
 produtos.forEach(p=>{
  const card=document.createElement("div");
  card.className="card";
  card.innerHTML=`
   <div class="nota ${p.cor}">
    <span>DINHEIRO FICTÍCIO</span>
   </div>
   <h3>${p.nome}</h3>
   <p>Modelo cenográfico para brincadeiras e jogos.</p>
   <div class="preco">${dinheiro(p.preco)}</div>
   <button class="adicionar">Adicionar ao carrinho</button>`;
  card.querySelector("button").onclick=()=>{
   carrinho[p.id]=(carrinho[p.id]||0)+1;
   atualizarCarrinho();
  };
  area.appendChild(card);
 });
}

function atualizarCarrinho(){
 const area=document.getElementById("itens");
 area.innerHTML="";
 const selecionados=produtos.filter(p=>carrinho[p.id]);

 if(!selecionados.length){
  area.textContent="Seu carrinho está vazio.";
 }

 selecionados.forEach(p=>{
  const linha=document.createElement("div");
  linha.className="item";
  const info=document.createElement("span");
  info.textContent=p.nome+" — "+dinheiro(p.preco);
  const controles=document.createElement("div");
  const menos=document.createElement("button");
  menos.className="secundario";
  menos.textContent="−";
  menos.onclick=()=>{
   carrinho[p.id]--;
   if(carrinho[p.id]<=0) delete carrinho[p.id];
   atualizarCarrinho();
  };
  const quantidade=document.createElement("span");
  quantidade.textContent=" "+carrinho[p.id]+" ";
  const mais=document.createElement("button");
  mais.className="secundario";
  mais.textContent="+";
  mais.onclick=()=>{
   carrinho[p.id]++;
   atualizarCarrinho();
  };
  controles.append(menos,quantidade,mais);
  linha.append(info,controles);
  area.appendChild(linha);
 });

 document.getElementById("total").textContent=dinheiro(
  produtos.reduce((s,p)=>s+p.preco*(carrinho[p.id]||0),0)
 );
}

function textoPedido(){
 const linhas=produtos.filter(p=>carrinho[p.id]).map(
  p=>`${carrinho[p.id]} x ${p.nome} — ${dinheiro(p.preco*carrinho[p.id])}`
 );
 const total=produtos.reduce((s,p)=>s+p.preco*(carrinho[p.id]||0),0);
 return "Pedido RS La Casa de Papel\n"+
 linhas.join("\n")+"\nTotal: "+dinheiro(total)+
 "\nChave Pix: "+PIX;
}

function finalizar(){
 if(!produtos.some(p=>carrinho[p.id])){
  alert("Adicione algum produto ao carrinho primeiro.");
  return;
 }
 document.getElementById("pagamento").hidden=false;
 document.getElementById("resumo").textContent=textoPedido();
 document.getElementById("pagamento").scrollIntoView({behavior:"smooth"});
}

function limpar(){
 Object.keys(carrinho).forEach(k=>delete carrinho[k]);
 atualizarCarrinho();
 document.getElementById("pagamento").hidden=true;
}

async function copiarPix(){
 try{
  await navigator.clipboard.writeText(PIX);
  alert("Chave Pix copiada!");
 }catch(e){
  prompt("Copie a chave Pix:",PIX);
 }
}

async function compartilharPedido(){
 const texto=textoPedido();
 if(navigator.share){
  try{await navigator.share({title:"Meu pedido",text:texto});}
  catch(e){}
 }else{
  try{
   await navigator.clipboard.writeText(texto);
   alert("Resumo copiado! Você pode enviá-lo ao vendedor.");
  }catch(e){
   prompt("Copie seu pedido:",texto);
  }
 }
}

montarProdutos();
atualizarCarrinho();
</script>
</body>
</html>
