
<html lang="zh-TW">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <title>北海道大冒險2.0</title>
  
  <!-- Tailwind CSS & FontAwesome Icons -->
  <script src="https://cdn.tailwindcss.com"></script>
  <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
  
  <!-- Google Fonts: 繁體中文無襯線字體 Noto Sans TC -->
  <link rel="preconnect" href="https://fonts.googleapis.com">
  <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
  <link href="https://fonts.googleapis.com/css2?family=Noto+Sans+TC:wght@400;500;700;900&display=swap" rel="stylesheet">

  <style>
    * {
      font-family: 'Noto Sans TC', -apple-system, BlinkMacSystemFont, "Segoe UI", Roboto, "Helvetica Neue", Arial, sans-serif !important;
    }

    body {
      background-color: #FFFDF9;
      color: #3E3A39;
      position: relative;
    }

    /* 全頁飄雪 Canvas 背景 */
    #snow-canvas {
      position: fixed;
      top: 0;
      left: 0;
      width: 100vw;
      height: 100vh;
      pointer-events: none;
      z-index: 30;
    }

    .pixel-box {
      background-color: #FFFDF9;
      border: 3px solid #3E3A39;
      box-shadow: 4px 4px 0px #3E3A39;
      border-radius: 12px;
      transition: transform 0.2s cubic-bezier(0.34, 1.56, 0.64, 1), box-shadow 0.2s ease;
    }
    
    .pixel-box-bounce:hover {
      transform: translateY(-3px);
      box-shadow: 6px 6px 0px #3E3A39;
    }

    .pixel-btn {
      border: 3px solid #3E3A39;
      box-shadow: 3px 3px 0px #3E3A39;
      transition: all 0.15s cubic-bezier(0.34, 1.56, 0.64, 1);
      cursor: pointer;
      font-weight: 700;
      border-radius: 8px;
    }

    .pixel-btn:hover {
      transform: translate(-2px, -3px) scale(1.02);
      box-shadow: 5px 6px 0px #3E3A39;
    }

    .pixel-btn:active {
      transform: translate(2px, 2px) scale(0.98);
      box-shadow: 1px 1px 0px #3E3A39;
    }

    .svg-pixel {
      image-rendering: pixelated;
      image-rendering: crisp-edges;
      transition: transform 0.3s cubic-bezier(0.34, 1.56, 0.64, 1);
    }
    .svg-pixel:hover {
      transform: scale(1.15) rotate(3deg);
    }

    .animal-opt {
      border: 2px solid #3E3A39;
      border-radius: 8px;
      padding: 6px;
      cursor: pointer;
      font-size: 24px;
      text-align: center;
      transition: all 0.2s cubic-bezier(0.34, 1.56, 0.64, 1);
    }
    .animal-opt:hover, .animal-opt.selected {
      background-color: #FAD77B;
      transform: scale(1.1);
    }

    .progress-bar-bg {
      background-color: #FFFDF9;
      border: 2.5px solid #3E3A39;
      border-radius: 9999px;
      overflow: hidden;
      height: 22px;
      box-shadow: 2px 2px 0px #3E3A39;
    }
    .progress-bar-fill {
      background-color: #F4A69A;
      height: 100%;
      transition: width 0.5s ease;
    }

    /* Modal 彈跳動畫 */
    .modal-bounce {
      animation: popIn 0.25s cubic-bezier(0.34, 1.56, 0.64, 1) forwards;
    }
    @keyframes popIn {
      0% { transform: scale(0.85); opacity: 0; }
      100% { transform: scale(1); opacity: 1; }
    }
  </style>
