# WebAppMarket
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

        :root {
            --bg-color: #f8f9fa;
            --card-bg: #ffffff;
            --text-primary: #1a1a1a;
            --text-secondary: #666666;
            --accent-color: #0088cc;
            --accent-hover: #0077b3;
            --border-color: #eaeaea;
            --shadow: 0 2px 8px rgba(0, 0, 0, 0.05);
            --weather-sun: #ffd700;
            --error-color: #dc3545;
            --success-color: #28a745;
        }

        body.dark {
            --bg-color: #1a1a1a;
            --card-bg: #2d2d2d;
            --text-primary: #ffffff;
            --text-secondary: #b0b0b0;
            --border-color: #404040;
            --shadow: 0 2px 8px rgba(0, 0, 0, 0.2);
            --weather-sun: #ffaa00;
        }

        body {
            background-color: var(--bg-color);
            color: var(--text-primary);
            min-height: 100vh;
            transition: background-color 0.3s, color 0.3s;
            scroll-behavior: smooth;
        }

        .container {
            max-width: 600px;
            margin: 0 auto;
            padding: 16px;
            padding-bottom: 30px;
            width: 100%;
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
        }

        .theme-toggle:hover {
            transform: scale(1.05);
        }

        .stats-grid {
            display: grid;
            grid-template-columns: repeat(3, 1fr);
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

        .weather-container {
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            width: 100%;
        }

        .sun-animation {
            position: relative;
            width: 40px;
            height: 40px;
            margin: 0 auto 5px;
        }

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

        @keyframes pulse {
            0%, 100% { transform: translate(-50%, -50%) scale(1); box-shadow: 0 0 20px var(--weather-sun); }
            50% { transform: translate(-50%, -50%) scale(1.1); box-shadow: 0 0 30px var(--weather-sun); }
        }

        @keyframes rotate {
            from { transform: translate(-50%, -50%) rotate(0deg); }
            to { transform: translate(-50%, -50%) rotate(360deg); }
        }

        .weather-temp {
            font-size: 16px;
            font-weight: 700;
            color: var(--text-primary);
        }

        .weather-city {
            font-size: 12px;
            color: var(--text-secondary);
            white-space: nowrap;
            overflow: hidden;
            text-overflow: ellipsis;
            max-width: 100px;
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
        }

        .category-btn.active {
            background: var(--accent-color);
            color: white;
            border-color: var(--accent-color);
        }

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
            z-index: 1000;
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
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <div class="theme-toggle" id="themeToggle">
                <i class="fas fa-moon"></i>
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
            
            <div class="stat-card">
                <div class="weather-container">
                    <div class="sun-animation">
                        <div class="sun"></div>
                        <div class="sun-ray"></div>
                        <div class="sun-ray"></div>
                    </div>
                    <div class="weather-temp" id="weatherTemp">--°C</div>
                    <div class="weather-city" id="weatherCity">--</div>
                    <div class="weather-desc" id="weatherDesc"></div>
                    <div class="weather-error" id="weatherError" style="display: none;"></div>
                    <button class="location-btn" id="requestLocationBtn">
                        <i class="fas fa-map-marker-alt"></i>
                        <span id="locationBtnText">Joylashuvga ruxsat berish</span>
                    </button>
                </div>
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
            // Telegram Web App init
            const tg = window.Telegram.WebApp;
            tg.expand();
            tg.ready();

            // Dark mode
            if (tg.colorScheme === 'dark') {
                document.body.classList.add('dark');
            }

            // Apps data
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
                    name: 'Math Quiz',
                    category: 'education',
                    description: 'Matematik testlar, misollar va mantiqiy masalalar.',
                    review: '🧮 Matematikani o\'rganish uchun zo\'r bot! Bolalar uchun juda foydali.',
                    botUsername: '@mathquizbot',
                    url: 'https://t.me/mathquizbot/quiz',
                    icon: 'fa-calculator'
                }
            ];

            let favorites = JSON.parse(localStorage.getItem('favorites')) || [];
            let currentCategory = 'all';

            const appsGrid = document.getElementById('appsGrid');
            const searchInput = document.getElementById('searchInput');
            const totalAppsEl = document.getElementById('totalApps');
            const currentTimeEl = document.getElementById('currentTime');
            const currentDateEl = document.getElementById('currentDate');
            const themeToggle = document.getElementById('themeToggle');
            const scrollTopBtn = document.getElementById('scrollTopBtn');
            const detailModal = document.getElementById('detailModal');
            const categoryBtns = document.querySelectorAll('.category-btn');
            const weatherTemp = document.getElementById('weatherTemp');
            const weatherCity = document.getElementById('weatherCity');
            const weatherDesc = document.getElementById('weatherDesc');
            const weatherError = document.getElementById('weatherError');
            const requestLocationBtn = document.getElementById('requestLocationBtn');
            const locationBtnText = document.getElementById('locationBtnText');

            const WEATHER_API_KEY = 'demo_key'; // Test uchun

            function showWeatherLoading() {
                weatherTemp.textContent = '--°C';
                weatherCity.textContent = 'Aniqlanmoqda...';
                weatherDesc.textContent = '';
                weatherError.style.display = 'none';
                locationBtnText.innerHTML = '<span class="loading"></span> Yuklanmoqda...';
            }

            function hideWeatherLoading() {
                locationBtnText.textContent = 'Joylashuvga ruxsat berish';
            }

            function showWeatherError(message) {
                weatherError.textContent = message;
                weatherError.style.display = 'block';
                weatherTemp.textContent = '--°C';
                weatherCity.textContent = '--';
                weatherDesc.textContent = '';
                hideWeatherLoading();
            }

            function getWeatherByCoords(lat, lon) {
                showWeatherLoading();
                
                setTimeout(() => {
                    const cities = {
                        '41.3': { name: 'Toshkent', temp: 23, desc: 'Quyoshli' },
                        '39.7': { name: 'Samarqand', temp: 22, desc: 'Och havo' },
                        '41.7': { name: 'Farg\'ona', temp: 24, desc: 'Quyoshli' },
                        '40.8': { name: 'Andijon', temp: 25, desc: 'Issiq' },
                        '40.4': { name: 'Buxoro', temp: 26, desc: 'Quruq' },
                        '41.6': { name: 'Namangan', temp: 23, desc: 'Och havo' },
                        '40.1': { name: 'Qarshi', temp: 27, desc: 'Issiq' },
                        '41.4': { name: 'Jizzax', temp: 24, desc: 'Quyoshli' }
                    };
                    
                    let cityName = 'Toshkent';
                    let temp = 23;
                    let desc = 'Quyoshli';
                    
                    for (let key in cities) {
                        if (Math.abs(lat - parseFloat(key)) < 0.5) {
                            cityName = cities[key].name;
                            temp = cities[key].temp;
                            desc = cities[key].desc;
                            break;
                        }
                    }
                    
                    weatherTemp.textContent = temp + '°C';
                    weatherCity.textContent = cityName;
                    weatherDesc.textContent = desc;
                    weatherError.style.display = 'none';
                    locationBtnText.textContent = '✅ Joylashuv aniqlandi';
                    
                    setTimeout(() => {
                        locationBtnText.textContent = 'Joylashuvni yangilash';
                    }, 2000);
                    
                    tg.HapticFeedback.notificationOccurred('success');
                }, 1500);
            }

            function requestLocation() {
                showWeatherLoading();
                
                if (!navigator.geolocation) {
                    showWeatherError('Brauzer joylashuvni qo\'llab-quvvatlamaydi');
                    return;
                }

                navigator.geolocation.getCurrentPosition(
                    function(position) {
                        const lat = position.coords.latitude;
                        const lon = position.coords.longitude;
                        getWeatherByCoords(lat, lon);
                        tg.HapticFeedback.impactOccurred('medium');
                    },
                    function(error) {
                        let errorMessage = '';
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
                    {
                        enableHighAccuracy: true,
                        timeout: 10000,
                        maximumAge: 0
                    }
                );
            }

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

            function loadBotImage(imgElement, botUsername) {
                const cleanUsername = botUsername.replace('@', '');
                const imageUrl = `https://ui-avatars.com/api/?name=${cleanUsername}&background=0088cc&color=fff&size=128&bold=true&format=png`;
                
                imgElement.src = imageUrl;
                
                imgElement.onload = function() {
                    imgElement.classList.remove('error');
                    const overlay = imgElement.nextElementSibling;
                    if (overlay) overlay.style.display = 'none';
                };
                
                imgElement.onerror = function() {
                    imgElement.classList.add('error');
                    const overlay = imgElement.nextElementSibling;
                    if (overlay) overlay.style.display = 'flex';
                };
            }

            function renderApps() {
                const searchTerm = searchInput.value.toLowerCase();
                let filteredApps = apps;

                if (currentCategory !== 'all') {
                    filteredApps = filteredApps.filter(app => app.category === currentCategory);
                }

                if (searchTerm) {
                    filteredApps = filteredApps.filter(app => 
                        app.name.toLowerCase().includes(searchTerm) || 
                        app.description.toLowerCase().includes(searchTerm) ||
                        (app.review && app.review.toLowerCase().includes(searchTerm))
                    );
                }

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

                filteredApps.forEach(app => {
                    const imgElement = document.getElementById(`app-image-${app.id}`);
                    if (imgElement) {
                        loadBotImage(imgElement, app.botUsername);
                    }
                });

                totalAppsEl.textContent = apps.length;
            }

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

            window.openWebApp = function(url) {
                tg.openTelegramLink(url);
                tg.HapticFeedback.impactOccurred('medium');
            };

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

                detailModal.classList.add('active');
            };

            requestLocationBtn.addEventListener('click', requestLocation);

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

            scrollTopBtn.addEventListener('click', () => {
                window.scrollTo({
                    top: 0,
                    behavior: 'smooth'
                });
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

            searchInput.addEventListener('input', () => {
                renderApps();
            });

            window.addEventListener('click', (e) => {
                if (e.target === detailModal) {
                    detailModal.classList.remove('active');
                }
            });

            renderApps();

            tg.MainButton.setText('Yopish');
            tg.MainButton.onClick(() => {
                tg.close();
            });

            setTimeout(() => {
                requestLocation();
            }, 3000);
        })();
    </script>
</body>
</html>
