<!DOCTYPE html>
<html>
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>WebAppMarket</title>
    
    <!-- Telegram Web App SDK - Telegram bilan ishlash uchun -->
    <script src="https://telegram.org/js/telegram-web-app.js"></script>
    
    <!-- Font Awesome Icons - chiroyli ikonkalar uchun -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    
    <!-- GitHub Pages uchun base URL - sayt manzili -->
    <base href="/">
    
    <style>
        /* ============= GLOBAL STILLAR ============= */
        /* Barcha elementlar uchun asosiy stillar */
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: -apple-system, BlinkMacSystemFont, 'Segoe UI', Roboto, Helvetica, Arial, sans-serif;
        }

        /* Ranglar sxemasi (light mode) - asosiy ranglar */
        :root {
            --bg-color: #f8f9fa;           /* Fon rangi */
            --card-bg: #ffffff;              /* Kartochka foni */
            --text-primary: #1a1a1a;         /* Asosiy matn rangi */
            --text-secondary: #666666;        /* Ikkilamchi matn rangi */
            --accent-color: #0088cc;          /* Telegram rangi - accent */
            --accent-hover: #0077b3;           /* Hover holatidagi rang */
            --border-color: #eaeaea;           /* Chegara rangi */
            --shadow: 0 2px 8px rgba(0, 0, 0, 0.05);  /* Soya */
            --weather-sun: #ffd700;            /* Quyosh rangi */
            --error-color: #dc3545;             /* Xatolik rangi */
            --success-color: #28a745;           /* Muvaffaqiyat rangi */
        }

        /* Dark mode ranglari - Telegram dark mode ga mos */
        body.dark {
            --bg-color: #1a1a1a;
            --card-bg: #2d2d2d;
            --text-primary: #ffffff;
            --text-secondary: #b0b0b0;
            --border-color: #404040;
            --shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
            --weather-sun: #ffaa00;
        }

        /* Body stillari */
        body {
            background-color: var(--bg-color);
            color: var(--text-primary);
            min-height: 100vh;
            transition: background-color 0.3s, color 0.3s;
            scroll-behavior: smooth;
            margin: 0;
            padding: 0;
            position: relative;
            top: -2px; /* DOCTYPE ni yashirish uchun body ni 2px tepaga */
        }

        /* Asosiy container - barcha elementlarni o'rab turadi */
        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 18px 16px 30px 16px;
            width: 100%;
            position: relative;
            z-index: 10;
            background-color: var(--bg-color);
            min-height: calc(100vh - 20px);
        }

        /* ============= HEADER ============= */
        .header {
            display: flex;
            align-items: center;
            justify-content: flex-end;
            margin-bottom: 24px;
            padding-bottom: 12px;
            border-bottom: 1px solid var(--border-color);
        }

        /* Dark mode tugmasi */
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
        }

        .theme-toggle:hover {
            transform: scale(1.05);
        }

        /* ============= STATISTIKA KARTOCHKALARI ============= */
        /* 3 ta kartochka: Jami applar, Vaqt, Ob-havo */
        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
            gap: 12px;
            margin-bottom: 24px;
        }

        /* Har bir kartochka uchun umumiy stillar */
        .stat-card {
            background: var(--card-bg);
            border-radius: 12px;
            padding: 16px 8px;
            text-align: center;
            box-shadow: var(--shadow);
            border: 1px solid var(--border-color);
            height: auto;
            min-height: 120px;
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
            line-height: 1.3;
        }

        .stat-card .stat-label {
            font-size: 12px;
            color: var(--text-secondary);
            margin-top: 4px;
        }

        /* Vaqt uchun maxsus stillar */
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

        /* ============= OB-HAVO KARTOCHKASI ============= */
        .weather-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100%;
        }

        /* Quyosh animatsiyasi */
        .sun-animation {
            position: relative;
            width: 40px;
            height: 40px;
            margin: 0 auto 5px;
        }

        /* Quyoshning o'zi */
        .sun {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 24px;
            height: 24px;
            background: var(--weather-sun);
            border-radius: 50%;
            box-shadow: 0 0 20px var(--weather-sun);
            animation: pulse 2s infinite ease-in-out;
        }

        /* Quyosh nurlari */
        .sun-ray {
            position: absolute;
            top: 50%;
            left: 50%;
            width: 36px;
            height: 36px;
            border-radius: 50%;
            border: 2px solid var(--weather-sun);
            opacity: 0.6;
            animation: rotate 4s infinite linear;
        }

        .sun-ray:nth-child(2) {
            width: 48px;
            height: 48px;
            animation: rotate 6s infinite linear reverse;
            opacity: 0.3;
        }

        /* Quyosh animatsiyasi - pulsatsiya */
        @keyframes pulse {
            0%, 100% { transform: translate(-50%, -50%) scale(1); box-shadow: 0 0 20px var(--weather-sun); }
            50% { transform: translate(-50%, -50%) scale(1.1); box-shadow: 0 0 30px var(--weather-sun); }
        }

        /* Quyosh nurlari aylanishi */
        @keyframes rotate {
            from { transform: translate(-50%, -50%) rotate(0deg); }
            to { transform: translate(-50%, -50%) rotate(360deg); }
        }

        /* Ob-havo ma'lumotlari */
        .weather-temp {
            font-size: 16px;
            font-weight: 700;
            color: var(--text-primary);
        }

        /* Viloyat nomi - yashirin! Faqat ob-havo ko'rinadi */
        .weather-city {
            display: none; /* Viloyat nomini yashiramiz */
        }

        .weather-desc {
            font-size: 11px;
            color: var(--text-secondary);
            text-transform: capitalize;
            margin-top: 2px;
        }

        .weather-error {
            font-size: 11px;
            color: var(--error-color);
            margin-top: 4px;
        }

        /* Joylashuv tugmasi */
        .location-btn {
            background: var(--accent-color);
            color: white;
            border: none;
            border-radius: 20px;
            padding: 6px 12px;
            font-size: 11px;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 4px;
            margin-top: 6px;
            transition: all 0.3s;
            width: fit-content;
            margin-left: auto;
            margin-right: auto;
        }

        .location-btn:hover {
            background: var(--accent-hover);
            transform: scale(1.05);
        }

        .location-btn i {
            font-size: 12px;
            color: white;
        }

        /* ============= QIDIRUV ============= */
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

        /* ============= KATEGORIYALAR ============= */
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
        }

        .category-btn.active {
            background: var(--accent-color);
            color: white;
            border-color: var(--accent-color);
        }

        /* ============= APPLAR ============= */
        .section-title {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-bottom: 16px;
        }

        .section-title h2 {
            font-size: 18px;
            font-weight: 600;
        }

        /* Applar grid - 2 ustun */
        .apps-grid {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 16px;
            margin-bottom: 30px;
        }

        /* App kartochkasi */
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

        /* App rasmi */
        .app-image {
            width: 100%;
            height: 120px;
            background: linear-gradient(135deg, var(--accent-color), #005580);
            display: flex;
            align-items: center;
            justify-content: center;
            position: relative;
            overflow: hidden;
        }

        .app-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            transition: transform 0.3s;
        }

        .app-card:hover .app-image img {
            transform: scale(1.05);
        }

        /* Rasm yuklanmaganda ko'rsatiladigan ikonka */
        .app-image .icon-overlay {
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            display: flex;
            align-items: center;
            justify-content: center;
            background: linear-gradient(135deg, var(--accent-color), #005580);
            color: white;
            font-size: 48px;
            transition: opacity 0.3s;
            pointer-events: none;
        }

        /* App kategoriya belgisi */
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

        /* App ma'lumotlari */
        .app-info {
            padding: 12px;
        }

        .app-info h3 {
            font-size: 16px;
            font-weight: 600;
            margin-bottom: 4px;
            color: var(--text-primary);
        }

        .app-description {
            font-size: 12px;
            color: var(--text-secondary);
            margin-bottom: 8px;
            display: -webkit-box;
            -webkit-line-clamp: 2;
            -webkit-box-orient: vertical;
            overflow: hidden;
            line-height: 1.4;
        }

        /* App sharhi */
        .app-review {
            font-size: 11px;
            color: var(--text-secondary);
            padding: 4px 8px;
            background: var(--bg-color);
            border-radius: 12px;
            display: inline-block;
            margin-top: 4px;
            font-style: italic;
        }

        /* App meta ma'lumotlari */
        .app-meta {
            display: flex;
            align-items: center;
            justify-content: space-between;
            margin-top: 8px;
            font-size: 11px;
        }

        .app-meta i {
            color: var(--accent-color);
            font-size: 10px;
        }

        /* Sevimlilar tugmasi */
        .favorite-btn {
            cursor: pointer;
            transition: transform 0.2s;
        }

        .favorite-btn:hover {
            transform: scale(1.1);
        }

        /* ============= YUQORIGA CHIQISH TUGMASI ============= */
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

        /* ============= MODAL OYNA ============= */
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

        /* App detal ma'lumotlari */
        .app-detail {
            text-align: center;
        }

        .app-detail-image {
            width: 120px;
            height: 120px;
            border-radius: 30px;
            margin: 0 auto 20px;
            overflow: hidden;
            position: relative;
        }

        .app-detail-image img {
            width: 100%;
            height: 100%;
            object-fit: cover;
        }

        .app-detail-image .default-icon {
            width: 100%;
            height: 100%;
            background: linear-gradient(135deg, var(--accent-color), #005580);
            display: flex;
            align-items: center;
            justify-content: center;
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

        /* Tugmalar */
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
            margin-bottom: 10px;
        }

        .open-app-btn {
            background: var(--accent-color);
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
        }

        /* Yuklanayotgan animatsiya */
        .loading {
            display: inline-block;
            width: 16px;
            height: 16px;
            border: 2px solid rgba(255,255,255,0.3);
            border-radius: 50%;
            border-top-color: white;
            animation: spin 1s ease-in-out infinite;
        }

        @keyframes spin {
            to { transform: rotate(360deg); }
        }

        /* Telefonlar uchun moslashuv */
        @media (max-width: 600px) {
            .container {
                padding-top: 20px;
            }
            
            body {
                top: -1px;
            }
        }
    </style>
</head>
<body>
    <!-- Asosiy container - barcha elementlar shu yerda -->
    <div class="container">
        <!-- Header - dark mode tugmasi bilan -->
        <div class="header">
            <div class="theme-toggle" id="themeToggle">
                <i class="fas fa-moon"></i>
            </div>
        </div>

        <!-- Statistika kartochkalari (3 ta) -->
        <div class="stats-grid">
            <!-- 1-kartochka: Jami applar soni -->
            <div class="stat-card">
                <i class="fas fa-rocket"></i>
                <div class="stat-value" id="totalApps">0</div>
                <div class="stat-label">Jami applar</div>
            </div>
            
            <!-- 2-kartochka: O'zbekiston vaqti -->
            <div class="stat-card">
                <i class="fas fa-clock"></i>
                <div class="time-value" id="currentTime">00:00</div>
                <div class="date-value" id="currentDate">01.01.2026</div>
                <div class="stat-label">Toshkent vaqti</div>
            </div>
            
            <!-- 3-kartochka: Ob-havo (viloyat nomi ko'rinmaydi) -->
            <div class="stat-card">
                <div class="weather-container">
                    <!-- Quyosh animatsiyasi -->
                    <div class="sun-animation">
                        <div class="sun"></div>
                        <div class="sun-ray"></div>
                        <div class="sun-ray"></div>
                    </div>
                    <!-- Harorat -->
                    <div class="weather-temp" id="weatherTemp">--°C</div>
                    <!-- Viloyat nomi (yashirin) -->
                    <div class="weather-city" id="weatherCity">--</div>
                    <!-- Ob-havo tavsifi -->
                    <div class="weather-desc" id="weatherDesc"></div>
                    <!-- Xatolik xabari -->
                    <div class="weather-error" id="weatherError" style="display: none;"></div>
                    <!-- Joylashuv tugmasi -->
                    <button class="location-btn" id="requestLocationBtn">
                        <i class="fas fa-map-marker-alt"></i>
                        <span id="locationBtnText">Joylashuvga ruxsat berish</span>
                    </button>
                </div>
            </div>
        </div>

        <!-- Qidiruv bo'limi -->
        <div class="search-section">
            <div class="search-box">
                <i class="fas fa-search"></i>
                <input type="text" id="searchInput" placeholder="Applar qidirish...">
            </div>
        </div>

        <!-- Kategoriya tugmalari -->
        <div class="categories" id="categories">
            <button class="category-btn active" data-category="all">Barchasi</button>
            <button class="category-btn" data-category="game">O'yinlar</button>
            <button class="category-btn" data-category="tool">Asboblar</button>
            <button class="category-btn" data-category="social">Ijtimoiy</button>
            <button class="category-btn" data-category="education">Ta'lim</button>
        </div>

        <!-- Barcha applar bo'limi -->
        <div class="section-title">
            <h2>Barcha applar</h2>
        </div>

        <!-- Applar grid - bu yerga JavaScript orqali applar chiqariladi -->
        <div class="apps-grid" id="appsGrid"></div>
    </div>

    <!-- Yuqoriga chiqish tugmasi (pastki o'ng burchak) -->
    <div class="scroll-top-btn" id="scrollTopBtn">
        <i class="fas fa-arrow-up"></i>
    </div>

    <!-- App detal ma'lumotlari modal oynasi -->
    <div class="modal" id="detailModal">
        <div class="modal-content app-detail" id="appDetailContent"></div>
    </div>

    <!-- ============= JAVASCRIPT KODLARI ============= -->
    <script>
        (function() {
            // ============= TELEGRAM WEB APP SOZLAMALARI =============
            // Telegram Web App obyektini olish
            const tg = window.Telegram.WebApp;
            // Web appni to'liq ekran qilish
            tg.expand();
            // Web app tayyorligini bildirish
            tg.ready();

            // Telegram dark mode ni aniqlash
            if (tg.colorScheme === 'dark') {
                document.body.classList.add('dark');
            }

            // ============= O'ZBEKISTON VILOYATLARI MA'LUMOTLARI =============
            // 12 ta viloyat + Toshkent shahar
            // Har bir viloyat uchun: markazi koordinatalari, harorat, ob-havo
            const UZBEKISTAN_REGIONS = [
                { name: 'Toshkent',      lat: 41.3,  lon: 69.2,  temp: 23, desc: 'Quyoshli' },
                { name: 'Samarqand',     lat: 39.7,  lon: 66.9,  temp: 22, desc: 'Och havo' },
                { name: 'Buxoro',        lat: 39.8,  lon: 64.4,  temp: 26, desc: 'Quruq issiq' },
                { name: 'Xorazm',        lat: 41.4,  lon: 60.4,  temp: 27, desc: 'Issiq' },
                { name: 'Navoiy',        lat: 40.1,  lon: 65.4,  temp: 25, desc: 'Quruq' },
                { name: 'Qashqadaryo',    lat: 38.9,  lon: 65.8,  temp: 26, desc: 'Issiq' },
                { name: 'Surxondaryo',    lat: 37.2,  lon: 67.3,  temp: 28, desc: 'Juda issiq' },
                { name: 'Jizzax',        lat: 40.1,  lon: 67.8,  temp: 24, desc: 'Quyoshli' },
                { name: 'Sirdaryo',      lat: 40.8,  lon: 68.7,  temp: 24, desc: 'Och havo' },
                { name: 'Farg\'ona',      lat: 40.4,  lon: 71.8,  temp: 25, desc: 'Issiq' },
                { name: 'Namangan',      lat: 41.0,  lon: 71.7,  temp: 24, desc: 'Quyoshli' },
                { name: 'Andijon',       lat: 40.8,  lon: 72.3,  temp: 26, desc: 'Issiq' },
                { name: 'Qoraqalpog\'iston', lat: 42.5, lon: 59.6, temp: 26, desc: 'Quruq issiq' }
            ];

            // ============= APPLAR RO'YXATI =============
            // Yangi app qo'shish uchun shu yerga qo'shing
            const apps = [
                {
                    id: 1,
                    name: 'Mini Game Bot',
                    category: 'game',
                    description: 'Qiziqarli mini o\'yinlar to\'plami. Puzzle, arcade va strategiya o\'yinlari mavjud.',
                    review: '🎮 Ajoyib o\'yinlar! Eng zo\'r pazzillar va arkada o\'yinlari to\'plami. Har kuni yangilanadi.',
                    botUsername: '@mygamebot',
                    url: 'https://t.me/mygamebot/game',
                    icon: 'fa-gamepad'
                },
                {
                    id: 2,
                    name: 'Task Manager',
                    category: 'tool',
                    description: 'Kundalik vazifalarni boshqarish, eslatmalar va rejalashtirish.',
                    review: '📋 Kunlik ishlarni rejalashtirish uchun eng yaxshi bot! Eslatmalar juda qulay.',
                    botUsername: '@taskbot',
                    url: 'https://t.me/taskbot/app',
                    icon: 'fa-tasks'
                },
                {
                    id: 3,
                    name: 'Chat App',
                    category: 'social',
                    description: 'Tezkor messenger, guruhli chatlar va fayl almashish.',
                    review: '💬 Do\'stlar bilan muloqot qilish uchun ajoyib platforma. Tez va qulay.',
                    botUsername: '@chatappbot',
                    url: 'https://t.me/chatappbot/start',
                    icon: 'fa-comments'
                },
                {
                    id: 4,
                    name: 'TestGO',
                    category: 'education',
                    description: 'Matematik testlar, misollar va mantiqiy masalalar.',
                    review: '🧮 Matematikani o\'rganish uchun zo\'r bot! Bolalar uchun juda foydali.',
                    botUsername: '@Quiz00_01_bot',
                    url: 'https://shodiyor986.github.io/TestGo/',
                    icon: 'fa-calculator'
                }
            ];

            // Sevimlilar ro'yxati (localStorage dan o'qiladi)
            let favorites = JSON.parse(localStorage.getItem('favorites')) || [];
            
            // Joriy kategoriya (barchasi)
            let currentCategory = 'all';

            // ============= DOM ELEMENTLARNI OLISH =============
            const appsGrid = document.getElementById('appsGrid');
            const searchInput = document.getElementById('searchInput');
            const totalAppsEl = document.getElementById('totalApps');
            const currentTimeEl = document.getElementById('currentTime');
            const currentDateEl = document.getElementById('currentDate');
            const themeToggle = document.getElementById('themeToggle');
            const scrollTopBtn = document.getElementById('scrollTopBtn');
            const detailModal = document.getElementById('detailModal');
            const categoryBtns = document.querySelectorAll('.category-btn');
            
            // Ob-havo elementlari
            const weatherTemp = document.getElementById('weatherTemp');
            const weatherCity = document.getElementById('weatherCity');
            const weatherDesc = document.getElementById('weatherDesc');
            const weatherError = document.getElementById('weatherError');
            const requestLocationBtn = document.getElementById('requestLocationBtn');
            const locationBtnText = document.getElementById('locationBtnText');

            // ============= OB-HAVO FUNKSIYALARI =============
            
            /**
             * Joylashuv bo'yicha eng yaqin viloyatni topish
             * @param {number} lat - Kenglik
             * @param {number} lon - Uzunlik
             * @returns {object} - Eng yaqin viloyat ma'lumotlari
             */
            function findNearestRegion(lat, lon) {
                let nearestRegion = UZBEKISTAN_REGIONS[0];
                let minDistance = Infinity;
                
                // Barcha viloyatlarni tekshirish
                UZBEKISTAN_REGIONS.forEach(region => {
                    // Masofani hisoblash (taxminiy)
                    const distance = Math.sqrt(
                        Math.pow(lat - region.lat, 2) + 
                        Math.pow(lon - region.lon, 2)
                    );
                    
                    // Eng yaqin viloyatni topish
                    if (distance < minDistance) {
                        minDistance = distance;
                        nearestRegion = region;
                    }
                });
                
                return nearestRegion;
            }

            /**
             * Yuklanayotgan holatni ko'rsatish
             */
            function showWeatherLoading() {
                weatherTemp.textContent = '--°C';
                weatherDesc.textContent = 'Aniqlanmoqda...';
                weatherError.style.display = 'none';
                locationBtnText.innerHTML = '<span class="loading"></span> Yuklanmoqda...';
            }

            /**
             * Yuklash tugaganini ko'rsatish
             */
            function hideWeatherLoading() {
                locationBtnText.textContent = 'Joylashuvga ruxsat berish';
            }

            /**
             * Xatolik xabarini ko'rsatish
             * @param {string} message - Xatolik matni
             */
            function showWeatherError(message) {
                weatherError.textContent = message;
                weatherError.style.display = 'block';
                weatherTemp.textContent = '--°C';
                weatherDesc.textContent = '';
                hideWeatherLoading();
            }

            /**
             * Koordinatalar bo'yicha ob-havoni olish
             * @param {number} lat - Kenglik
             * @param {number} lon - Uzunlik
             */
            function getWeatherByCoords(lat, lon) {
                showWeatherLoading();
                
                // Joylashuv bo'yicha eng yaqin viloyatni topish
                const region = findNearestRegion(lat, lon);
                
                // Viloyat nomini yashiramiz (ko'rsatilmaydi)
                weatherCity.textContent = region.name; // Bu yashirin
                
                // Bir oz kutish (real API ga o'xshab)
                setTimeout(() => {
                    // Viloyatga mos ob-havo ma'lumotlarini ko'rsatish
                    weatherTemp.textContent = region.temp + '°C';
                    weatherDesc.textContent = region.desc;
                    weatherError.style.display = 'none';
                    
                    // Tugma matnini o'zgartirish
                    locationBtnText.textContent = '✅ Joylashuv aniqlandi';
                    
                    // 2 soniyadan keyin tugma matnini qaytarish
                    setTimeout(() => {
                        locationBtnText.textContent = 'Joylashuvni yangilash';
                    }, 2000);
                    
                    // Telegram haptic feedback
                    tg.HapticFeedback.notificationOccurred('success');
                }, 1000);
            }

            /**
             * Joylashuvga ruxsat so'rash
             */
            function requestLocation() {
                showWeatherLoading();
                
                // Geolokatsiya mavjudligini tekshirish
                if (!navigator.geolocation) {
                    showWeatherError('Brauzer joylashuvni qo\'llab-quvvatlamaydi');
                    return;
                }

                // Joylashuv so'rash
                navigator.geolocation.getCurrentPosition(
                    // Muvaffaqiyatli bo'lsa
                    function(position) {
                        const lat = position.coords.latitude;
                        const lon = position.coords.longitude;
                        
                        // Ob-havoni olish
                        getWeatherByCoords(lat, lon);
                        
                        // Telegram haptic feedback
                        tg.HapticFeedback.impactOccurred('medium');
                    },
                    // Xatolik bo'lsa
                    function(error) {
                        let errorMessage = '';
                        
                        // Xatolik turlari
                        switch(error.code) {
                            case error.PERMISSION_DENIED:
                                errorMessage = '❌ Joylashuvga ruxsat berilmadi';
                                locationBtnText.innerHTML = '<i class="fas fa-map-marker-alt"></i> Ruxsat berish';
                                break;
                            case error.POSITION_UNAVAILABLE:
                                errorMessage = '📡 Joylashuv mavjud emas';
                                break;
                            case error.TIMEOUT:
                                errorMessage = '⏱ Vaqt tugadi';
                                break;
                            default:
                                errorMessage = '❌ Noma\'lum xatolik';
                        }
                        
                        showWeatherError(errorMessage);
                        tg.HapticFeedback.notificationOccurred('error');
                    },
                    // Sozlamalar
                    {
                        enableHighAccuracy: true,    // Yuqori aniqlik
                        timeout: 10000,               // 10 soniya kutish
                        maximumAge: 0                  // Har doim yangi ma'lumot
                    }
                );
            }

            // ============= VAQT FUNKSIYASI =============
            /**
             * O'zbekiston vaqtini yangilash
             */
            function updateUzbekTime() {
                const now = new Date();
                
                // O'zbekiston vaqti (UTC+5)
                const uzbekTime = new Date(now.getTime() + (5 * 60 * 60 * 1000));
                
                // Soat: daqiqa (00:00 format)
                const hours = uzbekTime.getUTCHours().toString().padStart(2, '0');
                const minutes = uzbekTime.getUTCMinutes().toString().padStart(2, '0');
                currentTimeEl.textContent = `${hours}:${minutes}`;
                
                // Sana: kun.oy.yil (01.01.2026 format)
                const day = uzbekTime.getUTCDate().toString().padStart(2, '0');
                const month = (uzbekTime.getUTCMonth() + 1).toString().padStart(2, '0');
                const year = uzbekTime.getUTCFullYear();
                currentDateEl.textContent = `${day}.${month}.${year}`;
            }

            // Vaqtni birinchi marta yangilash
            updateUzbekTime();
            
            // Har soniyada vaqtni yangilash
            setInterval(updateUzbekTime, 1000);

            // ============= BOT RASMINI YUKLASH =============
            /**
             * Bot username asosida rasm yuklash
             * @param {HTMLElement} imgElement - Rasm elementi
             * @param {string} botUsername - Bot username (masalan: @mybot)
             */
            function loadBotImage(imgElement, botUsername) {
                // @ belgisini olib tashlash
                const cleanUsername = botUsername.replace('@', '');
                
                // UI Avatars orqali rasm yaratish
                const imageUrl = `https://ui-avatars.com/api/?name=${cleanUsername}&background=0088cc&color=fff&size=128&bold=true&format=png`;
                
                // Rasm manzilini o'rnatish
                imgElement.src = imageUrl;
                
                // Rasm yuklanganda
                imgElement.onload = function() {
                    imgElement.classList.remove('error');
                    const overlay = imgElement.nextElementSibling;
                    if (overlay) overlay.style.display = 'none';
                };
                
                // Rasm yuklanmasa
                imgElement.onerror = function() {
                    imgElement.classList.add('error');
                    const overlay = imgElement.nextElementSibling;
                    if (overlay) overlay.style.display = 'flex';
                };
            }

            // ============= APPLARNI KO'RSATISH =============
            /**
             * Applarni filterlab ko'rsatish
             */
            function renderApps() {
                // Qidiruv so'zini olish
                const searchTerm = searchInput.value.toLowerCase();
                let filteredApps = apps;

                // Kategoriya bo'yicha filterlash
                if (currentCategory !== 'all') {
                    filteredApps = filteredApps.filter(app => app.category === currentCategory);
                }

                // Qidiruv bo'yicha filterlash
                if (searchTerm) {
                    filteredApps = filteredApps.filter(app => 
                        app.name.toLowerCase().includes(searchTerm) || 
                        app.description.toLowerCase().includes(searchTerm) ||
                        (app.review && app.review.toLowerCase().includes(searchTerm))
                    );
                }

                // Agar app topilmasa
                if (filteredApps.length === 0) {
                    appsGrid.innerHTML = `
                        <div style="grid-column: 1/-1; text-align: center; padding: 40px; color: var(--text-secondary);">
                            <i class="fas fa-box-open" style="font-size: 48px; margin-bottom: 16px;"></i>
                            <p>Hech qanday app topilmadi</p>
                        </div>
                    `;
                    totalAppsEl.textContent = apps.length;
                    return;
                }

                // Applarni HTML ko'rinishida chiqarish
                appsGrid.innerHTML = filteredApps.map(app => {
                    const isFavorite = favorites.includes(app.id);
                    const categoryNames = {
                        game: 'O\'yin',
                        tool: 'Asbob',
                        social: 'Ijtimoiy',
                        education: 'Ta\'lim'
                    };
                    
                    const imageId = `app-image-${app.id}`;

                    return `
                        <div class="app-card" onclick="window.showAppDetail(${app.id})">
                            <div class="app-image">
                                <img id="${imageId}" src="" alt="${app.name}" style="display: block;">
                                <div class="icon-overlay" style="display: none;">
                                    <i class="fas ${app.icon}"></i>
                                </div>
                                <span class="app-badge">${categoryNames[app.category]}</span>
                            </div>
                            <div class="app-info">
                                <h3>${app.name}</h3>
                                <div class="app-description">${app.description.substring(0, 60)}...</div>
                                <div class="app-review">${app.review || 'Sharh mavjud emas'}</div>
                                <div class="app-meta">
                                    <span><i class="fas fa-comment"></i> ${app.review ? 'Sharh bor' : 'Sharh yo\'q'}</span>
                                    <span class="favorite-btn" onclick="event.stopPropagation(); window.toggleFavorite(${app.id})">
                                        <i class="fas ${isFavorite ? 'fa-heart' : 'fa-heart-o'}" 
                                           style="color: ${isFavorite ? '#ff4444' : 'var(--accent-color)'};"></i>
                                    </span>
                                </div>
                            </div>
                        </div>
                    `;
                }).join('');

                // Har bir app uchun rasm yuklash
                filteredApps.forEach(app => {
                    const imgElement = document.getElementById(`app-image-${app.id}`);
                    if (imgElement) {
                        loadBotImage(imgElement, app.botUsername);
                    }
                });

                // Jami applarni yangilash
                totalAppsEl.textContent = apps.length;
            }

            // ============= GLOBAL FUNKSIYALAR =============
            // Bu funksiyalar window orqali chaqiriladi (onclick attributlardan)
            
            /**
             * Sevimlilarga qo'shish yoki olib tashlash
             * @param {number} appId - App ID si
             */
            window.toggleFavorite = function(appId) {
                const index = favorites.indexOf(appId);
                if (index === -1) {
                    favorites.push(appId); // Sevimlilarga qo'shish
                } else {
                    favorites.splice(index, 1); // Sevimlilardan olib tashlash
                }
                localStorage.setItem('favorites', JSON.stringify(favorites));
                renderApps(); // Applarni qayta ko'rsatish
                tg.HapticFeedback.impactOccurred('light');
            };

            /**
             * Botni ochish
             * @param {string} botUsername - Bot username
             */
            window.openBot = function(botUsername) {
                let username = botUsername;
                if (!username.startsWith('@')) {
                    username = '@' + username;
                }
                tg.openTelegramLink(`https://t.me/${username.replace('@', '')}`);
                tg.HapticFeedback.impactOccurred('medium');
            };

            /**
             * Web appni ochish
             * @param {string} url - Web app manzili
             */
            window.openWebApp = function(url) {
                tg.openTelegramLink(url);
                tg.HapticFeedback.impactOccurred('medium');
            };

            /**
             * App detal ma'lumotlarini ko'rsatish
             * @param {number} appId - App ID si
             */
            window.showAppDetail = function(appId) {
                const app = apps.find(a => a.id === appId);
                if (!app) return;

                const categoryNames = {
                    game: 'O\'yin',
                    tool: 'Asbob',
                    social: 'Ijtimoiy',
                    education: 'Ta\'lim'
                };

                const detailImageId = `detail-image-${app.id}`;

                // Detal HTML ni yaratish
                document.getElementById('appDetailContent').innerHTML = `
                    <div class="app-detail-image">
                        <img id="${detailImageId}" src="" alt="${app.name}" style="width: 100%; height: 100%; object-fit: cover; display: none;">
                        <div class="default-icon" id="detail-icon-${app.id}" style="display: flex;">
                            <i class="fas ${app.icon}"></i>
                        </div>
                    </div>
                    <h2>${app.name}</h2>
                    <div class="app-detail-category">${categoryNames[app.category]}</div>
                    
                    <div class="app-detail-review">
                        <i class="fas fa-quote-left"></i> ${app.review || 'Sharh mavjud emas'}
                    </div>
                    
                    <p class="app-detail-description">
                        <strong>Bot haqida:</strong><br>
                        ${app.description}
                    </p>

                    <button class="open-bot-btn" onclick="window.openBot('${app.botUsername}')">
                        <i class="fab fa-telegram"></i> Botga o'tish: ${app.botUsername}
                    </button>

                    <button class="open-app-btn" onclick="window.openWebApp('${app.url}')">
                        <i class="fas fa-external-link-alt"></i> Web Appni ochish
                    </button>
                `;

                // Detal rasmni yuklash
                const detailImg = document.getElementById(detailImageId);
                const detailIcon = document.getElementById(`detail-icon-${app.id}`);
                
                if (detailImg) {
                    const cleanUsername = app.botUsername.replace('@', '');
                    const imageUrl = `https://ui-avatars.com/api/?name=${cleanUsername}&background=0088cc&color=fff&size=128&bold=true&format=png`;
                    
                    detailImg.src = imageUrl;
                    detailImg.onload = function() {
                        detailImg.style.display = 'block';
                        if (detailIcon) detailIcon.style.display = 'none';
                    };
                    detailImg.onerror = function() {
                        detailImg.style.display = 'none';
                        if (detailIcon) detailIcon.style.display = 'flex';
                    };
                }

                // Modalni ko'rsatish
                detailModal.classList.add('active');
            };

            // ============= EVENT LISTENERLAR =============
            
            // Joylashuv tugmasi bosilganda
            requestLocationBtn.addEventListener('click', requestLocation);

            // Dark mode tugmasi bosilganda
            themeToggle.addEventListener('click', () => {
                document.body.classList.toggle('dark');
                const icon = themeToggle.querySelector('i');
                if (document.body.classList.contains('dark')) {
                    icon.className = 'fas fa-sun';
                } else {
                    icon.className = 'fas fa-moon';
                }
                tg.HapticFeedback.impactOccurred('light');
            });

            // Yuqoriga chiqish tugmasi bosilganda
            scrollTopBtn.addEventListener('click', () => {
                window.scrollTo({
                    top: 0,
                    behavior: 'smooth'
                });
                tg.HapticFeedback.impactOccurred('light');
            });

            // Kategoriya tugmalari bosilganda
            categoryBtns.forEach(btn => {
                btn.addEventListener('click', () => {
                    categoryBtns.forEach(b => b.classList.remove('active'));
                    btn.classList.add('active');
                    currentCategory = btn.dataset.category;
                    renderApps();
                    tg.HapticFeedback.impactOccurred('light');
                });
            });

            // Qidiruv o'zgarganda
            searchInput.addEventListener('input', () => {
                renderApps();
            });

            // Modalni tashqariga bosilganda yopish
            window.addEventListener('click', (e) => {
                if (e.target === detailModal) {
                    detailModal.classList.remove('active');
                }
            });

            // ============= ISHGA TUSHIRISH =============
            // Applarni ko'rsatish
            renderApps();

            // Telegram MainButton sozlamalari
            tg.MainButton.setText('Yopish');
            tg.MainButton.onClick(() => {
                tg.close();
            });

            // 3 soniyadan keyin avtomatik joylashuv so'rash
            setTimeout(() => {
                requestLocation();
            }, 3000);
        })();
    </script>
</body>
</html>
