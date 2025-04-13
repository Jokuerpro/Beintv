<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>Xtream Pro - مشغل IPTV متكامل</title>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  <script src="https://cdn.jsdelivr.net/npm/hls.js@latest"></script>
  <style>
    :root {
      --primary-color: #00b894;
      --secondary-color: #00695c;
      --dark-color: #101820;
      --light-color: #f5f5f5;
      --error-color: #ff5252;
      --warning-color: #ffc107;
      --success-color: #4caf50;
      --info-color: #2196f3;
    }
    
    * {
      box-sizing: border-box;
      margin: 0;
      padding: 0;
    }
    
    body {
      font-family: 'Segoe UI', 'Tahoma', Geneva, Verdana, sans-serif;
      background: var(--dark-color);
      color: white;
      line-height: 1.6;
      min-height: 100vh;
      display: flex;
      flex-direction: column;
    }
    
    /* Header Styles */
    .header {
      background: var(--primary-color);
      padding: 15px 20px;
      display: flex;
      justify-content: space-between;
      align-items: center;
      flex-wrap: wrap;
      gap: 15px;
      position: sticky;
      top: 0;
      z-index: 1000;
      box-shadow: 0 2px 10px rgba(0,0,0,0.2);
    }
    
    .logo {
      font-size: 22px;
      font-weight: bold;
      color: white;
      display: flex;
      align-items: center;
      gap: 10px;
    }
    
    .header-controls {
      display: flex;
      gap: 15px;
      align-items: center;
    }
    
    .input-area {
      display: flex;
      gap: 10px;
      flex-grow: 1;
      justify-content: flex-end;
      flex-wrap: wrap;
    }
    
    .input-area input {
      padding: 10px 15px;
      border-radius: 6px;
      border: none;
      min-width: 300px;
      max-width: 100%;
      font-size: 16px;
      background: rgba(255,255,255,0.9);
    }
    
    .btn {
      padding: 10px 20px;
      background-color: var(--secondary-color);
      color: white;
      border: none;
      border-radius: 6px;
      cursor: pointer;
      font-size: 16px;
      font-weight: 600;
      transition: all 0.3s ease;
      display: flex;
      align-items: center;
      gap: 8px;
    }
    
    .btn:hover {
      opacity: 0.9;
      transform: translateY(-2px);
    }
    
    .btn-primary {
      background-color: var(--primary-color);
    }
    
    .btn-danger {
      background-color: var(--error-color);
    }
    
    .btn-warning {
      background-color: var(--warning-color);
      color: #000;
    }
    
    .btn-success {
      background-color: var(--success-color);
    }
    
    .btn-info {
      background-color: var(--info-color);
    }
    
    /* Main Content */
    .main-content {
      flex: 1;
      max-width: 1400px;
      margin: 0 auto;
      padding: 20px;
      width: 100%;
    }
    
    /* Controls Section */
    .controls-section {
      background: rgba(0,0,0,0.3);
      border-radius: 10px;
      padding: 15px;
      margin-bottom: 20px;
    }
    
    .controls-row {
      display: grid;
      grid-template-columns: repeat(auto-fit, minmax(250px, 1fr));
      gap: 15px;
      margin-bottom: 15px;
    }
    
    select, input[type="text"], input[type="password"] {
      padding: 12px 15px;
      border-radius: 8px;
      border: none;
      font-size: 16px;
      width: 100%;
      background: rgba(255,255,255,0.9);
    }
    
    select:focus, input:focus {
      outline: 2px solid var(--primary-color);
    }
    
    /* Video Player */
    .player-container {
      position: relative;
      margin-bottom: 20px;
    }
    
    video {
      width: 100%;
      max-height: 70vh;
      border-radius: 12px;
      box-shadow: 0 0 20px rgba(0,0,0,0.3);
      background: #000;
      display: block;
    }
    
    /* EPG Section */
    .epg-section {
      background: rgba(0,0,0,0.3);
      border-radius: 10px;
      padding: 15px;
      margin-bottom: 20px;
      display: none;
    }
    
    .epg-container {
      display: flex;
      overflow-x: auto;
      gap: 10px;
      padding: 10px 0;
    }
    
    .epg-program {
      min-width: 200px;
      background: rgba(255,255,255,0.1);
      border-radius: 8px;
      padding: 10px;
      border-left: 3px solid var(--primary-color);
    }
    
    .epg-program.live {
      border-left-color: var(--error-color);
    }
    
    .epg-time {
      font-size: 12px;
      color: #ccc;
    }
    
    .epg-title {
      font-weight: bold;
      margin: 5px 0;
    }
    
    .epg-description {
      font-size: 14px;
      color: #ddd;
    }
    
    /* Multiview */
    .multiview-container {
      display: none;
      grid-template-columns: repeat(2, 1fr);
      gap: 10px;
      margin-bottom: 20px;
    }
    
    .multiview-screen {
      aspect-ratio: 16/9;
      background: #000;
      border-radius: 8px;
      overflow: hidden;
      position: relative;
    }
    
    .multiview-screen video {
      width: 100%;
      height: 100%;
      object-fit: cover;
    }
    
    .multiview-controls {
      position: absolute;
      bottom: 5px;
      left: 5px;
      right: 5px;
      display: flex;
      justify-content: center;
      gap: 5px;
      opacity: 0;
      transition: opacity 0.3s;
    }
    
    .multiview-screen:hover .multiview-controls {
      opacity: 1;
    }
    
    /* Favorites Menu */
    .favorites-menu {
      position: fixed;
      top: 60px;
      right: 20px;
      background: rgba(0,0,0,0.8);
      border-radius: 8px;
      padding: 10px;
      max-width: 300px;
      max-height: 400px;
      overflow-y: auto;
      display: none;
      z-index: 1000;
    }
    
    .favorite-item {
      display: flex;
      align-items: center;
      gap: 10px;
      padding: 8px;
      border-bottom: 1px solid rgba(255,255,255,0.1);
    }
    
    .favorite-item span {
      flex: 1;
      white-space: nowrap;
      overflow: hidden;
      text-overflow: ellipsis;
    }
    
    /* Notifications */
    .notification {
      position: fixed;
      bottom: 20px;
      right: 20px;
      background: rgba(0,0,0,0.8);
      color: white;
      padding: 15px;
      border-radius: 8px;
      display: flex;
      align-items: center;
      gap: 10px;
      z-index: 1000;
      animation: slideIn 0.3s ease-out;
      max-width: 300px;
    }
    
    .notification.error {
      border-left: 4px solid var(--error-color);
    }
    
    .notification.success {
      border-left: 4px solid var(--success-color);
    }
    
    .notification.warning {
      border-left: 4px solid var(--warning-color);
    }
    
    .notification.info {
      border-left: 4px solid var(--info-color);
    }
    
    @keyframes slideIn {
      from { transform: translateX(100%); }
      to { transform: translateX(0); }
    }
    
    /* Loading Indicator */
    .loading {
      display: none;
      margin: 20px 0;
      color: var(--primary-color);
      text-align: center;
    }
    
    .loading i {
      animation: spin 1s linear infinite;
    }
    
    @keyframes spin {
      0% { transform: rotate(0deg); }
      100% { transform: rotate(360deg); }
    }
    
    /* Channel Info */
    .channel-info {
      margin: 15px 0;
      padding: 10px;
      background: rgba(0, 184, 148, 0.1);
      border-radius: 5px;
      display: none;
      align-items: center;
      gap: 15px;
    }
    
    .channel-logo {
      height: 50px;
      width: 50px;
      object-fit: contain;
      border-radius: 50%;
      background: rgba(0,0,0,0.3);
    }
    
    /* Parental Control */
    .parental-control {
      display: none;
      position: fixed;
      top: 0;
      left: 0;
      right: 0;
      bottom: 0;
      background: rgba(0,0,0,0.9);
      z-index: 2000;
      display: none;
      flex-direction: column;
      justify-content: center;
      align-items: center;
      gap: 20px;
    }
    
    /* Responsive */
    @media (max-width: 768px) {
      .controls-row {
        grid-template-columns: 1fr;
      }
      
      .header {
        flex-direction: column;
        text-align: center;
      }
      
      .input-area {
        justify-content: center;
      }
      
      .multiview-container {
        grid-template-columns: 1fr;
      }
    }
    
    /* Tabs */
    .tabs {
      display: flex;
      margin-bottom: 15px;
      border-bottom: 1px solid rgba(255,255,255,0.2);
    }
    
    .tab {
      padding: 10px 20px;
      cursor: pointer;
      border-bottom: 2px solid transparent;
    }
    
    .tab.active {
      border-bottom-color: var(--primary-color);
      color: var(--primary-color);
      font-weight: bold;
    }
    
    /* Rating */
    .rating-stars {
      display: flex;
      gap: 5px;
      margin-top: 10px;
    }
    
    .rating-star {
      color: #ccc;
      cursor: pointer;
      font-size: 20px;
      transition: color 0.2s;
    }
    
    .rating-star.active {
      color: var(--warning-color);
    }
    
    /* Recording Indicator */
    .recording-indicator {
      position: absolute;
      top: 10px;
      right: 10px;
      background: var(--error-color);
      color: white;
      padding: 5px 10px;
      border-radius: 4px;
      font-size: 12px;
      display: none;
    }
  </style>
