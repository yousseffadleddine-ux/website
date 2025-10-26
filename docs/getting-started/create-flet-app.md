<!DOCTYPE html>
<html lang="fr">
<head>
  <meta charset="UTF-8" />
  <meta name="viewport" content="width=device-width, initial-scale=1.0" />
  <title>Homeal - Deluxe v3</title>
  <script src="https://cdn.tailwindcss.com"></script>
  <style>
    body {
      box-sizing: border-box;
      background: #f9fafb;
      overflow-x: hidden;
    }

    .mobile-container {
      max-width: 375px;
      margin: 0 auto;
      min-height: 100vh;
      background: white;
      box-shadow: 0 0 20px rgba(0,0,0,0.1);
      overflow: hidden;
      position: relative;
    }

    /* Pages avec transition floue */
    .page {
      position: absolute;
      top: 0;
      left: 100%;
      width: 100%;
      height: 100%;
      opacity: 0;
      transform: translateX(100%);
      transition: all 0.7s cubic-bezier(0.65, 0, 0.35, 1);
      backdrop-filter: blur(0px);
    }

    .page.active {
      left: 0;
      opacity: 1;
      transform: translateX(0);
      backdrop-filter: blur(6px);
    }

    .page.slide-out-left {
      transform: translateX(-100%);
      opacity: 0;
      backdrop-filter: blur(10px);
    }

    /* Couleurs & boutons */
    .btn-primary {
      background: #A8E6CF;
      color: #2d5a3d;
      font-weight: 600;
      transition: all 0.25s ease;
      transform: scale(1);
    }

    .btn-primary:hover {
      background: #95d9bb;
    }

    /* 💥 Rebond au clic */
    .btn-primary:active {
      transform: scale(0.94);
      box-shadow: 0 0 8px rgba(168,230,207,0.8);
    }

    .text-primary { color: #2d5a3d; }
    .bg-primary-light { background: #f0faf5; }

    /* Barre de navigation */
    .nav-bar {
      position: fixed;
      bottom: 0;
      left: 50%;
      transform: translateX(-50%);
      width: 100%;
      max-width: 375px;
      background: white;
      border-top: 1px solid #e5e7eb;
      display: flex;
      justify-content: space-around;
      padding: 0.75rem 0;
      z-index: 20;
    }

    .nav-item {
      text-align: center;
      color: #9ca3af;
      font-size: 0.9rem;
      cursor: pointer;
      transition: color 0.3s ease;
    }

    .nav-item.active {
      color: #2d5a3d;
      font-weight: 600;
    }

    /* Logo animation */
    #lid-group {
      animation: liftAndDisappear 3s ease-out 0.2s forwards;
    }

    @keyframes liftAndDisappear {
      0% { transform: translate(0, 8px); opacity: 1; }
      80% { transform: translate(0, -25px); opacity: 1; }
      100% { transform: translate(0, -25px); opacity: 0; }
    }

    #steam-group {
      animation: steamRise 2s ease-in-out infinite;
    }

    @keyframes steamRise {
      0% { opacity: 0.6; transform: scale(1); }
      50% { opacity: 1; transform: scale(1.2) translateY(-3px); }
      100% { opacity: 0.6; transform: scale(1); }
    }

    /* Splash */
    .splash-screen {
      position: absolute;
      inset: 0;
      background: #A8E6CF;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      animation: splashZoom 2s ease forwards;
      z-index: 100;
    }

    @keyframes splashZoom {
      0% { opacity: 1; transform: scale(1); }
      80% { transform: scale(1.05); }
      100% { opacity: 0; transform: scale(1.1); visibility: hidden; }
    }

    /* Intro */
    .intro-screen {
      position: absolute;
      inset: 0;
      background: white;
      display: flex;
      align-items: center;
      justify-content: center;
      flex-direction: column;
      z-index: 50;
      opacity: 1;
      animation: fadeOutIntro 2s ease 3s forwards;
    }

    @keyframes fadeOutIntro {
      0% { opacity: 1; transform: scale(1); }
      100% { opacity: 0; transform: scale(1.1); visibility: hidden; }
    }
  </style>