</head>
<body class="pb-16">

  <!-- 飄雪 Canvas 畫布 -->
  <canvas id="snow-canvas"></canvas>

  <!-- 頂部固定導覽列 -->
  <nav class="sticky top-0 z-40 bg-[#EAF4FC] border-b-4 border-[#3E3A39] px-4 py-3 shadow-sm">
    <div class="max-w-4xl mx-auto flex justify-between items-center">
      <div class="flex items-center gap-2">
        <span class="text-2xl animate-bounce">❄️</span>
        <h1 class="font-black text-xl tracking-wide text-[#3E3A39]">北海道大冒險2.0</h1>
      </div>
      <div class="flex items-center gap-1.5 md:gap-2">
        <a href="#flight" class="pixel-btn text-xs px-2.5 py-1.5 bg-[#FAD77B] text-[#3E3A39]">航班</a>
        <a href="#members" class="pixel-btn text-xs px-2.5 py-1.5 bg-[#F4A69A] text-white">快樂夥伴</a>
        <a href="#schedule" class="pixel-btn text-xs px-2.5 py-1.5 bg-[#A4D4EC] text-[#3E3A39]">行程</a>
        <a href="#notes-sec" class="pixel-btn text-xs px-2.5 py-1.5 bg-[#C2E0B4] text-[#3E3A39]">注意事項</a>
      </div>
    </div>
  </nav>

  <!-- Banner 與倒數進度條 -->
  <div class="max-w-4xl mx-auto my-4 p-4">
    <div class="pixel-box pixel-box-bounce bg-[#EAF4FC] p-6 text-center relative overflow-hidden">
      <div class="flex justify-center items-end gap-6 my-3">
        <svg class="svg-pixel cursor-pointer" width="55" height="55" viewBox="0 0 16 16">
          <rect x="5" y="3" width="6" height="11" fill="#3E3A39"/>
          <rect x="6" y="5" width="4" height="8" fill="#FFFDF9"/>
          <rect x="7" y="6" width="1" height="1" fill="#3E3A39"/>
          <rect x="9" y="6" width="1" height="1" fill="#3E3A39"/>
          <rect x="7" y="7" width="2" height="1" fill="#FAD77B"/>
          <rect x="5" y="14" width="2" height="1" fill="#FAD77B"/>
          <rect x="9" y="14" width="2" height="1" fill="#FAD77B"/>
        </svg>
        <svg class="svg-pixel cursor-pointer" width="65" height="65" viewBox="0 0 16 16">
          <rect x="6" y="2" width="4" height="4" fill="#FFFDF9"/>
          <rect x="4" y="6" width="8" height="8" fill="#FFFDF9"/>
          <rect x="6" y="3" width="1" height="1" fill="#3E3A39"/>
          <rect x="9" y="3" width="1" height="1" fill="#3E3A39"/>
          <rect x="7" y="4" width="3" height="1" fill="#F4A69A"/>
          <rect x="4" y="6" width="8" height="2" fill="#F4A69A"/>
          <rect x="7" y="8" width="2" height="2" fill="#3E3A39"/>
        </svg>
        <svg class="svg-pixel cursor-pointer" width="55" height="55" viewBox="0 0 16 16">
          <rect x="5" y="2" width="6" height="5" fill="#F4A69A"/>
          <rect x="6" y="4" width="4" height="3" fill="#EADFD2"/>
          <rect x="7" y="5" width="1" height="1" fill="#3E3A39"/>
          <rect x="9" y="5" width="1" height="1" fill="#3E3A39"/>
          <rect x="4" y="7" width="8" height="6" fill="#A4D4EC"/>
          <rect x="6" y="13" width="1" height="2" fill="#3E3A39"/>
          <rect x="9" y="13" width="1" height="2" fill="#3E3A39"/>
        </svg>
      </div>

      <h2 class="text-3xl font-black tracking-wider text-[#3E3A39] mb-1">HOKKAIDO SNOW TRIP</h2>
      <p class="text-sm inline-block bg-[#FFFDF9] px-4 py-1 border-2 border-[#3E3A39] font-bold rounded-full mb-4">
        📅 2027/01/14 - 2027/01/20 (7 天冒險)
      </p>

      <div class="max-w-md mx-auto bg-[#FFFDF9] border-2 border-[#3E3A39] p-3.5 rounded-xl text-[#3E3A39]">
        <div id="countdown-status" class="font-bold text-sm mb-1.5">⏳ 正在計算出發倒數...</div>
        <div class="progress-bar-bg relative">
          <div id="trip-progress-bar" class="progress-bar-fill" style="width: 0%;"></div>
          <span id="progress-text" class="absolute inset-0 flex items-center justify-center text-xs font-black text-[#3E3A39]">0%</span>
        </div>
      </div>
    </div>
  </div>

  <!-- 航班資訊 -->
  <div id="flight" class="max-w-4xl mx-auto my-6 p-4">
    <div class="pixel-box pixel-box-bounce p-4 bg-[#FFFBF3]">
      <div class="flex justify-between items-center mb-3">
        <h3 class="text-lg font-bold flex items-center gap-2">
          <span>✈️</span> 航班資訊卡片
        </h3>
        <button onclick="openFlightModal()" class="pixel-btn text-xs px-3 py-1 bg-[#FAD77B]">✏️ 編輯航班</button>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-3" id="flight-info-grid"></div>
    </div>
  </div>

  <!-- 成員角色面板 -->
  <div id="members" class="max-w-4xl mx-auto my-6 p-4">
    <div class="flex justify-between items-center mb-3">
      <h3 class="text-xl font-bold flex items-center gap-2">
        <span>👥</span> 4 位冒險成員隊伍
      </h3>
      <span class="text-xs text-gray-500 font-medium">點擊 ✏️ 可自訂動物與姓名</span>
    </div>
    <div class="grid grid-cols-2 md:grid-cols-4 gap-3" id="members-grid"></div>
  </div>

  <!-- 7 天行程時間軸、住宿點與獨立多種運具 -->
  <div id="schedule" class="max-w-4xl mx-auto my-6 p-4">
    <h3 class="text-xl font-bold mb-3 flex items-center gap-2">
      <span>📅</span> 7 天雪地冒險行程表
    </h3>
    
    <div class="flex flex-wrap gap-2 mb-4" id="day-buttons"></div>

    <!-- 每日行程列表 -->
    <div class="pixel-box pixel-box-bounce p-4 bg-[#FFFDF9] mb-4">
      <div class="flex justify-between items-center mb-4">
        <h4 id="current-day-title" class="font-bold text-lg text-[#3E3A39]">Day 1：抵達札幌與熱鬧市區</h4>
        <button onclick="openSpotModal(null)" class="pixel-btn text-xs px-3 py-1 bg-[#C2E0B4]">＋ 新增景點</button>
      </div>
      <div id="schedule-timeline" class="space-y-3"></div>
    </div>

    <!-- 每日住宿點區塊 -->
    <div id="hotel-sec" class="pixel-box pixel-box-bounce p-4 bg-[#EAF4FC] mb-4">
      <div class="flex justify-between items-center mb-2">
        <h4 id="hotel-title" class="text-base font-bold flex items-center gap-2 text-[#3E3A39]">
          <span>🏨</span> Day 1 當日住宿點
        </h4>
        <button onclick="openHotelModal()" class="pixel-btn text-xs px-3 py-1 bg-[#FFFDF9]">✏️ 編輯住宿</button>
      </div>
      <div id="hotel-container" class="text-sm"></div>
    </div>

    <!-- 每日交通重點區 (包含多種獨立運具管理 + 搭乘班次) -->
    <div id="transit" class="pixel-box pixel-box-bounce p-4 bg-[#FEECA6]">
      <div class="mb-4">
        <div class="flex justify-between items-center mb-2">
          <h4 id="transit-modes-title" class="text-base font-bold flex items-center gap-2 text-[#3E3A39]">
            <span>🚌</span> Day 1 主要運具 (可新增多種)
          </h4>
          <button onclick="openModeModal(null)" class="pixel-btn text-xs px-3 py-1 bg-[#FFFDF9]">＋ 新增運具</button>
        </div>
        <div id="modes-container" class="grid grid-cols-1 sm:grid-cols-2 gap-2 text-sm"></div>
      </div>

      <div class="border-t-2 border-[#3E3A39]/20 pt-3">
        <div class="flex justify-between items-center mb-2">
          <h4 id="transit-trains-title" class="text-sm font-bold text-[#3E3A39] flex items-center gap-1">
            <span>🚆</span> 建議搭乘班次 / 時間表
          </h4>
          <button onclick="openTrainModal(null)" class="pixel-btn text-xs px-2.5 py-1 bg-[#FFFDF9]">＋ 新增班次</button>
        </div>
        <div id="trains-container" class="space-y-2 text-sm"></div>
      </div>
    </div>
  </div>

  <!-- 團隊留言板 -->
  <div id="board" class="max-w-4xl mx-auto my-6 p-4">
    <div class="pixel-box pixel-box-bounce p-4 bg-[#FFFDF9]">
      <h3 class="text-xl font-bold mb-3 flex items-center gap-2">
        <span>💬</span> 冒險隊伍留言板
      </h3>
      
      <div class="flex gap-2 mb-3">
        <input type="text" id="board-name" placeholder="名字/暱稱" class="w-28 p-2.5 border-2 border-[#3E3A39] rounded-lg font-bold text-sm outline-none focus:bg-[#FFFBF3]">
        <input type="text" id="board-input" placeholder="寫下給隊友的留言或注意事項..." class="flex-1 p-2.5 border-2 border-[#3E3A39] rounded-lg font-medium text-sm outline-none focus:bg-[#FFFBF3]">
        <button onclick="postComment()" class="pixel-btn px-4 py-2 bg-[#FAD77B] font-bold text-sm">送出留言</button>
      </div>

      <div id="board-comments" class="space-y-3 max-h-72 overflow-y-auto pr-1"></div>
    </div>
  </div>

  <!-- 可編輯注意事項區塊 (支援新增、修改、刪除、圖片) -->
  <div id="notes-sec" class="max-w-4xl mx-auto my-6 p-4">
    <div class="pixel-box pixel-box-bounce p-4 bg-[#FCEFEF]">
      <div class="flex justify-between items-center mb-3">
        <h3 class="text-xl font-bold flex items-center gap-2">
          <span>📌</span> 旅程重要注意事項與備忘錄
        </h3>
        <button onclick="openNoteModal(null)" class="pixel-btn text-xs px-3 py-1 bg-[#F4A69A] text-white">＋ 新增注意事項</button>
      </div>
      <div class="grid grid-cols-1 md:grid-cols-2 gap-3" id="notes-grid"></div>
    </div>
  </div>

  <!-- Modal 1: 成員編輯 Modal -->
  <div id="edit-member-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm p-4 hidden">
    <div class="pixel-box modal-bounce bg-[#FFFDF9] max-w-sm w-full p-5 text-center relative">
      <h3 class="text-lg font-bold mb-3 text-[#3E3A39]">✏️ 編輯隊員資料</h3>
      
      <div class="text-left text-xs font-bold mb-1">選擇代表動物：</div>
      <div class="grid grid-cols-4 gap-2 mb-4" id="animal-options">
        <div class="animal-opt" onclick="pickAnimal('🐧')">🐧</div>
        <div class="animal-opt" onclick="pickAnimal('⛄')">⛄</div>
        <div class="animal-opt" onclick="pickAnimal('🦭')">🦭</div>
        <div class="animal-opt" onclick="pickAnimal('🐰')">🐰</div>
        <div class="animal-opt" onclick="pickAnimal('🦊')">🦊</div>
        <div class="animal-opt" onclick="pickAnimal('🐻')">🐻</div>
        <div class="animal-opt" onclick="pickAnimal('🦉')">🦉</div>
        <div class="animal-opt" onclick="pickAnimal('🦌')">🦌</div>
      </div>

      <div class="text-left text-xs font-bold mb-1">隊員名字：</div>
      <input type="text" id="edit-name-input" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-bold text-sm outline-none">

      <div class="text-left text-xs font-bold mb-1">隊內職責：</div>
      <input type="text" id="edit-role-input" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-4 font-medium text-sm outline-none">

      <div class="flex gap-2">
        <button onclick="closeEditModal()" class="pixel-btn flex-1 py-2 bg-gray-200 text-xs">取消</button>
        <button onclick="saveMemberEdit()" class="pixel-btn flex-1 py-2 bg-[#FAD77B] text-xs font-bold">儲存更新</button>
      </div>
    </div>
  </div>

  <!-- Modal 2: 行程編輯 Modal (包含獨立圖片 URL 輸入與獨立刪除圖片功能) -->
  <div id="edit-spot-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm p-4 hidden">
    <div class="pixel-box modal-bounce bg-[#FFFDF9] max-w-sm w-full p-5 text-center relative">
      <h3 id="spot-modal-title" class="text-lg font-bold mb-3 text-[#3E3A39]">✏️ 修改行程與圖片</h3>
      
      <div class="text-left text-xs font-bold mb-1">預計時間：</div>
      <input type="text" id="spot-time-input" placeholder="例: 15:30" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-bold text-sm outline-none">

      <div class="text-left text-xs font-bold mb-1">景點/活動名稱：</div>
      <input type="text" id="spot-title-input" placeholder="例: 新千歲機場 Check-in" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-bold text-sm outline-none">

      <div class="text-left text-xs font-bold mb-1">行程亮點 / 注意事項 (選填)：</div>
      <textarea id="spot-note-input" placeholder="例: 需提前預約觀光船票、注意保暖衣物..." rows="2" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-medium text-xs outline-none resize-none"></textarea>

      <div class="text-left text-xs font-bold mb-1">景點圖片網址 (選填，包含 https://)：</div>
      <div class="flex gap-1.5 mb-4">
        <input type="text" id="spot-img-input" placeholder="例: https://..." class="flex-1 p-2 border-2 border-[#3E3A39] rounded-lg font-medium text-xs outline-none">
        <button id="delete-spot-img-btn" onclick="clearSpotImgInput()" class="pixel-btn px-2 bg-red-100 text-red-700 text-xs font-bold hidden">🗑️ 刪圖片</button>
      </div>

      <div class="flex gap-2">
        <button onclick="closeSpotModal()" class="pixel-btn flex-1 py-2 bg-gray-200 text-xs">取消</button>
        <button onclick="saveSpotEdit()" class="pixel-btn flex-1 py-2 bg-[#FAD77B] text-xs font-bold">儲存變更</button>
      </div>
    </div>
  </div>

  <!-- Modal 3: 獨立主要運具編輯 Modal -->
  <div id="edit-mode-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm p-4 hidden">
    <div class="pixel-box modal-bounce bg-[#FFFDF9] max-w-sm w-full p-5 text-center relative">
      <h3 id="mode-modal-title" class="text-lg font-bold mb-3 text-[#3E3A39]">🚌 編輯交通運具</h3>
      
      <div class="text-left text-xs font-bold mb-1">運具種類/圖示名稱：</div>
      <input type="text" id="mode-name-input" placeholder="例: JR 特急 Lilac / 路線巴士" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-bold text-sm outline-none">

      <div class="text-left text-xs font-bold mb-1">運具備註 (路線/票券提醒)：</div>
      <input type="text" id="mode-note-input" placeholder="例: 札幌至旭川，建議事先購買指定席" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-4 font-medium text-xs outline-none">

      <div class="flex gap-2">
        <button onclick="closeModeModal()" class="pixel-btn flex-1 py-2 bg-gray-200 text-xs">取消</button>
        <button onclick="saveModeEdit()" class="pixel-btn flex-1 py-2 bg-[#FAD77B] text-xs font-bold">儲存運具</button>
      </div>
    </div>
  </div>

  <!-- Modal 4: 搭乘班次編輯 Modal -->
  <div id="edit-train-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm p-4 hidden">
    <div class="pixel-box modal-bounce bg-[#FFFDF9] max-w-sm w-full p-5 text-center relative">
      <h3 id="train-modal-title" class="text-lg font-bold mb-3 text-[#3E3A39]">🚆 編輯搭乘班次</h3>
      
      <div class="text-left text-xs font-bold mb-1">發車時間：</div>
      <input type="text" id="train-time-input" placeholder="例: 09:30" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-bold text-sm outline-none">

      <div class="text-left text-xs font-bold mb-1">起訖路線：</div>
      <input type="text" id="train-route-input" placeholder="例: JR 札幌站 ➔ JR 小樽站" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-bold text-sm outline-none">

      <div class="text-left text-xs font-bold mb-1">班次備註：</div>
      <input type="text" id="train-note-input" placeholder="例: 搭乘區間快速 (約 45 分鐘)" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-4 font-medium text-xs outline-none">

      <div class="flex gap-2">
        <button onclick="closeTrainModal()" class="pixel-btn flex-1 py-2 bg-gray-200 text-xs">取消</button>
        <button onclick="saveTrainEdit()" class="pixel-btn flex-1 py-2 bg-[#FAD77B] text-xs font-bold">儲存班次</button>
      </div>
    </div>
  </div>

  <!-- Modal 5: 航班資訊編輯 Modal -->
  <div id="edit-flight-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm p-4 hidden">
    <div class="pixel-box modal-bounce bg-[#FFFDF9] max-w-md w-full p-5 text-center relative max-h-[90vh] overflow-y-auto">
      <h3 class="text-lg font-bold mb-3 text-[#3E3A39]">✈️ 編輯航班資訊</h3>
      
      <div class="text-left font-bold text-xs text-[#3E3A39] mb-1">【去程 2027/01/14】</div>
      <input type="text" id="fl-out-no" placeholder="去程班次 (例: 星宇 JX850)" class="w-full p-2 border-2 border-[#3E3A39] rounded mb-2 text-xs font-bold">
      <input type="text" id="fl-out-route" placeholder="路線 (例: 桃園 T1 ➔ 札幌新千歲 T2)" class="w-full p-2 border-2 border-[#3E3A39] rounded mb-2 text-xs">
      <input type="text" id="fl-out-time" placeholder="時間 (例: 09:25 起飛 - 14:00 抵達)" class="w-full p-2 border-2 border-[#3E3A39] rounded mb-4 text-xs">

      <div class="text-left font-bold text-xs text-[#3E3A39] mb-1">【回程 2027/01/20】</div>
      <input type="text" id="fl-in-no" placeholder="回程班次 (例: 星宇 JX851)" class="w-full p-2 border-2 border-[#3E3A39] rounded mb-2 text-xs font-bold">
      <input type="text" id="fl-in-route" placeholder="路線 (例: 札幌新千歲 T2 ➔ 桃園 T1)" class="w-full p-2 border-2 border-[#3E3A39] rounded mb-2 text-xs">
      <input type="text" id="fl-in-time" placeholder="時間 (例: 15:15 起飛 - 18:50 抵達)" class="w-full p-2 border-2 border-[#3E3A39] rounded mb-4 text-xs">

      <div class="flex gap-2">
        <button onclick="closeFlightModal()" class="pixel-btn flex-1 py-2 bg-gray-200 text-xs">取消</button>
        <button onclick="saveFlightEdit()" class="pixel-btn flex-1 py-2 bg-[#FAD77B] text-xs font-bold">儲存航班</button>
      </div>
    </div>
  </div>

  <!-- Modal 6: 住宿編輯 Modal -->
  <div id="edit-hotel-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm p-4 hidden">
    <div class="pixel-box modal-bounce bg-[#FFFDF9] max-w-sm w-full p-5 text-center relative">
      <h3 class="text-lg font-bold mb-3 text-[#3E3A39]">🏨 編輯當日住宿點</h3>
      
      <div class="text-left text-xs font-bold mb-1">飯店名稱：</div>
      <input type="text" id="hotel-name-input" placeholder="例: 札幌大通蒙特利酒店" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-bold text-sm outline-none">

      <div class="text-left text-xs font-bold mb-1">飯店備註 / Check-in 提醒：</div>
      <input type="text" id="hotel-note-input" placeholder="例: 15:00 可辦理入住，近大通站 3 號出口" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-4 font-medium text-xs outline-none">

      <div class="flex gap-2">
        <button onclick="closeHotelModal()" class="pixel-btn flex-1 py-2 bg-gray-200 text-xs">取消</button>
        <button onclick="saveHotelEdit()" class="pixel-btn flex-1 py-2 bg-[#FAD77B] text-xs font-bold">儲存住宿</button>
      </div>
    </div>
  </div>

  <!-- Modal 7: 注意事項編輯 Modal -->
  <div id="edit-note-modal" class="fixed inset-0 z-50 flex items-center justify-center bg-black/50 backdrop-blur-sm p-4 hidden">
    <div class="pixel-box modal-bounce bg-[#FFFDF9] max-w-sm w-full p-5 text-center relative">
      <h3 id="note-modal-title" class="text-lg font-bold mb-3 text-[#3E3A39]">📌 編輯注意事項</h3>
      
      <div class="text-left text-xs font-bold mb-1">標題 / 主旨：</div>
      <input type="text" id="note-title-input" placeholder="例: 冬季保暖裝備與穿搭" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-bold text-sm outline-none">

      <div class="text-left text-xs font-bold mb-1">詳細說明：</div>
      <textarea id="note-content-input" placeholder="例: 建議穿著洋蔥式穿法，配備發熱衣、毛帽、防滑雪靴..." rows="3" class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-3 font-medium text-xs outline-none resize-none"></textarea>

      <div class="text-left text-xs font-bold mb-1">圖片網址 (選填，包含 https://)：</div>
      <input type="text" id="note-img-input" placeholder="例: https://images.unsplash.com/..." class="w-full p-2 border-2 border-[#3E3A39] rounded-lg mb-4 font-medium text-xs outline-none">

      <div class="flex gap-2">
        <button onclick="closeNoteModal()" class="pixel-btn flex-1 py-2 bg-gray-200 text-xs">取消</button>
        <button onclick="saveNoteEdit()" class="pixel-btn flex-1 py-2 bg-[#F4A69A] text-white text-xs font-bold">儲存卡片</button>
      </div>
    </div>
  </div>

  <script>
    // 1. 全頁 Canvas 飄雪動效
    function initSnowCanvas() {
      const canvas = document.getElementById('snow-canvas');
      const ctx = canvas.getContext('2d');

      let width = window.innerWidth;
      let height = window.innerHeight;
      canvas.width = width;
      canvas.height = height;

      window.addEventListener('resize', () => {
        width = window.innerWidth;
        height = window.innerHeight;
        canvas.width = width;
        canvas.height = height;
      });

      const numFlakes = 65;
      const flakes = [];

      for (let i = 0; i < numFlakes; i++) {
        flakes.push({
          x: Math.random() * width,
          y: Math.random() * height,
          r: Math.random() * 3 + 1.5,
          speed: Math.random() * 1.2 + 0.5,
          swing: Math.random() * 0.03,
          swingStep: Math.random() * Math.PI * 2,
          opacity: Math.random() * 0.7 + 0.3
        });
      }

      function draw() {
        ctx.clearRect(0, 0, width, height);
        ctx.fillStyle = '#FFFFFF';

        flakes.forEach(f => {
          f.y += f.speed;
          f.swingStep += f.swing;
          f.x += Math.sin(f.swingStep) * 0.6;

          if (f.y > height) {
            f.y = -10;
            f.x = Math.random() * width;
          }

          ctx.beginPath();
          ctx.arc(f.x, f.y, f.r, 0, Math.PI * 2);
          ctx.globalAlpha = f.opacity;
          ctx.fill();
        });

        requestAnimationFrame(draw);
      }
      draw();
    }

    // 時間設定 (2027/01/14 - 2027/01/20)
    const startDate = new Date("2027-01-14T00:00:00");
    const endDate = new Date("2027-01-20T23:59:59");

    const defaultFlightData = {
      outbound: { date: "2027/01/14 (四)", no: "星宇航空 JX850", route: "桃園國際機場 (T1) ➔ 札幌新千歲 (INT)", time: "09:25 起飛 ➔ 14:00 抵達" },
      inbound: { date: "2027/01/20 (三)", no: "星宇航空 JX851", route: "札幌新千歲 (INT) ➔ 桃園國際機場 (T1)", time: "15:15 起飛 ➔ 18:50 抵達" }
    };
    let flightData = JSON.parse(localStorage.getItem('hokkaido_flight')) || defaultFlightData;

    let defaultMembers = [
      { id: 0, animal: "🐧", name: "小明", role: "隊長 / 行程總指揮", bg: "#FFFBF3" },
      { id: 1, animal: "⛄", name: "小美", role: "總務與記帳", bg: "#FCEFEF" },
      { id: 2, animal: "🦭", name: "阿傑", role: "美食指南 & 攝影", bg: "#EAF4FC" },
      { id: 3, animal: "🐰", name: "小雅", role: "交通路線導航", bg: "#C2E0B4" }
    ];
    let members = JSON.parse(localStorage.getItem('hokkaido_members')) || defaultMembers;
    let editingMemberId = null;
    let selectedAnimalIcon = "🐧";

    // 每日住宿預設資料
    const defaultHotels = {
      1: { name: "札幌大通蒙特利酒店", note: "大通站 3 號出口，15:00 可 Check-in 存放行李" },
      2: { name: "小樽運河索尼婭酒店 (Hotel Sonia Otaru)", note: "緊鄰小樽運河，附設頂樓天然溫泉大浴場" },
      3: { name: "旭川 OMO7 星野集團飯店", note: "旭川市中心，提供動物園導覽與免費接駁" },
      4: { name: "新富良野王子大飯店", note: "飯店直通精靈露台，滑雪場直達溫泉" },
      5: { name: "洞爺湖萬世閣湖畔 Spa 飯店", note: "全館湖景溫泉房，含豐富自助晚餐與溫泉體驗" },
      6: { name: "札幌 JR 塔日航酒店", note: "直通 JR 札幌站，高層夜景 Spa 體驗" },
      7: { name: "溫馨的家 (旅程結束返家)", note: "回程航班搭乘前記得在機場退稅與採買" }
    };
    let hotelsData = JSON.parse(localStorage.getItem('hokkaido_hotels')) || defaultHotels;

    // 預設 7 天行程與獨立多運具
    const defaultTripData = {
      1: {
        title: "Day 1：抵達札幌",
        modes: [
          { id: 101, name: "JR 快速 Airport 號", note: "新千歲機場 ➔ 札幌站 (約 37 分鐘)" },
          { id: 102, name: "札幌地下鐵", note: "南北線/東西線 (市區移動)" }
        ],
        trains: [
          { id: 201, time: "14:18", route: "新千歲機場 ➔ 札幌車站", note: "搭乘快速エアポート號" },
          { id: 202, time: "18:00", route: "飯店 ➔ 大通公園/電視塔", note: "地下步行空間前往" }
        ],
        spots: [
          { id: 301, time: "15:30", title: "新千歲機場 ➔ 札幌飯店 Check-in", note: "先存放行李再輕鬆出門散步", img: "" },
          { id: 302, time: "18:00", title: "札幌電視塔與大通公園燈飾觀賞", note: "頂部觀景台可俯瞰雪景燈光秀", img: "https://images.unsplash.com/photo-1542051841857-5f90071e7989?w=500&auto=format&fit=crop&q=60" },
          { id: 303, time: "19:30", title: "元祖札幌拉麵橫丁 (享用北海道味噌拉麵)", note: "熱門名店人潮較多，建議預留排隊時間", img: "" }
        ]
      },
      2: {
        title: "Day 2：北邊一日遊",
        modes: [
          { id: 103, name: "JR 函館本線", note: "區間快速 (札幌 ➔ 小樽)" },
          { id: 104, name: "徒步散冊", note: "小樽運河與周邊商圈" }
        ],
        trains: [
          { id: 203, time: "09:30", route: "JR 札幌站 ➔ JR 小樽站", note: "搭乘區間快速 (約 45 分鐘)" },
          { id: 204, time: "20:00", route: "JR 小樽站 ➔ JR 札幌站", note: "返程返回札幌飯店" }
        ],
        spots: [
          { id: 304, time: "10:30", title: "小樽運河散策與童話十字路口", note: "沿途石造倉庫群超好拍照", img: "https://images.unsplash.com/photo-1578637387939-43c525550085?w=500&auto=format&fit=crop&q=60" },
          { id: 305, time: "13:00", title: "北一硝子館與音樂盒堂購物", note: "注意玻璃製品易碎，結帳可退稅", img: "" },
          { id: 306, time: "17:30", title: "小樽雪燈之路賞夜景與海鮮丼饗宴", note: "夜晚氣溫較低，防寒保暖要做好！", img: "" }
        ]
      },
      3: {
        title: "Day 3：夢迴小樽",
        modes: [
          { id: 105, name: "JR 特急 Lilac 號", note: "札幌 ➔ 旭川 (約 1 小時 25 分)" },
          { id: 106, name: "旭川路線巴士", note: "旭川站前 ➔ 旭山動物園" }
        ],
        trains: [
          { id: 205, time: "07:49", route: "JR 札幌站 ➔ JR 旭川站", note: "特急ライラック號" },
          { id: 206, time: "09:40", route: "旭川站前 ➔ 旭山動物園", note: "旭川電氣軌道巴士 (41/47號)" }
        ],
        spots: [
          { id: 307, time: "10:30", title: "旭山動物園觀賞「企鵝冬季散步」", note: "企鵝遊行每天兩場（11:00 / 14:30），建議提早 20 分鐘佔位", img: "" },
          { id: 308, time: "14:00", title: "旭川拉麵村享用醬油拉麵", note: "品嚐濃郁黑醬油高湯麵", img: "" }
        ]
      },
      4: {
        title: "Day 4：羊蹄山頑皮鬼",
        modes: [
          { id: 107, name: "JR 富良野線", note: "旭川 ➔ 美瑛" },
          { id: 108, name: "美瑛觀光巴士 / 包車", note: "景點接駁" }
        ],
        trains: [
          { id: 207, time: "09:00", route: "旭川 ➔ 美瑛站", note: "JR 富良野線普通列車" }
        ],
        spots: [
          { id: 309, time: "11:00", title: "白金青池與白鬚瀑布點燈", note: "冬季青池水面結冰積雪，點燈時刻極夢幻", img: "" },
          { id: 310, time: "17:00", title: "富良野精靈露台手作森林小屋", note: "森林木屋小路防滑鞋必備", img: "" }
        ]
      },
      5: {
        title: "Day 5：函館喝剩",
        modes: [
          { id: 109, name: "JR 特急 Hokuto (北斗號)", note: "札幌 ➔ 洞爺 (約 1 小時 50 分)" },
          { id: 110, name: "道南巴士", note: "洞爺站 ➔ 洞爺湖溫泉街" },
          { id: 111, name: "有珠山纜車", note: "登上山頂觀景台" }
        ],
        trains: [
          { id: 208, time: "08:39", route: "JR 札幌站 ➔ JR 洞爺站", note: "特急北斗號" }
        ],
        spots: [
          { id: 311, time: "11:30", title: "洞爺湖筒倉展望台觀賞湖光雪景", note: "可俯瞰洞爺湖全景與羊蹄山 snow view", img: "" },
          { id: 312, time: "14:00", title: "洞爺湖中島觀光船與湖畔溫泉街散策", note: "搭乘城堡形觀光船", img: "" }
        ]
      },
      6: {
        title: "Day 6：湯之川",
        modes: [
          { id: 112, name: "札幌地下鐵全線 Pass", note: "南北線 / 東西線 / 東豐線" },
          { id: 113, name: "札幌路面電車 (市電)", note: "藻岩山與市區環狀線" }
        ],
        trains: [
          { id: 209, time: "10:00", route: "飯店 ➔ 北海道神宮", note: "地下鐵東西線 (圓山公園站)" }
        ],
        spots: [
          { id: 313, time: "10:30", title: "北海道神宮參拜與雪景雪麻糬", note: "可購買限定判官樣麻糬與御守", img: "" },
          { id: 314, time: "18:00", title: "狸小路商店街與藥妝大採購", note: "準備好護照享有免稅折扣", img: "" }
        ]
      },
      7: {
        title: "Day 7：歡迎歸國",
        modes: [
          { id: 114, name: "JR 快速 Airport 號", note: "札幌站 ➔ 新千歲機場" }
        ],
        trains: [
          { id: 210, time: "11:30", route: "札幌車站 ➔ 新千歲機場", note: "提前 2.5 小時抵達機場採購伴手禮" }
        ],
        spots: [
          { id: 315, time: "08:30", title: "二条市場享用豪華帝王蟹鮭魚卵海鮮丼", note: "品嚐早晨新鮮海產", img: "" },
          { id: 316, time: "12:00", title: "新千歲機場甜點街道與免稅店購物", note: "多保留時間逛機場拉麵道場", img: "" }
        ]
      }
    };
    let tripData = JSON.parse(localStorage.getItem('hokkaido_trip_data')) || defaultTripData;

    // 注意事項
    const defaultNotes = [
      {
        id: 1,
        title: "🧤 必備冬季衣物與防潮裝備",
        content: "採用洋蔥式穿法，外層需防風防水羽絨外套。手套、毛帽、圍巾必備，鞋子請加上防滑冰爪。",
        img: "https://images.unsplash.com/photo-1517048676732-d65bc937f952?w=500&auto=format&fit=crop&q=60"
      },
      {
        id: 2,
        title: "📱 電子設備低溫防耗電提醒",
        content: "雪地低溫手機電池消耗極快，隨身暖暖包可與行動電源一同放在口袋保溫。",
        img: ""
      }
    ];
    let notesData = JSON.parse(localStorage.getItem('hokkaido_notes')) || defaultNotes;

    // 操作暫存 ID
    let currentDay = 1;
    let editingSpotId = null;
    let editingModeId = null;
    let editingTrainId = null;
    let editingNoteId = null;

    function saveTripData() {
      localStorage.setItem('hokkaido_trip_data', JSON.stringify(tripData));
    }

    function updateCountdownAndProgress() {
      const now = new Date();
      const statusEl = document.getElementById('countdown-status');
      const barFillEl = document.getElementById('trip-progress-bar');
      const textEl = document.getElementById('progress-text');

      if (now < startDate) {
        const diff = startDate - now;
        const days = Math.floor(diff / (1000 * 60 * 60 * 24));
        const hours = Math.floor((diff / (1000 * 60 * 60)) % 24);
        const minutes = Math.floor((diff / (1000 * 60)) % 60);
        const seconds = Math.floor((diff / 1000) % 60);

        statusEl.innerText = `⏳ 距離 2027 北海道出發還有：${days} 天 ${hours} 小時 ${minutes} 分 ${seconds} 秒`;
        barFillEl.style.width = '0%';
        textEl.innerText = `哇庫哇酷 (0%)`;
      } else if (now >= startDate && now <= endDate) {
        const totalDuration = endDate - startDate;
        const elapsed = now - startDate;
        const progressPercent = Math.min(100, Math.max(0, (elapsed / totalDuration) * 100)).toFixed(1);

        statusEl.innerText = `🎉 北海道雪國冒險進行中！黑皮黑皮 ❄️`;
        barFillEl.style.width = `${progressPercent}%`;
        textEl.innerText = `旅程進度 ${progressPercent}%`;
      } else {
        statusEl.innerText = `❄️ 2027 北海道冒險圓滿完成！社畜回歸 🏠`;
        barFillEl.style.width = '100%';
        barFillEl.style.backgroundColor = '#C2E0B4';
        textEl.innerText = `已完成 (100%)`;
      }
    }

    function renderFlightInfo() {
      const container = document.getElementById('flight-info-grid');
      container.innerHTML = `
        <div class="p-3 border-2 border-[#3E3A39] bg-[#FFFDF9] rounded-lg shadow-sm">
          <div class="flex justify-between items-center mb-1">
            <span class="font-bold text-xs px-2 py-0.5 bg-[#F4A69A] text-white rounded border border-[#3E3A39]">去程 Flight</span>
            <span class="text-xs font-bold text-gray-500">${flightData.outbound.date}</span>
          </div>
          <div class="font-bold text-base text-[#3E3A39] mb-1">${flightData.outbound.no}</div>
          <div class="text-xs font-medium text-gray-700">${flightData.outbound.route}</div>
          <div class="text-xs font-bold text-[#4A90E2] mt-1">🕒 ${flightData.outbound.time}</div>
        </div>

        <div class="p-3 border-2 border-[#3E3A39] bg-[#FFFDF9] rounded-lg shadow-sm">
          <div class="flex justify-between items-center mb-1">
            <span class="font-bold text-xs px-2 py-0.5 bg-[#A4D4EC] text-[#3E3A39] rounded border border-[#3E3A39]">回程 Flight</span>
            <span class="text-xs font-bold text-gray-500">${flightData.inbound.date}</span>
          </div>
          <div class="font-bold text-base text-[#3E3A39] mb-1">${flightData.inbound.no}</div>
          <div class="text-xs font-medium text-gray-700">${flightData.inbound.route}</div>
          <div class="text-xs font-bold text-[#4A90E2] mt-1">🕒 ${flightData.inbound.time}</div>
        </div>
      `;
    }

    function openFlightModal() {
      document.getElementById('fl-out-no').value = flightData.outbound.no;
      document.getElementById('fl-out-route').value = flightData.outbound.route;
      document.getElementById('fl-out-time').value = flightData.outbound.time;
      document.getElementById('fl-in-no').value = flightData.inbound.no;
      document.getElementById('fl-in-route').value = flightData.inbound.route;
      document.getElementById('fl-in-time').value = flightData.inbound.time;
      document.getElementById('edit-flight-modal').classList.remove('hidden');
    }

    function closeFlightModal() {
      document.getElementById('edit-flight-modal').classList.add('hidden');
    }

    function saveFlightEdit() {
      flightData.outbound.no = document.getElementById('fl-out-no').value.trim() || defaultFlightData.outbound.no;
      flightData.outbound.route = document.getElementById('fl-out-route').value.trim() || defaultFlightData.outbound.route;
      flightData.outbound.time = document.getElementById('fl-out-time').value.trim() || defaultFlightData.outbound.time;
      
      flightData.inbound.no = document.getElementById('fl-in-no').value.trim() || defaultFlightData.inbound.no;
      flightData.inbound.route = document.getElementById('fl-in-route').value.trim() || defaultFlightData.inbound.route;
      flightData.inbound.time = document.getElementById('fl-in-time').value.trim() || defaultFlightData.inbound.time;

      localStorage.setItem('hokkaido_flight', JSON.stringify(flightData));
      renderFlightInfo();
      closeFlightModal();
    }

    function renderMembers() {
      const container = document.getElementById('members-grid');
      container.innerHTML = members.map(m => `
        <div class="pixel-box p-3 text-center relative" style="background-color: ${m.bg}">
          <button onclick="openEditModal(${m.id})" class="absolute top-2 right-2 text-xs bg-white/80 border border-[#3E3A39] px-1.5 py-0.5 rounded font-bold hover:bg-white">✏️</button>
          <div class="text-3xl mb-1">${m.animal}</div>
          <div class="font-bold text-sm text-[#3E3A39]">${m.name}</div>
          <div class="text-xs text-gray-500 font-medium mt-0.5">${m.role}</div>
        </div>
      `).join('');
    }

    function openEditModal(id) {
      editingMemberId = id;
      const member = members.find(m => m.id === id);
      selectedAnimalIcon = member.animal;
      document.getElementById('edit-name-input').value = member.name;
      document.getElementById('edit-role-input').value = member.role;
      highlightAnimal(selectedAnimalIcon);
      document.getElementById('edit-member-modal').classList.remove('hidden');
    }

    function closeEditModal() {
      document.getElementById('edit-member-modal').classList.add('hidden');
    }

    function pickAnimal(icon) {
      selectedAnimalIcon = icon;
      highlightAnimal(icon);
    }

    function highlightAnimal(icon) {
      document.querySelectorAll('.animal-opt').forEach(opt => {
        if(opt.innerText === icon) opt.classList.add('selected');
        else opt.classList.remove('selected');
      });
    }

    function saveMemberEdit() {
      if(editingMemberId !== null) {
        const name = document.getElementById('edit-name-input').value.trim() || "成員";
        const role = document.getElementById('edit-role-input').value.trim() || "冒險隊員";
        const index = members.findIndex(m => m.id === editingMemberId);
        if(index !== -1) {
          members[index].animal = selectedAnimalIcon;
          members[index].name = name;
          members[index].role = role;
          localStorage.setItem('hokkaido_members', JSON.stringify(members));
          renderMembers();
        }
      }
      closeEditModal();
    }

    // 渲染 Day 視圖
    function updateDayView(day) {
      currentDay = day;
      const data = tripData[day];
      
      document.getElementById('current-day-title').innerText = data.title;
      renderDayButtons();

      // 1. 每日行程時間軸 (包含獨立景點圖片及獨立圖片刪除)
      const timeline = document.getElementById('schedule-timeline');
      if(!data.spots || data.spots.length === 0) {
        timeline.innerHTML = `<div class="text-xs text-gray-400 text-center py-4">本日尚無行程，點擊右上角「＋ 新增景點」來新增吧！</div>`;
      } else {
        timeline.innerHTML = data.spots.map(s => `
          <div class="p-3 border-2 border-[#3E3A39] bg-[#FFFDF9] rounded-lg shadow-sm">
            <div class="flex justify-between items-center mb-1">
              <div class="flex items-center gap-2">
                <span class="px-2 py-0.5 rounded bg-[#A4D4EC] border border-[#3E3A39] text-xs font-bold text-[#3E3A39]">
                  ${s.time}
                </span>
                <div class="font-bold text-base text-[#3E3A39]">${s.title}</div>
              </div>
              <div class="flex items-center gap-1.5">
                <button onclick="openSpotModal(${s.id})" class="text-xs bg-gray-100 hover:bg-gray-200 border border-[#3E3A39] px-2 py-1 rounded font-bold">✏️ 編輯</button>
                <button onclick="deleteSpot(${s.id})" class="text-xs bg-red-100 hover:bg-red-200 border border-[#3E3A39] px-2 py-1 rounded font-bold">🗑️ 刪除</button>
              </div>
            </div>

            ${s.note && s.note.trim() !== '' ? `
              <div class="mt-2 text-xs bg-[#FFFBF3] p-2 rounded border border-[#3E3A39]/30 text-gray-600 flex items-start gap-1 font-medium">
                <span class="text-amber-500 font-bold flex-shrink-0">✨ 亮點/備註:</span>
                <span>${s.note}</span>
              </div>
            ` : ''}

            ${s.img && s.img.trim() !== '' ? `
              <div class="mt-2.5 overflow-hidden rounded-lg border-2 border-[#3E3A39] max-h-48 relative group">
                <img src="${s.img}" alt="${s.title} 圖片" class="w-full h-full object-cover">
                <button onclick="deleteSpotImgDirect(${s.id})" class="absolute top-2 right-2 bg-red-600 text-white text-xs px-2 py-1 rounded border border-white font-bold opacity-90 hover:opacity-100 shadow">
                  🗑️ 刪除圖片
                </button>
              </div>
            ` : ''}
          </div>
        `).join('');
      }

      // 2. 當日住宿點
      document.getElementById('hotel-title').innerHTML = `<span>🏨</span> Day ${day} 當日住宿點`;
      const hotelBox = document.getElementById('hotel-container');
      const curHotel = hotelsData[day] || { name: "未設定住宿飯店", note: "點擊右上角按鈕即可新增飯店資訊" };
      hotelBox.innerHTML = `
        <div class="p-2.5 border-2 border-[#3E3A39] bg-[#FFFDF9] rounded-lg">
          <div class="font-bold text-[#3E3A39] text-base">${curHotel.name}</div>
          <div class="text-xs text-gray-500 font-medium mt-0.5">${curHotel.note}</div>
        </div>
      `;

      // 3. 多種獨立交通運具卡片 (每一種都有獨立空間，可單獨編輯/刪除)
      document.getElementById('transit-modes-title').innerHTML = `<span>🚌</span> Day ${day} 主要運具 (${data.modes ? data.modes.length : 0} 種)`;
      const modesBox = document.getElementById('modes-container');
      if(!data.modes || data.modes.length === 0) {
        modesBox.innerHTML = `<div class="text-xs text-gray-500 py-2 col-span-2">尚無運具，點擊右上角「＋ 新增運具」來新增交通方式吧！</div>`;
      } else {
        modesBox.innerHTML = data.modes.map(m => `
          <div class="p-2.5 border-2 border-[#3E3A39] bg-[#FFFDF9] rounded-lg flex justify-between items-center shadow-sm">
            <div>
              <div class="font-bold text-[#3E3A39] text-sm">🚆 ${m.name}</div>
              <div class="text-xs text-gray-500 font-medium mt-0.5">${m.note || '主要交通搭乘'}</div>
            </div>
            <div class="flex items-center gap-1">
              <button onclick="openModeModal(${m.id})" class="text-xs bg-gray-100 hover:bg-gray-200 border border-[#3E3A39] px-1.5 py-0.5 rounded font-bold">✏️</button>
              <button onclick="deleteMode(${m.id})" class="text-xs bg-red-100 hover:bg-red-200 border border-[#3E3A39] px-1.5 py-0.5 rounded font-bold">🗑️</button>
            </div>
          </div>
        `).join('');
      }

      // 4. 搭乘班次時間表 (獨立編輯/刪除)
      document.getElementById('transit-trains-title').innerHTML = `<span>🚆</span> Day ${day} 建議搭乘班次 / 時間表`;
      const trainsBox = document.getElementById('trains-container');
      if(!data.trains || data.trains.length === 0) {
        trainsBox.innerHTML = `<div class="text-xs text-gray-500 py-1">尚無建議班次。</div>`;
      } else {
        trainsBox.innerHTML = data.trains.map(t => `
          <div class="p-2 border-2 border-[#3E3A39] bg-[#FFFDF9] flex justify-between items-center rounded-lg">
            <div class="flex items-center gap-2">
              <span class="font-bold px-2 py-0.5 bg-[#A4D4EC] border border-[#3E3A39] text-xs rounded">${t.time}</span>
              <span class="font-medium text-[#3E3A39] text-xs">${t.route}</span>
              <span class="text-xs text-gray-500 font-medium hidden sm:inline">(${t.note})</span>
            </div>
            <div class="flex items-center gap-1">
              <button onclick="openTrainModal(${t.id})" class="text-xs bg-gray-100 hover:bg-gray-200 border border-[#3E3A39] px-1.5 py-0.5 rounded font-bold">✏️</button>
              <button onclick="deleteTrain(${t.id})" class="text-xs bg-red-100 hover:bg-red-200 border border-[#3E3A39] px-1.5 py-0.5 rounded font-bold">🗑️</button>
            </div>
          </div>
        `).join('');
      }
    }

    function renderDayButtons() {
      const btnContainer = document.getElementById('day-buttons');
      btnContainer.innerHTML = '';
      for(let i=1; i<=7; i++) {
        const btn = document.createElement('button');
        btn.className = `pixel-btn text-xs px-3 py-1.5 ${i === currentDay ? 'bg-[#FAD77B]' : 'bg-[#FFFDF9]'}`;
        btn.innerText = `Day ${i}`;
        btn.onclick = () => updateDayView(i);
        btnContainer.appendChild(btn);
      }
    }

    // --- 行程 modal 邏輯 ---
    function openSpotModal(id) {
      editingSpotId = id;
      const deleteImgBtn = document.getElementById('delete-spot-img-btn');
      
      if(id !== null) {
        const spot = tripData[currentDay].spots.find(s => s.id === id);
        document.getElementById('spot-modal-title').innerText = "✏️ 修改行程與圖片";
        document.getElementById('spot-time-input').value = spot.time;
        document.getElementById('spot-title-input').value = spot.title;
        document.getElementById('spot-note-input').value = spot.note || "";
        document.getElementById('spot-img-input').value = spot.img || "";
        if(spot.img && spot.img.trim() !== '') deleteImgBtn.classList.remove('hidden');
        else deleteImgBtn.classList.add('hidden');
      } else {
        document.getElementById('spot-modal-title').innerText = "＋ 新增行程";
        document.getElementById('spot-time-input').value = "14:00";
        document.getElementById('spot-title-input').value = "";
        document.getElementById('spot-note-input').value = "";
        document.getElementById('spot-img-input').value = "";
        deleteImgBtn.classList.add('hidden');
      }
      document.getElementById('edit-spot-modal').classList.remove('hidden');
    }

    function closeSpotModal() {
      document.getElementById('edit-spot-modal').classList.add('hidden');
    }

    function clearSpotImgInput() {
      document.getElementById('spot-img-input').value = "";
      document.getElementById('delete-spot-img-btn').classList.add('hidden');
    }

    function deleteSpotImgDirect(id) {
      if(confirm("確定要刪除這張行程圖片嗎？")) {
        const spot = tripData[currentDay].spots.find(s => s.id === id);
        if(spot) {
          spot.img = "";
          saveTripData();
          updateDayView(currentDay);
        }
      }
    }

    function saveSpotEdit() {
      const time = document.getElementById('spot-time-input').value.trim() || "全天";
      const title = document.getElementById('spot-title-input').value.trim() || "行程項目";
      const note = document.getElementById('spot-note-input').value.trim();
      const img = document.getElementById('spot-img-input').value.trim();

      if(!tripData[currentDay].spots) tripData[currentDay].spots = [];

      if(editingSpotId !== null) {
        const spot = tripData[currentDay].spots.find(s => s.id === editingSpotId);
        if(spot) {
          spot.time = time;
          spot.title = title;
          spot.note = note;
          spot.img = img;
        }
      } else {
        const newSpot = { id: Date.now(), time, title, note, img };
        tripData[currentDay].spots.push(newSpot);
      }
      
      saveTripData();
      updateDayView(currentDay);
      closeSpotModal();
    }

    function deleteSpot(id) {
      if(confirm("確定要刪除這項行程嗎？")) {
        tripData[currentDay].spots = tripData[currentDay].spots.filter(s => s.id !== id);
        saveTripData();
        updateDayView(currentDay);
      }
    }

    // --- 獨立交通運具 Modal 邏輯 ---
    function openModeModal(id) {
      editingModeId = id;
      if(id !== null) {
        const mode = tripData[currentDay].modes.find(m => m.id === id);
        document.getElementById('mode-modal-title').innerText = "🚌 修改交通運具";
        document.getElementById('mode-name-input').value = mode.name;
        document.getElementById('mode-note-input').value = mode.note || "";
      } else {
        document.getElementById('mode-modal-title').innerText = "＋ 新增交通運具";
        document.getElementById('mode-name-input').value = "";
        document.getElementById('mode-note-input').value = "";
      }
      document.getElementById('edit-mode-modal').classList.remove('hidden');
    }

    function closeModeModal() {
      document.getElementById('edit-mode-modal').classList.add('hidden');
    }

    function saveModeEdit() {
      const name = document.getElementById('mode-name-input').value.trim() || "自訂交通方式";
      const note = document.getElementById('mode-note-input').value.trim() || "";

      if(!tripData[currentDay].modes) tripData[currentDay].modes = [];

      if(editingModeId !== null) {
        const mode = tripData[currentDay].modes.find(m => m.id === editingModeId);
        if(mode) {
          mode.name = name;
          mode.note = note;
        }
      } else {
        tripData[currentDay].modes.push({ id: Date.now(), name, note });
      }

      saveTripData();
      updateDayView(currentDay);
      closeModeModal();
    }

    function deleteMode(id) {
      if(confirm("確定要刪除此項交通運具嗎？")) {
        tripData[currentDay].modes = tripData[currentDay].modes.filter(m => m.id !== id);
        saveTripData();
        updateDayView(currentDay);
      }
    }

    // --- 搭乘班次 Modal 邏輯 ---
    function openTrainModal(id) {
      editingTrainId = id;
      if(id !== null) {
        const train = tripData[currentDay].trains.find(t => t.id === id);
        document.getElementById('train-modal-title').innerText = "🚆 修改搭乘班次";
        document.getElementById('train-time-input').value = train.time;
        document.getElementById('train-route-input').value = train.route;
        document.getElementById('train-note-input').value = train.note || "";
      } else {
        document.getElementById('train-modal-title').innerText = "＋ 新增搭乘班次";
        document.getElementById('train-time-input').value = "09:30";
        document.getElementById('train-route-input').value = "";
        document.getElementById('train-note-input').value = "";
      }
      document.getElementById('edit-train-modal').classList.remove('hidden');
    }

    function closeTrainModal() {
      document.getElementById('edit-train-modal').classList.add('hidden');
    }

    function saveTrainEdit() {
      const time = document.getElementById('train-time-input').value.trim() || "預定班次";
      const route = document.getElementById('train-route-input').value.trim() || "起訖路線";
      const note = document.getElementById('train-note-input').value.trim() || "";

      if(!tripData[currentDay].trains) tripData[currentDay].trains = [];

      if(editingTrainId !== null) {
        const train = tripData[currentDay].trains.find(t => t.id === editingTrainId);
        if(train) {
          train.time = time;
          train.route = route;
          train.note = note;
        }
      } else {
        tripData[currentDay].trains.push({ id: Date.now(), time, route, note });
      }

      saveTripData();
      updateDayView(currentDay);
      closeTrainModal();
    }

    function deleteTrain(id) {
      if(confirm("確定要刪除這班班次資訊嗎？")) {
        tripData[currentDay].trains = tripData[currentDay].trains.filter(t => t.id !== id);
        saveTripData();
        updateDayView(currentDay);
      }
    }

    // --- 住宿 modal 邏輯 ---
    function openHotelModal() {
      const curHotel = hotelsData[currentDay] || { name: "", note: "" };
      document.getElementById('hotel-name-input').value = curHotel.name;
      document.getElementById('hotel-note-input').value = curHotel.note;
      document.getElementById('edit-hotel-modal').classList.remove('hidden');
    }

    function closeHotelModal() {
      document.getElementById('edit-hotel-modal').classList.add('hidden');
    }

    function saveHotelEdit() {
      const name = document.getElementById('hotel-name-input').value.trim() || "未設定飯店";
      const note = document.getElementById('hotel-note-input').value.trim() || "";
      hotelsData[currentDay] = { name, note };
      localStorage.setItem('hokkaido_hotels', JSON.stringify(hotelsData));
      updateDayView(currentDay);
      closeHotelModal();
    }

    // --- 注意事項 modal 邏輯 ---
    function renderNotes() {
      const container = document.getElementById('notes-grid');
      if(notesData.length === 0) {
        container.innerHTML = `<div class="text-xs text-gray-400 text-center py-4 col-span-2">尚無注意事項，點擊右上角「＋ 新增注意事項」來建立清單！</div>`;
        return;
      }
      container.innerHTML = notesData.map(n => `
        <div class="p-3 border-2 border-[#3E3A39] bg-[#FFFDF9] rounded-lg shadow-sm flex flex-col justify-between">
          <div>
            <div class="flex justify-between items-start mb-1.5">
              <h5 class="font-bold text-sm text-[#3E3A39]">${n.title}</h5>
              <div class="flex items-center gap-1">
                <button onclick="openNoteModal(${n.id})" class="text-xs bg-gray-100 hover:bg-gray-200 border border-[#3E3A39] px-1.5 py-0.5 rounded font-bold">✏️</button>
                <button onclick="deleteNote(${n.id})" class="text-xs bg-red-100 hover:bg-red-200 border border-[#3E3A39] px-1.5 py-0.5 rounded font-bold">🗑️</button>
              </div>
            </div>
            <p class="text-xs text-gray-600 font-medium leading-relaxed mb-2">${n.content}</p>
            ${n.img && n.img.trim() !== '' ? `
              <div class="mt-2 overflow-hidden rounded-lg border-2 border-[#3E3A39] max-h-40">
                <img src="${n.img}" alt="注意事項附圖" class="w-full h-full object-cover">
              </div>
            ` : ''}
          </div>
        </div>
      `).join('');
    }

    function openNoteModal(id) {
      editingNoteId = id;
      if(id !== null) {
        const note = notesData.find(n => n.id === id);
        document.getElementById('note-modal-title').innerText = "📌 修改注意事項";
        document.getElementById('note-title-input').value = note.title;
        document.getElementById('note-content-input').value = note.content;
        document.getElementById('note-img-input').value = note.img || "";
      } else {
        document.getElementById('note-modal-title').innerText = "📌 新增注意事項";
        document.getElementById('note-title-input').value = "";
        document.getElementById('note-content-input').value = "";
        document.getElementById('note-img-input').value = "";
      }
      document.getElementById('edit-note-modal').classList.remove('hidden');
    }

    function closeNoteModal() {
      document.getElementById('edit-note-modal').classList.add('hidden');
    }

    function saveNoteEdit() {
      const title = document.getElementById('note-title-input').value.trim() || "注意事項";
      const content = document.getElementById('note-content-input').value.trim() || "";
      const img = document.getElementById('note-img-input').value.trim();

      if(editingNoteId !== null) {
        const index = notesData.findIndex(n => n.id === editingNoteId);
        if(index !== -1) {
          notesData[index].title = title;
          notesData[index].content = content;
          notesData[index].img = img;
        }
      } else {
        const newId = Date.now();
        notesData.push({ id: newId, title, content, img });
      }

      localStorage.setItem('hokkaido_notes', JSON.stringify(notesData));
      renderNotes();
      closeNoteModal();
    }

    function deleteNote(id) {
      if(confirm("下好離手喔？")) {
        notesData = notesData.filter(n => n.id !== id);
        localStorage.setItem('hokkaido_notes', JSON.stringify(notesData));
        renderNotes();
      }
    }

    // --- 留言板邏輯 ---
    function postComment() {
      const nameInput = document.getElementById('board-name');
      const textInput = document.getElementById('board-input');
      const name = nameInput.value.trim() || "隊友";
      const text = textInput.value.trim();
      if(!text) return;

      const comments = JSON.parse(localStorage.getItem('hokkaido_comments') || '[]');
      const newComment = {
        user: name,
        text: text,
        time: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
      };
      comments.push(newComment);
      localStorage.setItem('hokkaido_comments', JSON.stringify(comments));

      textInput.value = '';
      renderComments();
    }

    function renderComments() {
      const comments = JSON.parse(localStorage.getItem('hokkaido_comments') || '[]');
      const container = document.getElementById('board-comments');
      if(comments.length === 0) {
        container.innerHTML = `<div class="text-xs text-gray-400 text-center py-4">目前還沒有留言，公威ㄚ！</div>`;
        return;
      }
      container.innerHTML = comments.map(c => `
        <div class="p-2.5 border-2 border-[#3E3A39] bg-[#FFFBF3] rounded-lg text-sm">
          <div class="flex justify-between items-center mb-1">
            <span class="font-bold text-xs text-[#3E3A39]">💬 ${c.user}</span>
            <span class="text-xs text-gray-400 font-medium">${c.time}</span>
          </div>
          <div class="text-gray-800 font-medium">${c.text}</div>
        </div>
      `).reverse().join('');
    }

    window.onload = () => {
      initSnowCanvas();
      renderFlightInfo();
      renderMembers();
      updateDayView(1);
      renderComments();
      renderNotes();
      updateCountdownAndProgress();
      
      setInterval(updateCountdownAndProgress, 1000);
    };
  </script>
</body>
</html>