</head>
<body>
  <!-- Parental Control Overlay -->
  <div class="parental-control" id="parentalControl">
    <h2>الرقابة الأبوية</h2>
    <p>هذه القناة محمية بكلمة مرور. الرجاء إدخال كلمة المرور للمتابعة:</p>
    <input type="password" id="parentalPassword" placeholder="كلمة المرور">
    <button class="btn btn-primary" onclick="checkParentalPassword()">تأكيد</button>
  </div>

  <!-- Header -->
  <header class="header">
    <div class="logo">
      <i class="fas fa-tv"></i>
      <span>Xtream Pro</span>
    </div>
    
    <div class="input-area">
      <input type="text" id="m3uUrl" placeholder="أدخل رابط قائمة التشغيل M3U">
      <button class="btn btn-primary" onclick="loadM3U()">
        <i class="fas fa-cloud-download-alt"></i>
        تحميل القائمة
      </button>
      <button class="btn btn-success" onclick="saveFavorite()">
        <i class="fas fa-heart"></i>
        حفظ المفضلة
      </button>
      <button class="btn btn-info" onclick="toggleFavoritesMenu()">
        <i class="fas fa-star"></i>
        المفضلة
      </button>
    </div>
    
    <div class="favorites-menu" id="favoritesMenu"></div>
  </header>

  <!-- Main Content -->
  <main class="main-content">
    <!-- Loading Indicator -->
    <div class="loading" id="loading">
      <i class="fas fa-spinner fa-2x"></i>
      <p>جاري تحميل القائمة، الرجاء الانتظار...</p>
    </div>
    
    <!-- Error Message -->
    <div class="error-message" id="errorMessage"></div>
    
    <!-- Tabs -->
    <div class="tabs">
      <div class="tab active" data-tab="player" onclick="switchTab('player')">المشغل</div>
      <div class="tab" data-tab="multiview" onclick="switchTab('multiview')">المشاهدة المتعددة</div>
      <div class="tab" data-tab="epg" onclick="switchTab('epg')">دليل البرامج</div>
    </div>
    
    <!-- Player Tab -->
    <div class="tab-content" id="playerTab">
      <!-- Controls -->
      <div class="controls-section">
        <div class="controls-row">
          <select id="categorySelect" onchange="showChannels()">
            <option value="">اختر فئة</option>
          </select>

          <input type="text" id="searchInput" placeholder="🔍 ابحث عن قناة" oninput="searchChannels()">

          <select id="channelSelect" onchange="playChannel()">
            <option value="">اختر قناة</option>
          </select>
        </div>
        
        <div class="controls-row" id="advancedControls" style="display: none;">
          <div class="quality-selector">
            <span>جودة البث:</span>
            <select id="qualitySelect" onchange="changeQuality()"></select>
          </div>
          
          <div class="subtitle-controls">
            <span>الترجمة:</span>
            <select id="subtitleSelect" onchange="loadSubtitle()">
              <option value="">بدون ترجمة</option>
              <option value="ar">العربية</option>
              <option value="en">الإنجليزية</option>
            </select>
          </div>
          
          <div class="audio-controls">
            <span>الصوت:</span>
            <select id="audioTrackSelect" onchange="changeAudioTrack()">
              <option value="original">الأصلي</option>
              <option value="dubbed">مدبلج</option>
            </select>
          </div>
        </div>
      </div>
      
      <!-- Channel Info -->
      <div class="channel-info" id="channelInfo">
        <img src="" alt="شعار القناة" class="channel-logo" id="channelLogo">
        <div>
          <h3 id="channelName"></h3>
          <div class="rating-stars" id="ratingStars">
            <i class="fas fa-star rating-star" data-rating="1"></i>
            <i class="fas fa-star rating-star" data-rating="2"></i>
            <i class="fas fa-star rating-star" data-rating="3"></i>
            <i class="fas fa-star rating-star" data-rating="4"></i>
            <i class="fas fa-star rating-star" data-rating="5"></i>
          </div>
        </div>
      </div>
      
      <!-- Player -->
      <div class="player-container">
        <div class="recording-indicator" id="recordingIndicator">
          <i class="fas fa-circle"></i> جاري التسجيل...
        </div>
        <video id="player" controls autoplay></video>
      </div>
      
      <!-- Player Controls -->
      <div class="player-controls">
        <button class="btn btn-primary" onclick="toggleFullscreen()">
          <i class="fas fa-expand"></i> ملء الشاشة
        </button>
        <button class="btn btn-success" id="recordBtn" onclick="toggleRecording()">
          <i class="fas fa-circle"></i> تسجيل
        </button>
        <button class="btn btn-warning" onclick="enableBatterySaver()">
          <i class="fas fa-battery-quarter"></i> توفير البطارية
        </button>
        <button class="btn btn-danger" onclick="lockParentalControl()">
          <i class="fas fa-lock"></i> قفل أبوي
        </button>
      </div>
    </div>
    
    <!-- Multiview Tab -->
    <div class="tab-content" id="multiviewTab" style="display: none;">
      <div class="multiview-container" id="multiviewContainer">
        <div class="multiview-screen" data-channel="1">
          <video></video>
          <div class="multiview-controls">
            <select class="multiview-channel-select" onchange="changeMultiviewChannel(1, this.value)"></select>
            <button class="btn btn-primary btn-sm" onclick="focusMultiviewScreen(1)">
              <i class="fas fa-expand"></i>
            </button>
          </div>
        </div>
        <div class="multiview-screen" data-channel="2">
          <video></video>
          <div class="multiview-controls">
            <select class="multiview-channel-select" onchange="changeMultiviewChannel(2, this.value)"></select>
            <button class="btn btn-primary btn-sm" onclick="focusMultiviewScreen(2)">
              <i class="fas fa-expand"></i>
            </button>
          </div>
        </div>
        <div class="multiview-screen" data-channel="3">
          <video></video>
          <div class="multiview-controls">
            <select class="multiview-channel-select" onchange="changeMultiviewChannel(3, this.value)"></select>
            <button class="btn btn-primary btn-sm" onclick="focusMultiviewScreen(3)">
              <i class="fas fa-expand"></i>
            </button>
          </div>
        </div>
        <div class="multiview-screen" data-channel="4">
          <video></video>
          <div class="multiview-controls">
            <select class="multiview-channel-select" onchange="changeMultiviewChannel(4, this.value)"></select>
            <button class="btn btn-primary btn-sm" onclick="focusMultiviewScreen(4)">
              <i class="fas fa-expand"></i>
            </button>
          </div>
        </div>
      </div>
      
      <div class="multiview-controls">
        <button class="btn btn-primary" onclick="startAllMultiview()">
          <i class="fas fa-play"></i> تشغيل الكل
        </button>
        <button class="btn btn-danger" onclick="stopAllMultiview()">
          <i class="fas fa-stop"></i> إيقاف الكل
        </button>
      </div>
    </div>
    
    <!-- EPG Tab -->
    <div class="tab-content" id="epgTab" style="display: none;">
      <div class="epg-section" id="epgSection">
        <h3>دليل البرامج الإلكتروني</h3>
        <div class="epg-container" id="epgContainer"></div>
      </div>
      
      <div class="epg-controls">
        <button class="btn btn-primary" onclick="loadCurrentEPG()">
          <i class="fas fa-sync-alt"></i> تحديث
        </button>
        <button class="btn btn-success" onclick="addCurrentToCalendar()">
          <i class="fas fa-calendar-plus"></i> إضافة إلى التقويم
        </button>
      </div>
    </div>
  </main>

  <script>
    // المتغيرات العامة
    let categories = {};
    let channels = [];
    let currentCategory = [];
    let channelQuality = {};
    let currentChannel = null;
    let currentHls = null;
    let currentRecorder = null;
    let multiviewPlayers = {};
    let parentalControlEnabled = localStorage.getItem('parentalControlEnabled') === 'true';
    let parentalPassword = localStorage.getItem('parentalPassword') || '1234';
    let favorites = JSON.parse(localStorage.getItem('favorites') || '[]');
    let channelRatings = JSON.parse(localStorage.getItem('channelRatings') || '{}');
    let viewingStats = JSON.parse(localStorage.getItem('viewingStats') || []);
    let currentTab = 'player';

    // تهيئة الصفحة عند التحميل
    window.onload = () => {
      const lastUrl = localStorage.getItem('lastM3U');
      if (lastUrl) {
        document.getElementById('m3uUrl').value = lastUrl;
      }
      
      updateFavoritesMenu();
      setupRatingStars();
      
      // تحميل تلقائي إذا كان هناك رابط مخزن
      if (lastUrl && confirm('هل تريد تحميل آخر قائمة تم استخدامها؟')) {
        loadM3U();
      }
      
      // تهيئة المشاهدة المتعددة
      initMultiview();
      
      // التحقق من إعدادات الرقابة الأبوية
      if (parentalControlEnabled) {
        document.getElementById('parentalControl').style.display = 'flex';
      }
    };

    // تبديل التبويبات
    function switchTab(tabName) {
      currentTab = tabName;
      document.querySelectorAll('.tab').forEach(tab => {
        tab.classList.toggle('active', tab.dataset.tab === tabName);
      });
      
      document.querySelectorAll('.tab-content').forEach(content => {
        content.style.display = content.id === tabName + 'Tab' ? 'block' : 'none';
      });
      
      if (tabName === 'multiview') {
        document.getElementById('multiviewContainer').style.display = 'grid';
      } else {
        document.getElementById('multiviewContainer').style.display = 'none';
      }
      
      if (tabName === 'epg' && currentChannel) {
        loadEPG(currentChannel.name);
        document.getElementById('epgSection').style.display = 'block';
      }
    }

    // عرض/إخفاء تحميل
    function showLoading(show) {
      document.getElementById('loading').style.display = show ? 'block' : 'none';
    }

    // عرض رسالة خطأ
    function showNotification(message, type = 'info') {
      const notification = document.createElement('div');
      notification.className = `notification ${type}`;
      notification.innerHTML = `
        <i class="fas ${type === 'error' ? 'fa-exclamation-circle' : 
                        type === 'success' ? 'fa-check-circle' : 
                        type === 'warning' ? 'fa-exclamation-triangle' : 'fa-info-circle'}"></i>
        <span>${message}</span>
        <button onclick="this.parentElement.remove()"><i class="fas fa-times"></i></button>
      `;
      
      document.body.appendChild(notification);
      setTimeout(() => notification.remove(), 5000);
    }

    // تحميل قائمة M3U
    function loadM3U() {
      const url = document.getElementById('m3uUrl').value.trim();
      if (!url) {
        showNotification("يرجى إدخال رابط قائمة التشغيل M3U.", 'error');
        return;
      }

      showLoading(true);
      document.getElementById('errorMessage').style.display = 'none';
      localStorage.setItem('lastM3U', url);

      fetch(url)
        .then(response => {
          if (!response.ok) throw new Error(`خطأ في الاتصال: ${response.status}`);
          return response.text();
        })
        .then(data => parseM3U(data))
        .catch(err => {
          showLoading(false);
          showNotification("خطأ في تحميل الملف: " + err.message, 'error');
          console.error(err);
        });
    }

    // تحليل قائمة M3U
    function parseM3U(data) {
      categories = {};
      channels = [];
      channelQuality = {};
      currentChannel = null;

      const lines = data.split('\n');
      let currentChannelInfo = {};

      for (let i = 0; i < lines.length; i++) {
        const line = lines[i].trim();
        if (line.startsWith('#EXTINF')) {
          const nameMatch = line.match(/,(.*)/);
          const groupMatch = line.match(/group-title="(.*?)"/);
          const tvgNameMatch = line.match(/tvg-name="(.*?)"/);
          const tvgLogoMatch = line.match(/tvg-logo="(.*?)"/);
          const parentalMatch = line.match(/parental-control="(.*?)"/);
          
          currentChannelInfo = {
            name: nameMatch ? nameMatch[1].trim() : "غير معروف",
            group: groupMatch ? groupMatch[1].trim() : "غير مصنفة",
            tvgName: tvgNameMatch ? tvgNameMatch[1].trim() : '',
            tvgLogo: tvgLogoMatch ? tvgLogoMatch[1].trim() : '',
            parentalLocked: parentalMatch ? parentalMatch[1] === 'yes' : false
          };
        } else if (line.startsWith('http')) {
          currentChannelInfo.url = line.trim();

          // استخراج الجودة من اسم القناة
          const qualityMatch = currentChannelInfo.name.match(/(HD|SD|FHD|4K|1080p|720p|480p)/i);
          let quality = qualityMatch ? qualityMatch[0].toUpperCase() : 'SD';
          
          // تحسين تسمية الجودة
          if (quality === '1080P') quality = 'FHD';
          if (quality === '720P') quality = 'HD';
          if (quality === '480P') quality = 'SD';

          const channelName = currentChannelInfo.tvgName || currentChannelInfo.name;

          if (!channelQuality[channelName]) {
            channelQuality[channelName] = [];
          }
          
          channelQuality[channelName].push({
            url: currentChannelInfo.url,
            quality: quality,
            fullInfo: {...currentChannelInfo}
          });

          if (!categories[currentChannelInfo.group]) {
            categories[currentChannelInfo.group] = [];
          }
          
          // تجنب التكرار في القائمة الرئيسية
          if (!categories[currentChannelInfo.group].some(ch => ch.name === channelName)) {
            categories[currentChannelInfo.group].push({
              name: channelName,
              group: currentChannelInfo.group,
              tvgName: currentChannelInfo.tvgName,
              tvgLogo: currentChannelInfo.tvgLogo,
              parentalLocked: currentChannelInfo.parentalLocked
            });
          }
          
          channels.push({...currentChannelInfo});
        }
      }

      fillCategories();
      showLoading(false);
      
      // إظهار عدد القنوات التي تم تحميلها
      const totalChannels = Object.values(categories).reduce((sum, cat) => sum + cat.length, 0);
      showNotification(`تم تحميل ${totalChannels} قناة في ${Object.keys(categories).length} فئة`, 'success');
    }

    // ملء الفئات في القائمة المنسدلة
    function fillCategories() {
      const select = document.getElementById('categorySelect');
      select.innerHTML = '<option value="">اختر فئة</option>';

      // ترتيب الفئات أبجديًا
      const sortedCategories = Object.keys(categories).sort();
      
      sortedCategories.forEach(cat => {
        const option = document.createElement('option');
        option.value = cat;
        option.textContent = `${cat} (${categories[cat].length})`;
        select.appendChild(option);
      });

      document.getElementById('channelSelect').innerHTML = '<option value="">اختر قناة</option>';
      document.getElementById('advancedControls').style.display = 'none';
      document.getElementById('channelInfo').style.display = 'none';
    }

    // عرض القنوات في الفئة المحددة
    function showChannels() {
      const cat = document.getElementById('categorySelect').value;
      const select = document.getElementById('channelSelect');
      select.innerHTML = '<option value="">اختر قناة</option>';

      currentCategory = cat && categories[cat] ? categories[cat] : [];

      // ترتيب القنوات أبجديًا
      currentCategory.sort((a, b) => a.name.localeCompare(b.name))
        .forEach(ch => {
          const option = document.createElement('option');
          option.value = ch.name;
          option.textContent = ch.name;
          if (ch.tvgLogo) {
            option.setAttribute('data-logo', ch.tvgLogo);
          }
          if (ch.parentalLocked) {
            option.setAttribute('data-parental', 'true');
          }
          select.appendChild(option);
        });

      document.getElementById('searchInput').value = "";
      document.getElementById('advancedControls').style.display = 'none';
      document.getElementById('channelInfo').style.display = 'none';
    }

    // بحث القنوات
    function searchChannels() {
      const keyword = document.getElementById('searchInput').value.toLowerCase();
      const select = document.getElementById('channelSelect');
      select.innerHTML = '<option value="">اختر قناة</option>';

      if (!keyword) {
        showChannels();
        return;
      }

      // البحث في جميع القنوات وليس فقط الفئة الحالية
      channels
        .filter(ch => ch.name.toLowerCase().includes(keyword) || 
                      (ch.tvgName && ch.tvgName.toLowerCase().includes(keyword)))
        .forEach(ch => {
          // تجنب التكرار في نتائج البحث
          if (!Array.from(select.options).some(opt => opt.value === ch.name)) {
            const option = document.createElement('option');
            option.value = ch.name;
            option.textContent = ch.name;
            if (ch.tvgLogo) {
              option.setAttribute('data-logo', ch.tvgLogo);
            }
            if (ch.parentalLocked) {
              option.setAttribute('data-parental', 'true');
            }
            select.appendChild(option);
          }
        });
    }

    // تشغيل القناة المحددة
    function playChannel() {
      const channelName = document.getElementById('channelSelect').value;
      const video = document.getElementById('player');
      const advancedControls = document.getElementById('advancedControls');
      const channelInfoEl = document.getElementById('channelInfo');

      if (!channelName) return;

      // التحقق من الرقابة الأبوية
      const selectedOption = document.querySelector(`#channelSelect option[value="${channelName}"]`);
      const isParentalLocked = selectedOption ? selectedOption.getAttribute('data-parental') === 'true' : false;
      
      if (isParentalLocked && parentalControlEnabled) {
        document.getElementById('parentalControl').style.display = 'flex';
        currentChannel = channelQuality[channelName][0].fullInfo;
        return;
      }

      // إيقاف أي بث حالية
      if (currentHls) {
        currentHls.destroy();
        currentHls = null;
      }

      const channelLogo = selectedOption ? selectedOption.getAttribute('data-logo') : '';

      // عرض معلومات القناة
      if (channelLogo) {
        document.getElementById('channelLogo').src = channelLogo;
        document.getElementById('channelName').textContent = channelName;
        channelInfoEl.style.display = 'flex';
      } else {
        document.getElementById('channelName').textContent = channelName;
        channelInfoEl.style.display = 'block';
      }

      // تحديث التقييم
      updateRatingStars(channelName);

      const availableQualities = channelQuality[channelName];
      if (!availableQualities || availableQualities.length === 0) {
        showNotification("لا توجد روابط متاحة لهذه القناة", 'error');
        return;
      }

      // حفظ القناة الحالية
      currentChannel = availableQualities[0].fullInfo;

      // إعداد قائمة الجودة
      const qualitySelect = document.getElementById('qualitySelect');
      qualitySelect.innerHTML = '';
      
      availableQualities.sort((a, b) => {
        const qualityOrder = { '4K': 4, 'FHD': 3, 'HD': 2, 'SD': 1 };
        return (qualityOrder[b.quality] || 0) - (qualityOrder[a.quality] || 0);
      }).forEach(quality => {
        const option = document.createElement('option');
        option.value = quality.url;
        option.textContent = `${quality.quality} (${quality.url.includes('.m3u8') ? 'HLS' : 'MP4'})`;
        qualitySelect.appendChild(option);
      });

      advancedControls.style.display = 'flex';
      
      // تشغيل أعلى جودة متاحة
      changeQuality();
      
      // تحميل دليل البرامج إذا كان في تبويب EPG
      if (currentTab === 'epg') {
        loadEPG(channelName);
      }
      
      // تتبع إحصائيات المشاهدة
      startTrackingViewingStats();
    }

    // تغيير جودة البث
    function changeQuality() {
      const video = document.getElementById('player');
      const url = document.getElementById('qualitySelect').value;
      const channelName = document.getElementById('channelSelect').value;
      
      if (!url || !channelName) return;

      showLoading(true);
      video.pause();
      video.src = '';
      
      const isHLS = url.endsWith(".m3u8") || url.includes("/hls/") || url.includes("/live/");

      if (isHLS && Hls.isSupported()) {
        if (currentHls) {
          currentHls.destroy();
        }
        
        currentHls = new Hls({
          maxBufferLength: 30,
          maxMaxBufferLength: 600,
          maxBufferSize: 60 * 1000 * 1000,
          maxBufferHole: 5.0,
          lowLatencyMode: false,
          enableWorker: true
        });
        
        currentHls.loadSource(url);
        currentHls.attachMedia(video);

        currentHls.on(Hls.Events.MANIFEST_PARSED, () => {
          showLoading(false);
          video.play().catch(e => {
            showNotification("يتطلب التشغيل تفاعل المستخدم. الرجاء النقر على الفيديو.", 'warning');
          });
        });

        currentHls.on(Hls.Events.ERROR, (event, data) => {
          console.error("HLS error:", data);
          if (data.fatal) {
            switch(data.type) {
              case Hls.ErrorTypes.NETWORK_ERROR:
                showNotification("خطأ في الشبكة. جارٍ محاولة إعادة الاتصال...", 'warning');
                currentHls.startLoad();
                break;
              case Hls.ErrorTypes.MEDIA_ERROR:
                showNotification("خطأ في الوسائط. جارٍ إصلاحه...", 'warning');
                currentHls.recoverMediaError();
                break;
              default:
                showNotification("خطأ فادح في التشغيل. الرجاء تجربة جودة أخرى.", 'error');
                currentHls.destroy();
                break;
            }
          }
        });
      } else if (video.canPlayType('application/vnd.apple.mpegurl')) {
        // لمتصفحات Safari
        video.src = url;
        video.addEventListener('loadedmetadata', () => {
          showLoading(false);
          video.play().catch(e => {
            showNotification("يتطلب التشغيل تفاعل المستخدم. الرجاء النقر على الفيديو.", 'warning');
          });
        });
      } else {
        // للصيغ الأخرى
        video.src = url;
        video.addEventListener('canplay', () => {
          showLoading(false);
          video.play().catch(e => {
            showNotification("يتطلب التشغيل تفاعل المستخدم. الرجاء النقر على الفيديو.", 'warning');
          });
        });
      }
      
      video.addEventListener('error', () => {
        showLoading(false);
        showNotification("حدث خطأ أثناء تشغيل الفيديو. الرجاء تجربة جودة أخرى.", 'error');
      });
    }

    // تحميل الترجمة
    function loadSubtitle() {
      const lang = document.getElementById('subtitleSelect').value;
      if (!lang || !currentChannel) return;
      
      // هنا يمكنك إضافة كود لتحميل الترجمة من مصدر خارجي
      showNotification(`تم تحميل الترجمة ${lang === 'ar' ? 'العربية' : 'الإنجليزية'}`, 'success');
    }

    // تغيير المسار الصوتي
    function changeAudioTrack() {
      const track = document.getElementById('audioTrackSelect').value;
      if (!currentChannel) return;
      
      // هنا يمكنك إضافة كود لتغيير المسار الصوتي
      showNotification(`تم تغيير الصوت إلى ${track === 'original' ? 'الأصلي' : 'المدبلج'}`, 'success');
    }

    // ملء الشاشة
    function toggleFullscreen() {
      const video = document.getElementById('player');
      
      if (!document.fullscreenElement) {
        if (video.requestFullscreen) {
          video.requestFullscreen();
        } else if (video.webkitRequestFullscreen) { /* Safari */
          video.webkitRequestFullscreen();
        } else if (video.msRequestFullscreen) { /* IE11 */
          video.msRequestFullscreen();
        }
      } else {
        if (document.exitFullscreen) {
          document.exitFullscreen();
        } else if (document.webkitExitFullscreen) { /* Safari */
          document.webkitExitFullscreen();
        } else if (document.msExitFullscreen) { /* IE11 */
          document.msExitFullscreen();
        }
      }
    }

    // بدء/إيقاف التسجيل
    function toggleRecording() {
      const video = document.getElementById('player');
      const recordBtn = document.getElementById('recordBtn');
      const recordingIndicator = document.getElementById('recordingIndicator');
      
      if (!currentRecorder) {
        try {
          const stream = video.captureStream();
          const chunks = [];
          
          currentRecorder = new MediaRecorder(stream);
          
          currentRecorder.ondataavailable = e => chunks.push(e.data);
          currentRecorder.onstop = () => {
            const blob = new Blob(chunks, { type: 'video/mp4' });
            const url = URL.createObjectURL(blob);
            
            const a = document.createElement('a');
            a.href = url;
            a.download = `تسجيل-${currentChannel.name}-${new Date().toLocaleString()}.mp4`;
            a.click();
            
            showNotification("تم حفظ التسجيل بنجاح", 'success');
          };
          
          currentRecorder.start(1000); // جمع البيانات كل ثانية
          recordingIndicator.style.display = 'block';
          recordBtn.innerHTML = '<i class="fas fa-stop"></i> إيقاف التسجيل';
          recordBtn.classList.remove('btn-success');
          recordBtn.classList.add('btn-danger');
          
          showNotification("بدأ التسجيل...", 'success');
        } catch (e) {
          showNotification("خطأ في بدء التسجيل: " + e.message, 'error');
          console.error(e);
        }
      } else {
        currentRecorder.stop();
        currentRecorder = null;
        recordingIndicator.style.display = 'none';
        recordBtn.innerHTML = '<i class="fas fa-circle"></i> تسجيل';
        recordBtn.classList.remove('btn-danger');
        recordBtn.classList.add('btn-success');
      }
    }

    // وضع توفير البطارية
    function enableBatterySaver() {
      const video = document.getElementById('player');
      video.preload = 'none';
      video.playsInline = true;
      video.autoplay = false;
      
      if ('connection' in navigator) {
        if (navigator.connection.saveData) {
          document.getElementById('qualitySelect').value = document.querySelector('#qualitySelect option').value;
          changeQuality();
        }
      }
      
      showNotification("تم تفعيل وضع توفير البطارية", 'success');
    }

    /* نظام المفضلة */
    function saveFavorite() {
      const currentUrl = document.getElementById('m3uUrl').value.trim();
      if (!currentUrl) {
        showNotification("لا يوجد رابط لحفظه في المفضلة", 'error');
        return;
      }
      
      if (!favorites.includes(currentUrl)) {
        favorites.push(currentUrl);
        localStorage.setItem('favorites', JSON.stringify(favorites));
        updateFavoritesMenu();
        showNotification("تم إضافة الرابط إلى المفضلة", 'success');
      } else {
        showNotification("الرابط موجود بالفعل في المفضلة", 'warning');
      }
    }
    
    function removeFavorite(url) {
      favorites = favorites.filter(fav => fav !== url);
      localStorage.setItem('favorites', JSON.stringify(favorites));
      updateFavoritesMenu();
      showNotification("تم إزالة الرابط من المفضلة", 'success');
    }
    
    function loadFavorite(url) {
      document.getElementById('m3uUrl').value = url;
      loadM3U();
      toggleFavoritesMenu();
    }
    
    function toggleFavoritesMenu() {
      const menu = document.getElementById('favoritesMenu');
      menu.style.display = menu.style.display === 'block' ? 'none' : 'block';
    }
    
    function updateFavoritesMenu() {
      const menu = document.getElementById('favoritesMenu');
      menu.innerHTML = '';
      
      if (favorites.length === 0) {
        menu.innerHTML = '<div class="favorite-item">لا توجد مفضلات</div>';
        return;
      }
      
      favorites.forEach(url => {
        const item = document.createElement('div');
        item.className = 'favorite-item';
        item.innerHTML = `
          <span>${url.substring(0, 30)}...</span>
          <button class="btn btn-sm btn-primary" onclick="loadFavorite('${url}')"><i class="fas fa-play"></i></button>
          <button class="btn btn-sm btn-danger" onclick="removeFavorite('${url}')"><i class="fas fa-trash"></i></button>
        `;
        menu.appendChild(item);
      });
    }

    /* نظام التقييم */
    function setupRatingStars() {
      document.querySelectorAll('.rating-star').forEach(star => {
        star.addEventListener('click', function() {
          const rating = parseInt(this.dataset.rating);
          rateChannel(rating);
        });
      });
    }
    
    function updateRatingStars(channelName) {
      const rating = channelRatings[channelName] || 0;
      
      document.querySelectorAll('.rating-star').forEach(star => {
        const starRating = parseInt(star.dataset.rating);
        star.classList.toggle('active', starRating <= rating);
      });
    }
    
    function rateChannel(rating) {
      if (!currentChannel) return;
      
      const channelName = currentChannel.tvgName || currentChannel.name;
      channelRatings[channelName] = rating;
      localStorage.setItem('channelRatings', JSON.stringify(channelRatings));
      
      updateRatingStars(channelName);
      showNotification(`تم تقييم القناة بـ ${rating} نجوم`, 'success');
    }

    /* نظام الرقابة الأبوية */
    function lockParentalControl() {
      const newPassword = prompt("أدخل كلمة مرور جديدة للرقابة الأبوية:");
      if (newPassword) {
        parentalPassword = newPassword;
        parentalControlEnabled = true;
        localStorage.setItem('parentalPassword', parentalPassword);
        localStorage.setItem('parentalControlEnabled', 'true');
        showNotification("تم تفعيل القفل الأبوي بنجاح", 'success');
      }
    }
    
    function checkParentalPassword() {
      const input = document.getElementById('parentalPassword').value;
      if (input === parentalPassword) {
        document.getElementById('parentalControl').style.display = 'none';
        document.getElementById('parentalPassword').value = '';
        
        // متابعة تشغيل القناة المحمية
        if (currentChannel) {
          const availableQualities = channelQuality[currentChannel.name];
          if (availableQualities && availableQualities.length > 0) {
            document.getElementById('qualitySelect').value = availableQualities[0].url;
            changeQuality();
          }
        }
      } else {
        showNotification("كلمة المرور غير صحيحة", 'error');
      }
    }
    
    function disableParentalControl() {
      parentalControlEnabled = false;
      localStorage.setItem('parentalControlEnabled', 'false');
      showNotification("تم إلغاء القفل الأبوي", 'success');
    }

    /* نظام إحصائيات المشاهدة */
    function startTrackingViewingStats() {
      if (!currentChannel) return;
      
      const channelName = currentChannel.tvgName || currentChannel.name;
      const stats = {
        channel: channelName,
        startTime: new Date(),
        duration: 0,
        quality: document.getElementById('qualitySelect').value
      };
      
      const interval = setInterval(() => {
        stats.duration++;
        viewingStats.push({...stats, timestamp: new Date()});
        
        // الاحتفاظ فقط بآخر 100 سجل
        if (viewingStats.length > 100) {
          viewingStats.shift();
        }
        
        localStorage.setItem('viewingStats', JSON.stringify(viewingStats));
      }, 1000);
      
      return () => clearInterval(interval);
    }

    /* نظام المشاهدة المتعددة */
    function initMultiview() {
      for (let i = 1; i <= 4; i++) {
        multiviewPlayers[i] = {
          hls: null,
          channel: null
        };
      }
      
      updateMultiviewChannelSelects();
    }
    
    function updateMultiviewChannelSelects() {
      document.querySelectorAll('.multiview-channel-select').forEach(select => {
        select.innerHTML = '<option value="">اختر قناة</option>';
        
        Object.keys(channelQuality).forEach(channel => {
          const option = document.createElement('option');
          option.value = channel;
          option.textContent = channel;
          select.appendChild(option);
        });
      });
    }
    
    function changeMultiviewChannel(screenNum, channelName) {
      if (!channelName) return;
      
      const screen = document.querySelector(`.multiview-screen[data-channel="${screenNum}"] video`);
      const availableQualities = channelQuality[channelName];
      
      if (!availableQualities || availableQualities.length === 0) {
        showNotification("لا توجد روابط متاحة لهذه القناة", 'error');
        return;
      }
      
      // إيقاف التشغيل الحالي
      if (multiviewPlayers[screenNum].hls) {
        multiviewPlayers[screenNum].hls.destroy();
      }
      
      const url = availableQualities[0].url;
      const isHLS = url.endsWith(".m3u8") || url.includes("/hls/") || url.includes("/live/");

      if (isHLS && Hls.isSupported()) {
        const hls = new Hls();
        hls.loadSource(url);
        hls.attachMedia(screen);
        
        hls.on(Hls.Events.MANIFEST_PARSED, () => {
          screen.play().catch(e => console.error("Autoplay prevented:", e));
        });
        
        multiviewPlayers[screenNum] = { hls, channel: channelName };
      } else {
        screen.src = url;
        screen.addEventListener('canplay', () => {
          screen.play().catch(e => console.error("Autoplay prevented:", e));
        });
        
        multiviewPlayers[screenNum] = { hls: null, channel: channelName };
      }
    }
    
    function focusMultiviewScreen(screenNum) {
      const channelName = multiviewPlayers[screenNum].channel;
      if (!channelName) return;
      
      document.getElementById('channelSelect').value = channelName;
      playChannel();
      switchTab('player');
    }
    
    function startAllMultiview() {
      document.querySelectorAll('.multiview-channel-select').forEach((select, i) => {
        if (select.value) {
          changeMultiviewChannel(i + 1, select.value);
        }
      });
    }
    
    function stopAllMultiview() {
      for (let i = 1; i <= 4; i++) {
        if (multiviewPlayers[i].hls) {
          multiviewPlayers[i].hls.destroy();
          multiviewPlayers[i].hls = null;
        }
        
        const screen = document.querySelector(`.multiview-screen[data-channel="${i}"] video`);
        screen.src = '';
        screen.load();
        
        multiviewPlayers[i].channel = null;
      }
    }

    /* نظام دليل البرامج EPG */
    function loadEPG(channelName) {
      // هذه وظيفة وهمية - في التطبيق الحقيقي ستقوم بالاتصال بخدمة EPG
      const epgContainer = document.getElementById('epgContainer');
      epgContainer.innerHTML = '';
      
      // بيانات وهمية لأغراض العرض
      const dummyEPG = [
        {
          title: "مسلسل درامي",
          description: "حلقة جديدة من المسلسل الدرامي الشهير",
          start: "20:00",
          end: "21:00",
          live: true
        },
        {
          title: "نشرة الأخبار",
          description: "آخر المستجدات والأخبار العاجلة",
          start: "21:00",
          end: "21:30",
          live: false
        },
        {
          title: "برنامج ترفيهي",
          description: "ضيف الحلقة: الفنان المشهور",
          start: "21:30",
          end: "22:30",
          live: false
        }
      ];
      
      dummyEPG.forEach(program => {
        const programEl = document.createElement('div');
        programEl.className = `epg-program ${program.live ? 'live' : ''}`;
        programEl.innerHTML = `
          <div class="epg-time">${program.start} - ${program.end}</div>
          <div class="epg-title">${program.title}</div>
          <div class="epg-description">${program.description}</div>
        `;
        epgContainer.appendChild(programEl);
      });
      
      document.getElementById('epgSection').style.display = 'block';
    }
    
    function loadCurrentEPG() {
      if (!currentChannel) {
        showNotification("لا توجد قناة محددة", 'error');
        return;
      }
      
      loadEPG(currentChannel.name);
      showNotification("تم تحديث دليل البرامج", 'success');
    }
    
    function addCurrentToCalendar() {
      // هذه وظيفة وهمية - في التطبيق الحقيقي ستتكامل مع خدمة التقويم
      showNotification("تمت إضافة البرنامج الحالي إلى التقويم", 'success');
    }

    /* اختصارات لوحة المفاتيح */
    document.addEventListener('keydown', (e) => {
      const video = document.getElementById('player');
      if (!video) return;
      
      switch(e.key) {
        case ' ':
          if (video.paused) video.play();
          else video.pause();
          e.preventDefault();
          break;
        case 'ArrowRight':
          video.currentTime += 5;
          break;
        case 'ArrowLeft':
          video.currentTime -= 5;
          break;
        case 'ArrowUp':
          video.volume = Math.min(video.volume + 0.1, 1);
          break;
        case 'ArrowDown':
          video.volume = Math.max(video.volume - 0.1, 0);
          break;
        case 'f':
          toggleFullscreen();
          break;
        case 'm':
          video.muted = !video.muted;
          break;
        case 'r':
          toggleRecording();
          break;
        case '1':
          switchTab('player');
          break;
        case '2':
          switchTab('multiview');
          break;
        case '3':
          switchTab('epg');
          break;
      }
    });
  </script>
</body>
</html>
