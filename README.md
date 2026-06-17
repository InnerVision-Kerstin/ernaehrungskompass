<!DOCTYPE html>
<html lang="de">
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0">
<title>Ernährungskompass · InnerVision</title>
<style>
  @import url('https://fonts.googleapis.com/css2?family=Playfair+Display:wght@400;700&family=Lato:wght@300;400&display=swap');
  :root {
    --sage:#8FA67A;--apricot:#F4C27E;--rose:#D3B1C2;--dark:#2C1F1A;--mauve:#4A3040;--bg:#FAF8F5;
    --pp:#2e7d5e;--pp-bg:#d4f0e4;--g:#5a9e6f;--g-bg:#edf7f0;--n:#888;--n-bg:#f4f4f4;--v:#c0392b;--v-bg:#fdf0ef;
  }
  *{box-sizing:border-box;margin:0;padding:0;}
  body{font-family:'Lato',sans-serif;font-weight:300;background:var(--bg);color:var(--dark);max-width:640px;margin:0 auto;min-height:100vh;}
  header{background:var(--sage);padding:28px 20px 22px;text-align:center;}
  header h1{font-family:'Playfair Display',serif;font-size:26px;color:#fff;font-weight:400;}
  header p{font-size:12px;color:rgba(255,255,255,0.75);margin-top:4px;}
  .bt-sel{display:flex;background:#fff;border-bottom:2px solid #ece7e2;}
  .bt-btn{flex:1;padding:12px 4px;border:none;background:transparent;font-family:'Playfair Display',serif;font-size:18px;color:#bbb;cursor:pointer;border-bottom:3px solid transparent;margin-bottom:-2px;transition:all .2s;}
  .bt-btn span{display:block;font-family:'Lato',sans-serif;font-size:9px;letter-spacing:1px;margin-top:2px;font-weight:300;}
  .bt-btn.active{color:var(--sage);border-bottom-color:var(--sage);}
  .profile-box{margin:14px 16px 0;padding:12px 14px;background:#fff;border-left:3px solid var(--sage);}
  .profile-box h3{font-family:'Playfair Display',serif;font-size:14px;font-weight:400;color:var(--mauve);margin-bottom:6px;}
  .profile-box p{font-size:12px;color:#777;line-height:1.6;}
  .info-box{margin:10px 16px 0;padding:12px 14px;background:#fdf8f0;border-left:3px solid var(--apricot);font-size:12px;color:#666;line-height:1.6;}
  .info-box b{color:var(--mauve);display:block;margin-bottom:4px;font-size:11px;letter-spacing:1px;text-transform:uppercase;}
  .search-wrap{padding:14px 16px 8px;}
  .search-wrap input{width:100%;padding:11px 14px;border:1.5px solid #ddd;border-radius:2px;font-family:'Lato',sans-serif;font-weight:300;font-size:15px;background:#fff;color:var(--dark);outline:none;}
  .search-wrap input:focus{border-color:var(--sage);}
  .search-wrap input::placeholder{color:#bbb;}
  .cat-tabs{padding:0 16px 10px;display:flex;flex-wrap:wrap;gap:6px;}
  .cat-tab{padding:5px 12px;border:1px solid #ddd;border-radius:20px;background:transparent;font-size:12px;font-weight:300;color:var(--mauve);cursor:pointer;transition:all .2s;}
  .cat-tab.active,.cat-tab:hover{background:var(--sage);border-color:var(--sage);color:#fff;}
  .filter-row{padding:0 16px 12px;display:flex;gap:6px;}
  .fb{flex:1;padding:7px 2px;border:none;border-radius:3px;font-size:11px;font-weight:400;cursor:pointer;opacity:.4;transition:all .2s;text-align:center;}
  .fb.active{opacity:1;}
  .fb.pp{background:var(--pp-bg);color:var(--pp);border:1.5px solid var(--pp);}
  .fb.g{background:var(--g-bg);color:var(--g);border:1.5px solid var(--g);}
  .fb.n{background:var(--n-bg);color:var(--n);border:1.5px solid #ccc;}
  .fb.v{background:var(--v-bg);color:var(--v);border:1.5px solid var(--v);}
  .list{padding:0 16px 60px;display:flex;flex-direction:column;gap:8px;}
  .item{background:#fff;padding:12px 14px;display:flex;align-items:center;justify-content:space-between;gap:12px;border-left:3px solid transparent;}
  .item.pp{border-left-color:var(--pp);}
  .item.g{border-left-color:var(--g);}
  .item.n{border-left-color:#ccc;}
  .item.v{border-left-color:var(--v);}
  .item-left{display:flex;align-items:center;gap:10px;flex:1;min-width:0;}
  .item-emoji{font-size:20px;flex-shrink:0;}
  .item-name{font-size:15px;font-weight:400;color:var(--dark);}
  .item-cat{font-size:11px;color:#aaa;margin-top:1px;}
  .badge{padding:3px 9px;border-radius:20px;font-size:11px;font-weight:400;flex-shrink:0;white-space:nowrap;}
  .badge.pp{background:var(--pp-bg);color:var(--pp);}
  .badge.g{background:var(--g-bg);color:var(--g);}
  .badge.n{background:var(--n-bg);color:var(--n);}
  .badge.v{background:var(--v-bg);color:var(--v);}
  .legend{display:flex;gap:10px;padding:8px 16px 12px;font-size:11px;color:#999;flex-wrap:wrap;}
  .legend span{display:flex;align-items:center;gap:4px;}
  .dot{width:8px;height:8px;border-radius:50%;flex-shrink:0;}
  .empty{text-align:center;padding:40px 24px;color:#bbb;font-size:15px;}
  .source{margin:8px 16px;font-size:10px;color:#bbb;line-height:1.5;}
</style>
</head>
<body>
<header>
  <h1>Ernährungskompass</h1>
  <p>Nach D'Adamo · InnerVision · Kerstin Püringer</p>
</header>
<div class="bt-sel" id="btSel"></div>
<div class="profile-box" id="profileBox"></div>
<div id="infoBox"></div>
<div class="search-wrap"><input type="text" id="search" placeholder="Lebensmittel suchen…" oninput="render()"></div>
<div class="legend">
  <span><div class="dot" style="background:var(--pp)"></div> ++ Sehr bekömmlich</span>
  <span><div class="dot" style="background:var(--g)"></div> + Bekömmlich</span>
  <span><div class="dot" style="background:#ccc"></div> Neutral</span>
  <span><div class="dot" style="background:var(--v)"></div> Vermeiden</span>
</div>
<div class="cat-tabs" id="catTabs"></div>
<div class="filter-row">
  <button class="fb pp active" onclick="toggleF('pp',this)">++ Sehr bekömml.</button>
  <button class="fb g active" onclick="toggleF('g',this)">+ Bekömmlich</button>
  <button class="fb n active" onclick="toggleF('n',this)">~ Neutral</button>
  <button class="fb v active" onclick="toggleF('v',this)">✗ Vermeiden</button>
</div>
<div class="list" id="list"></div>
<div class="source">Quelle: D'Adamo, Peter J. „4 Blutgruppen – Vier Strategien für ein gesundes Leben", Piper Verlag. Im Buchhandel erhältlich. · InnerVision · Kerstin Püringer · innervision.at</div>

<script>
const profiles = {
  "0": { name:"0 · Der Jäger", text:"Robuster Verdauungstrakt · Überaktives Immunsystem · Reagiert überempfindlich auf Ernährungsumstellungen · Begegnet Stress am besten mit starker körperlicher Beanspruchung · Braucht leistungsfähigen Stoffwechsel um schlank und energiegeladen zu bleiben" },
  "A": { name:"A · Der Landwirt", text:"Erntet was er sät · Empfindlicher Magen-Darm-Trakt · Tolerantes Immunsystem · Passt sich festen Ernährungs- und Umweltbedingungen gut an · Reagiert auf Stress am besten durch Entspannungstechniken · Braucht pflanzliche Kost um schlank und produktiv zu bleiben" },
  "B": { name:"B · Der Nomade", text:"Ausgeglichen · Starkes Immunsystem · Tolerantes Verdauungssystem · Verträgt die meisten Lebensmittel gut · Verzehrt Milchprodukte · Reagiert auf Stress am besten mit Kreativität · Benötigt Ausgleich zwischen körperlicher und geistiger Tätigkeit um schlank und geistig frisch zu bleiben" },
  "AB": { name:"AB · Der Moderne", text:"Kamelionartige Reaktion auf Veränderungen · Empfindlicher Verdauungstrakt · Übermäßig tolerantes Immunsystem · Reagiert auf Stress am besten seelisch-geistig mit körperlichem Schwung und schöpferischer Energie · Selten: nur 2-5% der Bevölkerung · Kombination aus A und B, eine Art Blutgruppenzentaur" }
};

const infoBoxes = {
  "0": {
    "Getreide & Brot": "Getreide ist die Achillesferse der Blutgruppe 0. Viele Sorten verursachen Entzündungen und bringen den Hormonhaushalt durcheinander. Weizenprodukte vollständig meiden – Gluten stört den Stoffwechsel und begünstigt Gewichtszunahme. Mengenbeschränkung: max. 2 Scheiben Brot pro Woche · max. ½ Tasse Getreide 3x pro Woche · max. ½ Tasse Pasta 3x pro Woche.",
    "Getränke": "Gemüsesäften den Vorrang vor Fruchtsäften geben. Ananassaft hilft gegen Wassereinlagerungen. Bier in Maßen okay, aber nicht beim Abnehmen. Rotwein in bescheidenen Mengen erlaubt, nicht täglich. Kaffee erhöht die Magensäure – Blutgruppe 0 hat ohnehin zu viel. Langsam reduzieren und weglassen. Grüner Tee ist die beste Alternative.",
    "Süßungsmittel & Zusätze": "Keine sehr bekömmlichen Süßungsmittel für Blutgruppe 0. Honig, Zucker und Schokolade schaden nicht, aber nur gelegentlich. Maissirup komplett meiden. Alle sauer eingelegten Lebensmittel reizen die Magenschleimhaut. Essig aller Art meiden. Würzen stattdessen mit Olivenöl, Zitronensaft und Knoblauch."
  },
  "A": {
    "Fleisch & Geflügel": "Die beste Wirkung erzielt Blutgruppe A, wenn Sie auf Fleisch jeder Art verzichtet. Mit einer Paleo-Ernährung werden Sie nicht glücklich. Huhn, Pute und Strauß sind als neutrale Ausnahmen erlaubt.",
    "Milch & Eier": "Fermentierte Milchprodukte sind in kleineren Mengen bekömmlich. Alle Vollmilcherzeugnisse meiden. Eierkonsum auf gelegentliche Bio-Eier beschränken. Fettfreie und durch Bakterienkulturen gesäuerte Milchprodukte bevorzugen.",
    "Getränke": "Jeden Morgen mit einem Glas warmem Wasser mit dem Saft einer halben Zitrone beginnen. Kaffee ist für Blutgruppe A gut – seine Antioxidantien unterstützen Verdauung und Immunsystem. Abwechselnd Kaffee und Grüntee trinken. Reichlich reines Wasser trinken.",
    "Obst": "Dreimal täglich ein Stück Obst essen. Basenbildende Früchte wie Beeren und Pflaumen bevorzugen. Honigmelone ganz meiden. Ananas ist ein ausgezeichnetes verdauungsförderndes Mittel für Blutgruppe A.",
    "Getreide & Brot": "Neutral bedeutet nicht unbegrenzt. Diese Lebensmittel schaden nicht, fördern aber die Gesundheit nicht aktiv. In Maßen verwenden.",
    "Süßungsmittel & Zusätze": "Tipp beim Einkaufen: Zutatenlisten lesen! Maissirup, Dextrose, Aspartam und Maltodextrin sind häufige Zusätze in Fertigprodukten und sollten gemieden werden."
  },
  "B": {
    "Getränke": "Blutgruppe B beschränkt sich bei Getränken am besten auf Kräutertees, Grüntee, Wasser und Säfte. Kaffee, Schwarztee und Wein sind nicht wirklich schädlich, aber das Ziel ist maximale Leistung. Grüntee ist die bessere Alternative. Ambrosia-Morgendrink: 1 TL Leinöl + 1 TL Lezitingranulat + 180-240 ml Obstsaft verrühren und trinken. Lezitingranulat in Reformhäusern erhältlich. Stärkt Stoffwechsel und Immunsystem.",
    "Getreide & Brot": "Weißmehl ist für Blutgruppe B neutral, aber Weizenkleie und Weizenschrot sind zu vermeiden. Durch die starke Verarbeitung verliert Weißmehl die meisten Lektine. Kleie und Schrot enthalten diese Lektine noch vollständig. Hier gilt ausnahmsweise: je verarbeiteter, desto verträglicher."
  },
  "AB": {
    "Getränke": "Den Tag mit einem Glas warmem Wasser mit dem Saft einer halben Zitrone beginnen. Das wirkt blutverdünnend, was für Blutgruppe AB wünschenswert ist, und hilft bei der Ausscheidung. Danach ein Glas verdünnten Grapefruit- oder Papayasaft trinken. Bevorzugt stark basenbildende Fruchtsäfte wie Schwarzkirschsaft, Preiselbeersaft oder Traubensaft."
  }
};

const foods = [
  // FLEISCH & GEFLÜGEL
  {name:"Bison / Büffel",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"n",AB:"v"},
  {name:"Hammel / Schaf",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"pp",AB:"n"},
  {name:"Kalb",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"n",AB:"n"},
  {name:"Kalbsleber",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"n",AB:"n"},
  {name:"Lamm",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"pp",AB:"n"},
  {name:"Rind",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"n",AB:"v"},
  {name:"Rindsleber",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"n",AB:"n"},
  {name:"Rinderzunge",emoji:"🥩",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"n",AB:"v"},
  {name:"Wild",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"pp",AB:"v"},
  {name:"Ziege",emoji:"🥩",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"pp",AB:"n"},
  {name:"Kaninchen",emoji:"🥩",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"pp",AB:"n"},
  {name:"Hirsch",emoji:"🥩",kat:"Fleisch & Geflügel","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Truthahn / Pute",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Fasan",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"n",AB:"n"},
  {name:"Strauß",emoji:"🥩",kat:"Fleisch & Geflügel","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Knochensuppe aus erlaubten Fleischsorten",emoji:"🍲",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"n",AB:"v"},
  {name:"Markknochensuppe",emoji:"🍲",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"n",AB:"v"},
  {name:"Huhn / Hühnerfleisch",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Hühnerleber",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Brathähnchen",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Perlhuhn",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Ente",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Entenleber",emoji:"🥩",kat:"Fleisch & Geflügel","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Gans",emoji:"🍗",kat:"Fleisch & Geflügel","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Gänseleber",emoji:"🥩",kat:"Fleisch & Geflügel","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Kalbsbries",emoji:"🥩",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Pferd",emoji:"🥩",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Rebhuhn",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Rinderherz",emoji:"🥩",kat:"Fleisch & Geflügel","0":"pp",A:"v",B:"v",AB:"v"},
  {name:"Wachtel",emoji:"🍗",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Wildziege",emoji:"🥩",kat:"Fleisch & Geflügel","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Schinken / Speck",emoji:"🥓",kat:"Fleisch & Geflügel","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Schweinefleisch",emoji:"🥓",kat:"Fleisch & Geflügel","0":"v",A:"v",B:"v",AB:"v"},

  // FISCH & MEERESFRÜCHTE
  {name:"Heilbutt",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"pp",A:"v",B:"pp",AB:"v"},
  {name:"Kabeljau / Dorsch",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Wilde Forelle / Regenbogenforelle",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"pp",A:"pp",B:"v",AB:"v"},
  {name:"Roter Schnapper",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"pp",A:"pp",B:"v",AB:"v"},
  {name:"Lachs / Rotlachs / Königslachs",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Sardine",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Makrele / Atlantische Makrele",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"g",A:"v",B:"pp",AB:"pp"},
  {name:"Hecht",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"g",A:"n",B:"pp",AB:"pp"},
  {name:"Zander",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"pp",B:"v",AB:"pp"},
  {name:"Goldmakrele",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Meerbrasse",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Seeteufel",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"pp",B:"pp",AB:"pp"},
  {name:"Stör",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"g",A:"v",B:"pp",AB:"pp"},
  {name:"Zackenbarsch",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"g",A:"pp",B:"pp",AB:"pp"},
  {name:"Seehecht",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"g",A:"v",B:"pp",AB:"n"},
  {name:"Seezunge",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"pp",A:"v",B:"v",AB:"n"},
  {name:"Flussbarsch",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"g",A:"pp",B:"v",AB:"n"},
  {name:"Karpfen",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"pp",B:"v",AB:"n"},
  {name:"Kaviar",emoji:"🫙",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"pp",AB:"n"},
  {name:"Schwertfisch",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"g",A:"v",B:"n",AB:"n"},
  {name:"Dorsch jung",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"n",AB:"n"},
  {name:"Goldbarsch",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Hering frisch",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"n",AB:"n"},
  {name:"Steinbutt",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Tintenfisch / Kalamari",emoji:"🦑",kat:"Fisch & Meeresfrüchte","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Wels / Zwergwels",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Miesmuscheln",emoji:"🦪",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"v",AB:"n"},
  {name:"Jakobsmuscheln",emoji:"🦪",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"pp",AB:"v"},
  {name:"Austern",emoji:"🦪",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Garnelen",emoji:"🦐",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Hering eingelegt / geräuchert",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Krabben",emoji:"🦀",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Lachsforelle",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Räucherlachs",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Scholle",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"n",A:"v",B:"pp",AB:"v"},
  {name:"Aal",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Blaubarsch",emoji:"🐟",kat:"Fisch & Meeresfrüchte","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Frosch",emoji:"🐸",kat:"Fisch & Meeresfrüchte","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Kraken",emoji:"🦑",kat:"Fisch & Meeresfrüchte","0":"v",A:"v",B:"v",AB:"v"},

  // MILCHPRODUKTE & EIER
  {name:"Pecorino",emoji:"🧀",kat:"Milch & Eier","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Urda (rumänischer Frischkäse)",emoji:"🧀",kat:"Milch & Eier","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Fetakäse / Schafskäse",emoji:"🧀",kat:"Milch & Eier","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Hüttenkäse",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"pp",AB:"pp"},
  {name:"Joghurt",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"n",B:"pp",AB:"pp"},
  {name:"Kefir",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"n",B:"pp",AB:"pp"},
  {name:"Ziegenmilch",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"n",B:"pp",AB:"pp"},
  {name:"Mozzarella (alle Arten)",emoji:"🧀",kat:"Milch & Eier","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Ricotta",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"n",B:"pp",AB:"pp"},
  {name:"Sauerrahm",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"n",B:"n",AB:"pp"},
  {name:"Topfen / Quark",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"n",B:"n",AB:"pp"},
  {name:"Ziegenkäse",emoji:"🧀",kat:"Milch & Eier","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Hühnerei",emoji:"🥚",kat:"Milch & Eier","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Ei von Gans",emoji:"🥚",kat:"Milch & Eier","0":"v",A:"n",B:"v",AB:"n"},
  {name:"Ei von Wachtel",emoji:"🥚",kat:"Milch & Eier","0":"v",A:"n",B:"v",AB:"n"},
  {name:"Emmentaler",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Gouda",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Kuhmilch Magerstufe / 2%",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"v",B:"pp",AB:"n"},
  {name:"Molkenprotein",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Doppelrahmfrischkäse",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Butter",emoji:"🧈",kat:"Milch & Eier","0":"n",A:"v",B:"n",AB:"v"},
  {name:"Buttermilch",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Ei von Ente",emoji:"🥚",kat:"Milch & Eier","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Blauschimmelkäse",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Brie",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Camembert",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Cheddar",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Eiscreme",emoji:"🍦",kat:"Milch & Eier","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Gorgonzola",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Kaffeesahne",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Kuhmilch Vollmilch",emoji:"🥛",kat:"Milch & Eier","0":"v",A:"v",B:"pp",AB:"v"},
  {name:"Parmesan",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Romanokäse",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Roquefort",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Schmelzkäse",emoji:"🧀",kat:"Milch & Eier","0":"v",A:"v",B:"v",AB:"v"},

  // ÖLE & FETTE
  {name:"Olivenöl",emoji:"🫒",kat:"Öle & Fette","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Walnussöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"pp",B:"n",AB:"pp"},
  {name:"Leindotteröl",emoji:"🫒",kat:"Öle & Fette","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Hanfsamenöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Leinöl",emoji:"🫒",kat:"Öle & Fette","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Beerenöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Reisöl",emoji:"🫒",kat:"Öle & Fette","0":"pp",A:"n",B:"pp",AB:"n"},
  {name:"Lebertran",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Erdnussöl",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"v",B:"v",AB:"n"},
  {name:"Haselnussöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"v",B:"n",AB:"n"},
  {name:"Macadamianussöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Mandelöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Nachtkerzenöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Rapsöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Sojabohnenöl",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"n",B:"n",AB:"n"},
  {name:"Avocadoöl",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Kokosöl",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Kürbiskernöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Maiskeimöl",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Margarine",emoji:"🧈",kat:"Öle & Fette","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Palmöl",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Distelöl",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Schweinefett / Schmalz",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Sesamöl",emoji:"🫒",kat:"Öle & Fette","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Sonnenblumenöl",emoji:"🫒",kat:"Öle & Fette","0":"v",A:"n",B:"n",AB:"v"},

  // NÜSSE & SAMEN
  {name:"Walnüsse",emoji:"🌰",kat:"Nüsse & Samen","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Erdnüsse",emoji:"🥜",kat:"Nüsse & Samen","0":"v",A:"pp",B:"v",AB:"pp"},
  {name:"Erdnussbutter",emoji:"🥜",kat:"Nüsse & Samen","0":"v",A:"pp",B:"v",AB:"pp"},
  {name:"Erdnussmehl",emoji:"🥜",kat:"Nüsse & Samen","0":"v",A:"pp",B:"v",AB:"pp"},
  {name:"Maronen / Esskastanien",emoji:"🌰",kat:"Nüsse & Samen","0":"v",A:"v",B:"n",AB:"pp"},
  {name:"Kürbiskerne",emoji:"🌱",kat:"Nüsse & Samen","0":"pp",A:"pp",B:"v",AB:"n"},
  {name:"Leinsamen",emoji:"🌱",kat:"Nüsse & Samen","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Hanfsamen",emoji:"🌱",kat:"Nüsse & Samen","0":"pp",A:"n",B:"n",AB:"n"},
  {name:"Johannisbrot",emoji:"🌱",kat:"Nüsse & Samen","0":"pp",A:"n",B:"n",AB:"n"},
  {name:"Haselnüsse",emoji:"🌰",kat:"Nüsse & Samen","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Mohnsamen",emoji:"🌱",kat:"Nüsse & Samen","0":"v",A:"n",B:"v",AB:"n"},
  {name:"Sesammehl",emoji:"🌱",kat:"Nüsse & Samen","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Sesammus / Tahini",emoji:"🌱",kat:"Nüsse & Samen","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Sesamsamen",emoji:"🌱",kat:"Nüsse & Samen","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Sonnenblumenkerne",emoji:"🌻",kat:"Nüsse & Samen","0":"v",A:"n",B:"v",AB:"n"},
  {name:"Sonnenblumenkernmus",emoji:"🌻",kat:"Nüsse & Samen","0":"v",A:"n",B:"v",AB:"n"},
  {name:"Butternüsse",emoji:"🥜",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Cashewmus",emoji:"🥜",kat:"Nüsse & Samen","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Cashewnüsse",emoji:"🥜",kat:"Nüsse & Samen","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Chiasamen",emoji:"🌱",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Macadamianüsse",emoji:"🥜",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Mandelcreme",emoji:"🥜",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Mandeldrink",emoji:"🥛",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Mandelkäse",emoji:"🧀",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Mandeln",emoji:"🥜",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Paranüsse",emoji:"🥜",kat:"Nüsse & Samen","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Pecanusscreme",emoji:"🥜",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Pecanüsse",emoji:"🥜",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Pinienkerne",emoji:"🌱",kat:"Nüsse & Samen","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Pistazien",emoji:"🥜",kat:"Nüsse & Samen","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Bucheckern",emoji:"🌰",kat:"Nüsse & Samen","0":"v",A:"n",B:"n",AB:"v"},
  {name:"Hickorynüsse",emoji:"🥜",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Wassermelonenkerne",emoji:"🌱",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Pecanüsse",emoji:"🥜",kat:"Nüsse & Samen","0":"n",A:"n",B:"n",AB:"v"},

  // BOHNEN & HÜLSENFRÜCHTE
  {name:"Adzukibohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"pp",A:"pp",B:"v",AB:"v"},
  {name:"Augenbohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"pp",A:"pp",B:"v",AB:"v"},
  {name:"Kidneybohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"v",A:"v",B:"pp",AB:"v"},
  {name:"Tofu",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"v",AB:"pp"},
  {name:"Sojabohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"v",AB:"pp"},
  {name:"Sojakäse",emoji:"🧀",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"v",AB:"pp"},
  {name:"Sojasprossen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"v",AB:"pp"},
  {name:"Miso aus Soja",emoji:"🫙",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"v",AB:"pp"},
  {name:"Grüne Linsen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"v",A:"pp",B:"v",AB:"pp"},
  {name:"Navibohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Wachtelbohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Brechbohnen",emoji:"🫛",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Grüne Bohnen",emoji:"🫛",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Linsen (alle Arten)",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"v",A:"pp",B:"v",AB:"n"},
  {name:"Schwarze Bohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Sojabohnengranulat / Lecithin",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Sojadrink",emoji:"🥛",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"v",AB:"n"},
  {name:"Sojaflocken",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"v",AB:"n"},
  {name:"Sojamehl",emoji:"🌾",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"pp",B:"v",AB:"n"},
  {name:"Sojanudeln",emoji:"🍝",kat:"Bohnen & Hülsenfrüchte","0":"v",A:"pp",B:"n",AB:"n"},
  {name:"Erbsen grün / gelb",emoji:"🫛",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Mungbohnen / Mungbohnensprossen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Prinzessbohnen",emoji:"🫛",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Weiße Bohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Käferbohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Kichererbsen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Limabohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"v",B:"n",AB:"v"},
  {name:"Dicke Bohnen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Bohnensprossen",emoji:"🫘",kat:"Bohnen & Hülsenfrüchte","0":"n",A:"n",B:"n",AB:"v"},

  // GETREIDE & BROT
  {name:"Essener Brot",emoji:"🍞",kat:"Getreide & Brot","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Amaranth",emoji:"🌾",kat:"Getreide & Brot","0":"g",A:"pp",B:"v",AB:"pp"},
  {name:"Haferflocken / Hafermehl / Haferkleie",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"pp",B:"pp",AB:"pp"},
  {name:"Roggen / Roggenmehl",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"n",B:"v",AB:"pp"},
  {name:"Reis (Basmati, Naturreis, Weißreis)",emoji:"🍚",kat:"Getreide & Brot","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Reiskleie / Reismehl",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Reiswaffeln / Puffreis",emoji:"🍘",kat:"Getreide & Brot","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Hirse",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Dinkel (Korn, Mehl, Nudeln)",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Sojamehl (Getreide)",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"pp",B:"n",AB:"pp"},
  {name:"Leinsamenbrot",emoji:"🍞",kat:"Getreide & Brot","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Buchweizen (Korn, Mehl, Nudeln)",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"pp",B:"v",AB:"v"},
  {name:"Weizen gekeimt",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"pp",B:"n",AB:"n"},
  {name:"Linsenmehl / Dal",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"pp",B:"v",AB:"n"},
  {name:"Bulgur",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"n",B:"n",AB:"n"},
  {name:"Gerste / Gerstenmehl",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"n",B:"n",AB:"n"},
  {name:"Quinoa",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Weizenmehl / Weißmehl",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"n",B:"n",AB:"n"},
  {name:"Weizenkeimmehl",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"n",B:"n",AB:"n"},
  {name:"Couscous / Hartweizen",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"n",B:"v",AB:"n"},
  {name:"Wildreis",emoji:"🍚",kat:"Getreide & Brot","0":"n",A:"n",B:"v",AB:"pp"},
  {name:"Cornflakes",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Grahammehl",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Grieß / Grießbrei",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Kichererbsenmehl",emoji:"🌾",kat:"Getreide & Brot","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Mais / Polenta / Maismehl",emoji:"🌽",kat:"Getreide & Brot","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Puffweizen",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Weizenvollkorn / Weizenschrot",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Weizenkleie",emoji:"🌾",kat:"Getreide & Brot","0":"v",A:"v",B:"v",AB:"n"},

  // GEMÜSE
  {name:"Brokkoli",emoji:"🥦",kat:"Gemüse","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Blattkohl",emoji:"🥬",kat:"Gemüse","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Grünkohl",emoji:"🥬",kat:"Gemüse","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Algen / Seetang / Spirulina",emoji:"🌿",kat:"Gemüse","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Knoblauch",emoji:"🧄",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Petersilie",emoji:"🌿",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Pastinaken",emoji:"🥕",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Löwenzahn",emoji:"🌿",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Mangold",emoji:"🥬",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Stangensellerie",emoji:"🌿",kat:"Gemüse","0":"n",A:"pp",B:"n",AB:"pp"},
  {name:"Rote Rüben / Rote Bete",emoji:"🫐",kat:"Gemüse","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Süßkartoffel",emoji:"🍠",kat:"Gemüse","0":"pp",A:"v",B:"pp",AB:"pp"},
  {name:"Blumenkohl",emoji:"🥦",kat:"Gemüse","0":"v",A:"n",B:"pp",AB:"pp"},
  {name:"Romanesco",emoji:"🥦",kat:"Gemüse","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Brauner Senf",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Maitakepilze",emoji:"🍄",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Palmherzen",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Rübstiele",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Weinblätter",emoji:"🍃",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Yamswurzel",emoji:"🍠",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Artischocke",emoji:"🌿",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"v"},
  {name:"Chicorée",emoji:"🥬",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Fenchel",emoji:"🌿",kat:"Gemüse","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Gartenkürbis / Pumpkin",emoji:"🎃",kat:"Gemüse","0":"pp",A:"pp",B:"v",AB:"n"},
  {name:"Ingwer",emoji:"🫚",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Kohlrabi",emoji:"🥬",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Lauch",emoji:"🧅",kat:"Gemüse","0":"v",A:"pp",B:"n",AB:"n"},
  {name:"Meerrettich",emoji:"🌿",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Römischer Salat",emoji:"🥗",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Speiserübe / Herbstrübe",emoji:"🥕",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Spinat",emoji:"🥬",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Zwiebeln (alle Arten)",emoji:"🧅",kat:"Gemüse","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Karotten / Möhren",emoji:"🥕",kat:"Gemüse","0":"n",A:"pp",B:"pp",AB:"n"},
  {name:"Paprika alle Farben",emoji:"🫑",kat:"Gemüse","0":"n",A:"v",B:"pp",AB:"n"},
  {name:"Aubergine",emoji:"🍆",kat:"Gemüse","0":"v",A:"v",B:"pp",AB:"n"},
  {name:"Champignons",emoji:"🍄",kat:"Gemüse","0":"v",A:"pp",B:"n",AB:"n"},
  {name:"Shiitakepilze",emoji:"🍄",kat:"Gemüse","0":"v",A:"pp",B:"n",AB:"v"},
  {name:"Austernpilze",emoji:"🍄",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Bambussprossen",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Blattsalate (Eisberg, gemischt)",emoji:"🥗",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Brunnenkresse",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Dille",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Frühlingszwiebeln",emoji:"🧅",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Gurke",emoji:"🥒",kat:"Gemüse","0":"v",A:"n",B:"n",AB:"pp"},
  {name:"Knollensellerie",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Kohlsprossen / Rosenkohl",emoji:"🥦",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Kopfkohl",emoji:"🥬",kat:"Gemüse","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Kürbis",emoji:"🎃",kat:"Gemüse","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Oliven grün",emoji:"🫒",kat:"Gemüse","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Pak Choi",emoji:"🥬",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Radicchio",emoji:"🥬",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Rucola",emoji:"🥬",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Sauerkraut",emoji:"🥬",kat:"Gemüse","0":"n",A:"v",B:"n",AB:"n"},
  {name:"Schwarzwurzel / Haferwurzel",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Spargel",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Steckrüben",emoji:"🥕",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Tomaten",emoji:"🍅",kat:"Gemüse","0":"n",A:"v",B:"n",AB:"n"},
  {name:"Winterendivie",emoji:"🥬",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Zucchini",emoji:"🥒",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Kartoffeln (alle Sorten)",emoji:"🥔",kat:"Gemüse","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Eingelegtes Gemüse (alle Sorten)",emoji:"🫙",kat:"Gemüse","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Kapern",emoji:"🫙",kat:"Gemüse","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Mais / Popcorn",emoji:"🌽",kat:"Gemüse","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Oliven schwarz",emoji:"🫒",kat:"Gemüse","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Jalapeños / Chili",emoji:"🌶️",kat:"Gemüse","0":"n",A:"v",B:"pp",AB:"v"},
  {name:"Rettich / Rettichsprossen",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Rhabarber",emoji:"🌿",kat:"Gemüse","0":"n",A:"v",B:"n",AB:"v"},
  {name:"Radieschen",emoji:"🌿",kat:"Gemüse","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Topinambur",emoji:"🌿",kat:"Gemüse","0":"pp",A:"pp",B:"v",AB:"v"},
  {name:"Aloe Vera",emoji:"🌿",kat:"Gemüse","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Avocado",emoji:"🥑",kat:"Gemüse","0":"v",A:"n",B:"v",AB:"v"},

  // OBST
  {name:"Ananas",emoji:"🍍",kat:"Obst","0":"n",A:"pp",B:"pp",AB:"pp"},
  {name:"Cranberry",emoji:"🫐",kat:"Obst","0":"n",A:"pp",B:"pp",AB:"pp"},
  {name:"Kirsche",emoji:"🍒",kat:"Obst","0":"g",A:"pp",B:"n",AB:"pp"},
  {name:"Stachelbeere",emoji:"🍓",kat:"Obst","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Wassermelone",emoji:"🍉",kat:"Obst","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Weintrauben",emoji:"🍇",kat:"Obst","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Zitrone",emoji:"🍋",kat:"Obst","0":"n",A:"pp",B:"n",AB:"pp"},
  {name:"Grapefruit",emoji:"🍊",kat:"Obst","0":"n",A:"pp",B:"n",AB:"pp"},
  {name:"Kiwi",emoji:"🥝",kat:"Obst","0":"v",A:"v",B:"n",AB:"pp"},
  {name:"Feige",emoji:"🍑",kat:"Obst","0":"g",A:"pp",B:"n",AB:"pp"},
  {name:"Pflaume / Zwetschke",emoji:"🍑",kat:"Obst","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Backpflaume",emoji:"🍑",kat:"Obst","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Blaubeere",emoji:"🫐",kat:"Obst","0":"g",A:"pp",B:"n",AB:"n"},
  {name:"Brombeere",emoji:"🫐",kat:"Obst","0":"v",A:"pp",B:"n",AB:"n"},
  {name:"Limone / Limette",emoji:"🍋",kat:"Obst","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Marille / Aprikose",emoji:"🍑",kat:"Obst","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Apfel",emoji:"🍎",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Birne",emoji:"🍐",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Dattel",emoji:"🌴",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Erdbeere",emoji:"🍓",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Himbeere",emoji:"🍓",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Holunderbeere",emoji:"🫐",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Johannisbeere",emoji:"🫐",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Nektarine",emoji:"🍑",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Papaya",emoji:"🍑",kat:"Obst","0":"n",A:"n",B:"pp",AB:"n"},
  {name:"Pfirsich",emoji:"🍑",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Preiselbeere",emoji:"🍓",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Rosinen",emoji:"🍇",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Tangerine / Mandarine",emoji:"🍊",kat:"Obst","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Zuckermelone",emoji:"🍈",kat:"Obst","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Banane",emoji:"🍌",kat:"Obst","0":"g",A:"v",B:"pp",AB:"v"},
  {name:"Avocado",emoji:"🥑",kat:"Obst","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Granatapfel",emoji:"🍎",kat:"Obst","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Heidelbeere",emoji:"🫐",kat:"Obst","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Kaki",emoji:"🍑",kat:"Obst","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Kokosnuss",emoji:"🥥",kat:"Obst","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Mango",emoji:"🥭",kat:"Obst","0":"g",A:"v",B:"n",AB:"v"},
  {name:"Orange",emoji:"🍊",kat:"Obst","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Honigmelone",emoji:"🍈",kat:"Obst","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Nashibirne",emoji:"🍐",kat:"Obst","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Litschi",emoji:"🍑",kat:"Obst","0":"v",A:"v",B:"n",AB:"n"},

  // GETRÄNKE
  {name:"Wasser",emoji:"💧",kat:"Getränke","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Wasser mit Zitrone",emoji:"💧",kat:"Getränke","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Grüntee",emoji:"🍵",kat:"Getränke","0":"n",A:"pp",B:"pp",AB:"pp"},
  {name:"Hagebuttentee",emoji:"🍵",kat:"Getränke","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Ingwerwurzeltee",emoji:"🍵",kat:"Getränke","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Ananassaft",emoji:"🧃",kat:"Getränke","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Kirschsaft / Schwarzkirschsaft",emoji:"🧃",kat:"Getränke","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Kamillentee",emoji:"🍵",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"pp"},
  {name:"Cranberrysaft",emoji:"🧃",kat:"Getränke","0":"n",A:"pp",B:"pp",AB:"pp"},
  {name:"Traubensaft",emoji:"🧃",kat:"Getränke","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Wassermelonensaft",emoji:"🧃",kat:"Getränke","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Gemüsesaft (aus bekömmlichen Sorten)",emoji:"🧃",kat:"Getränke","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Erdbeerblättertee",emoji:"🍵",kat:"Getränke","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Ginseng-Tee",emoji:"🍵",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"pp"},
  {name:"Reisdrink",emoji:"🥛",kat:"Getränke","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Salbeitee",emoji:"🍵",kat:"Getränke","0":"n",A:"n",B:"pp",AB:"n"},
  {name:"Kaffee",emoji:"☕",kat:"Getränke","0":"v",A:"pp",B:"n",AB:"v"},
  {name:"Rotwein",emoji:"🍷",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Grapefruitsaft",emoji:"🧃",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Johanniskrauttee",emoji:"🍵",kat:"Getränke","0":"v",A:"pp",B:"n",AB:"n"},
  {name:"Klettentee",emoji:"🍵",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Mariendisteltee",emoji:"🍵",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Weißdorntee",emoji:"🍵",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"pp"},
  {name:"Baldriantee",emoji:"🍵",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Echinacea-Tee",emoji:"🍵",kat:"Getränke","0":"n",A:"pp",B:"pp",AB:"n"},
  {name:"Limonensaft",emoji:"🧃",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Pfefferminztee",emoji:"🍵",kat:"Getränke","0":"pp",A:"n",B:"n",AB:"n"},
  {name:"Petersilientee",emoji:"🍵",kat:"Getränke","0":"pp",A:"n",B:"pp",AB:"n"},
  {name:"Apfelsaft / Apfelwein",emoji:"🧃",kat:"Getränke","0":"v",A:"n",B:"n",AB:"n"},
  {name:"Birnensaft",emoji:"🧃",kat:"Getränke","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Bier",emoji:"🍺",kat:"Getränke","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Eisenkrauttee",emoji:"🍵",kat:"Getränke","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Himbeerblättertee",emoji:"🍵",kat:"Getränke","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Holundersaft / Holundertee",emoji:"🧃",kat:"Getränke","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Löwenzahntee",emoji:"🍵",kat:"Getränke","0":"pp",A:"n",B:"n",AB:"n"},
  {name:"Mandeldrink",emoji:"🥛",kat:"Getränke","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Nektarinensaft",emoji:"🧃",kat:"Getränke","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Schafgarbentee",emoji:"🍵",kat:"Getränke","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Thymiantee",emoji:"🍵",kat:"Getränke","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Tomatensaft",emoji:"🧃",kat:"Getränke","0":"v",A:"v",B:"v",AB:"n"},
  {name:"Weißwein",emoji:"🥂",kat:"Getränke","0":"v",A:"n",B:"n",AB:"n"},
  {name:"Marilensaft",emoji:"🧃",kat:"Getränke","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Sojadrink (Getränk)",emoji:"🥛",kat:"Getränke","0":"n",A:"pp",B:"v",AB:"n"},
  {name:"Lindenblütentee",emoji:"🍵",kat:"Getränke","0":"pp",A:"n",B:"v",AB:"v"},
  {name:"Hopfentee",emoji:"🍵",kat:"Getränke","0":"pp",A:"pp",B:"v",AB:"v"},
  {name:"Rhabarbertee",emoji:"🍵",kat:"Getränke","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Rotkleetee",emoji:"🍵",kat:"Getränke","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Cola / Gesüßte Sprudelgetränke",emoji:"🥤",kat:"Getränke","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Granatapfelsaft",emoji:"🧃",kat:"Getränke","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Guavensaft",emoji:"🧃",kat:"Getränke","0":"pp",A:"n",B:"n",AB:"v"},
  {name:"Kokosmilch",emoji:"🥛",kat:"Getränke","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Mangosaft",emoji:"🧃",kat:"Getränke","0":"pp",A:"v",B:"n",AB:"v"},
  {name:"Orangensaft",emoji:"🧃",kat:"Getränke","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Schnaps / Spirituosen",emoji:"🥃",kat:"Getränke","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Schwarztee (alle Formen)",emoji:"🍵",kat:"Getränke","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Hirtentäscheltee",emoji:"🍵",kat:"Getränke","0":"v",A:"n",B:"v",AB:"n"},
  {name:"Sodawasser / Mineralwasser mit Kohlensäure",emoji:"💧",kat:"Getränke","0":"n",A:"v",B:"v",AB:"n"},
  {name:"Aloe Vera Saft",emoji:"🧃",kat:"Getränke","0":"v",A:"v",B:"n",AB:"n"},

  // GEWÜRZE & KRÄUTER
  {name:"Ingwer (Gewürz)",emoji:"🫚",kat:"Gewürze & Kräuter","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Knoblauch (Gewürz)",emoji:"🧄",kat:"Gewürze & Kräuter","0":"pp",A:"pp",B:"n",AB:"pp"},
  {name:"Meerrettich (Gewürz)",emoji:"🌿",kat:"Gewürze & Kräuter","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Petersilie (Gewürz)",emoji:"🌿",kat:"Gewürze & Kräuter","0":"pp",A:"pp",B:"pp",AB:"pp"},
  {name:"Curry",emoji:"🌿",kat:"Gewürze & Kräuter","0":"pp",A:"n",B:"pp",AB:"pp"},
  {name:"Oregano",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"pp"},
  {name:"Cayennepfeffer",emoji:"🌶️",kat:"Gewürze & Kräuter","0":"pp",A:"v",B:"pp",AB:"v"},
  {name:"Kurkuma",emoji:"🌿",kat:"Gewürze & Kräuter","0":"pp",A:"pp",B:"n",AB:"n"},
  {name:"Fenchel (Gewürz)",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Senfpulver",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Rotalge",emoji:"🌿",kat:"Gewürze & Kräuter","0":"pp",A:"n",B:"n",AB:"n"},
  {name:"Rote Pfefferflocken",emoji:"🌶️",kat:"Gewürze & Kräuter","0":"pp",A:"v",B:"n",AB:"v"},
  {name:"Anis",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Basilikum",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Bergamotte",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Bohnenkraut",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Chilipulver",emoji:"🌶️",kat:"Gewürze & Kräuter","0":"n",A:"v",B:"n",AB:"n"},
  {name:"Dill",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Estragon",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Gewürznelke",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Kardamom",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Kerbel",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Koriander / Korianderblätter",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Kreuzkümmel / Kümmel",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Lorbeerblatt",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Majoran",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Muskatnuss",emoji:"🌿",kat:"Gewürze & Kräuter","0":"v",A:"n",B:"n",AB:"n"},
  {name:"Paprikapulver",emoji:"🌶️",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Pfefferminze",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Pfeilwurzel",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Piment",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Rosmarin",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Safran",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Salbei",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Salz / Meersalz",emoji:"🧂",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Schnittlauch",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Dunkle Schokolade",emoji:"🍫",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Süßholzwurzel",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Thymian",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Vanille",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Weinstein",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Wintergrün",emoji:"🌿",kat:"Gewürze & Kräuter","0":"v",A:"v",B:"n",AB:"n"},
  {name:"Zimt",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Gummi Arabicum",emoji:"🌿",kat:"Gewürze & Kräuter","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Maisstärke",emoji:"🌿",kat:"Gewürze & Kräuter","0":"v",A:"n",B:"v",AB:"n"},
  {name:"Schwarzer Pfeffer",emoji:"🌶️",kat:"Gewürze & Kräuter","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Weißer Pfeffer",emoji:"🌶️",kat:"Gewürze & Kräuter","0":"v",A:"v",B:"n",AB:"v"},

  // SÜSUNGSMITTEL & ZUSÄTZE
  {name:"Miso",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"pp",B:"v",AB:"pp"},
  {name:"Rohrmelasse",emoji:"🍯",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"pp",AB:"pp"},
  {name:"Gerstenmalz",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"pp",B:"v",AB:"v"},
  {name:"Sojasoße / Tamari (weizenfrei)",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"pp",B:"n",AB:"n"},
  {name:"Agavensirup",emoji:"🍯",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Ahornsirup",emoji:"🍯",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Apfelpektin",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Backpulver / Natron",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Bierhefe / Backhefe",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Dextrose",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Essig (alle Arten)",emoji:"🍶",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Fruktose",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Honig",emoji:"🍯",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Konfitüre / Marmelade (aus zulässigen Sorten)",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Lecithin",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Mayonnaise",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Obstpektin / Pflanzliches Glycerin",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Reissirup",emoji:"🍯",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Salatmarinade (aus zulässigen Zutaten)",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Senf ohne Weizen und Essig",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Stevia",emoji:"🌿",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"v",AB:"n"},
  {name:"Zucker (braun und weiß)",emoji:"🍬",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"n"},
  {name:"Senf mit Essig und Weizen",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Worcestersoße",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"n",AB:"v"},
  {name:"Aspartam",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Fruktosereicher Maissirup",emoji:"🍯",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Gelatine",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"v",B:"v",AB:"v"},
  {name:"Guarkernmehl",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Invertzucker",emoji:"🍯",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Ketchup",emoji:"🍅",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Maissirup / Maisstärke",emoji:"🍯",kat:"Süßungsmittel & Zusätze","0":"v",A:"n",B:"v",AB:"v"},
  {name:"Maltodextrin",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Mandelextrakt",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"n",AB:"v"},
  {name:"Natriumcarboxymethylcellulose",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Polysorbat 80",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Karageen",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"n",A:"n",B:"v",AB:"v"},
  {name:"Pflaumenessig",emoji:"🍶",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"v",AB:"v"},
  {name:"Ursüße",emoji:"🫙",kat:"Süßungsmittel & Zusätze","0":"v",A:"v",B:"v",AB:"v"},
];

const BTS = ["0","A","B","AB"];
let activeBT = "0", activeCat = "Alle";
let activeF = {pp:true,g:true,n:true,v:true};

function toggleF(t,btn){activeF[t]=!activeF[t];btn.classList.toggle('active',activeF[t]);render();}
function badgeLabel(v){return{pp:"++ Sehr bekömml.",g:"+ Bekömmlich",n:"Neutral",v:"Vermeiden"}[v];}
function getKats(){return["Alle",...new Set(foods.map(f=>f.kat))];}

function render(){
  const q=document.getElementById("search").value.toLowerCase();
  document.getElementById("btSel").innerHTML=BTS.map(bt=>`
    <button class="bt-btn ${activeBT===bt?'active':''}" onclick="setBT('${bt}')">
      ${bt}<span>${{0:"Jäger",A:"Landwirt",B:"Nomade",AB:"Zentaur"}[bt]}</span>
    </button>`).join("");
  const p=profiles[activeBT];
  document.getElementById("profileBox").innerHTML=`<h3>${p.name}</h3><p>${p.text}</p>`;
  const ib=document.getElementById("infoBox");
  if(infoBoxes[activeBT]&&infoBoxes[activeBT][activeCat]){
    ib.innerHTML=`<div class="info-box"><b>ℹ Hinweis aus dem Buch</b>${infoBoxes[activeBT][activeCat]}</div>`;
  } else { ib.innerHTML=""; }
  document.getElementById("catTabs").innerHTML=getKats().map(k=>`
    <button class="cat-tab ${activeCat===k?'active':''}" onclick="setCat('${k}')">${k}</button>`).join("");
  let filtered=foods;
  if(activeCat!=="Alle") filtered=filtered.filter(f=>f.kat===activeCat);
  filtered=filtered.filter(f=>activeF[f[activeBT]]);
  if(q) filtered=filtered.filter(f=>f.name.toLowerCase().includes(q)||f.kat.toLowerCase().includes(q));
  const el=document.getElementById("list");
  if(!filtered.length){el.innerHTML=`<div class="empty">Nichts gefunden.</div>`;return;}
  el.innerHTML=filtered.map(f=>{const v=f[activeBT];return`
    <div class="item ${v}">
      <div class="item-left">
        <span class="item-emoji">${f.emoji}</span>
        <div><div class="item-name">${f.name}</div><div class="item-cat">${f.kat}</div></div>
      </div>
      <span class="badge ${v}">${badgeLabel(v)}</span>
    </div>`;}).join("");
}
function setBT(bt){activeBT=bt;render();}
function setCat(k){activeCat=k;render();}
render();
</script>
</body>
</html>
