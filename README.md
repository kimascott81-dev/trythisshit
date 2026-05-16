<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
<title>Midtown Mixology</title>
<style>
body { font-family: Arial, sans-serif; background: #0a0a12; color: #fff; margin: 0; padding: 0; height: 100vh; overflow: hidden; }
#app { display: flex; flex-direction: column; height: 100vh; }
header { background: #1a1a2e; padding: 12px 20px; display: flex; justify-content: space-between; align-items: center; border-bottom: 1px solid #333; }
.logo { font-size: 1.4rem; font-weight: bold; color: #0ff; }
.nav-btn { background: #222; color: #fff; border: 1px solid #444; padding: 8px 16px; border-radius: 8px; font-size: 1rem; cursor: pointer; }
.nav-btn:active { background: #0ff; color: #000; }
main { flex: 1; overflow-y: auto; padding: 20px; }
.view { display: none; }
.view.active { display: block; }
h1 { font-size: 1.6rem; margin: 0 0 10px; }
h2 { font-size: 1.4rem; margin: 0 0 8px; }
p.sub { color: #888; margin: 0 0 15px; }

.home-grid { display: grid; grid-template-columns: 1fr 1fr; gap: 15px; max-width: 600px; margin: 20px auto; }
.home-card { background: #1a1a2e; border: 2px solid #333; border-radius: 16px; padding: 25px; text-align: center; cursor: pointer; }
.home-card:active { border-color: #0ff; background: #16213e; }
.home-icon { font-size: 3rem; margin-bottom: 8px; }
.home-title { font-size: 1.2rem; font-weight: bold; margin-bottom: 4px; }
.home-desc { font-size: 0.85rem; color: #aaa; }
@media(max-width: 500px) { .home-grid { grid-template-columns: 1fr; } }

.filter-bar { display: flex; gap: 8px; overflow-x: auto; margin-bottom: 15px; padding-bottom: 5px; }
.filter-btn { background: #222; border: 1px solid #444; color: #fff; padding: 8px 16px; border-radius: 20px; cursor: pointer; white-space: nowrap; font-size: 0.9rem; }
.filter-btn.active { background: #0ff; color: #000; font-weight: bold; }

.card { background: #1a1a2e; border: 1px solid #333; border-radius: 12px; padding: 14px; margin-bottom: 10px; cursor: pointer; position: relative; }
.card:active { border-color: #0ff; }
.card-title { font-weight: bold; font-size: 1rem; margin-bottom: 4px; }
.card-meta { color: #888; font-size: 0.8rem; margin-top: 3px; }
.card-price { color: #ffd700; font-weight: bold; font-size: 0.95rem; margin-top: 4px; }
.item-selected { border-color: #0f0 !important; background: rgba(0,255,0,0.08) !important; }
.item-selected::after { content: 'OK'; position: absolute; top: 6px; right: 10px; color: #0f0; font-weight: 900; font-size: 0.8rem; }
.premium-tag { color: #ffd700; font-size: 0.75rem; font-weight: bold; margin-top: 4px; }

.btn { display: block; width: 100%; padding: 14px; margin-top: 10px; border: none; border-radius: 10px; font-size: 1rem; font-weight: bold; cursor: pointer; }
.btn-primary { background: linear-gradient(90deg, #0ff, #f0f); color: #000; }
.btn-outline { background: transparent; border: 2px solid #444; color: #fff; }
.btn-gold { background: #ffd700; color: #000; }
.btn-danger { background: #ff3366; color: #fff; }
.btn-small { width: auto; display: inline-block; padding: 10px 20px; font-size: 0.9rem; }
.btn-row { display: flex; gap: 10px; margin-top: 12px; }
.btn-row .btn { margin-top: 0; }

.form-group { margin-bottom: 15px; }
label { display: block; margin-bottom: 5px; color: #0ff; font-weight: bold; font-size: 0.85rem; text-transform: uppercase; }
input, textarea, select { width: 100%; padding: 12px; border-radius: 8px; border: 1px solid #444; background: #111; color: #fff; font-size: 1rem; }
textarea { min-height: 80px; resize: vertical; }
.checkbox-row { display: flex; align-items: center; gap: 10px; margin-top: 8px; }
.checkbox-row input { width: auto; }

.recipe-box { background: #1a1a2e; border: 1px solid #333; border-radius: 16px; padding: 20px; margin-bottom: 15px; }
.recipe-name { font-size: 1.7rem; font-weight: bold; text-align: center; margin-bottom: 5px; }
.recipe-creator { text-align: center; color: #ffd700; font-weight: bold; margin-bottom: 10px; }
.badge { display: inline-block; background: rgba(0,255,255,0.15); color: #0ff; padding: 4px 12px; border-radius: 20px; font-size: 0.8rem; font-weight: bold; margin: 0 3px; border: 1px solid #0ff; }
.price-tag { display: inline-block; background: rgba(255,215,0,0.15); color: #ffd700; padding: 5px 14px; border-radius: 20px; font-size: 1rem; font-weight: bold; margin: 0 3px; border: 1px solid #ffd700; }
.counter { color: #888; font-size: 0.85rem; margin-top: 8px; text-align: center; }
.ingredient-row { display: flex; justify-content: space-between; padding: 8px 0; border-bottom: 1px solid #222; }
.ingredient-row:last-child { border-bottom: none; }
.qty { color: #0ff; font-weight: bold; }
.notes-box { background: #111; border-left: 3px solid #ffd700; padding: 12px; margin: 12px 0; border-radius: 0 8px 8px 0; color: #ddd; font-style: italic; }

.step-indicator { display: flex; gap: 5px; margin-bottom: 20px; justify-content: center; }
.step-dot { width: 40px; height: 5px; background: #333; border-radius: 3px; }
.step-dot.active { background: #0ff; }
.step-dot.done { background: #0f0; }

.empty { text-align: center; padding: 40px; color: #888; }
.empty-emoji { font-size: 4rem; opacity: 0.5; margin-bottom: 10px; }

.add-form { background: #111; border: 1px solid #333; border-radius: 12px; padding: 15px; margin-bottom: 20px; display: none; }
.add-form.show { display: block; }

.export-box { width: 100%; min-height: 100px; background: #111; border: 1px solid #444; color: #0f0; font-family: monospace; padding: 10px; border-radius: 8px; margin: 10px 0; word-break: break-all; }

#debug-bar { position: fixed; bottom: 0; left: 0; right: 0; background: #000; color: #0f0; padding: 8px; font-family: monospace; font-size: 12px; max-height: 80px; overflow-y: auto; border-top: 2px solid #0f0; z-index: 9999; }

@media print { header, .no-print, #debug-bar { display: none !important; } body { background: #fff; color: #000; } .recipe-box { border: 2px solid #000; } }
</style>
<base target="_blank">
</head>
<body>
<div id="app">
  <header>
    <div class="logo">MIDTOWN MIXOLOGY</div>
    <button class="nav-btn no-print" onclick="goHome()">Home</button>
  </header>
  <main>
    <div id="view-home" class="view active">
      <h1 style="text-align:center;">What are we making tonight?</h1>
      <p class="sub" style="text-align:center;">Tap an option to get started</p>
      <div class="home-grid">
        <button class="home-card" onclick="btnBrowse()" ontouchstart="btnBrowse()">
          <div class="home-icon">[MENU]</div>
          <div class="home-title">Browse Menu</div>
          <div class="home-desc">Signature drinks & shots</div>
        </button>
        <button class="home-card" onclick="btnInventory()" ontouchstart="btnInventory()">
          <div class="home-icon">[INV]</div>
          <div class="home-title">Our Inventory</div>
          <div class="home-desc">Spirits, mixers & more</div>
        </button>
        <button class="home-card" onclick="btnCreate()" ontouchstart="btnCreate()">
          <div class="home-icon">[LAB]</div>
          <div class="home-title">Create Your Own</div>
          <div class="home-desc">Build a custom drink</div>
        </button>
        <button class="home-card" onclick="btnSaved()" ontouchstart="btnSaved()">
          <div class="home-icon">[SAVE]</div>
          <div class="home-title">My Recipes</div>
          <div class="home-desc">Saved this session</div>
        </button>
      </div>
    </div>
    <div id="view-browse" class="view"><h2>Drink Menu</h2><p class="sub">Tap any drink to see recipe, price & print count</p><div class="filter-bar" id="browseFilters"></div><div id="browseGrid"></div></div>
    <div id="view-inventory" class="view"><h2>Inventory</h2><p class="sub">Everything behind our bar. Tap + to add items.</p><div class="no-print" style="margin-bottom: 15px;"><button class="btn btn-outline btn-small" onclick="toggleAddForm()">+ Add Item</button></div><div class="add-form" id="addForm"><div class="form-group"><label>Item Name</label><input type="text" id="newItemName" placeholder="e.g., Grey Goose"></div><div class="form-group"><label>Category</label><input type="text" id="newItemCat" placeholder="e.g., Vodka"></div><div class="form-group"><label>Default Amount</label><input type="text" id="newItemQty" placeholder="e.g., 1.5 oz" value="1.5 oz"></div><div class="checkbox-row"><input type="checkbox" id="newItemPremium"><label style="margin:0;text-transform:none;">Premium (+$2.00 upgrade)</label></div><div class="btn-row"><button class="btn btn-primary" onclick="addInventoryItem()">Add to Inventory</button><button class="btn btn-outline" onclick="toggleAddForm()">Cancel</button></div></div><div class="filter-bar" id="invFilters"></div><div id="invGrid"></div></div>
    <div id="view-create" class="view"><h2 id="createTitle">Drink Lab</h2><p class="sub">Craft your masterpiece</p><div class="step-indicator" id="stepIndicator"><div class="step-dot active"></div><div class="step-dot"></div><div class="step-dot"></div><div class="step-dot"></div></div><div id="create-step-1"><div class="form-group"><label>Drink Name</label><input type="text" id="createName" placeholder="e.g., The Midnight Spark"></div><div class="form-group"><label>Created By</label><input type="text" id="createCreator" placeholder="e.g., Alex"></div><div class="form-group"><label>Type</label><select id="createType"><option>Cocktail</option><option>Shot</option><option>Mocktail</option><option>Special</option></select></div><button class="btn btn-primary" onclick="nextStep(2)">Next: Pick Ingredients</button></div><div id="create-step-2" style="display:none;"><div class="filter-bar" id="createFilters"></div><div style="margin-bottom: 10px; color: #0f0; font-weight: bold;">Selected: <span id="selectedCount">0</span></div><div id="createGrid"></div><div class="btn-row"><button class="btn btn-outline" onclick="prevStep(1)">Back</button><button class="btn btn-primary" onclick="nextStep(3)">Next: Details</button></div></div><div id="create-step-3" style="display:none;"><div id="selectedReview"></div><div class="form-group" style="margin-top: 15px;"><label>Glass Type</label><select id="createGlass"><option>Rocks Glass</option><option>Collins Glass</option><option>Martini Glass</option><option>Shot Glass</option><option>Mug</option><option>Other</option></select></div><div class="form-group"><label>Instructions / Build Order</label><textarea id="createInstructions" placeholder="1. Fill shaker with ice...&#10;2. Add vodka and...&#10;3. Shake and strain into..."></textarea></div><div class="form-group"><label>Bartender Notes</label><textarea id="createNotes" placeholder="Special instructions for the bartender..."></textarea></div><div class="btn-row"><button class="btn btn-outline" onclick="prevStep(2)">Back</button><button class="btn btn-primary" onclick="nextStep(4)">Next: Preview</button></div></div><div id="create-step-4" style="display:none;"><div id="createPreview"></div><div class="btn-row"><button class="btn btn-outline" onclick="prevStep(3)">Edit</button><button class="btn btn-gold" onclick="saveCreation()">Save Recipe</button></div></div></div>
    <div id="view-recipe" class="view"><div id="recipeContent"></div><div class="no-print" id="recipeActions" style="margin-top: 15px;"></div></div>
    <div id="view-myrecipes" class="view"><h2>My Creations</h2><p class="sub">Saved during this session. Export before closing.</p><div id="myRecipesList"></div></div>
  </main>
</div>
<div id="debug-bar">Loading...</div>

<script>
function log(msg) {
  var d = document.getElementById('debug-bar');
  if (d) d.innerHTML = d.innerHTML + '<br>' + msg;
}

log('SCRIPT START');

var INVENTORY = [
  {id:'vod1',name:"Tito's Vodka",category:'Vodka',premium:false,defaultQty:'1.5 oz'},
  {id:'vod2',name:'Ketel One',category:'Vodka',premium:true,defaultQty:'1.5 oz'},
  {id:'rum1',name:'Bacardi Superior',category:'Rum',premium:false,defaultQty:'1.5 oz'},
  {id:'rum2',name:'Captain Morgan',category:'Rum',premium:false,defaultQty:'1.5 oz'},
  {id:'gin1',name:'Bombay Sapphire',category:'Gin',premium:true,defaultQty:'1.5 oz'},
  {id:'gin2',name:'Tanqueray',category:'Gin',premium:false,defaultQty:'1.5 oz'},
  {id:'teq1',name:'Patron Silver',category:'Tequila',premium:true,defaultQty:'1.5 oz'},
  {id:'teq2',name:'Jose Cuervo Gold',category:'Tequila',premium:false,defaultQty:'1.5 oz'},
  {id:'whi1',name:"Jack Daniel's",category:'Whiskey',premium:false,defaultQty:'1.5 oz'},
  {id:'whi2',name:'Jameson',category:'Whiskey',premium:false,defaultQty:'1.5 oz'},
  {id:'whi3',name:'Fireball',category:'Whiskey',premium:false,defaultQty:'1.5 oz'},
  {id:'liq1',name:'Triple Sec',category:'Liqueur',premium:false,defaultQty:'0.5 oz'},
  {id:'liq2',name:'Blue Curacao',category:'Liqueur',premium:false,defaultQty:'0.5 oz'},
  {id:'liq3',name:'Peach Schnapps',category:'Liqueur',premium:false,defaultQty:'0.5 oz'},
  {id:'liq4',name:'Sour Apple Pucker',category:'Liqueur',premium:false,defaultQty:'0.5 oz'},
  {id:'mix1',name:'Orange Juice',category:'Mixer',premium:false,defaultQty:'3 oz'},
  {id:'mix2',name:'Cranberry Juice',category:'Mixer',premium:false,defaultQty:'3 oz'},
  {id:'mix3',name:'Pineapple Juice',category:'Mixer',premium:false,defaultQty:'3 oz'},
  {id:'mix4',name:'Sour Mix',category:'Mixer',premium:false,defaultQty:'2 oz'},
  {id:'mix5',name:'Ginger Beer',category:'Mixer',premium:false,defaultQty:'4 oz'},
  {id:'mix6',name:'Club Soda',category:'Mixer',premium:false,defaultQty:'2 oz'},
  {id:'mix7',name:'Cola',category:'Mixer',premium:false,defaultQty:'4 oz'},
  {id:'mix8',name:'Grenadine',category:'Mixer',premium:false,defaultQty:'0.5 oz'},
  {id:'gar1',name:'Lime Wedge',category:'Garnish',premium:false,defaultQty:'1 wedge'},
  {id:'gar2',name:'Cherry',category:'Garnish',premium:false,defaultQty:'1 cherry'},
  {id:'gar3',name:'Olive',category:'Garnish',premium:false,defaultQty:'2 olives'},
  {id:'gar4',name:'Mint Sprig',category:'Garnish',premium:false,defaultQty:'2 sprigs'},
  {id:'ext1',name:'Simple Syrup',category:'Extra',premium:false,defaultQty:'0.5 oz'},
  {id:'ext2',name:'Whipped Cream',category:'Extra',premium:false,defaultQty:'dollop'},
  {id:'ext3',name:'Salt Rim',category:'Extra',premium:false,defaultQty:'rim'},
  {id:'ext4',name:'Sugar Rim',category:'Extra',premium:false,defaultQty:'rim'}
];

var DEFAULT_DRINKS = [
  {id:'d1',name:'Midtown Mule',creator:'House',type:'Cocktail',glass:'Mug',instructions:'Fill copper mug with ice. Add vodka and lime juice. Top with ginger beer. Garnish with lime wedge.',notes:'',ingredients:[{id:'vod1',qty:'2 oz'},{id:'mix5',qty:'4 oz'},{id:'gar1',qty:'1 wedge'}],printCount:0},
  {id:'d2',name:'Tropical Sunset',creator:'House',type:'Cocktail',glass:'Collins Glass',instructions:'Build over ice: rum, pineapple, orange juice. Float grenadine. Garnish with cherry.',notes:'',ingredients:[{id:'rum1',qty:'1.5 oz'},{id:'mix3',qty:'3 oz'},{id:'mix1',qty:'2 oz'},{id:'mix8',qty:'0.5 oz'},{id:'gar2',qty:'1 cherry'}],printCount:0},
  {id:'d3',name:'Blue Lightning',creator:'House',type:'Shot',glass:'Shot Glass',instructions:'Shake vodka, blue curacao, and sour mix with ice. Strain into shot glass.',notes:'',ingredients:[{id:'vod1',qty:'1 oz'},{id:'liq2',qty:'0.5 oz'},{id:'mix4',qty:'0.5 oz'}],printCount:0},
  {id:'d4',name:'Peach Whiskey Smash',creator:'House',type:'Cocktail',glass:'Rocks Glass',instructions:'Muddle mint and peach schnapps. Add whiskey and ice. Stir gently.',notes:'',ingredients:[{id:'whi1',qty:'2 oz'},{id:'liq3',qty:'1 oz'},{id:'gar4',qty:'2 sprigs'}],printCount:0},
  {id:'d5',name:'Firecracker',creator:'House',type:'Shot',glass:'Shot Glass',instructions:'Layer Fireball and peach schnapps in shot glass.',notes:'',ingredients:[{id:'whi3',qty:'1 oz'},{id:'liq3',qty:'1 oz'}],printCount:0},
  {id:'d6',name:'Gin Garden',creator:'House',type:'Cocktail',glass:'Collins Glass',instructions:'Build gin, sour mix, and club soda over ice. Top with mint.',notes:'',ingredients:[{id:'gin1',qty:'2 oz'},{id:'mix4',qty:'2 oz'},{id:'mix6',qty:'2 oz'},{id:'gar4',qty:'3 leaves'}],printCount:0},
  {id:'d7',name:'Cran-Teq Cooler',creator:'House',type:'Cocktail',glass:'Rocks Glass',instructions:'Shake tequila, triple sec, and cranberry with ice. Strain over fresh ice. Lime wedge garnish.',notes:'',ingredients:[{id:'teq1',qty:'2 oz'},{id:'liq1',qty:'0.5 oz'},{id:'mix2',qty:'3 oz'},{id:'gar1',qty:'1 wedge'}],printCount:0},
  {id:'d8',name:'Apple Pie Shot',creator:'House',type:'Shot',glass:'Shot Glass',instructions:'Shake whiskey and sour apple pucker with ice. Strain. Top with whipped cream.',notes:'',ingredients:[{id:'whi2',qty:'1 oz'},{id:'liq4',qty:'1 oz'},{id:'ext2',qty:'dollop'}],printCount:0}
];

var currentStep = 1;
var selectedIngredients = {};
var lastView = 'home';
var allDrinks = [];
var myRecipes = [];
var editModeId = null;
var isVariation = false;
var currentRecipeId = null;

log('VARS OK');

function getInv(id) {
  for (var i = 0; i < INVENTORY.length; i++) if (INVENTORY[i].id === id) return INVENTORY[i];
  return null;
}
function getDrink(id) {
  for (var i = 0; i < allDrinks.length; i++) if (allDrinks[i].id === id) return allDrinks[i];
  return null;
}
function getCategories() {
  var cats = [];
  var seen = {};
  for (var i = 0; i < INVENTORY.length; i++) {
    if (!seen[INVENTORY[i].category]) { seen[INVENTORY[i].category] = true; cats.push(INVENTORY[i].category); }
  }
  return cats;
}
function calcPrice(ingredients) {
  var hasPrem = false;
  for (var i = 0; i < ingredients.length; i++) {
    var it = getInv(ingredients[i].id);
    if (it && it.premium) hasPrem = true;
  }
  return hasPrem ? 6.50 : 4.50;
}
function formatPrice(p) { return '$' + p.toFixed(2); }
function clone(obj) { return JSON.parse(JSON.stringify(obj)); }

log('HELPERS OK');

function showView(name) {
  log('CLICK: showView(' + name + ')');
  var views = document.getElementsByClassName('view');
  log('Found ' + views.length + ' views');
  for (var i = 0; i < views.length; i++) views[i].classList.remove('active');
  var t = document.getElementById('view-' + name);
  log('Target element: ' + (t ? 'FOUND' : 'NOT FOUND'));
  if (t) t.classList.add('active');
  if (name !== 'recipe') lastView = name;
  if (name === 'myrecipes') renderMyRecipes();
  log('showView done');
}
function goHome() { showView('home'); }

function btnBrowse() { log('BTN: Browse clicked'); showView('browse'); }
function btnInventory() { log('BTN: Inventory clicked'); showView('inventory'); }
function btnCreate() { log('BTN: Create clicked'); startCreate(); }
function btnSaved() { log('BTN: Saved clicked'); showView('myrecipes'); }

log('NAV OK');

function renderBrowseFilters() {
  var cats = ['All','Cocktail','Shot','Mocktail','Special'];
  var bar = document.getElementById('browseFilters');
  if (!bar) return;
  var html = '';
  for (var i = 0; i < cats.length; i++) {
    html += '<div class="filter-btn ' + (cats[i] === 'All' ? 'active' : '') + '" onclick="filterBrowse(this,'' + cats[i] + '')">' + cats[i] + '</div>';
  }
  bar.innerHTML = html;
}
function filterBrowse(el, cat) {
  var chips = el.parentNode.getElementsByClassName('filter-btn');
  for (var i = 0; i < chips.length; i++) chips[i].classList.remove('active');
  el.classList.add('active');
  renderBrowse(cat);
}
function renderBrowse(filter) {
  var grid = document.getElementById('browseGrid');
  if (!grid) return;
  var items = [];
  for (var i = 0; i < allDrinks.length; i++) {
    if (filter === 'All' || allDrinks[i].type === filter) items.push(allDrinks[i]);
  }
  if (items.length === 0) { grid.innerHTML = '<div class="empty"><div class="empty-emoji">[DRINK]</div><h3>No drinks found</h3></div>'; return; }
  var html = '';
  for (var i = 0; i < items.length; i++) {
    var d = items[i];
    var price = calcPrice(d.ingredients);
    var pc = d.printCount || 0;
    html += '<div class="card" onclick="openRecipe('' + d.id + '')"><div class="card-title">' + d.name + '</div><div class="card-meta">' + d.type + ' - ' + d.glass + '</div><div class="card-price">' + formatPrice(price) + '</div>';
    if (pc > 0) html += '<div class="counter">Printed ' + pc + ' times</div>';
    html += '</div>';
  }
  grid.innerHTML = html;
}

function toggleAddForm() {
  var f = document.getElementById('addForm');
  if (f.classList.contains('show')) f.classList.remove('show');
  else f.classList.add('show');
}
function renderInvFilters() {
  var cats = getCategories();
  cats.unshift('All');
  var bar = document.getElementById('invFilters');
  if (!bar) return;
  var html = '';
  for (var i = 0; i < cats.length; i++) {
    html += '<div class="filter-btn ' + (cats[i] === 'All' ? 'active' : '') + '" onclick="filterInv(this,'' + cats[i] + '')">' + cats[i] + '</div>';
  }
  bar.innerHTML = html;
}
function filterInv(el, cat) {
  var chips = el.parentNode.getElementsByClassName('filter-btn');
  for (var i = 0; i < chips.length; i++) chips[i].classList.remove('active');
  el.classList.add('active');
  renderInventory(cat);
}
function renderInventory(filter) {
  var grid = document.getElementById('invGrid');
  if (!grid) return;
  var html = '';
  for (var i = 0; i < INVENTORY.length; i++) {
    var it = INVENTORY[i];
    if (filter !== 'All' && it.category !== filter) continue;
    html += '<div class="card"><div class="card-title">' + it.name + '</div><div class="card-meta">' + it.category;
    if (it.premium) html += ' - <span style="color:#ffd700;font-weight:bold;">PREMIUM</span>';
    html += '</div><div class="card-meta">Default: ' + it.defaultQty + '</div></div>';
  }
  grid.innerHTML = html;
}
function addInventoryItem() {
  var name = document.getElementById('newItemName').value.trim();
  var cat = document.getElementById('newItemCat').value.trim();
  var qty = document.getElementById('newItemQty').value.trim() || '1.5 oz';
  var prem = document.getElementById('newItemPremium').checked;
  if (!name || !cat) { alert('Enter name and category'); return; }
  var id = 'ing_' + Date.now();
  INVENTORY.push({id:id, name:name, category:cat, premium:prem, defaultQty:qty});
  document.getElementById('newItemName').value = '';
  document.getElementById('newItemCat').value = '';
  document.getElementById('newItemQty').value = '1.5 oz';
  document.getElementById('newItemPremium').checked = false;
  toggleAddForm();
  renderInvFilters();
  renderInventory('All');
  alert(name + ' added to inventory!');
}

log('RENDER OK');

function startCreate() {
  log('startCreate called');
  editModeId = null;
  isVariation = false;
  selectedIngredients = {};
  currentStep = 1;
  document.getElementById('createTitle').textContent = 'Drink Lab';
  document.getElementById('createName').value = '';
  document.getElementById('createCreator').value = '';
  document.getElementById('createType').value = 'Cocktail';
  document.getElementById('createGlass').value = 'Rocks Glass';
  document.getElementById('createInstructions').value = '';
  document.getElementById('createNotes').value = '';
  updateStepIndicator();
  showStep(1);
  showView('create');
}
function startEdit(id) {
  var d = getDrink(id);
  if (!d) return;
  editModeId = id;
  isVariation = false;
  selectedIngredients = {};
  for (var i = 0; i < d.ingredients.length; i++) selectedIngredients[d.ingredients[i].id] = true;
  currentStep = 1;
  document.getElementById('createTitle').textContent = 'Edit Drink';
  document.getElementById('createName').value = d.name;
  document.getElementById('createCreator').value = d.creator;
  document.getElementById('createType').value = d.type;
  document.getElementById('createGlass').value = d.glass;
  document.getElementById('createInstructions').value = d.instructions;
  document.getElementById('createNotes').value = d.notes || '';
  updateStepIndicator();
  showStep(1);
  showView('create');
}
function startVariation(id) {
  var d = getDrink(id);
  if (!d) return;
  editModeId = null;
  isVariation = true;
  selectedIngredients = {};
  for (var i = 0; i < d.ingredients.length; i++) selectedIngredients[d.ingredients[i].id] = true;
  currentStep = 1;
  document.getElementById('createTitle').textContent = 'Make Variation';
  document.getElementById('createName').value = d.name + ' (Variation)';
  document.getElementById('createCreator').value = d.creator;
  document.getElementById('createType').value = d.type;
  document.getElementById('createGlass').value = d.glass;
  document.getElementById('createInstructions').value = d.instructions;
  document.getElementById('createNotes').value = d.notes || '';
  updateStepIndicator();
  showStep(1);
  showView('create');
}
function updateStepIndicator() {
  var dots = document.getElementById('stepIndicator').getElementsByClassName('step-dot');
  for (var i = 0; i < dots.length; i++) {
    dots[i].classList.remove('active', 'done');
    if (i + 1 < currentStep) dots[i].classList.add('done');
    else if (i + 1 === currentStep) dots[i].classList.add('active');
  }
}
function showStep(n) {
  for (var i = 1; i <= 4; i++) {
    var el = document.getElementById('create-step-' + i);
    if (el) el.style.display = (i === n ? 'block' : 'none');
  }
  if (n === 2) { renderCreateFilters(); renderCreateGrid('All'); }
  if (n === 3) renderSelectedReview();
  if (n === 4) renderPreview();
  currentStep = n;
  updateStepIndicator();
}
function nextStep(n) {
  if (n === 2) {
    if (!document.getElementById('createName').value.trim()) { alert('Enter a drink name!'); return; }
  }
  if (n === 3) {
    var count = 0;
    for (var k in selectedIngredients) if (selectedIngredients[k]) count++;
    if (count === 0) { alert('Select at least one ingredient!'); return; }
  }
  showStep(n);
}
function prevStep(n) { showStep(n); }

function renderCreateFilters() {
  var cats = getCategories();
  cats.unshift('All');
  var bar = document.getElementById('createFilters');
  if (!bar) return;
  var html = '';
  for (var i = 0; i < cats.length; i++) {
    html += '<div class="filter-btn ' + (cats[i] === 'All' ? 'active' : '') + '" onclick="filterCreate(this,'' + cats[i] + '')">' + cats[i] + '</div>';
  }
  bar.innerHTML = html;
}
function filterCreate(el, cat) {
  var chips = el.parentNode.getElementsByClassName('filter-btn');
  for (var i = 0; i < chips.length; i++) chips[i].classList.remove('active');
  el.classList.add('active');
  renderCreateGrid(cat);
}
function renderCreateGrid(filter) {
  var grid = document.getElementById('createGrid');
  if (!grid) return;
  var html = '';
  for (var i = 0; i < INVENTORY.length; i++) {
    var it = INVENTORY[i];
    if (filter !== 'All' && it.category !== filter) continue;
    var sel = selectedIngredients[it.id];
    html += '<div class="card ' + (sel ? 'item-selected' : '') + '" onclick="toggleIngredient('' + it.id + '')" style="position:relative;"><div class="card-title">' + it.name + '</div><div class="card-meta">' + it.category + '</div>';
    if (it.premium) html += '<div class="premium-tag">* PREMIUM</div>';
    html += '</div>';
  }
  grid.innerHTML = html;
  var count = 0;
  for (var k in selectedIngredients) if (selectedIngredients[k]) count++;
  document.getElementById('selectedCount').textContent = count;
}
function toggleIngredient(id) {
  if (selectedIngredients[id]) delete selectedIngredients[id];
  else selectedIngredients[id] = true;
  var activeChip = document.querySelector('#createFilters .filter-btn.active');
  var activeText = 'All';
  if (activeChip) activeText = activeChip.textContent || activeChip.innerText || 'All';
  renderCreateGrid(activeText);
}
function renderSelectedReview() {
  var container = document.getElementById('selectedReview');
  if (!container) return;
  var html = '';
  for (var id in selectedIngredients) {
    if (!selectedIngredients[id]) continue;
    var it = getInv(id);
    if (!it) continue;
    html += '<div class="ingredient-row"><span>' + it.name;
    if (it.premium) html += ' <span style="color:#ffd700;">(* Premium)</span>';
    html += '</span><span class="qty">' + it.defaultQty + '</span></div>';
  }
  container.innerHTML = '<div style="background:rgba(0,0,0,0.3);border-radius:12px;padding:14px;margin-bottom:14px;"><h3 style="margin-top:0;color:#0ff;font-size:1.1rem;">Selected Ingredients</h3>' + (html || '<div style="color:#888">None selected</div>') + '</div>';
}
function renderPreview() {
  var name = document.getElementById('createName').value;
  var creator = document.getElementById('createCreator').value || 'Anonymous';
  var type = document.getElementById('createType').value;
  var glass = document.getElementById('createGlass').value;
  var instructions = document.getElementById('createInstructions').value;
  var notes = document.getElementById('createNotes').value;
  var ingredients = [];
  var ingRows = '';
  for (var id in selectedIngredients) {
    if (!selectedIngredients[id]) continue;
    var it = getInv(id);
    if (!it) continue;
    ingredients.push({id:id, qty:it.defaultQty});
    ingRows += '<div class="ingredient-row"><span>' + it.name + '</span><span class="qty">' + it.defaultQty + '</span></div>';
  }
  var price = calcPrice(ingredients);
  var notesHtml = notes ? '<div class="notes-box">NOTES: ' + notes + '</div>' : '';
  document.getElementById('createPreview').innerHTML = '<div class="recipe-box"><div class="recipe-name">' + name + '</div><div class="recipe-creator">Created by ' + creator + '</div><div style="text-align:center;"><span class="badge">' + type + '</span><span class="badge">' + glass + '</span><span class="price-tag">' + formatPrice(price) + '</span></div><div class="ingredient-list" style="background:rgba(0,0,0,0.3);border-radius:12px;padding:14px;margin:14px 0;">' + ingRows + '</div><div style="text-align:left;white-space:pre-wrap;font-size:1rem;line-height:1.5;">' + instructions + '</div>' + notesHtml + '</div>';
}
function saveCreation() {
  var name = document.getElementById('createName').value.trim();
  var creator = document.getElementById('createCreator').value.trim() || 'Anonymous';
  var type = document.getElementById('createType').value;
  var glass = document.getElementById('createGlass').value;
  var instructions = document.getElementById('createInstructions').value;
  var notes = document.getElementById('createNotes').value;
  var ingredients = [];
  for (var id in selectedIngredients) {
    if (!selectedIngredients[id]) continue;
    var it = getInv(id);
    if (it) ingredients.push({id:id, qty:it.defaultQty});
  }
  var newId = editModeId ? editModeId : 'custom_' + Date.now();
  var drink = {id:newId, name:name, creator:creator, type:type, glass:glass, instructions:instructions, notes:notes, ingredients:ingredients, isCustom:true, printCount:0};

  if (editModeId) {
    for (var i = 0; i < allDrinks.length; i++) {
      if (allDrinks[i].id === editModeId) {
        drink.printCount = allDrinks[i].printCount || 0;
        allDrinks[i] = drink;
        break;
      }
    }
    for (var i = 0; i < myRecipes.length; i++) {
      if (myRecipes[i].id === editModeId) { myRecipes[i] = drink; break; }
    }
    alert(name + ' updated!');
  } else {
    allDrinks.push(drink);
    myRecipes.push(drink);
    alert(name + ' by ' + creator + ' saved!');
  }
  editModeId = null;
  isVariation = false;
  renderBrowse('All');
  showView('browse');
}

log('CREATE OK');

function openRecipe(id) {
  log('CLICK: openRecipe(' + id + ')');
  currentRecipeId = id;
  var d = getDrink(id);
  if (!d) return;
  var price = calcPrice(d.ingredients);
  var pc = d.printCount || 0;
  var ingRows = '';
  for (var i = 0; i < d.ingredients.length; i++) {
    var it = getInv(d.ingredients[i].id);
    var name = it ? it.name : d.ingredients[i].id;
    ingRows += '<div class="ingredient-row"><span>' + name + '</span><span class="qty">' + d.ingredients[i].qty + '</span></div>';
  }
  var notesHtml = d.notes ? '<div class="notes-box">BARTENDER NOTES: ' + d.notes + '</div>' : '';
  document.getElementById('recipeContent').innerHTML = '<div class="recipe-box"><div class="recipe-name">' + d.name + '</div><div class="recipe-creator">' + (d.isCustom ? 'Crafted by ' : 'House Recipe - ') + d.creator + '</div><div style="text-align:center;"><span class="badge">' + d.type + '</span><span class="badge">' + d.glass + '</span><span class="price-tag">' + formatPrice(price) + '</span></div><div class="counter">Printed ' + pc + ' times</div><div class="ingredient-list" style="background:rgba(0,0,0,0.3);border-radius:12px;padding:14px;margin:14px 0;">' + ingRows + '</div><div style="text-align:left;white-space:pre-wrap;font-size:1.05rem;line-height:1.6;">' + d.instructions + '</div>' + notesHtml + '</div>';

  var actions = '<div class="btn-row"><button class="btn btn-primary" onclick="printRecipe()">Print Ticket (' + formatPrice(price) + ')</button></div>';
  actions += '<div class="btn-row"><button class="btn btn-outline btn-small" onclick="startEdit('' + d.id + '')">Edit</button><button class="btn btn-outline btn-small" onclick="startVariation('' + d.id + '')">Make Variation</button></div>';
  if (d.isCustom) actions += '<div class="btn-row"><button class="btn btn-danger btn-small" onclick="deleteRecipe('' + d.id + '')">Delete</button></div>';
  actions += '<div class="btn-row"><button class="btn btn-outline" onclick="showView(lastView)">Back</button></div>';
  document.getElementById('recipeActions').innerHTML = actions;
  showView('recipe');
}
function printRecipe() {
  var d = getDrink(currentRecipeId);
  if (d) d.printCount = (d.printCount || 0) + 1;
  window.print();
  openRecipe(currentRecipeId);
}
function deleteRecipe(id) {
  if (!confirm('Delete this recipe?')) return;
  var filtered = [];
  for (var i = 0; i < allDrinks.length; i++) if (allDrinks[i].id !== id) filtered.push(allDrinks[i]);
  allDrinks = filtered;
  var filtered2 = [];
  for (var i = 0; i < myRecipes.length; i++) if (myRecipes[i].id !== id) filtered2.push(myRecipes[i]);
  myRecipes = filtered2;
  renderBrowse('All');
  showView('myrecipes');
}

log('RECIPE OK');

function renderMyRecipes() {
  var container = document.getElementById('myRecipesList');
  if (!container) return;
  if (myRecipes.length === 0) {
    container.innerHTML = '<div class="empty"><div class="empty-emoji">[EMPTY]</div><h3>No creations yet</h3><p>Go to "Create Your Own" to make one!</p><button class="btn btn-primary" style="max-width:300px;margin:15px auto 0;" onclick="startCreate()">Create Now</button></div>';
    return;
  }
  var cards = '';
  for (var i = 0; i < myRecipes.length; i++) {
    var d = myRecipes[i];
    var price = calcPrice(d.ingredients);
    cards += '<div class="card" onclick="openRecipe('' + d.id + '')"><div class="card-title">' + d.name + '</div><div class="card-meta">by ' + d.creator + '</div><div class="card-price">' + formatPrice(price) + '</div></div>';
  }
  var exportText = '';
  for (var i = 0; i < myRecipes.length; i++) {
    var d = myRecipes[i];
    var ing = '';
    for (var j = 0; j < d.ingredients.length; j++) {
      var it = getInv(d.ingredients[j].id);
      if (ing) ing += ', ';
      ing += (it ? it.name : d.ingredients[j].id) + ': ' + d.ingredients[j].qty;
    }
    var price = calcPrice(d.ingredients);
    if (exportText) exportText += '

';
    exportText += (i + 1) + '. ' + d.name + ' (' + d.type + ') by ' + d.creator + '
   Price: ' + formatPrice(price) + '
   Glass: ' + d.glass + '
   Ingredients: ' + ing + '
   Instructions: ' + d.instructions.replace(/
/g, ' ') + (d.notes ? '
   Notes: ' + d.notes : '');
  }
  container.innerHTML = '<div>' + cards + '</div><div style="margin-top:20px;background:#1a1a2e;border:1px solid #333;border-radius:16px;padding:18px;"><h3 style="margin-top:0;color:#ffd700;font-size:1.1rem;">Export Recipes</h3><p style="color:#888;font-size:0.9rem;">Copy this before closing the page. Recipes are NOT saved after you close.</p><textarea class="export-box" readonly onclick="this.select()">' + exportText + '</textarea><button class="btn btn-outline btn-small" style="margin-top:8px;width:auto;display:inline-block;" onclick="downloadRecipes()">Download as File</button><button class="btn btn-danger btn-small" style="margin-top:10px;width:auto;display:inline-block;" onclick="clearMyRecipes()">Clear All</button></div>';
}
function downloadRecipes() {
  var exportText = '';
  for (var i = 0; i < myRecipes.length; i++) {
    var d = myRecipes[i];
    var ing = '';
    for (var j = 0; j < d.ingredients.length; j++) {
      var it = getInv(d.ingredients[j].id);
      if (ing) ing += ', ';
      ing += (it ? it.name : d.ingredients[j].id) + ': ' + d.ingredients[j].qty;
    }
    var price = calcPrice(d.ingredients);
    if (exportText) exportText += '

';
    exportText += (i + 1) + '. ' + d.name + ' (' + d.type + ') by ' + d.creator + '
   Price: ' + formatPrice(price) + '
   Glass: ' + d.glass + '
   Ingredients: ' + ing + '
   Instructions: ' + d.instructions.replace(/
/g, ' ') + (d.notes ? '
   Notes: ' + d.notes : '');
  }
  var blob = new Blob([exportText], {type: 'text/plain'});
  var url = URL.createObjectURL(blob);
  var a = document.createElement('a');
  a.href = url;
  a.download = 'midtown-recipes.txt';
  document.body.appendChild(a);
  a.click();
  document.body.removeChild(a);
  URL.revokeObjectURL(url);
  alert('Recipes downloaded!');
}
function clearMyRecipes() {
  if (!confirm('Delete all custom recipes?')) return;
  var filtered = [];
  for (var i = 0; i < allDrinks.length; i++) if (!allDrinks[i].isCustom) filtered.push(allDrinks[i]);
  allDrinks = filtered;
  myRecipes = [];
  renderMyRecipes();
  renderBrowse('All');
}

log('MYRECIPES OK');

function init() {
  log('INIT started');
  allDrinks = clone(DEFAULT_DRINKS);
  myRecipes = [];
  renderBrowseFilters();
  renderBrowse('All');
  renderInvFilters();
  renderInventory('All');
  log('INIT done');
}

init();
log('=== APP READY ===');
</script>
</body>
</html>
