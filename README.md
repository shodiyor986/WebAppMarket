<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>WebAppMarket</title>
    
    <!-- Telegram Web App SDK -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    
    <!-- Font Awesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- GitHub Pages uchun base URL -->
    <base href="/">
    
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        html {
            margin: 0;
            padding: 0;
            height: 100%;
            overflow: hidden;
        }
        
        body {
            margin: 0;
            padding: 0;
            height: 100%;
            background-color: var(--bg-color);
            color: var(--text-primary);
            transition: background-color 0.3s, color 0.3s;
            position: relative;
            overflow: hidden;
        }

        :root {
            --bg-color: #f8f9fa;
            --card-bg: #ffffff;
            --text-primary: #1a1a1a;
            --text-secondary: #666666;
            --accent-color: #0088cc;
            --accent-hover: #0077b3;
            --border-color: #eaeaea;
            --shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
        }

        body.dark {
            --bg-color: #1a1a1a;
            --card-bg: #2d2d2d;
            --text-primary: #ffffff;
            --text-secondary: #b0b0b0;
            --border-color: #404040;
            --shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 0 16px 30px 16px;
            width: 100%;
            background-color: var(--bg-color);
            overflow-y: auto;
            overflow-x: hidden;
            position: relative;
            z-index: 10;
            transform: translateY(-40px);
            height: calc(100vh + 40px);
            padding-top: 56px;
        }

        .container::-webkit-scrollbar {
            width: 4px;
        }

        .container::-webkit-scrollbar-track {
            background: transparent;
        }

        .container::-webkit-scrollbar-thumb {
            background: var(--accent-color);
            border-radius: 4px;
        }

        .header {
            display: flex;
            align-items: center;
            justify-content: flex-end;
            margin-bottom: 24px;
            padding-bottom: 12px;
            border-bottom: 1px solid var(--border-color);
        }

        .theme-toggle {
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 50%;
            width: 40px;
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--text-primary);
            font-size: 18px;
            transition: all 0.3s;
            box-shadow: var(--shadow);
        }

        .theme-toggle:hover {
            transform: scale(1.05);
        }

        .language-section {
            margin-bottom: 24px;
        }

        .language-title {
            font-size: 14px;
            color: var(--text-secondary);
            margin-bottom: 10px;
            text-align: center;
        }

        .language-buttons {
            display: flex;
            gap: 10px;
            justify-content: center;
            align-items: center;
            flex-wrap: wrap;
        }

        .lang-btn {
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 30px;
            padding: 10px 20px;
            font-size: 15px;
            font-weight: 500;
            color: var(--text-secondary);
            cursor: pointer;
            display: flex;
            align-items: center;
            gap: 8px;
            transition: all 0.3s;
            box-shadow: var(--shadow);
            min-width: 100px;
            justify-content: center;
        }

        .lang-btn i {
            font-size: 18px;
        }

        .lang-btn.active {
            background: var(--accent-color);
            color: white;
            border-color: var(--accent-color);
            transform: scale(1.05);
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin-bottom: 24px;
        }

        .stat-card {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 16px 8px;
            text-align: center;
            box-shadow: var(--shadow);
            border: 1px solid var(--border-color);
            min-height: 100px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
        }

        .stat-card i {
            font-size: 24px;
            color: var(--accent-color);
            margin-bottom: 8px;
        }

        .stat-card .stat-value {
            font-size: 20px;
            font-weight: 700;
            color: var(--text-primary);
        }

        .stat-card .stat-label {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 4px;
        }

        .time-value {
            font-size: 18px;
            font-weight: 700;
            color: var(--text-primary);
        }
        
        .date-value {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 2px;
        }

        .search-section {
            margin-bottom: 20px;
        }

        .search-box {
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 25px;
            padding: 12px 20px;
            display: flex;
            align-items: center;
            gap: 10px;
            box-shadow: var(--shadow);
        }

        .search-box i {
            color: var(--text-secondary);
            font-size: 16px;
        }

        .search-box input {
            flex: 1;
            border: none;
            background: none;
            outline: none;
            color: var(--text-primary);
            font-size: 16px;
            width: 100%;
        }

        .search-box input::placeholder {
            color: var(--text-secondary);
        }

        .categories {
            display: flex;
            gap: 10px;
            overflow-x: auto;
            padding-bottom: 12px;
            margin-bottom: 20px;
            scrollbar-width: none;
        }

        .categories::-webkit-scrollbar {
            display: none;
        }

        .category-btn {
            background: var(--card-bg);
            border: 1px solid var(--border-color);
            border-radius: 20px;
            padding: 8px 16px;
            font-size: 14px;
            color: var(--text-secondary);
            cursor: pointer;
            white-space: nowrap;
            transition: all 0.3s;
            box-shadow: var(--shadow);
        }

        .category-btn.active {
            background: var(--accent-color);
            color: white;
            border-color: var(--accent-color);
        }

        .section-title {
            margin-bottom: 16px;
        }

        .section-title h2 {
            font-size: 18px;
            font-weight: 600;
        }

        .apps-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 16px;
            margin-bottom: 30px;
        }

        .app-card {
            background: var(--card-bg);
            border-radius: 16px;
            overflow: hidden;
            box-shadow: var(--shadow);
            border: 1px solid var(--border-color);
            transition: transform 0.2s, box-shadow 0.2s;
            cursor: pointer;
        }

        .app-card:hover {
            transform: translateY(-2px);
            box-shadow: 0 4px 12px rgba(0, 0, 0, 0.1);
        }

        .app-image {
            width: 100%;
            height: 120px;
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
            transition: all 0.3s;
        }

        .app-image i {
            font-size: 48px;
            color: white;
            transition: transform 0.3s;
        }

        .app-card:hover .app-image i {
            transform: scale(1.1);
        }

        .app-badge {
            position: absolute;
            top: 8px;
            right: 8px;
            background: rgba(255, 255, 255, 0.9);
            color: var(--accent-color);
            padding: 4px 8px;
            border-radius: 12px;
            font-size: 10px;
            font-weight: 600;
            z-index: 2;
        }

        .app-info {
            padding: 12px;
        }

        .app-info h3 {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 4px;
            color: var(--text-primary);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .app-description {
            font-size: 12px;
            color: var(--text-secondary);
            margin-bottom: 8px;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
        }

        .app-review {
            font-size: 11px;
            color: var(--text-secondary);
            padding: 4px 8px;
            background: var(--bg-color);
            border-radius: 12px;
            display: inline-block;
            margin-top: 4px;
            font-style: italic;
            max-width: 100%;
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
        }

        .app-meta {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-top: 8px;
            font-size: 11px;
        }

        .favorite-btn {
            cursor: pointer;
            transition: transform 0.2s;
        }

        .favorite-btn:hover {
            transform: scale(1.1);
        }

        .scroll-top-btn {
            position: fixed;
            bottom: 30px;
            right: 20px;
            background: var(--accent-color);
            border: none;
            border-radius: 50%;
            width: 56px;
            height: 56px;
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: white;
            font-size: 24px;
            transition: all 0.3s;
            box-shadow: 0 4px 12px rgba(0, 136, 204, 0.3);
            z-index: 1000;
        }

        .scroll-top-btn:hover {
            transform: scale(1.1);
            background: var(--accent-hover);
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            right: 0;
            bottom: 0;
            background: rgba(0, 0, 0, 0.5);
            z-index: 2000;
            align-items: center;
            justify-content: center;
            padding: 16px;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: var(--card-bg);
            border-radius: 24px;
            padding: 24px;
            width: 100%;
            max-width: 500px;
            max-height: 80vh;
            overflow-y: auto;
        }

        .app-detail {
            text-align: center;
        }

        .app-detail-image {
            width: 120px;
            height: 120px;
            border-radius: 60px;
            margin: 0 auto 20px;
            display: flex;
            align-items: center;
            justify-content: center;
            transition: all 0.3s;
        }

        .app-detail-image i {
            font-size: 60px;
            color: white;
        }

        .app-detail h2 {
            font-size: 24px;
            margin-bottom: 8px;
        }

        .app-detail-category {
            color: var(--accent-color);
            font-size: 14px;
            margin-bottom: 8px;
        }

        .app-detail-review {
            font-size: 13px;
            color: var(--text-secondary);
            background: var(--bg-color);
            padding: 12px;
            border-radius: 12px;
            margin: 16px 0;
            font-style: italic;
            text-align: left;
        }

        .app-detail-description {
            color: var(--text-secondary);
            margin-bottom: 24px;
            line-height: 1.6;
            text-align: left;
        }

        .open-bot-btn {
            background: #28a745;
            color: white;
            border: none;
            border-radius: 12px;
            padding: 16px;
            width: 100%;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 8px;
            margin-top: 10px;
            transition: all 0.3s;
        }

        .open-bot-btn:hover {
            background: #218838;
            transform: scale(1.02);
        }

        @media (max-width: 600px) {
            .container {
                padding: 0 12px 20px 12px;
                padding-top: 56px;
            }
            
            .lang-btn {
                min-width: 80px;
                padding: 8px 12px;
                font-size: 14px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="theme-toggle" id="themeToggle">
                <i class="fas fa-moon"></i>
            </div>
        </div>

        <div class="language-section">
            <div class="language-title">Tilni tanlang / Выберите язык / Select language</div>
            <div class="language-buttons">
                <button class="lang-btn active" id="langUz">
                    <i class="fas fa-flag">🇺🇿</i>
                    <span>O'zbek</span>
                </button>
                <button class="lang-btn" id="langRu">
                    <i class="fas fa-flag">🇷🇺</i>
                    <span>Русский</span>
                </button>
                <button class="lang-btn" id="langEn">
                    <i class="fas fa-flag">🇬🇧</i>
                    <span>English</span>
                </button>
            </div>
        </div>

        <div class="stats-grid">
            <div class="stat-card">
                <i class="fas fa-rocket"></i>
                <div class="stat-value" id="totalApps">0</div>
                <div class="stat-label">Jami applar</div>
            </div>
            <div class="stat-card">
                <i class="fas fa-clock"></i>
                <div class="time-value" id="currentTime">00:00</div>
                <div class="date-value" id="currentDate">01.01.2026</div>
                <div class="stat-label">Toshkent vaqti</div>
            </div>
        </div>

        <div class="search-section">
            <div class="search-box">
                <i class="fas fa-search"></i>
                <input type="text" id="searchInput" placeholder="Applar qidirish...">
            </div>
        </div>

        <div class="categories" id="categories">
            <button class="category-btn active" data-category="all">Barchasi</button>
            <button class="category-btn" data-category="game">O'yinlar</button>
            <button class="category-btn" data-category="tool">Asboblar</button>
            <button class="category-btn" data-category="social">Ijtimoiy</button>
            <button class="category-btn" data-category="education">Ta'lim</button>
        </div>

        <div class="section-title">
            <h2>Barcha applar</h2>
        </div>

        <div class="apps-grid" id="appsGrid"></div>
    </div>

    <div class="scroll-top-btn" id="scrollTopBtn">
        <i class="fas fa-arrow-up"></i>
    </div>

    <div class="modal" id="detailModal">
        <div class="modal-content app-detail" id="appDetailContent"></div>
    </div>

    <script>
        (function() {
            const tg = window.Telegram.WebApp;
            tg.expand();
            tg.ready();

            if (tg.colorScheme === 'dark') {
                document.body.classList.add('dark');
            }

            // ============= TARJIMALAR =============
            const translations = {
                uz: {
                    total_apps: 'Jami applar',
                    tashkent_time: 'Toshkent vaqti',
                    search: 'Applar qidirish...',
                    all: 'Barchasi',
                    games: 'O\'yinlar',
                    tools: 'Asboblar',
                    social: 'Ijtimoiy',
                    education: 'Ta\'lim',
                    all_apps: 'Barcha applar',
                    review_exists: 'Sharh bor',
                    no_review: 'Sharh yo\'q',
                    no_review_text: 'Sharh mavjud emas',
                    open_bot: 'Botga o\'tish',
                    about_bot: 'Bot haqida',
                    close: 'Yopish',
                    not_found: 'Hech qanday app topilmadi'
                },
                ru: {
                    total_apps: 'Всего приложений',
                    tashkent_time: 'Ташкент время',
                    search: 'Поиск приложений...',
                    all: 'Все',
                    games: 'Игры',
                    tools: 'Инструменты',
                    social: 'Социальные',
                    education: 'Образование',
                    all_apps: 'Все приложения',
                    review_exists: 'Есть отзыв',
                    no_review: 'Нет отзыва',
                    no_review_text: 'Отзыва нет',
                    open_bot: 'Перейти к боту',
                    about_bot: 'О боте',
                    close: 'Закрыть',
                    not_found: 'Приложений не найдено'
                },
                en: {
                    total_apps: 'Total apps',
                    tashkent_time: 'Tashkent time',
                    search: 'Search apps...',
                    all: 'All',
                    games: 'Games',
                    tools: 'Tools',
                    social: 'Social',
                    education: 'Education',
                    all_apps: 'All apps',
                    review_exists: 'Has review',
                    no_review: 'No review',
                    no_review_text: 'No review available',
                    open_bot: 'Open bot',
                    about_bot: 'About bot',
                    close: 'Close',
                    not_found: 'No apps found'
                }
            };

            let currentLang = 'uz';

            // ============= APPLAR RO'YXATI =============
            const apps = [
                {
                    id: 1,
                    name: 'Mini Game Bot',
                    category: 'game',
                    description: 'Qiziqarli mini o\'yinlar to\'plami. Puzzle, arcade va strategiya o\'yinlari.',
                    review: '🎮 Ajoyib o\'yinlar! Eng zo\'r pazzillar to\'plami.',
                    botUsername: 'mygamebot',
                    icon: 'fa-gamepad'
                },
                {
                    id: 2,
                    name: 'Spotify Downloader',
                    category: 'tool',
                    description: 'Musiqa yuklash va tinglash uchun bot. Millionlab qo\'shiqlar.',
                    review: '🎵 Ajoyib musiqa boti! Sifatli ovoz va yuklash imkoniyati.',
                    botUsername: 'SpotifyMusicDownloaderBot',
                    icon: 'fa-spotify'
                },
                {
                    id: 3,
                    name: 'Wallet',
                    category: 'tool',
                    description: 'Kriptovalyuta hamyoni va kurslarini kuzatish.',
                    review: '💰 Ishonchli va qulay kripto hamyon.',
                    botUsername: 'wallet',
                    icon: 'fa-wallet'
                },
                {
                    id: 4,
                    name: 'Chat App',
                    category: 'social',
                    description: 'Tezkor messenger va guruhli chatlar.',
                    review: '💬 Do\'stlar bilan muloqot uchun ajoyib platforma.',
                    botUsername: 'chatappbot',
                    icon: 'fa-comments'
                },
                {
                    id: 5,
                    name: 'TestGO',
                    category: 'education',
                    description: 'Matematik testlar va mantiqiy masalalar.',
                    review: '🧮 Matematikani o\'rganish uchun zo\'r bot!',
                    botUsername: 'Quiz00_01_bot',
                    icon: 'fa-calculator'
                },
                {
                    id: 6,
                    name: 'Tingla',
                    category: 'tool',
                    description: 'Audio kitoblar va podkastlar.',
                    review: '🎧 Zo\'r audio kitoblar to\'plami!',
                    botUsername: 'TinglaBot',
                    icon: 'fa-headphones'
                }
            ];

            let favorites = JSON.parse(localStorage.getItem('favorites')) || [];
            let currentCategory = 'all';

            // DOM elements
            const appsGrid = document.getElementById('appsGrid');
            const searchInput = document.getElementById('searchInput');
            const totalAppsEl = document.getElementById('totalApps');
            const currentTimeEl = document.getElementById('currentTime');
            const currentDateEl = document.getElementById('currentDate');
            const themeToggle = document.getElementById('themeToggle');
            const scrollTopBtn = document.getElementById('scrollTopBtn');
            const detailModal = document.getElementById('detailModal');
            const categoryBtns = document.querySelectorAll('.category-btn');
            const langUz = document.getElementById('langUz');
            const langRu = document.getElementById('langRu');
            const langEn = document.getElementById('langEn');

            // ============= RANGLAR PALITRASI =============
            const colors = [
                '#FF6B6B', // qizil
                '#4ECDC4', // turkuaz
                '#45B7D1', // och ko'k
                '#96CEB4', // yashil
                '#FFEAA7', // sariq
                '#DDA0DD', // och siyoh
                '#98D8C8', // yalpiz
                '#F7DC6F', // oltin
                '#BB8FCE', // siyohrang
                '#82E0AA', // yashil
                '#F1948A', // och qizil
                '#85C1E2', // ko'k
                '#F0B27A', // apelsin
                '#A569BD', // binafsha
                '#5DADE2', // havo rang
                '#48C9B0'  // firuza
            ];

            // ============= APP NOMIGA QARAB ICON VA RANG TANLASH =============
            function getAppIconAndColor(appName, category, defaultIcon) {
                const name = appName.toLowerCase();
                let icon = defaultIcon;
                let colorIndex = 0;
                
                // App nomiga qarab icon tanlash
                if (name.includes('game') || name.includes('oyin') || name.includes('o\'yin')) {
                    icon = 'fa-gamepad';
                    colorIndex = 1;
                } else if (name.includes('spotify') || name.includes('music') || name.includes('musiqa')) {
                    icon = 'fa-spotify';
                    colorIndex = 2;
                } else if (name.includes('wallet') || name.includes('hamyon') || name.includes('crypto')) {
                    icon = 'fa-wallet';
                    colorIndex = 3;
                } else if (name.includes('chat') || name.includes('message') || name.includes('social')) {
                    icon = 'fa-comments';
                    colorIndex = 4;
                } else if (name.includes('test') || name.includes('quiz') || name.includes('math')) {
                    icon = 'fa-calculator';
                    colorIndex = 5;
                } else if (name.includes('tingla') || name.includes('audio') || name.includes('podcast')) {
                    icon = 'fa-headphones';
                    colorIndex = 6;
                } else if (name.includes('video') || name.includes('movie') || name.includes('kino')) {
                    icon = 'fa-video';
                    colorIndex = 7;
                } else if (name.includes('photo') || name.includes('rasm') || name.includes('image')) {
                    icon = 'fa-camera';
                    colorIndex = 8;
                } else if (name.includes('book') || name.includes('kitob') || name.includes('library')) {
                    icon = 'fa-book';
                    colorIndex = 9;
                } else if (name.includes('news') || name.includes('yangilik') || name.includes('xabar')) {
                    icon = 'fa-newspaper';
                    colorIndex = 10;
                } else if (name.includes('weather') || name.includes('ob-havo') || name.includes('havo')) {
                    icon = 'fa-cloud-sun';
                    colorIndex = 11;
                } else if (name.includes('map') || name.includes('xarita') || name.includes('location')) {
                    icon = 'fa-map-location-dot';
                    colorIndex = 12;
                } else if (name.includes('translate') || name.includes('tarjima') || name.includes('til')) {
                    icon = 'fa-language';
                    colorIndex = 13;
                } else {
                    // Kategoriya bo'yicha default icon
                    switch(category) {
                        case 'game': icon = 'fa-dice'; colorIndex = 1; break;
                        case 'tool': icon = 'fa-tools'; colorIndex = 2; break;
                        case 'social': icon = 'fa-users'; colorIndex = 4; break;
                        case 'education': icon = 'fa-graduation-cap'; colorIndex = 5; break;
                        default: icon = 'fa-robot'; colorIndex = 0;
                    }
                }
                
                // App nomi bo'yicha rang indeksini hisoblash
                const nameSum = name.split('').reduce((acc, char) => acc + char.charCodeAt(0), 0);
                colorIndex = (colorIndex + nameSum) % colors.length;
                
                return {
                    icon: icon,
                    color: colors[colorIndex]
                };
            }

            // ============= TIL FUNKSIYASI =============
            function setLanguage(lang) {
                currentLang = lang;
                
                document.querySelectorAll('.lang-btn').forEach(btn => btn.classList.remove('active'));
                if (lang === 'uz') langUz.classList.add('active');
                if (lang === 'ru') langRu.classList.add('active');
                if (lang === 'en') langEn.classList.add('active');

                // Matnlarni yangilash
                document.querySelector('.stat-card .stat-label').textContent = translations[lang].total_apps;
                document.querySelectorAll('.stat-card .stat-label')[1].textContent = translations[lang].tashkent_time;
                searchInput.placeholder = translations[lang].search;
                
                document.querySelectorAll('.category-btn[data-category="all"]')[0].textContent = translations[lang].all;
                document.querySelectorAll('.category-btn[data-category="game"]')[0].textContent = translations[lang].games;
                document.querySelectorAll('.category-btn[data-category="tool"]')[0].textContent = translations[lang].tools;
                document.querySelectorAll('.category-btn[data-category="social"]')[0].textContent = translations[lang].social;
                document.querySelectorAll('.category-btn[data-category="education"]')[0].textContent = translations[lang].education;
                
                document.querySelector('.section-title h2').textContent = translations[lang].all_apps;
                
                renderApps();
                tg.HapticFeedback.impactOccurred('light');
            }

            langUz.addEventListener('click', () => setLanguage('uz'));
            langRu.addEventListener('click', () => setLanguage('ru'));
            langEn.addEventListener('click', () => setLanguage('en'));

            // ============= VAQT =============
            function updateUzbekTime() {
                const now = new Date();
                const uzbekTime = new Date(now.getTime() + (5 * 60 * 60 * 1000));
                
                const hours = uzbekTime.getUTCHours().toString().padStart(2, '0');
                const minutes = uzbekTime.getUTCMinutes().toString().padStart(2, '0');
                currentTimeEl.textContent = `${hours}:${minutes}`;
                
                const day = uzbekTime.getUTCDate().toString().padStart(2, '0');
                const month = (uzbekTime.getUTCMonth() + 1).toString().padStart(2, '0');
                const year = uzbekTime.getUTCFullYear();
                currentDateEl.textContent = `${day}.${month}.${year}`;
            }

            updateUzbekTime();
            setInterval(updateUzbekTime, 1000);

            // ============= APPLARNI KO'RSATISH =============
            function renderApps() {
                const searchTerm = searchInput.value.toLowerCase();
                let filteredApps = apps;

                if (currentCategory !== 'all') {
                    filteredApps = filteredApps.filter(app => app.category === currentCategory);
                }

                if (searchTerm) {
                    filteredApps = filteredApps.filter(app => 
                        app.name.toLowerCase().includes(searchTerm) || 
                        app.description.toLowerCase().includes(searchTerm)
                    );
                }

                if (filteredApps.length === 0) {
                    appsGrid.innerHTML = `
                        <div style="grid-column: 1/-1; text-align: center; padding: 40px;">
                            <i class="fas fa-box-open" style="font-size: 48px; color: var(--text-secondary);"></i>
                            <p style="color: var(--text-secondary); margin-top: 16px;">${translations[currentLang].not_found}</p>
                        </div>
                    `;
                    totalAppsEl.textContent = apps.length;
                    return;
                }

                appsGrid.innerHTML = filteredApps.map(app => {
                    const isFavorite = favorites.includes(app.id);
                    const categoryNames = {
                        game: translations[currentLang].games,
                        tool: translations[currentLang].tools,
                        social: translations[currentLang].social,
                        education: translations[currentLang].education
                    };
                    
                    // App nomiga qarab icon va rang tanlash
                    const { icon, color } = getAppIconAndColor(app.name, app.category, app.icon);

                    return `
                        <div class="app-card" onclick="window.showAppDetail(${app.id})">
                            <div class="app-image" style="background: linear-gradient(135deg, ${color}, ${adjustColor(color, -20)});">
                                <i class="fas ${icon}"></i>
                                <span class="app-badge">${categoryNames[app.category]}</span>
                            </div>
                            <div class="app-info">
                                <h3>${app.name}</h3>
                                <div class="app-description">${app.description.substring(0, 60)}...</div>
                                <div class="app-review">${app.review || translations[currentLang].no_review_text}</div>
                                <div class="app-meta">
                                    <span><i class="fas fa-comment"></i> ${app.review ? translations[currentLang].review_exists : translations[currentLang].no_review}</span>
                                    <span class="favorite-btn" onclick="event.stopPropagation(); window.toggleFavorite(${app.id})">
                                        <i class="fas ${isFavorite ? 'fa-heart' : 'fa-heart-o'}" style="color: ${isFavorite ? '#ff4444' : 'var(--accent-color)'};"></i>
                                    </span>
                                </div>
                            </div>
                        </div>
                    `;
                }).join('');

                totalAppsEl.textContent = apps.length;
            }

            // Rangni ochroq yoki to'qroq qilish uchun yordamchi funksiya
            function adjustColor(hex, percent) {
                // Hex rangni RGB ga o'tkazish va sozlash
                let R = parseInt(hex.substring(1,3), 16);
                let G = parseInt(hex.substring(3,5), 16);
                let B = parseInt(hex.substring(5,7), 16);
                
                R = Math.min(255, Math.max(0, R + percent));
                G = Math.min(255, Math.max(0, G + percent));
                B = Math.min(255, Math.max(0, B + percent));
                
                return `#${((1 << 24) + (R << 16) + (G << 8) + B).toString(16).slice(1)}`;
            }

            // ============= GLOBAL FUNKSIYALAR =============
            window.toggleFavorite = function(appId) {
                const index = favorites.indexOf(appId);
                if (index === -1) {
                    favorites.push(appId);
                } else {
                    favorites.splice(index, 1);
                }
                localStorage.setItem('favorites', JSON.stringify(favorites));
                renderApps();
                tg.HapticFeedback.impactOccurred('light');
            };

            window.openBot = function(botUsername) {
                let username = botUsername;
                if (!username.startsWith('@')) {
                    username = '@' + username;
                }
                tg.openTelegramLink(`https://t.me/${username.replace('@', '')}`);
                tg.HapticFeedback.impactOccurred('medium');
            };

            window.showAppDetail = function(appId) {
                const app = apps.find(a => a.id === appId);
                if (!app) return;

                const categoryNames = {
                    game: translations[currentLang].games,
                    tool: translations[currentLang].tools,
                    social: translations[currentLang].social,
                    education: translations[currentLang].education
                };
                
                const { icon, color } = getAppIconAndColor(app.name, app.category, app.icon);

                document.getElementById('appDetailContent').innerHTML = `
                    <div class="app-detail-image" style="background: linear-gradient(135deg, ${color}, ${adjustColor(color, -20)});">
                        <i class="fas ${icon}"></i>
                    </div>
                    <h2>${app.name}</h2>
                    <div class="app-detail-category">${categoryNames[app.category]}</div>
                    
                    <div class="app-detail-review">
                        <i class="fas fa-quote-left"></i> ${app.review || translations[currentLang].no_review_text}
                    </div>
                    
                    <p class="app-detail-description">
                        <strong>${translations[currentLang].about_bot}:</strong><br>
                        ${app.description}
                    </p>

                    <button class="open-bot-btn" onclick="window.openBot('${app.botUsername}')">
                        <i class="fab fa-telegram"></i> ${translations[currentLang].open_bot}
                    </button>
                `;

                detailModal.classList.add('active');
            };

            // ============= EVENT LISTENERLAR =============
            themeToggle.addEventListener('click', () => {
                document.body.classList.toggle('dark');
                const icon = themeToggle.querySelector('i');
                icon.className = document.body.classList.contains('dark') ? 'fas fa-sun' : 'fas fa-moon';
                tg.HapticFeedback.impactOccurred('light');
            });

            scrollTopBtn.addEventListener('click', () => {
                document.querySelector('.container').scrollTo({ top: 0, behavior: 'smooth' });
                tg.HapticFeedback.impactOccurred('light');
            });

            categoryBtns.forEach(btn => {
                btn.addEventListener('click', () => {
                    categoryBtns.forEach(b => b.classList.remove('active'));
                    btn.classList.add('active');
                    currentCategory = btn.dataset.category;
                    renderApps();
                    tg.HapticFeedback.impactOccurred('light');
                });
            });

            searchInput.addEventListener('input', () => renderApps());

            window.addEventListener('click', (e) => {
                if (e.target === detailModal) {
                    detailModal.classList.remove('active');
                }
            });

            // ============= ISHGA TUSHIRISH =============
            renderApps();
            setLanguage('uz');

            tg.MainButton.setText(translations.uz.close);
            tg.MainButton.onClick(() => tg.close());
        })();
    </script>
</body>
</html>****
