<!doctype html>
<html lang="pl">
<head>
  <meta charset="utf-8" />
  <meta name="viewport" content="width=device-width,initial-scale=1" />
  <title>Sklep Real Madryt — Koszulki i Sprzęt</title>

  <!-- GSAP (animacje) -->
  <script src="https://cdnjs.cloudflare.com/ajax/libs/gsap/3.12.2/gsap.min.js" defer></script>
  <!-- Pixi Confetti (opcjonalne) -->
  <script src="https://cdn.jsdelivr.net/npm/pixi-confetti@1.2.0/dist/pixi-confetti.min.js" defer></script>

  <style>
    /* --------- KOLORY: biało-czarne (Real) z akcentem złota --------- */
    :root{
      --bg-white: #ffffff;
      --text-black: #0b0b0b;
      --muted: #6b7280;
      --accent: #d4af37; /* złoty akcent */
      --glass: rgba(11,11,11,0.03);
      --card: linear-gradient(180deg,#ffffff,#f7f7f7);
      --radius: 12px;
    }
    *{box-sizing:border-box}
    html,body{height:100%}
    body{
      margin:0;
      font-family:Inter, system-ui, -apple-system, "Segoe UI", Roboto, "Helvetica Neue", Arial;
      background:var(--bg-white);
      color:var(--text-black);
      -webkit-font-smoothing:antialiased;
      -moz-osx-font-smoothing:grayscale;
      padding:24px;
    }

    .wrap{max-width:1240px;margin:0 auto}

    /* HEADER */
    header{display:flex;align-items:center;justify-content:space-between;gap:12px;margin-bottom:18px}
    .brand{display:flex;gap:12px;align-items:center}
    .logo{
      width:76px;height:76px;border-radius:16px;background:#000;color:var(--bg-white);display:flex;align-items:center;justify-content:center;font-weight:900;font-size:26px;box-shadow:0 6px 18px rgba(0,0,0,0.08);
      border:1px solid rgba(0,0,0,0.06)
    }
    .brand h1{margin:0;font-size:20px}
    .brand p{margin:0;color:var(--muted);font-size:13px}
    .header-actions{display:flex;align-items:center;gap:12px}

    /* SEARCH */
    .search{display:flex;align-items:center;gap:8px;background:var(--glass);padding:8px;border-radius:999px;border:1px solid rgba(0,0,0,0.06);}    
    .search input{background:transparent;border:none;outline:none;color:var(--text-black);width:220px}

    /* HERO */
    .hero{display:grid;grid-template-columns:1fr 380px;gap:18px;align-items:center;margin-bottom:20px}
    .hero-card{background:var(--card);padding:22px;border-radius:var(--radius);position:relative;overflow:hidden;border:1px solid rgba(0,0,0,0.04)}
    .hero-card h2{margin:0;font-size:28px}
    .hero-card p{color:var(--muted);margin-top:8px}
    .cta{margin-top:12px;background:var(--text-black);color:var(--bg-white);border:none;padding:10px 14px;border-radius:999px;font-weight:800;cursor:pointer}

    .hero-image{border-radius:var(--radius);height:260px;background-size:cover;background-position:center;box-shadow:0 26px 60px rgba(0,0,0,0.06);border:1px solid rgba(0,0,0,0.04)}

    /* FILTERS */
    .filters{display:flex;gap:10px;margin:18px 0;flex-wrap:wrap}
    .chip{padding:8px 14px;border-radius:999px;background:transparent;border:1px solid rgba(0,0,0,0.06);cursor:pointer;color:var(--text-black);font-weight:700}
    .chip.active{background:var(--text-black);color:var(--bg-white);box-shadow:0 6px 20px rgba(0,0,0,0.06)}

    /* GRID */
    .grid{display:grid;grid-template-columns:repeat(auto-fill,minmax(240px,1fr));gap:16px}
    .card{background:linear-gradient(180deg,#fff,#fafafa);border-radius:12px;padding:12px;position:relative;overflow:hidden;border:1px solid rgba(0,0,0,0.04);transition:transform .22s ease,box-shadow .22s ease}
    .card:hover{transform:translateY(-8px);box-shadow:0 18px 40px rgba(0,0,0,0.06)}
    .thumb{height:190px;border-radius:10px;background-size:cover;background-position:center;cursor:pointer;box-shadow:inset 0 -30px 60px rgba(0,0,0,0.03)}
    .title{font-weight:800;margin-top:10px}
    .price{color:var(--text-black);margin-top:6px;font-weight:800}
    .meta{color:var(--muted);font-size:13px;margin-top:6px}
    .actions{display:flex;gap:8px;margin-top:12px}
    .btn{padding:8px 12px;border-radius:10px;border:none;cursor:pointer;font-weight:800}
    .btn.primary{background:var(--text-black);color:var(--bg-white)}
    .btn.ghost{background:transparent;border:1px solid rgba(0,0,0,0.06);color:var(--text-black)}

    /* CART SIDEBAR */
    .cart-sidebar{position:fixed;right:22px;top:90px;width:360px;max-width:92vw;background:linear-gradient(180deg,#fff,#fbfbfb);padding:12px;border-radius:12px;border:1px solid rgba(0,0,0,0.04);box-shadow:0 26px 80px rgba(0,0,0,0.06);z-index:60;transform:translateX(120%);transition:transform .32s cubic-bezier(.2,.9,.3,1)}
    .cart-sidebar.open{transform:translateX(0)}
    .cart-item{display:flex;gap:10px;align-items:center;padding:10px 0;border-bottom:1px dashed rgba(0,0,0,0.04)}
    .cart-item img{width:56px;height:56px;object-fit:cover;border-radius:8px}
    .qty{display:flex;gap:6px;align-items:center}
    .qty button{background:transparent;border:1px solid rgba(0,0,0,0.06);padding:6px 8px;border-radius:8px;cursor:pointer;color:var(--text-black)}

    /* MODAL */
    .modal{position:fixed;inset:0;display:flex;align-items:center;justify-content:center;background:rgba(0,0,0,0.35);z-index:80}
    .modal-card{width:min(980px,96%);background:var(--bg-white);border-radius:12px;padding:18px;display:grid;grid-template-columns:1fr 360px;gap:14px}
    .modal-img{height:440px;border-radius:10px;background-size:cover;background-position:center;box-shadow:0 20px 50px rgba(0,0,0,0.06)}
    .thumbs{display:flex;gap:8px;margin-top:8px}
    .thumbs img{width:72px;height:48px;object-fit:cover;border-radius:8px;cursor:pointer;border:2px solid transparent}
    .thumbs img.active{border-color:var(--text-black)}

    .pay-panel{background:transparent;padding:10px;border-radius:10px;margin-top:12px;border:1px solid rgba(0,0,0,0.04)}
    .field{display:flex;flex-direction:column;gap:6px;margin-bottom:8px}
    input[type="text"], input[type="tel"], select{background:transparent;border:1px solid rgba(0,0,0,0.06);padding:10px;border-radius:8px;color:var(--text-black);width:100%}
    label{font-size:13px;color:var(--muted)}

    footer{margin-top:36px;color:var(--muted);text-align:center;font-size:13px;padding-bottom:20px}

    /* toast */
    .toast{position:fixed;right:18px;top:18px;background:var(--text-black);color:var(--bg-white);padding:12px 16px;border-radius:10px;box-shadow:0 12px 30px rgba(0,0,0,0.12);z-index:9999}

    @media (max-width:980px){.hero{grid-template-columns:1fr}.modal-card{grid-template-columns:1fr}.modal-img{height:320px}.cart-sidebar{right:12px;top:auto;bottom:12px;width:92vw}}
  </style>
</head>
<body>
  <div class="wrap">
    <header>
      <div class="brand">
        <div class="logo">RM</div>
        <div>
          <h1>Sklep Real Madryt</h1>
          <p>Kolory: biel • czerń — oficjalny styl</p>
        </div>
      </div>

      <div class="header-actions">
        <div class="search" aria-hidden="false">
          <svg width="16" height="16" viewBox="0 0 24 24" fill="none" style="opacity:.9"><path d="M21 21l-4.35-4.35" stroke="black" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/><circle cx="11" cy="11" r="6" stroke="black" stroke-width="1.6" stroke-linecap="round" stroke-linejoin="round"/></svg>
          <input id="searchInput" placeholder="Szukaj: koszulka, korki..." />
        </div>
        <button id="openCartBtn" class="chip" title="Otwórz koszyk">Koszyk (<span id="cartCount">0</span>)</button>
      </div>
    </header>

    <!-- HERO -->
    <section class="hero">
      <div class="hero-card">
        <h2>Oficjalne koszulki i sprzęt</h2>
        <p>Wybierz swój produkt, dodaj do koszyka i zamów. Po złożeniu zamówienia pojawi się potwierdzenie "Dziękujemy za zakup".</p>
        <div style="display:flex;gap:10px;margin-top:12px;align-items:center">
          <button class="cta" id="shopNowBtn">Zobacz kolekcję</button>
          <button class="btn ghost" id="randomPromo">Promocja</button>
        </div>
      </div>

      <div class="hero-image" id="heroImage" style="background-image:url('https://blue.kumparan.com/image/upload/fl_progressive,fl_lossy,c_fill,f_auto,q_auto:best,w_640/v1634025439/01j30hwyesv2cbqkkkn12zxmyh.jpg')"></div>
    </section>

    <!-- FILTERS -->
    <div class="filters" id="filters">
      <div class="chip active" data-filter="all">Wszystkie</div>
      <div class="chip" data-filter="koszulki">Koszulki</div>
      <div class="chip" data-filter="korki">Korki</div>
      <div class="chip" data-filter="pilki">Piłki</div>
      <div class="chip" data-filter="retro">Retro</div>
      <div class="chip" data-filter="akcesoria">Akcesoria</div>
    </div>

    <!-- PRODUCTS GRID -->
    <main class="grid" id="productGrid" aria-live="polite"></main>

    <footer>© <span id="year"></span> Sklep Real Madryt</footer>
  </div>

  <!-- CART SIDEBAR -->
  <aside class="cart-sidebar" id="cartSidebar" aria-hidden="true">
    <div style="display:flex;justify-content:space-between;align-items:center">
      <h3 style="margin:0">Koszyk</h3>
      <button id="closeCart" class="btn ghost">Zamknij</button>
    </div>
    <div id="cartItems" style="margin-top:12px;max-height:420px;overflow:auto"></div>
    <div style="margin-top:12px;display:flex;justify-content:space-between;align-items:center">
      <div style="color:var(--muted)">Suma:</div>
      <div id="cartTotal" style="font-weight:900;color:var(--text-black)">0 PLN</div>
    </div>
    <div style="margin-top:12px;display:flex;gap:8px">
      <button id="checkoutBtn" class="btn primary" style="flex:1">Do kasy</button>
      <button id="clearCart" class="btn ghost" style="flex:1">Wyczyść</button>
    </div>
  </aside>

  <!-- TOAST ROOT -->
  <div id="toastRoot"></div>

  <!-- MODAL ROOT -->
  <div id="modalRoot" style="display:none;"></div>

  <script>
    /* ===========================
       Dane produktów (dodano 20+ produktów)
       =========================== */
    const PRODUCTS = [
      {id:1, sku:"RM-HOME-26", title:"Koszulka domowa Real Madrid 25/26 (Home)", price:299, tag:"koszulki", sizes:["S","M","L","XL"], images:[
        
        "https://thumblr.uniid.it/product/396488/c49df52f916b.jpg?width=1920&format=webp&q=75"
      ]},
      {id:2, sku:"RM-AWAY-26", title:"Koszulka wyjazdowa Real Madrid 25/26 (Away)", price:299, tag:"koszulki", sizes:["S","M","L","XL"], images:[
        
        "https://thumblr.uniid.it/product/396498/70868032a4a6.jpg?width=1920&format=webp&q=75"
      ]},
      {id:3, sku:"RM-RETRO-00", title:"Koszulka retro Real 2000 (Retro)", price:300, tag:"retro", sizes:["S","M","L"], images:[
        "https://www.outdoorkurtki.pl/satimg.php?autoimage=i476-52804660&prodId=21091225&sizex=560&sizey=560"
      ]},
      {id:4, sku:"RM-BOOTS-PRO", title:"Korki treningowe RM Pro", price:128, tag:"korki", sizes:["40","41","42","43"], images:[
        "https://contents.mediadecathlon.com/p2978262/k$1cb95af696ee6a5dd60cad1c412e3533/sq/buty-pilkarskie-adidas-predator-pro-fg.jpg?format=auto&f=969x969"
      ]},
      {id:5, sku:"RM-BALL-MATCH", title:"Piłka oficjalna Real – Match", price:98, tag:"pilki", sizes:[], images:[
        "https://sklepmartes.pl/media/catalog/product/cache/80a6882d891b4b1fb4b895f00df18d4b/_/4/_4068807960756_I_001-JN7360-RM-CLB-HOME-white-bold_gold-lgh_solid_grey.webp"
      ]},
      {id:6, sku:"RM-JACKET-TRAIN", title:"Kurtka treningowa Real", price:149, tag:"koszulki", sizes:["S","M","L"], images:[
        "https://thumblr.uniid.it/product/397018/a1513314deb6.jpg?width=1920&format=webp&q=75"
      ]},
      // dodatkowe 20 produktów
      {id:7, sku:"RM-KIDS-23", title:"Koszulka dziecięca Real Kids 23/24", price:229, tag:"koszulki", sizes:["XS","S","M"], images:["https://cdn.blazimg.com/1800/product/a/d/adidas_ia9977_white_3.webp"]},
      {id:8, sku:"RM-TRAIN-SET", title:"Zestaw treningowy (Bluza + Spodnie)", price:399, sizes:["S","M","L","XL"], images:["https://theretroshop.pl/environment/cache/images/productGfx_2297511_500_500/Zolty-Dres-Real-Madryt-1.jpg?overlay=1"]},
      {id:9, sku:"RM-MBAPPE", title:"Figurka RM Mbappe", price:149, tag:"akcesroia", images:["https://prod-api.mediaexpert.pl/api/images/gallery/thumbnails/images/72/7269496/Figurka-MINIX-Real-Madryt-Kylian-Mbappe-opakowanie.jpg"]},
      {id:10, sku:"RM-GLASSES", title:"Okulary przeciwsłoneczne RM", price:149, tag:"akcesoria", sizes:[], images:["https://image.ceneostatic.pl/data/products/50288171/i-rms-50004-okulary-przeciwsloneczne-real-madryt-junior.jpg"]},
      {id:11, sku:"RM-SOCKS-PRO", title:"Skarpety meczowe RM Pro", price:39, tag:"akcesoria", sizes:["S","M","L"], images:["https://thumblr.uniid.it/product/372680/72cc9fc92f50.jpg?width=3840&format=webp&q=75"]},
      {id:12, sku:"RM-BALL-TRAIN", title:"Piłka treningowa RM", price:129, tag:"pilki", sizes:[], images:["https://ocdn.eu/photo-offers-prod-transforms/1/Q5Ek9k3b2ZmZXJzLzIzNTgwMDIwNjhfNjE1OGQwYWUzMjNiYWFlODEyNWNhMzNiMzYyZWI3ZWIud2VicJGTCaY2MDNiZWUG3gABoTAF/pilka-treningowa-adidas-real-madryt-mini-home-r-1.webp"]},
      {id:13, sku:"RM-CAP-CLUB", title:"Czapka z daszkiem Real Club", price:89, tag:"akcesoria", sizes:[], images:["https://m.media-amazon.com/images/I/51TIuXXg8PL._AC_SX679_.jpg"]},
      {id:14, sku:"RM-BACKPACK", title:"Plecak treningowy Real", price:199, tag:"akcesoria", sizes:[], images:["https://contents.mediadecathlon.com/m17081520/k$8c256f35c65e10c0f979da567d320949/sq/plecak-worek-dzieciecy-real-madrid-cf.jpg?format=auto&f=969x969"]},
      {id:15, sku:"RM-HOODIE", title:"Bluza z kapturem Real", price:279, tag:"koszulki", sizes:["S","M","L","XL"], images:["https://thumblr.uniid.it/product/396804/9e5b6ae4a8b6.jpg?width=1920&format=webp&q=75"]},
      {id:16, sku:"RM-WATCH", title:"Zegarek RM Edition", price:599, tag:"akcesoria", sizes:[], images:["https://i.etsystatic.com/15198189/r/il/973e69/6708363173/il_1588xN.6708363173_dmyw.jpg"]},
      {id:17, sku:"RM-TRAIN-BAG", title:"Torba na buty i sprzęt", price:119, tag:"akcesoria", sizes:[], images:["https://www.tradeinn.com/f/14110/141104491/safta-real-madrid-first-kit-24-25-40-cm-torba.webp"]},
      {id:18, sku:"RM-THERMO", title:"Butelka termiczna RM", price:89, tag:"akcesoria", sizes:[], images:["https://contents.mediadecathlon.com/p2984810/k$50b192df8079c6207b63500d84744d68/sq/bidon-750-ml-real-madryt-2526.jpg?format=auto&f=969x969"]},
      {id:19, sku:"RM-COACH-JKT", title:"Kurtka trenera RM", price:329, tag:"koszulki", sizes:["S","M","L"], images:["https://thumblr.uniid.it/product/233513/622d489ed6ee.jpg?width=3840&format=webp&q=75"]},
      {id:20, sku:"RM-KEYCHAIN", title:"Brelok klubowy RM", price:29, tag:"akcesoria", sizes:[], images:["https://ecsmedia.pl/cdn-cgi/image/format=webp,width=544,height=544,/c/brelok-real-madryt-do-kluczy-auta-domu-prezent-dla-kibica-hala-madrit-pilka-b-iext201835230.jpg"]},
      {id:21, sku:"RM-SCARF-26", title:"Szalik kibica RM 25/26", price:79, tag:"akcesoria", sizes:[], images:["https://img01.ztat.net/article/spp-media-p1/9226b989c92545b1a9abf89010c1fc0f/fc7f85e432754d0ba907cacb39f7b196.jpg?imwidth=762&filter=packshot"]},
      {id:22, sku:"RM-TRAIN-GLOVES", title:"Rękawice treningowe RM", price:119, tag:"akcesoria", sizes:["S","M","L"], images:["https://pl.futbolemotion.com/imagesarticulos/228667/grandes/adidas-guantes-real-madrid-2024-2025-black-white-0.webp"]},
      {id:23, sku:"RM-MINI-BALL", title:"Mini piłka RM - na prezent", price:59, tag:"pilki", sizes:[], images:["https://pl.futbolemotion.com/imagesarticulos/280337/grandes/balon-adidas-mini-real-madrid-2025-2026-white-0.webp"]},
      {id:24, sku:"RM-PIN-SET", title:"Zestaw pinów RM", price:39, tag:"akcesoria", sizes:[], images:["https://www.fan-store.pl/gallery/products/middle/92828-54011.jpg"]},
      {id:25, sku:"RM-YOUTH-TRAIN", title:"Młodzieżowy komplet treningowy", price:119, tag:"koszulki", sizes:["XS","S","M"], images:["https://contents.mediadecathlon.com/m25114071/k$737857618cd57f708051cbe3269ed449/sq/dres-treningowy-real-madrid-meski-2526-granatowy.jpg?format=auto&f=969x969"]},
      {id:26, sku:"RM-LEGENDS-TEE", title:"Koszulka Legends Edition", price:499, tag:"koszulki", sizes:["M","L","XL"], images:["https://img01.ztat.net/article/spp-media-p1/6230c94120794a82ac9e2c061948a2f1/957a007ba5764e58bbcc348fab3075f0.jpg?imwidth=1800&filter=packshot"]}
    ];

    /* ================
       Stan aplikacji
       ================ */
    let cart = JSON.parse(localStorage.getItem('rm_demo_cart') || '[]');
    const grid = document.getElementById('productGrid');
    const cartCountEl = document.getElementById('cartCount');
    const cartSidebar = document.getElementById('cartSidebar');
    const cartItemsEl = document.getElementById('cartItems');
    const cartTotalEl = document.getElementById('cartTotal');
    const modalRoot = document.getElementById('modalRoot');
    const toastRoot = document.getElementById('toastRoot');
    const yearEl = document.getElementById('year');

    yearEl.textContent = new Date().getFullYear();

    /* ====================
       Pomocnicze funkcje
       ==================== */
    function formatMoney(n){ return n.toFixed(0) + ' PLN'; }
    function saveCart(){ localStorage.setItem('rm_demo_cart', JSON.stringify(cart)); }
    function updateCartUI(){
      cartCountEl.textContent = cart.reduce((s,i)=>s+i.qty,0);
      renderCartSidebar();
      saveCart();
      gsap.fromTo('#openCartBtn',{scale:1},{scale:1.06,duration:0.12,yoyo:true,repeat:1});
    }

    function addToCart(productId, size=null, qty=1){
      const prod = PRODUCTS.find(p=>p.id===productId);
      if(!prod) return;
      const key = `${productId}::${size||''}`;
      const existing = cart.find(c=>c.key===key);
      if(existing){ existing.qty += qty; } else { cart.push({ key, id:productId, title:prod.title, price:prod.price, size:size, qty:qty, img:prod.images[0] }); }
      updateCartUI();
    }

    function removeCartItem(key){ cart = cart.filter(c=>c.key!==key); updateCartUI(); }
    function changeQty(key, delta){ const it = cart.find(c=>c.key===key); if(!it) return; it.qty = Math.max(1, it.qty + delta); updateCartUI(); }
    function clearCart(){ cart = []; updateCartUI(); }
    function cartTotal(){ return cart.reduce((s,i)=>s + i.price * i.qty, 0); }

    /* ====================
       Render produkty
       ==================== */
    function createCardHTML(p){
      const div = document.createElement('article');
      div.className = 'card';
      div.innerHTML = `
        <div class="thumb" style="background-image:url('${p.images[0]}')" data-id="${p.id}" title="Kliknij aby otworzyć produkt"></div>
        <div class="title">${p.title}</div>
        <div class="meta">SKU: ${p.sku}</div>
        <div class="price">${formatMoney(p.price)}</div>
        <div class="actions">
          <button class="btn primary add-btn" data-id="${p.id}">Dodaj</button>
          <button class="btn ghost view-btn" data-id="${p.id}">Podgląd</button>
        </div>
      `;
      return div;
    }

    function renderProducts(filter='all', query=''){
      grid.innerHTML = '';
      let list = PRODUCTS.filter(p => filter==='all' ? true : p.tag===filter);
      if(query && query.trim()){
        const q = query.trim().toLowerCase();
        list = list.filter(p => (p.title + ' ' + p.sku).toLowerCase().includes(q));
      }
      list.forEach((p, idx) => { const card = createCardHTML(p); grid.appendChild(card); gsap.fromTo(card, {opacity:0,y:20,scale:0.995},{opacity:1,y:0,scale:1,duration:0.6,delay:idx*0.03,ease:'power3.out'}); });
    }

    /* ====================
       Render sidebar koszyk
       ==================== */
    function renderCartSidebar(){
      cartItemsEl.innerHTML = '';
      if(cart.length===0){ cartItemsEl.innerHTML = '<div style="color:var(--muted);padding:12px">Koszyk pusty — dodaj produkt!</div>'; } else {
        cart.forEach(item=>{
          const div = document.createElement('div');
          div.className = 'cart-item';
          div.innerHTML = `
            <img src="${item.img}" alt="">
            <div style="flex:1">
              <div style="font-weight:800">${item.title}</div>
              <div style="color:var(--muted);font-size:13px">${item.size? 'Rozmiar: ' + item.size : ''}</div>
              <div style="margin-top:8px;display:flex;gap:8px;align-items:center;justify-content:space-between">
                <div class="qty">
                  <button class="decrease" data-key="${item.key}">-</button>
                  <div style="padding:0 8px">${item.qty}</div>
                  <button class="increase" data-key="${item.key}">+</button>
                </div>
                <div style="font-weight:900;color:var(--text-black)">${formatMoney(item.price * item.qty)}</div>
              </div>
            </div>
            <div style="margin-left:8px">
              <button class="btn ghost remove" data-key="${item.key}">Usuń</button>
            </div>
          `;
          cartItemsEl.appendChild(div);
        });
      }
      cartTotalEl.textContent = formatMoney(cartTotal());
      cartItemsEl.querySelectorAll('.remove').forEach(btn => btn.addEventListener('click', ()=> removeCartItem(btn.dataset.key)));
      cartItemsEl.querySelectorAll('.increase').forEach(btn => btn.addEventListener('click', ()=> changeQty(btn.dataset.key, +1)));
      cartItemsEl.querySelectorAll('.decrease').forEach(btn => btn.addEventListener('click', ()=> changeQty(btn.dataset.key, -1)));
    }

    /* ====================
       Modal produkt + checkout
       ==================== */
    function openProductModal(productId){
      const p = PRODUCTS.find(x=>x.id===productId); if(!p) return;
      modalRoot.style.display = 'block';
      modalRoot.innerHTML = `
        <div class="modal" role="dialog" aria-modal="true">
          <div class="modal-card">
            <div>
              <div class="modal-img" id="modalMain" style="background-image:url('${p.images[0]}')"></div>
              <div class="thumbs" id="modalThumbs">
                ${p.images.map((img,idx)=>`<img src="${img}" data-idx="${idx}" class="${idx===0? 'active' : ''}" />`).join('')}
              </div>
            </div>
            <aside>
              <h3 style="margin:0">${p.title}</h3>
              <div style="color:var(--muted);margin-top:8px">SKU: ${p.sku}</div>
              <div style="margin-top:10px;font-weight:900;color:var(--text-black)">${formatMoney(p.price)}</div>
              <div style="margin-top:12px;color:var(--muted)">Wybierz rozmiar i ilość, następnie zapłać lub dodaj do koszyka.</div>

              <div class="pay-panel">
                <div class="field"><label>Rozmiar</label>
                  <select id="selectSize">
                    ${ (p.sizes && p.sizes.length) ? '<option value="">Wybierz rozmiar</option>' + p.sizes.map(s=>`<option value="${s}">${s}</option>`).join('') : '<option value="">Brak rozmiarów</option>'}
                  </select>
                </div>

                <div class="field"><label>Ilość</label><input type="text" id="qtyInput" value="1" /></div>

                <div style="display:flex;gap:8px">
                  <button class="btn primary" id="addToCartModal">Dodaj do koszyka</button>
                  <button class="btn ghost" id="openPayModal">Zapłać</button>
                </div>

                <div style="margin-top:8px;display:flex;gap:8px"><button class="btn ghost" id="closeModalBtn">Zamknij</button></div>
              </div>
            </aside>
          </div>
        </div>
      `;
      gsap.fromTo('.modal-card',{y:24,opacity:0,scale:0.98},{y:0,opacity:1,scale:1,duration:0.45,ease:'power3.out'});

      modalRoot.querySelectorAll('#modalThumbs img').forEach(img => { img.addEventListener('click', ()=> { modalRoot.querySelectorAll('#modalThumbs img').forEach(i=>i.classList.remove('active')); img.classList.add('active'); document.getElementById('modalMain').style.backgroundImage = `url('${img.src}')`; }); });

      modalRoot.querySelector('#addToCartModal').addEventListener('click', ()=>{ const size = document.getElementById('selectSize') ? document.getElementById('selectSize').value : null; const qty = Math.max(1, parseInt(document.getElementById('qtyInput').value) || 1); addToCart(p.id, size, qty); gsap.fromTo('#addToCartModal',{scale:1},{scale:1.06,duration:0.12,yoyo:true,repeat:1}); showToast('Dodano do koszyka'); });

      modalRoot.querySelector('#openPayModal').addEventListener('click', ()=> { const size = document.getElementById('selectSize') ? document.getElementById('selectSize').value : null; const qty = Math.max(1, parseInt(document.getElementById('qtyInput').value) || 1); openPaymentModal(p, size, qty); });

      modalRoot.querySelector('#closeModalBtn').addEventListener('click', closeModal);
      modalRoot.querySelector('.modal').addEventListener('click', (ev)=>{ if(ev.target.classList.contains('modal')) closeModal(); });
      document.addEventListener('keydown', escClose);
    }

    function clearModal(){ modalRoot.style.display='none'; modalRoot.innerHTML=''; document.removeEventListener('keydown', escClose); }
    function closeModal(){ gsap.to('.modal-card',{y:18,opacity:0,scale:0.98,duration:0.28,onComplete:clearModal}); }
    function escClose(e){ if(e.key==='Escape') closeModal(); }

    /* ============================
       Payment modal
       ============================ */
    function openPaymentModal(product, size, qty){
      modalRoot.innerHTML = `
        <div class="modal" role="dialog" aria-modal="true">
          <div class="modal-card" style="grid-template-columns:1fr;max-width:640px">
            <div style="display:flex;gap:12px;align-items:center">
              <div style="width:120px;height:120px;border-radius:12px;background-image:url('${product.images[0]}');background-size:cover;background-position:center"></div>
              <div>
                <div style="font-weight:900">${product.title}</div>
                <div style="color:var(--muted);margin-top:6px">Rozmiar: ${size|| '—'}</div>
                <div style="color:var(--text-black);font-weight:900;margin-top:8px">${formatMoney(product.price * qty)}</div>
              </div>
            </div>

            <div style="margin-top:12px" class="pay-panel">
              <div class="field"><label>Wybierz metodę</label>
                <div style="display:flex;gap:8px">
                  <button class="chip pay-method active" data-method="blik">BLIK</button>
                  <button class="chip pay-method" data-method="card">Karta</button>
                </div>
              </div>

              <div id="blikBox">
                <div class="field"><label>Kod BLIK</label><input id="blikInput" type="tel" placeholder="Wpisz dowolny kod"></div>
                <div style="display:flex;gap:8px">
                  <button class="btn primary" id="confirmBlik">Zapłać</button>
                  <button class="btn ghost" id="cancelPay">Anuluj</button>
                </div>
              </div>

              <div id="cardBox" style="display:none">
                <div class="field"><label>Numer karty</label><input id="cardNum" placeholder="0000 0000 0000 0000"></div>
                <div class="field" style="display:flex;gap:8px">
                  <input id="cardExp" placeholder="MM/RR" style="flex:1">
                  <input id="cardCVC" placeholder="CVC" style="width:120px">
                </div>
                <div style="display:flex;gap:8px">
                  <button class="btn primary" id="confirmCard">Zapłać</button>
                  <button class="btn ghost" id="cancelCard">Anuluj</button>
                </div>
              </div>
            </div>
          </div>
        </div>
      `;
      gsap.fromTo('.modal-card',{y:24,opacity:0,scale:0.98},{y:0,opacity:1,scale:1,duration:0.45,ease:'power3.out'});

      modalRoot.querySelectorAll('.pay-method').forEach(btn=>{ btn.addEventListener('click', ()=>{ modalRoot.querySelectorAll('.pay-method').forEach(b=>b.classList.remove('active')); btn.classList.add('active'); const m = btn.dataset.method; modalRoot.querySelector('#blikBox').style.display = m==='blik' ? 'block' : 'none'; modalRoot.querySelector('#cardBox').style.display = m==='card' ? 'block' : 'none'; }); });

      modalRoot.querySelector('#confirmBlik').addEventListener('click', (e)=>{ const code = document.getElementById('blikInput').value.trim(); if(!code){ gsap.fromTo('#blikInput',{x:-6},{x:0,duration:0.4,repeat:2,yoyo:true}); return showToast('Wpisz kod BLIK'); } e.disabled=true; e.textContent='Przetwarzam...'; setTimeout(()=>{ addToCart(product.id,size,qty); showThankYou(); e.disabled=false; e.textContent='Zapłać'; clearModal(); },900); });
      modalRoot.querySelector('#confirmCard').addEventListener('click', (e)=>{ e.disabled=true; e.textContent='Przetwarzam...'; setTimeout(()=>{ showThankYou(); e.disabled=false; e.textContent='Zapłać'; clearModal(); },900); });

      modalRoot.querySelector('#cancelPay').addEventListener('click', clearModal); modalRoot.querySelector('#cancelCard').addEventListener('click', clearModal);
      document.addEventListener('keydown', escClose);
    }

    /* ==========================
       Thank you + toast
       ========================== */
    function showThankYou(){
      // clear cart
      cart = [];
      saveCart();
      updateCartUI();

      modalRoot.style.display = 'block';
      modalRoot.innerHTML = `
        <div class="modal" role="dialog" aria-modal="true">
          <div style="width:min(760px,94%);padding:22px;border-radius:12px;background:var(--bg-white);text-align:center;box-shadow:0 20px 50px rgba(0,0,0,0.06)">
            <div style="font-size:44px">🎉</div>
            <div style="font-weight:900;font-size:22px;margin-top:8px">Dziękujemy za zakup!</div>
            <div style="color:var(--muted);margin-top:6px">Twoje zamówienie zostało przyjęte.</div>
            <div style="margin-top:18px"><button class="btn primary" id="closeThanks">Zamknij</button></div>
          </div>
        </div>
      `;
      // toast also
      showToast('Dziękujemy za zakup');

      gsap.fromTo('.modal > div',{y:26,opacity:0,scale:0.98},{y:0,opacity:1,scale:1,duration:0.5,ease:'back.out(1.2)'});
      modalRoot.querySelector('#closeThanks').addEventListener('click', clearModal);
      modalRoot.querySelector('.modal').addEventListener('click', (ev)=>{ if(ev.target.classList.contains('modal')) clearModal(); });
    }

    function showToast(text, ms=2600){
      const t = document.createElement('div'); t.className='toast'; t.textContent = text; toastRoot.appendChild(t);
      gsap.fromTo(t,{y:-10,opacity:0},{y:0,opacity:1,duration:0.36});
      setTimeout(()=>{ gsap.to(t,{y:-10,opacity:0,duration:0.36,onComplete:()=> t.remove()}); }, ms);
    }

    function createSimpleConfetti(canvas){ /* kept for compatibility but not used */ const ctx = canvas.getContext('2d'); canvas.width=innerWidth; canvas.height=innerHeight; let raf; function draw(){} return {play(){}, stop(){ if(raf) cancelAnimationFrame(raf);}} }

    /* ====================
       Event listeners
       ==================== */
    document.addEventListener('click', (e)=>{
      if(e.target.matches('.view-btn') || e.target.closest('.thumb')){ const id = Number(e.target.dataset.id || e.target.closest('.thumb')?.dataset.id); if(id) openProductModal(id); }
      else if(e.target.matches('.add-btn')){ const id = Number(e.target.dataset.id); addToCart(id, null, 1); showToast('Dodano do koszyka'); }
    });

    document.getElementById('openCartBtn').addEventListener('click', ()=>{ cartSidebar.classList.toggle('open'); });
    document.getElementById('closeCart').addEventListener('click', ()=> cartSidebar.classList.remove('open'));
    document.getElementById('clearCart').addEventListener('click', ()=> { if(confirm('Wyczyścić koszyk?')) clearCart(); });

    document.getElementById('checkoutBtn').addEventListener('click', ()=> { if(cart.length===0){ showToast('Koszyk pusty'); return; } openCartPaymentModal(); });

    function openCartPaymentModal(){ modalRoot.style.display = 'block'; const itemsHtml = cart.map(it => `<div style="display:flex;justify-content:space-between;padding:8px 0;border-bottom:1px dashed rgba(0,0,0,0.04)"><div>${escapeHtml(it.title)} ${it.size? '('+it.size+')':''} x${it.qty}</div><div style="color:var(--text-black)">${formatMoney(it.price*it.qty)}</div></div>`).join(''); modalRoot.innerHTML = `
      <div class="modal" role="dialog" aria-modal="true">
        <div class="modal-card" style="grid-template-columns:1fr">
          <div style="width:100%;padding:10px">
            <h3>Podsumowanie zamówienia</h3>
            <div style="margin-top:8px">${itemsHtml}</div>
            <div style="display:flex;justify-content:space-between;margin-top:10px"><div style="color:var(--muted)">Razem</div><div style="font-weight:900;color:var(--text-black)">${formatMoney(cartTotal())}</div></div>

            <div class="pay-panel" style="margin-top:12px">
              <div class="field"><label>Metoda płatności</label>
                <div style="display:flex;gap:8px">
                  <button class="chip pay-method active" data-method="blik">BLIK</button>
                  <button class="chip pay-method" data-method="card">Karta</button>
                </div>
              </div>

              <div id="blikBoxCart">
                <div class="field"><label>Kod BLIK</label><input id="blikInputCart" placeholder="Wpisz dowolny kod"></div>
                <div style="display:flex;gap:8px"><button class="btn primary" id="confirmBlikCart">Zapłać</button><button class="btn ghost" id="cancelCartPay">Anuluj</button></div>
              </div>

              <div id="cardBoxCart" style="display:none">
                <div class="field"><label>Numer karty</label><input id="cardNumCart" placeholder="0000 0000 0000 0000"></div>
                <div style="display:flex;gap:8px"><button class="btn primary" id="confirmCardCart">Zapłać</button><button class="btn ghost" id="cancelCardCart">Anuluj</button></div>
              </div>
            </div>
          </div>
        </div>
      </div>
    `;
      modalRoot.querySelectorAll('.pay-method').forEach(btn=>{ btn.addEventListener('click', ()=>{ modalRoot.querySelectorAll('.pay-method').forEach(b=>b.classList.remove('active')); btn.classList.add('active'); const m = btn.dataset.method; modalRoot.querySelector('#blikBoxCart').style.display = m==='blik' ? 'block' : 'none'; modalRoot.querySelector('#cardBoxCart').style.display = m==='card' ? 'block' : 'none'; }); });
      modalRoot.querySelector('#confirmBlikCart').addEventListener('click', ()=>{ const code = document.getElementById('blikInputCart').value.trim(); if(!code){ gsap.fromTo('#blikInputCart',{x:-6},{x:0,duration:0.4,repeat:2,yoyo:true}); return showToast('Wpisz kod BLIK'); } setTimeout(()=> { showThankYou(); clearModal(); }, 600); });
      modalRoot.querySelector('#confirmCardCart').addEventListener('click', ()=> { setTimeout(()=>{ showThankYou(); clearModal(); }, 700); });
      modalRoot.querySelector('#cancelCartPay').addEventListener('click', clearModal);
      modalRoot.querySelector('#cancelCardCart').addEventListener('click', clearModal);
      modalRoot.querySelector('.modal').addEventListener('click', (ev)=>{ if(ev.target.classList.contains('modal')) clearModal(); });
      document.addEventListener('keydown', escClose);
    }

    /* ====================
       Search + filters
       ==================== */
    document.getElementById('filters').addEventListener('click', (e)=>{ const chip = e.target.closest('.chip'); if(!chip) return; document.querySelectorAll('#filters .chip').forEach(c=>c.classList.remove('active')); chip.classList.add('active'); const f = chip.dataset.filter; renderProducts(f, document.getElementById('searchInput').value); });
    document.getElementById('searchInput').addEventListener('input', (e)=>{ const q = e.target.value; const active = document.querySelector('#filters .chip.active')?.dataset.filter || 'all'; renderProducts(active, q); });
    document.getElementById('shopNowBtn').addEventListener('click', ()=> { document.getElementById('filters').scrollIntoView({behavior:'smooth'}); gsap.from('#productGrid .card',{opacity:0,y:18,duration:0.6,stagger:0.05}); });
    document.getElementById('randomPromo').addEventListener('click', ()=>{ const r = Math.floor(Math.random()*PRODUCTS.length); const p = PRODUCTS[r]; alert('Promocja: -10% na ' + p.title); const el = [...document.querySelectorAll('.card')].find(c => c.querySelector('.title')?.textContent.includes(p.title.split(' ')[0])); if(el) gsap.fromTo(el,{scale:1},{scale:1.04,duration:.12,yoyo:true,repeat:3}); });

    /* ====================
       Init render + cart sync
       ==================== */
    renderProducts(); updateCartUI(); renderCartSidebar();

    // helper to escape html
    function escapeHtml(s){ return String(s).replaceAll('&','&amp;').replaceAll('<','&lt;').replaceAll('>','&gt;'); }
  </script>
</body>
</html>