</head>
<body>
  <div class="mobile-container">

    <!-- Splash -->
    <div id="splash" class="splash-screen">
      <svg width="120" height="120" viewBox="0 0 80 80">
        <g transform="translate(40,40)">
          <circle cx="0" cy="0" r="28" fill="white"/>
          <text x="0" y="10" text-anchor="middle" fill="#2d5a3d" font-family="sans-serif" font-weight="bold" font-size="16">Homeal</text>
        </g>
      </svg>
    </div>

    <!-- Intro -->
    <div id="intro" class="intro-screen">
      <svg width="160" height="160" viewBox="0 0 80 80">
        <g transform="translate(40,40)">
          <circle cx="0" cy="0" r="28" fill="#A8E6CF" stroke="#2d5a3d" stroke-width="2"/>
          <circle cx="0" cy="0" r="25" fill="white" stroke="#2d5a3d" stroke-width="1"/>
          <g id="steam-group">
            <path d="M-8 -15 C -6 -20, -2 -20, 0 -15" stroke="#2d5a3d" stroke-width="1.5" fill="none"/>
            <path d="M4 -15 C 6 -20, 10 -20, 12 -15" stroke="#2d5a3d" stroke-width="1.5" fill="none"/>
          </g>
          <g id="lid-group">
            <path d="M -18 0 Q 0 -14, 18 0 Z" fill="#2d5a3d"/>
            <circle cx="0" cy="-12" r="2" fill="#2d5a3d"/>
          </g>
        </g>
      </svg>
      <h1 class="mt-4 text-xl font-bold text-primary">Homeal</h1>
    </div>

    <!-- Connexion -->
    <div id="login" class="page bg-white px-6 py-8">
      <div class="text-center mb-12 mt-10">
        <h1 class="text-2xl font-bold text-primary">Bienvenue sur Homeal</h1>
        <p class="text-gray-500 text-sm mt-2">Vos repas faits maison, livrés avec amour 🍲</p>
      </div>
      <form id="loginForm" class="space-y-4">
        <input type="email" placeholder="Adresse e-mail" class="w-full border rounded-lg px-4 py-3 focus:outline-none focus:ring-2 focus:ring-[#A8E6CF]">
        <input type="password" placeholder="Mot de passe" class="w-full border rounded-lg px-4 py-3 focus:outline-none focus:ring-2 focus:ring-[#A8E6CF]">
        <button type="submit" class="w-full py-3 rounded-lg btn-primary text-lg">Se connecter</button>
      </form>
    </div>

    <!-- Accueil -->
    <div id="home" class="page bg-primary-light p-6">
      <h2 class="text-2xl font-bold text-primary mb-4">Accueil</h2>
      <p class="text-gray-600 mb-6">Bienvenue sur votre espace Homeal !</p>
      <button class="w-full py-3 rounded-lg btn-primary text-lg" onclick="showPage('discover')">Découvrir les repas 🍛</button>
    </div>

    <!-- Découvrir -->
    <div id="discover" class="page bg-white p-6">
      <h2 class="text-2xl font-bold text-primary mb-4">Découvrir les repas</h2>
      <div class="grid gap-4">
        <div class="p-4 rounded-lg shadow border border-gray-200">
          <h3 class="font-semibold text-lg text-primary">Lasagnes maison</h3>
          <p class="text-gray-600 text-sm mt-1">Préparées par <strong>Julie</strong></p>
        </div>
        <div class="p-4 rounded-lg shadow border border-gray-200">
          <h3 class="font-semibold text-lg text-primary">Curry de légumes</h3>
          <p class="text-gray-600 text-sm mt-1">Préparé par <strong>Karim</strong></p>
        </div>
      </div>
      <button class="mt-6 w-full py-3 rounded-lg bg-gray-200 text-primary font-semibold" onclick="showPage('profile')">Voir mon profil 👩‍🍳</button>
    </div>

    <!-- Profil -->
    <div id="profile" class="page bg-primary-light p-6">
      <h2 class="text-2xl font-bold text-primary mb-4">Profil cuisinier</h2>
      <div class="bg-white p-4 rounded-lg shadow">
        <p><strong>Nom :</strong> Julie Martin</p>
        <p><strong>Spécialité :</strong> Cuisine italienne 🍝</p>
        <p><strong>Note :</strong> ⭐⭐⭐⭐☆</p>
      </div>
      <button class="mt-6 w-full py-3 rounded-lg bg-gray-200 text-primary font-semibold" onclick="showPage('home')">Retour à l'accueil</button>
    </div>

    <!-- Navigation -->
    <div class="nav-bar">
      <div class="nav-item active" data-target="home">🏠<br>Accueil</div>
      <div class="nav-item" data-target="discover">🍴<br>Découvrir</div>
      <div class="nav-item" data-target="profile">👩‍🍳<br>Profil</div>
    </div>
  </div>

  <audio id="startupSound" preload="auto">
    <source src="https://cdn.pixabay.com/download/audio/2023/03/15/audio_173b40dfbb.mp3?filename=app-bootup-140928.mp3" type="audio/mpeg">
  </audio>

  <script>
    const pages = document.querySelectorAll('.page');
    let currentPage = null;

    function showPage(id) {
      if (id === currentPage) return;
      const current = currentPage ? document.getElementById(currentPage) : null;
      const next = document.getElementById(id);

      if (current) {
        current.classList.remove('active');
        current.classList.add('slide-out-left');
        setTimeout(() => current.classList.remove('slide-out-left'), 700);
      }

      next.classList.add('active');
      currentPage = id;

      document.querySelectorAll('.nav-item').forEach(n => n.classList.remove('active'));
      const nav = document.querySelector(`.nav-item[data-target="${id}"]`);
      if (nav) nav.classList.add('active');
    }

    document.getElementById('loginForm').addEventListener('submit', e => {
      e.preventDefault();
      showPage('home');
    });

    document.querySelectorAll('.nav-item').forEach(item => {
      item.addEventListener('click', () => showPage(item.dataset.target));
    });

    window.addEventListener('load', () => {
      const audio = document.getElementById('startupSound');
      audio.play().catch(() => {});
      setTimeout(() => (document.getElementById('splash').style.display = 'none'), 2000);
      setTimeout(() => {
        document.getElementById('intro').style.display = 'none';
        showPage('login');
      }, 4800);
    });
  </script>
</body>
</html>
