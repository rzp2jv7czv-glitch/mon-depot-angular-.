# rzp2jv7czv-glitch/mon-depot-angular-.  



<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MOBO AUTO Occasions - Vente de Véhicules Chinois Certifiés</title>
    <!-- Chargement de Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- Police Inter pour un look moderne et lisible -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@100..900&display=swap" rel="stylesheet">
    <style>
        /* Configuration de la police Inter pour tout le site */
        :root {
            font-family: 'Inter', sans-serif;
        }
        /* Configuration des couleurs personnalisées */
        .primary-color { background-color: #1a3c70; } /* Bleu foncé premium */
        .accent-color { background-color: #3b82f6; } /* Bleu électrique pour l'action */
        .text-primary-color { color: #1a3c70; }
        .text-accent-color { color: #3b82f6; }
        .border-accent-color { border-color: #3b82f6; }

        /* Classe pour masquer les vues */
        .view {
            display: none;
        }

        /* Afficher la vue active */
        .view.active {
            display: block;
        }
    </style>
</head>
<body class="bg-gray-50 text-gray-800">

    <!-- 1. MODAL (Remplacement d'alert/confirm) -->
    <div id="modalMessage" class="fixed inset-0 bg-gray-900 bg-opacity-75 z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-xl shadow-2xl p-6 md:p-8 w-full max-w-md transform transition-all">
            <h3 id="modalTitle" class="text-xl font-bold mb-3 text-primary-color">Action Enregistrée</h3>
            <p id="modalBody" class="text-gray-600 mb-6">Votre demande a été envoyée avec succès. Nous vous contacterons très bientôt !</p>
            <button onclick="hideModal()" class="w-full primary-color text-white py-2 rounded-lg font-semibold hover:bg-opacity-90 transition duration-150 primary-color">Fermer</button>
        </div>
    </div>

    <!-- 2. HEADER/NAVIGATION -->
    <header class="bg-white shadow-md sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-4 flex justify-between items-center">
            
            <!-- Logo -->
            <div class="text-2xl font-extrabold text-primary-color cursor-pointer" onclick="switchView('view-home')">
                MOBO<span class="text-accent-color"> AUTO</span>
            </div>
            
            <!-- Navigation Desktop -->
            <nav class="hidden md:flex space-x-8">
                <a href="#" onclick="switchView('view-catalogue')" class="nav-link text-gray-700 hover:text-accent-color font-medium transition duration-150" data-view="view-catalogue">Catalogue</a>
                <a href="#" onclick="switchView('view-certification')" class="nav-link text-gray-700 hover:text-accent-color font-medium transition duration-150" data-view="view-certification">Certification</a>
                <a href="#" onclick="switchView('view-services-garanties')" class="nav-link text-gray-700 hover:text-accent-color font-medium transition duration-150" data-view="view-services-garanties">Services</a>
                <a href="#" onclick="switchView('view-contact')" class="nav-link text-gray-700 hover:text-accent-color font-medium transition duration-150" data-view="view-contact">Contact</a>
            </nav>

            <!-- Panier et Auth Desktop -->
            <div class="hidden md:flex items-center space-x-3">
                
                <!-- Bouton Panier -->
                <button onclick="handleCartClick()" class="relative p-2 rounded-lg text-gray-700 hover:bg-gray-100 transition duration-150">
                    <!-- Icone Panier (Lucide: ShoppingCart) -->
                    <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="8" cy="21" r="1"></circle><circle cx="19" cy="21" r="1"></circle><path d="M2.05 2.05h2l2.66 12.42a2 2 0 0 0 2 1.58h9.72a2 2 0 0 0 2-1.58L23.95 5.23H6.38"></path></svg>
                    <!-- Compteur de panier -->
                    <span class="cart-count absolute -top-1 -right-1 bg-red-500 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center hidden">0</span>
                </button>

                <!-- Bouton Connexion/Profil -->
                <button id="authButton" onclick="handleAuthAction()" class="flex items-center space-x-2 bg-gray-100 text-primary-color py-2 px-4 rounded-lg font-semibold hover:bg-gray-200 transition duration-150 text-sm">
                    <!-- L'icône et le texte seront mis à jour par updateAuthUI() -->
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"></path><polyline points="10 17 15 12 10 7"></polyline><line x1="15" x2="3" y1="12" y2="12"></line></svg>
                    Connexion
                </button>
            </div>

            <!-- Bouton Menu Mobile -->
            <button id="menuButton" class="md:hidden p-2 rounded-lg text-gray-700 hover:bg-gray-100">
                <!-- Icone Menu (Lucide: Menu) -->
                <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-6 h-6"><line x1="4" x2="20" y1="12" y2="12"></line><line x1="4" x2="20" y1="6" y2="6"></line><line x1="4" x2="20" y1="18" y2="18"></line></svg>
            </button>
        </div>
        
        <!-- Menu Mobile -->
        <div id="mobileMenu" class="md:hidden hidden bg-white shadow-lg pb-4">
            <nav class="flex flex-col space-y-2 px-4">
                <a href="#" onclick="switchView('view-catalogue'); hideMobileMenu()" class="block py-2 text-gray-700 hover:text-accent-color hover:bg-gray-50 rounded-lg font-medium transition duration-150">Catalogue</a>
                <a href="#" onclick="switchView('view-certification'); hideMobileMenu()" class="block py-2 text-gray-700 hover:text-accent-color hover:bg-gray-50 rounded-lg font-medium transition duration-150">Certification</a>
                <a href="#" onclick="switchView('view-services-garanties'); hideMobileMenu()" class="block py-2 text-gray-700 hover:text-accent-color hover:bg-gray-50 rounded-lg font-medium transition duration-150">Services</a>
                <a href="#" onclick="switchView('view-contact'); hideMobileMenu()" class="block py-2 text-gray-700 hover:text-accent-color hover:bg-gray-50 rounded-lg font-medium transition duration-150">Contact</a>
            
                <div class="border-t border-gray-100 pt-2 mt-2"></div>

                <!-- Panier Mobile -->
                <a href="#" onclick="handleCartClick(); hideMobileMenu()" class="block py-2 text-gray-700 hover:text-accent-color hover:bg-gray-50 rounded-lg font-medium transition duration-150 flex items-center space-x-2">
                    <!-- Icone Panier -->
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><circle cx="8" cy="21" r="1"></circle><circle cx="19" cy="21" r="1"></circle><path d="M2.05 2.05h2l2.66 12.42a2 2 0 0 0 2 1.58h9.72a2 2 0 0 0 2-1.58L23.95 5.23H6.38"></path></svg>
                    <span>Mon Panier</span>
                    <span class="cart-count ml-auto bg-red-500 text-white text-xs font-bold rounded-full h-5 w-5 flex items-center justify-center hidden">0</span>
                </a>

                <!-- Auth Mobile -->
                <a id="authMobileButton" href="#" onclick="handleAuthAction(); hideMobileMenu()" class="block py-2 text-gray-700 hover:text-accent-color hover:bg-gray-50 rounded-lg font-medium transition duration-150 flex items-center space-x-2">
                    <!-- Icone Profil -->
                    <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg>
                    Connexion / Profil
                </a>
            </nav>
        </div>
    </header>

    <main id="app-container">
        
        <!-- ============================================== -->
        <!-- VIEW 1: HOME/HERO (Affichée par défaut) -->
        <!-- ============================================== -->
        <section id="view-home" class="view primary-color text-white py-16 md:py-24 active">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 text-center">
                <h1 class="text-4xl md:text-6xl font-extrabold mb-4 leading-tight">
                    L'Expert des Véhicules Chinois d'Occasion Certifiés
                </h1>
                <p class="text-xl md:text-2xl mb-8 font-light max-w-3xl mx-auto opacity-90">
                    Trouvez votre Haval, BYD, Geely ou Changan en toute confiance, avec garantie et historique vérifié.
                </p>
                <!-- Barre de Recherche Simplifiée -->
                <div class="bg-white p-2 rounded-xl shadow-xl max-w-lg mx-auto flex">
                    <input id="searchKeywordHome" type="text" placeholder="Rechercher par Marque (ex: BYD, Haval)..." class="flex-grow p-3 rounded-l-lg text-gray-800 focus:ring-0 focus:outline-none">
                    <button onclick="handleHomeSearch()" class="accent-color text-white p-3 rounded-r-lg font-semibold hover:bg-blue-600 transition duration-150">
                        <!-- Icone Loupe (Lucide: Search) -->
                        <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="w-6 h-6"><circle cx="11" cy="11" r="8"></circle><path d="m21 21-4.3-4.3"></path></svg>
                    </button>
                </div>
                <button onclick="switchView('view-catalogue')" class="mt-8 accent-color text-white py-3 px-8 rounded-lg font-bold text-lg hover:bg-blue-600 transition duration-150 shadow-lg">
                    Voir le Catalogue Complet
                </button>
            </div>
        </section>

        <!-- ============================================== -->
        <!-- VIEW 2: CATALOGUE VÉHICULES ET FILTRES -->
        <!-- ============================================== -->
        <section id="view-catalogue" class="view py-16 md:py-20 max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <h2 class="text-3xl font-bold text-center mb-12">Notre Sélection de Véhicules</h2>

            <!-- Zone de Filtres Avancés -->
            <div class="bg-white p-6 mb-8 rounded-xl shadow-lg border border-gray-100">
                
                <!-- Barre de Recherche par Mot-clé -->
                <h3 class="text-xl font-semibold mb-4 text-primary-color">Recherche par Mot-clé</h3>
                <div class="mb-6">
                    <input id="filtreKeyword" type="text" oninput="renderVehicles()" placeholder="Entrez un modèle, une marque ou un mot-clé (ex: H6, BYD...)" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color transition duration-150">
                </div>
                
                <!-- Filtres Structurés Existants -->
                <h3 class="text-xl font-semibold mb-4 text-primary-color border-t pt-4">Filtres Structurés</h3>
                <div class="grid grid-cols-2 md:grid-cols-4 gap-4">
                    <select id="filtreMarque" onchange="renderVehicles()" class="p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color">
                        <option value="">Toutes les marques</option>
                        <option value="Haval">Haval</option>
                        <option value="BYD">BYD</option>
                        <option value="Changan">Changan</option>
                        <option value="Geely">Geely</option>
                        <option value="NIO">NIO</option>
                    </select>
                    <select id="filtrePrix" onchange="renderVehicles()" class="p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color">
                        <option value="">Prix (max)</option>
                        <option value="15000">Jusqu'à €15,000</option>
                        <option value="25000">Jusqu'à €25,000</option>
                        <option value="999999">€35,000 +</option> 
                    </select>
                    <select id="filtreAnnee" onchange="renderVehicles()" class="p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color">
                        <option value="">Année (min)</option>
                        <option value="2024">2024 +</option>
                        <option value="2023">2023 +</option>
                        <option value="2022">2022 +</option>
                        <option value="2021">2021 +</option>
                        <option value="2020">2020 +</option>
                    </select>
                    <select id="filtreCarburant" onchange="renderVehicles()" class="p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color">
                        <option value="">Carburant</option>
                        <option value="Electrique">Électrique</option>
                        <option value="Essence">Essence</option>
                        <option value="Hybride">Hybride</option>
                    </select>
                </div>
            </div>
            
            <!-- Liste des Véhicules -->
            <div id="vehicleList" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-8">
                <!-- Les cartes de véhicules seront injectées ici par JavaScript -->
            </div>

            <div id="noResults" class="hidden text-center mt-12 text-xl text-gray-500">
                Aucun véhicule ne correspond à vos critères de recherche.
            </div>

        </section>

        <!-- ============================================== -->
        <!-- VIEW 3: HISTORIQUE ET CERTIFICATION -->
        <!-- ============================================== -->
        <section id="view-certification" class="view bg-gray-50 py-16 md:py-20">
            <div class="max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
                <h2 class="text-4xl font-extrabold text-center mb-10 text-primary-color">
                    Historique et Certification : La garantie de votre sérénité
                </h2>
                
                <div class="space-y-6 text-lg text-gray-700 leading-relaxed p-6 border border-gray-200 rounded-xl shadow-lg bg-white">
                    
                    <div class="flex items-center space-x-4 text-accent-color font-bold text-xl">
                         <!-- Icone (Lucide: Scan) -->
                         <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="flex-shrink-0"><path d="M3 7V5a2 2 0 0 1 2-2h2"></path><path d="M17 3h2a2 2 0 0 1 2 2v2"></path><path d="M21 17v2a2 2 0 0 1-2 2h-2"></path><path d="M7 21H5a2 2 0 0 1-2-2v-2"></path></svg>
                        <p>Inspection Rigoureuse en 150 Points</p>
                    </div>
                    
                    <p class="mt-4">
                        Chaque véhicule que nous proposons a fait l’objet d’une inspection rigoureuse en <strong>150 points essentiels</strong>, réalisée par nos experts automobiles certifiés. 
                        Cette vérification minutieuse garantit non seulement la fiabilité mécanique et esthétique de la voiture, mais aussi l’authenticité totale de son kilométrage.
                    </p>

                    <div class="flex items-center space-x-4 text-accent-color font-bold text-xl pt-4 border-t border-gray-100">
                         <!-- Icone (Lucide: FileText) -->
                         <svg xmlns="http://www.w3.org/2000/svg" width="32" height="32" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="flex-shrink-0"><path d="M15 2H6a2 2 0 0 0-2 2v16a2 2 0 0 0 2 2h12a2 2 0 0 0 2-2V7Z"></path><path d="M14 2v4a2 2 0 0 0 2 2h4"></path><path d="M10 9H8"></path><path d="M16 13H8"></path><path d="M16 17H8"></path></svg>
                        <p>Transparence Totale de l'Historique</p>
                    </div>

                    <p class="mt-4">
                        Nous nous engageons à vous fournir un historique <strong>clair, transparent et complet</strong> de chaque véhicule, afin que vous puissiez acheter en toute confiance, sans surprise ni doute. Grâce à notre certification, vous bénéficiez d’une assurance qualité renforcée, qui témoigne de notre sérieux et de notre volonté de vous offrir le meilleur de l’automobile chinoise d'occasion.
                    </p>

                    <div class="pt-6 text-center p-4 rounded-lg bg-gray-900 shadow-xl">
                        <p class="font-extrabold text-white text-2xl">
                            Avec nous, c’est la promesse d’une voiture contrôlée, fiable et prête à prendre la route en toute sécurité.
                        </p>
                    </div>
                    
                    <div class="pt-8 text-center">
                        <button onclick="switchView('view-catalogue')" class="accent-color text-white py-3 px-8 rounded-lg font-bold text-lg hover:bg-blue-600 transition duration-150 shadow-lg">
                            Découvrir nos véhicules certifiés
                        </button>
                    </div>
                </div>
            </div>
        </section>

        <!-- ============================================== -->
        <!-- VIEW 4: SERVICES ET GARANTIES (Anciennement VIEW-AVANTAGES) -->
        <!-- ============================================== -->
        <section id="view-services-garanties" class="view bg-gray-100 py-16 md:py-20">
            <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
                <h2 class="text-3xl font-bold text-center mb-12 text-primary-color">Nos Services et Garanties Complémentaires</h2>
                <div class="grid grid-cols-1 md:grid-cols-2 gap-8">

                    <!-- Carte 1: Financement -->
                    <div class="bg-white p-6 rounded-xl shadow-lg border-t-4 border-accent-color transition duration-300 hover:shadow-xl">
                        <!-- Icone (Lucide: Wallet) -->
                        <svg xmlns="http://www.w3.org/2000/svg" width="36" height="36" viewBox="0 0 24 24" fill="none" stroke="#3b82f6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mb-4"><path d="M21 12V7H5a2 2 0 0 1 0-4h14v2"></path><path d="M21 12V7H5a2 2 0 0 1 0-4h14v2"></path><path d="M21 12V7H5a2 2 0 0 1 0-4h14v2"></path><path d="M3 5v14a2 2 0 0 0 2 2h16.5"></path><path d="M18.5 12.5a.5.5 0 1 0 0 1"></path></svg>
                        <h3 class="text-xl font-semibold mb-2 text-primary-color">Financement Sur Mesure</h3>
                        <p class="text-gray-600">Nous proposons des solutions de financement adaptées à votre budget, directement en ligne. Simulez votre prêt en quelques clics.</p>
                        <button onclick="switchView('view-contact')" class="mt-4 text-accent-color font-semibold hover:underline">Simuler mon financement →</button>
                    </div>

                    <!-- Carte 2: Garantie -->
                    <div class="bg-white p-6 rounded-xl shadow-lg border-t-4 border-accent-color transition duration-300 hover:shadow-xl">
                        <!-- Icone (Lucide: ShieldCheck) -->
                        <svg xmlns="http://www.w3.org/2000/svg" width="36" height="36" viewBox="0 0 24 24" fill="none" stroke="#3b82f6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="mb-4"><path d="M20 13c0 5-2 9-5 11s-5 4-5 11-2-9-5-11-5-4-5-11V7l5-4 5 4 5-4z"></path><path d="m9 12 2 2 4-4"></path></svg>
                        <h3 class="text-xl font-semibold mb-2 text-primary-color">Garantie 12 Mois Incluse</h3>
                        <p class="text-gray-600">Pour chaque achat, vous bénéficiez d'une garantie minimum de 12 mois, extensible selon vos besoins pour une tranquillité d'esprit maximale.</p>
                        <button onclick="switchView('view-contact')" class="mt-4 text-accent-color font-semibold hover:underline">Détails de la garantie →</button>
                    </div>

                </div>
            </div>
        </section>

        <!-- ============================================== -->
        <!-- VIEW 5: CONTACT ET FORMULAIRE -->
        <!-- ============================================== -->
        <section id="view-contact" class="view py-16 md:py-20 max-w-4xl mx-auto px-4 sm:px-6 lg:px-8">
            <h2 class="text-3xl font-bold text-center mb-4">Contactez Notre Équipe</h2>
            <p class="text-center text-gray-600 mb-10">Une question ? Un essai à planifier ? Remplissez le formulaire ci-dessous.</p>

            <div class="bg-white p-8 rounded-xl shadow-2xl border border-gray-100">
                <form id="contactForm">
                    <div class="mb-5">
                        <label for="name" class="block text-sm font-medium text-gray-700 mb-2">Votre Nom Complet</label>
                        <input type="text" id="name" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color transition duration-150">
                    </div>
                    <div class="mb-5">
                        <label for="email" class="block text-sm font-medium text-gray-700 mb-2">Adresse E-mail</label>
                        <input type="email" id="email" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color transition duration-150">
                    </div>
                    <div class="mb-5">
                        <label for="message" class="block text-sm font-medium text-gray-700 mb-2">Votre Message / Demande</label>
                        <textarea id="message" rows="4" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color transition duration-150"></textarea>
                    </div>
                    <button type="submit" class="w-full accent-color text-white py-3 rounded-lg font-bold text-lg hover:bg-blue-600 transition duration-150 shadow-md">
                        Envoyer ma Demande
                    </button>
                </form>

                <!-- COORDONNÉES DIRECTES -->
                <div class="mt-8 pt-6 border-t border-gray-100">
                    <h3 class="text-xl font-bold mb-4 text-primary-color text-center">Nos Coordonnées Directes</h3>
                    
                    <div class="space-y-4 max-w-sm mx-auto">
                        <!-- Contact Item 1: Phone -->
                        <div class="flex items-center space-x-4 p-3 bg-gray-50 rounded-lg shadow-inner">
                            <!-- Icone Téléphone (Lucide: Phone) -->
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#3b82f6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="flex-shrink-0"><path d="M22 16.92v3a2 2 0 0 1-2.18 2 19.79 19.79 0 0 1-8.63-3.07 19.5 19.5 0 0 1-3.67-3.67A19.79 19.79 0 0 1 2 4.18 2 2 0 0 1 4.18 2h3a2 2 0 0 1 2 1.72 12.84 12.84 0 0 0 .7 2.81 2 2 0 0 1-.45 2.11L8.09 9.91a16 16 0 0 0 6 6l1.27-1.27a2 2 0 0 1 2.11-.45 12.84 12.84 0 0 0 2.81.7A2 2 0 0 1 22 16.92z"></path></svg>
                            <div>
                                <p class="text-xs font-medium text-gray-500">Téléphone</p>
                                <p class="font-semibold text-lg text-primary-color">0768857796</p>
                            </div>
                        </div>

                        <!-- Contact Item 2: Email -->
                        <div class="flex items-center space-x-4 p-3 bg-gray-50 rounded-lg shadow-inner">
                            <!-- Icone Email (Lucide: Mail) -->
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#3b82f6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="flex-shrink-0"><rect width="20" height="16" x="2" y="4" rx="2"></rect><path d="m22 7-8.97 5.7a1.94 1.94 0 0 1-2.06 0L2 7"></path></svg>
                            <div>
                                <p class="text-xs font-medium text-gray-500">E-mail</p>
                                <p class="font-semibold text-primary-color break-all">patrickguiro505@gmail.com</p>
                            </div>
                        </div>

                        <!-- Contact Item 3: Localisation -->
                        <div class="flex items-center space-x-4 p-3 bg-gray-50 rounded-lg shadow-inner">
                            <!-- Icone Location (Lucide: MapPin) -->
                            <svg xmlns="http://www.w3.org/2000/svg" width="24" height="24" viewBox="0 0 24 24" fill="none" stroke="#3b82f6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="flex-shrink-0"><path d="M20 10c0 6-8 12-8 12s-8-6-8-12a8 8 0 0 1 16 0Z"></path><circle cx="12" cy="10" r="3"></circle></svg>
                            <div>
                                <p class="text-xs font-medium text-gray-500">Ville / Pays</p>
                                <p class="font-semibold text-primary-color">Abidjan, Côte d'Ivoire</p>
                            </div>
                        </div>
                    </div>
                </div>
            </div>
        </section>

        <!-- ============================================== -->
        <!-- VIEW 6: AUTHENTIFICATION (LOGIN/REGISTER) -->
        <!-- ============================================== -->
        <section id="view-auth" class="view py-16 md:py-20 max-w-lg mx-auto px-4 sm:px-6 lg:px-8">
            <h2 class="text-3xl font-bold text-center mb-4 text-primary-color" id="authTitle">Connexion à votre compte</h2>
            <p class="text-center text-gray-600 mb-10" id="authSubtitle">Accédez à vos sélections et vos favoris.</p>

            <div class="bg-white p-8 rounded-xl shadow-2xl border border-gray-100">
                
                <!-- Zone de Formulaires -->
                <div id="loginFormContainer">
                    <form id="loginForm">
                        <div class="mb-5">
                            <label for="loginEmail" class="block text-sm font-medium text-gray-700 mb-2">E-mail</label>
                            <input type="email" id="loginEmail" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color transition duration-150">
                        </div>
                        <div class="mb-5">
                            <label for="loginPassword" class="block text-sm font-medium text-gray-700 mb-2">Mot de passe</label>
                            <input type="password" id="loginPassword" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color transition duration-150">
                        </div>
                        <button type="submit" class="w-full primary-color text-white py-3 rounded-lg font-bold text-lg hover:bg-opacity-90 transition duration-150 shadow-md">
                            Se Connecter
                        </button>
                    </form>
                    
                    <p class="mt-6 text-center text-sm">
                        Pas encore de compte ? 
                        <a href="#" onclick="showAuthView('register')" class="text-accent-color font-semibold hover:underline">Créer un compte</a>
                    </p>
                </div>

                <div id="registerFormContainer" class="hidden">
                    <form id="registerForm">
                        <div class="mb-5">
                            <label for="registerEmail" class="block text-sm font-medium text-gray-700 mb-2">E-mail</label>
                            <input type="email" id="registerEmail" required class="w-full p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color transition duration-150">
                        </div>
                        <div class="mb-5">
                            <label for="registerPassword" class="block text-sm font-medium text-gray-700 mb-2">Mot de passe (6 caractères min.)</label>
                            <input type="password" id="registerPassword" required minlength="6" class="w-full p-3 border border-gray-300 rounded-lg focus:ring-accent-color focus:border-accent-color transition duration-150">
                        </div>
                        <button type="submit" class="w-full accent-color text-white py-3 rounded-lg font-bold text-lg hover:bg-blue-600 transition duration-150 shadow-md">
                            S'inscrire
                        </button>
                    </form>

                    <p class="mt-6 text-center text-sm">
                        Déjà un compte ? 
                        <a href="#" onclick="showAuthView('login')" class="text-primary-color font-semibold hover:underline">Se connecter</a>
                    </p>
                </div>
            </div>
        </section>

    </main>

    <!-- 7. FOOTER -->
    <footer class="primary-color text-white py-12 mt-16">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="grid grid-cols-2 md:grid-cols-4 gap-8 mb-8">
                <!-- Navigation Rapide -->
                <div>
                    <h4 class="font-bold mb-4 text-accent-color">Navigation</h4>
                    <ul>
                        <li><a href="#" onclick="switchView('view-catalogue')" class="hover:underline">Catalogue</a></li>
                        <li><a href="#" onclick="switchView('view-certification')" class="hover:underline">Certification</a></li>
                        <li><a href="#" onclick="switchView('view-services-garanties')" class="hover:underline">Services</a></li>
                        <li><a href="#" onclick="switchView('view-contact')" class="hover:underline">Contact</a></li>
                    </ul>
                </div>
                <!-- Marques -->
                <div>
                    <h4 class="font-bold mb-4 text-accent-color">Marques Phares</h4>
                    <ul>
                        <li>Haval Occasion</li>
                        <li>BYD Occasion</li>
                        <li>Changan Occasion</li>
                        <li>NIO Occasion</li>
                    </ul>
                </div>
                <!-- Légal -->
                <div>
                    <h4 class="font-bold mb-4 text-accent-color">Information</h4>
                    <ul>
                        <li><a href="#" class="hover:underline">Mentions Légales</a></li>
                        <li><a href="#" class="hover:underline">Politique de Confidentialité (RGPD)</a></li>
                        <li><a href="#" class="hover:underline">Conditions Générales de Vente</a></li>
                    </ul>
                </div>
                <!-- Réseaux Sociaux -->
                <div>
                    <h4 class="font-bold mb-4 text-accent-color">Suivez-nous</h4>
                    <div class="flex space-x-3">
                        
                        <!-- Facebook Logo -->
                        <a href="#" target="_blank" aria-label="Facebook" class="w-8 h-8 bg-white rounded-full flex items-center justify-center text-primary-color hover:bg-gray-200 transition duration-150 cursor-pointer">
                            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="#3b82f6" stroke="none"><path d="M9 8h-3v4h3v12h5v-12h3.642l.358-4h-4v-1.667c0-.955.192-1.333 1.588-1.333h2.412v-3h-3.412c-2.885 0-4.588 1.509-4.588 4.332v2.668z"/></svg>
                        </a>

                        <!-- Instagram Logo -->
                        <a href="#" target="_blank" aria-label="Instagram" class="w-8 h-8 bg-white rounded-full flex items-center justify-center text-primary-color hover:bg-gray-200 transition duration-150 cursor-pointer">
                            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="#3b82f6" stroke-width="2" stroke-linecap="round" stroke-linejoin="round" class="feather feather-instagram"><rect x="2" y="2" width="20" height="20" rx="5" ry="5"></rect><path d="M16 11.37A4 4 0 1 1 12.63 8 4 4 0 0 1 16 11.37z"></path><line x1="17.5" y1="6.5" x2="17.51" y2="6.5"></line></svg>
                        </a>
                        
                        <!-- Twitter/X Logo -->
                        <a href="#" target="_blank" aria-label="Twitter / X" class="w-8 h-8 bg-white rounded-full flex items-center justify-center text-primary-color hover:bg-gray-200 transition duration-150 cursor-pointer">
                            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="#1a3c70" stroke="none">
                                <path d="M18.244 2.25h3.308l-7.227 8.26 8.502 11.24H16.17l-5.214-6.814L4.99 21.75H1.68l7.73-8.835L1.254 2.25H4.66l4.79 6.096L14.793 2.25zM17.456 19.53L19.54 22.083h-1.378L15.89 19.53zm-1.15-5.617L2.946 4.31H6.07L18.425 19.53H17.456z"/>
                            </svg>
                        </a>

                        <!-- YouTube Logo -->
                        <a href="#" target="_blank" aria-label="YouTube" class="w-8 h-8 bg-white rounded-full flex items-center justify-center text-primary-color hover:bg-gray-200 transition duration-150 cursor-pointer">
                            <svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="#ef4444" stroke="none"><path d="M21.58 7.19a2.76 2.76 0 0 0-1.95-1.95C18.17 5 12 5 12 5s-6.17 0-7.63.24a2.76 2.76 0 0 0-1.95 1.95C2 8.63 2 12 2 12s0 3.37.42 4.81a2.76 2.76 0 0 0 1.95 1.95C5.83 19 12 19 12 19s6.17 0 7.63-.42a2.76 2.76 0 0 0 1.95-1.95C22 15.37 22 12 22 12s0-3.37-.42-4.81z"/><path d="m10 15 5-3-5-3v6z" fill="#ffffff"/></svg>
                        </a>
                    </div>
                </div>
            </div>
            <div class="text-center pt-8 border-t border-blue-800">
                &copy; 2025 MOBO AUTO Occasions. Tous droits réservés.
            </div>
        </div>
    </footer>

    <!-- 8. JAVASCRIPT POUR LA LOGIQUE (Catalogue, Modal, Menu Mobile, Filtres, Navigation) -->
    <script type="module">
        // ===============================================
        // IMPORTS ET CONFIGURATION FIREBASE (MANDATORY)
        // ===============================================
        import { initializeApp } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-app.js";
        import { 
            getAuth, signInAnonymously, signInWithCustomToken, onAuthStateChanged, signOut, 
            signInWithEmailAndPassword, createUserWithEmailAndPassword // Ajout des fonctions Auth E-mail/Mdp
        } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-auth.js";
        import { getFirestore, setLogLevel } from "https://www.gstatic.com/firebasejs/11.6.1/firebase-firestore.js";

        // Variables globales du Canvas
        const appId = typeof __app_id !== 'undefined' ? __app_id : 'default-app-id';
        const firebaseConfig = typeof __firebase_config !== 'undefined' ? JSON.parse(__firebase_config) : null;
        const initialAuthToken = typeof __initial_auth_token !== 'undefined' ? __initial_auth_token : null;
        setLogLevel('Debug'); 

        let app, db, auth;
        let userAuthState = null; // Stocke l'objet utilisateur Firebase (null ou User)

        // Mock Data for Vehicles
        const vehicles = [
            { id: 1, marque: 'BYD', modele: 'Atto 3', annee: 2023, prix: 28990, km: 12000, carburant: 'Electrique', image: 'https://placehold.co/400x300/1a3c70/ffffff?text=BYD+ATTO+3' },
            { id: 2, marque: 'Haval', modele: 'Jolion', annee: 2022, prix: 18500, km: 35000, carburant: 'Essence', image: 'https://placehold.co/400x300/3b82f6/ffffff?text=HAVAL+JOLION' },
            { id: 3, marque: 'Changan', modele: 'CS75 Plus', annee: 2024, prix: 32000, km: 1500, carburant: 'Hybride', image: 'https://placehold.co/400x300/1a3c70/ffffff?text=CHANGAN+CS75' },
            { id: 4, marque: 'Geely', modele: 'Geometry C', annee: 2023, prix: 26500, km: 22000, carburant: 'Electrique', image: 'https://placehold.co/400x300/3b82f6/ffffff?text=GEELY+GEOM+C' },
            { id: 5, marque: 'NIO', modele: 'ES8', annee: 2022, prix: 45000, km: 50000, carburant: 'Electrique', image: 'https://placehold.co/400x300/1a3c70/ffffff?text=NIO+ES8' },
            { id: 6, marque: 'Haval', modele: 'H6', annee: 2021, prix: 15900, km: 60000, carburant: 'Essence', image: 'https://placehold.co/400x300/3b82f6/ffffff?text=HAVAL+H6' },
        ];

        let cart = []; // Panier simulé

        // ===============================================
        // LOGIQUE D'AUTHENTIFICATION ET INITIALISATION
        // ===============================================

        async function initFirebase() {
            try {
                if (firebaseConfig) {
                    app = initializeApp(firebaseConfig);
                    auth = getAuth(app);
                    db = getFirestore(app);

                    // Gérer l'authentification initiale (Custom Token > Anonyme)
                    if (initialAuthToken) {
                        await signInWithCustomToken(auth, initialAuthToken);
                    } else {
                        await signInAnonymously(auth);
                    }

                    // Écouter les changements d'état d'authentification
                    onAuthStateChanged(auth, (user) => {
                        userAuthState = user;
                        updateAuthUI(user);
                        console.log("Firebase Auth State Changed. User ID:", user ? user.uid : 'Anon/Logged out');
                    });
                }
            } catch (error) {
                console.error("Erreur lors de l'initialisation de Firebase:", error);
                showModal("Erreur de Service", "Problème de connexion aux services de données. Veuillez réessayer.");
            }
        }

        /**
         * Met à jour les boutons Connexion/Déconnexion
         * @param {Object | null} user 
         */
        function updateAuthUI(user) {
            const authButton = document.getElementById('authButton');
            const authMobileButton = document.getElementById('authMobileButton');

            if (user && !user.isAnonymous) {
                // Utilisateur connecté (Email/Mdp, etc.)
                const text = `Profil (${user.email || 'Utilisateur'})`;
                const icon = '<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M19 21v-2a4 4 0 0 0-4-4H9a4 4 0 0 0-4 4v2"></path><circle cx="12" cy="7" r="4"></circle></svg>';
                
                authButton.innerHTML = `${icon} ${text}`;
                authMobileButton.innerHTML = `${icon} <span>${text}</span>`;
            } else if (user && user.isAnonymous) {
                // Utilisateur anonyme
                const text = 'Connexion';
                const icon = '<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"></path><polyline points="10 17 15 12 10 7"></polyline><line x1="15" x2="3" y1="12" y2="12"></line></svg>';
                
                authButton.innerHTML = `${icon} ${text}`;
                authMobileButton.innerHTML = `${icon} <span>${text}</span>`;
            } else {
                 // Déconnecté (devrait rarement arriver avec signInAnonymously)
                const text = 'Connexion';
                const icon = '<svg xmlns="http://www.w3.org/2000/svg" width="20" height="20" viewBox="0 0 24 24" fill="none" stroke="currentColor" stroke-width="2" stroke-linecap="round" stroke-linejoin="round"><path d="M15 3h4a2 2 0 0 1 2 2v14a2 2 0 0 1-2 2h-4"></path><polyline points="10 17 15 12 10 7"></polyline><line x1="15" x2="3" y1="12" y2="12"></line></svg>';
                
                authButton.innerHTML = `${icon} ${text}`;
                authMobileButton.innerHTML = `${icon} <span>${text}</span>`;
            }
        }

        /**
         * Gère le clic sur le bouton Connexion/Déconnexion.
         */
        window.handleAuthAction = async function() {
            if (userAuthState && !userAuthState.isAnonymous) {
                // Utilisateur LOGGÉ (non-anonyme) -> Déconnexion
                try {
                    await signOut(auth);
                    showModal("Déconnexion réussie", "Vous avez été déconnecté. À bientôt !");
                } catch (error) {
                    showModal("Erreur de Déconnexion", "Impossible de se déconnecter. Réessayez.");
                    console.error("Erreur de déconnexion:", error);
                }
            } else {
                // Utilisateur ANONYME ou DÉCONNECTÉ -> Afficher la page de connexion
                switchView('view-auth');
                showAuthView('login');
            }
        }

        /**
         * Bascule entre le formulaire de connexion et d'inscription.
         * @param {string} view 'login' ou 'register'
         */
        window.showAuthView = function(view) {
            const loginForm = document.getElementById('loginFormContainer');
            const registerForm = document.getElementById('registerFormContainer');
            const title = document.getElementById('authTitle');
            const subtitle = document.getElementById('authSubtitle');

            if (view === 'login') {
                loginForm.classList.remove('hidden');
                registerForm.classList.add('hidden');
                title.textContent = "Connexion à votre compte";
                subtitle.textContent = "Accédez à vos sélections et vos favoris.";
            } else {
                loginForm.classList.add('hidden');
                registerForm.classList.remove('hidden');
                title.textContent = "Créer un nouveau compte";
                subtitle.textContent = "Rejoignez notre communauté en quelques secondes.";
            }
        }

        /**
         * Gère la connexion par E-mail/Mot de passe.
         */
        document.getElementById('loginForm').addEventListener('submit', async function(event) {
            event.preventDefault();
            const email = document.getElementById('loginEmail').value;
            const password = document.getElementById('loginPassword').value;

            try {
                // Tente la connexion
                const userCredential = await signInWithEmailAndPassword(auth, email, password);
                
                // Si la connexion réussit, bascule vers la page d'accueil
                showModal("Connexion réussie !", `Bienvenue, ${userCredential.user.email}.`);
                switchView('view-home');
            } catch (error) {
                console.error("Erreur de connexion:", error);
                let message = "Erreur de connexion. Vérifiez votre e-mail et votre mot de passe.";
                if (error.code === 'auth/user-not-found') {
                    message = "Aucun compte trouvé avec cet e-mail. Veuillez vous inscrire.";
                } else if (error.code === 'auth/wrong-password') {
                    message = "Mot de passe incorrect.";
                }
                showModal("Échec de la Connexion", message);
            }
        });

        /**
         * Gère l'inscription par E-mail/Mot de passe.
         */
        document.getElementById('registerForm').addEventListener('submit', async function(event) {
            event.preventDefault();
            const email = document.getElementById('registerEmail').value;
            const password = document.getElementById('registerPassword').value;

            try {
                // Tente l'inscription
                const userCredential = await createUserWithEmailAndPassword(auth, email, password);
                
                // Si l'inscription réussit, l'utilisateur est automatiquement connecté
                showModal("Inscription réussie !", `Compte créé pour ${userCredential.user.email}. Vous êtes maintenant connecté.`);
                switchView('view-home');
            } catch (error) {
                console.error("Erreur d'inscription:", error);
                let message = "Erreur lors de la création du compte. Réessayez.";
                if (error.code === 'auth/email-already-in-use') {
                    message = "Cet e-mail est déjà utilisé. Veuillez vous connecter.";
                } else if (error.code === 'auth/weak-password') {
                    message = "Le mot de passe doit contenir au moins 6 caractères.";
                }
                showModal("Échec de l'Inscription", message);
            }
        });

        // ===============================================
        // FONCTIONS D'INTERFACE UTILISATEUR GENERALES
        // ===============================================

        /**
         * Affiche un message modal personnalisé.
         */
        function showModal(title, body) {
            document.getElementById('modalTitle').textContent = title;
            document.getElementById('modalBody').innerHTML = body;
            document.getElementById('modalMessage').classList.remove('hidden');
        }

        /**
         * Cache le message modal.
         */
        window.hideModal = function() {
            document.getElementById('modalMessage').classList.add('hidden');
        }

        /**
         * Bascule l'affichage entre les différentes sections.
         * @param {string} viewId ID de la vue à afficher (ex: 'view-catalogue')
         */
        window.switchView = function(viewId) {
            document.querySelectorAll('.view').forEach(view => {
                view.classList.remove('active');
            });

            const targetView = document.getElementById(viewId);
            if (targetView) {
                targetView.classList.add('active');
                if (viewId === 'view-catalogue') {
                    // Charger les véhicules quand on accède au catalogue
                    renderVehicles();
                }
            } else {
                console.error(`Vue non trouvée: ${viewId}`);
            }
        }

        /**
         * Affiche/Cache le menu de navigation mobile.
         */
        function toggleMobileMenu() {
            document.getElementById('mobileMenu').classList.toggle('hidden');
        }

        /**
         * Cache le menu mobile (appelé par les liens du menu).
         */
        window.hideMobileMenu = function() {
            document.getElementById('mobileMenu').classList.add('hidden');
        }

        // Attachement de l'événement au bouton Menu Mobile
        document.getElementById('menuButton').addEventListener('click', toggleMobileMenu);
        
        // ===============================================
        // LOGIQUE MÉTIER (CATALOGUE, PANIER)
        // ===============================================

        /**
         * Gère le clic sur le bouton Panier.
         */
        window.handleCartClick = function() {
            if (cart.length === 0) {
                showModal("Mon Panier", "Votre panier est vide. Visitez le <a href='#' onclick='switchView(\"view-catalogue\"); hideModal()' class='text-accent-color font-bold hover:underline'>Catalogue</a> pour ajouter un véhicule.");
                return;
            }

            const cartDetails = cart.map((item, index) => 
                `<li>${index + 1}. ${item.marque} ${item.modele} - ${item.prix.toLocaleString('fr-FR')} €</li>`
            ).join('');

            showModal(
                "Mon Panier", 
                `<p>Vous avez <strong>${cart.length}</strong> véhicule(s) en sélection :</p>
                 <ul class="list-disc ml-6 mt-3 mb-4">${cartDetails}</ul>
                 <p class="text-sm text-gray-500">Note: Dans une application réelle, cette action mènerait à la page de commande.</p>
                 <button onclick="clearCart()" class="mt-4 w-full bg-red-500 text-white py-2 rounded-lg font-semibold hover:bg-red-600 transition duration-150">Vider le Panier</button>`
            );
        }

        /**
         * Ajoute un véhicule au panier simulé.
         * @param {number} vehicleId ID du véhicule à ajouter.
         */
        window.addToCart = function(vehicleId) {
            const vehicle = vehicles.find(v => v.id === vehicleId);
            if (vehicle) {
                if (cart.some(item => item.id === vehicleId)) {
                    showModal("Véhicule Déjà Ajouté", `${vehicle.marque} ${vehicle.modele} est déjà dans votre panier.`);
                } else {
                    cart.push(vehicle);
                    updateCartCount();
                    showModal("Ajout au Panier", `${vehicle.marque} ${vehicle.modele} a été ajouté à votre panier !`);
                }
            }
        }

        /**
         * Vide le panier et met à jour l'UI.
         */
        window.clearCart = function() {
            cart = [];
            updateCartCount();
            hideModal();
            showModal("Panier Vidé", "Votre panier a été vidé avec succès.");
        }
        
        /**
         * Met à jour les compteurs de panier dans le header.
         */
        function updateCartCount() {
            const count = cart.length;
            const elements = document.querySelectorAll('.cart-count');
            elements.forEach(el => {
                el.textContent = count;
                if (count > 0) {
                    el.classList.remove('hidden');
                } else {
                    el.classList.add('hidden');
                }
            });
        }


        /**
         * Gère la soumission du formulaire de contact.
         */
        document.getElementById('contactForm').addEventListener('submit', function(event) {
            event.preventDefault();
            const name = document.getElementById('name').value;
            const email = document.getElementById('email').value;
            const message = document.getElementById('message').value;

            // Logique de simulation d'envoi
            console.log("Formulaire envoyé:", { name, email, message });
            
            // Affichage de la modal de succès
            showModal(
                "Demande Envoyée", 
                `Merci ${name} pour votre message concernant la demande : <br><br> 
                *“${message.substring(0, 50)}...”* <br><br>
                Nous vous répondrons à l'adresse **${email}** dans les plus brefs délais.`
            );
            
            // Réinitialisation du formulaire
            this.reset();
        });


        /**
         * Gère la recherche simple depuis la page d'accueil.
         */
        window.handleHomeSearch = function() {
            const keyword = document.getElementById('searchKeywordHome').value.trim();
            // Met à jour le nouveau champ de recherche par mot-clé dans le catalogue
            const filtreKeywordInput = document.getElementById('filtreKeyword');
            if (filtreKeywordInput) {
                filtreKeywordInput.value = keyword;
            }
            // Bascule vers la vue catalogue, ce qui appellera renderVehicles et appliquera le filtre
            switchView('view-catalogue');
        }

        /**
         * Filtre les véhicules et met à jour le catalogue.
         */
        window.renderVehicles = function() {
            // Récupération du mot-clé de recherche
            const keyword = document.getElementById('filtreKeyword').value.toLowerCase().trim();

            const marque = document.getElementById('filtreMarque').value;
            const prixMax = parseFloat(document.getElementById('filtrePrix').value) || Infinity;
            const anneeMin = parseInt(document.getElementById('filtreAnnee').value) || 0;
            const carburant = document.getElementById('filtreCarburant').value;
            
            let filteredVehicles = vehicles.filter(v => {
                
                // Logique de filtre par mot-clé (marque ou modèle)
                const matchesKeyword = !keyword || 
                    v.marque.toLowerCase().includes(keyword) || 
                    v.modele.toLowerCase().includes(keyword);

                // Logique de filtre existante
                const matchesMarque = !marque || v.marque === marque;
                const matchesPrix = v.prix <= prixMax;
                const matchesAnnee = v.annee >= anneeMin;
                const matchesCarburant = !carburant || v.carburant === carburant;
                
                // Combinaison de tous les filtres, incluant le mot-clé
                return matchesKeyword && matchesMarque && matchesPrix && matchesAnnee && matchesCarburant;
            });

            const listContainer = document.getElementById('vehicleList');
            const noResults = document.getElementById('noResults');
            listContainer.innerHTML = '';

            if (filteredVehicles.length === 0) {
                noResults.classList.remove('hidden');
            } else {
                noResults.classList.add('hidden');
                filteredVehicles.forEach(vehicle => {
                    const card = `
                        <div class="bg-white rounded-xl shadow-lg overflow-hidden transition duration-300 hover:shadow-xl hover:-translate-y-1 border border-gray-100">
                            <img src="${vehicle.image}" alt="${vehicle.marque} ${vehicle.modele}" onerror="this.onerror=null;this.src='https://placehold.co/400x300/e5e7eb/333333?text=Image+Indisponible'" class="w-full h-48 object-cover">
                            <div class="p-5">
                                <h3 class="text-xl font-bold text-primary-color mb-1">${vehicle.marque} ${vehicle.modele}</h3>
                                <p class="text-3xl font-extrabold text-accent-color mb-3">${vehicle.prix.toLocaleString('fr-FR')} €</p>
                                <div class="text-sm text-gray-600 space-y-1 mb-4">
                                    <p>Année: <strong>${vehicle.annee}</strong></p>
                                    <p>Kilométrage: <strong>${vehicle.km.toLocaleString('fr-FR')} km</strong></p>
                                    <p>Carburant: <strong>${vehicle.carburant}</strong></p>
                                </div>
                                <button onclick="addToCart(${vehicle.id})" class="w-full primary-color text-white py-2 rounded-lg font-semibold text-sm hover:bg-opacity-90 transition duration-150">
                                    Ajouter à ma sélection
                                </button>
                            </div>
                        </div>
                    `;
                    listContainer.innerHTML += card;
                });
            }
        }


        // ===============================================
        // INITIALISATION AU CHARGEMENT DE LA PAGE
        // ===============================================
        window.onload = function() {
            initFirebase();
            switchView('view-home'); // Assurez-vous que la vue d'accueil est bien affichée
            updateCartCount(); // Initialiser le compteur de panier
        }

    </script>
<!-- Code injected by live-server -->
<script>
	// <![CDATA[  <-- For SVG support
	if ('WebSocket' in window) {
		(function () {
			function refreshCSS() {
				var sheets = [].slice.call(document.getElementsByTagName("link"));
				var head = document.getElementsByTagName("head")[0];
				for (var i = 0; i < sheets.length; ++i) {
					var elem = sheets[i];
					var parent = elem.parentElement || head;
					parent.removeChild(elem);
					var rel = elem.rel;
					if (elem.href && typeof rel != "string" || rel.length == 0 || rel.toLowerCase() == "stylesheet") {
						var url = elem.href.replace(/(&|\?)_cacheOverride=\d+/, '');
						elem.href = url + (url.indexOf('?') >= 0 ? '&' : '?') + '_cacheOverride=' + (new Date().valueOf());
					}
					parent.appendChild(elem);
				}
			}
			var protocol = window.location.protocol === 'http:' ? 'ws://' : 'wss://';
			var address = protocol + window.location.host + window.location.pathname + '/ws';
			var socket = new WebSocket(address);
			socket.onmessage = function (msg) {
				if (msg.data == 'reload') window.location.reload();
				else if (msg.data == 'refreshcss') refreshCSS();
			};
			if (sessionStorage && !sessionStorage.getItem('IsThisFirstTime_Log_From_LiveServer')) {
				console.log('Live reload enabled.');
				sessionStorage.setItem('IsThisFirstTime_Log_From_LiveServer', true);
			}
		})();
	}
	else {
		console.error('Upgrade your browser. This Browser is NOT supported WebSocket for Live-Reloading.');
	}
	// ]]>
</script>
</body>
</html>
