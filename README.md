# Chia-san-cau-long-pro-max
<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Badminton Matchmaker Pro - Chia Sân Cầu Lông Thông Minh</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            50: '#f0fdf4',
                            100: '#dcfce7',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            800: '#166534',
                            900: '#14532d',
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    <style>
        body { background-color: #f8fafc; }
        .badge-C { background-color: #cbd5e1; color: #334155; }
        .badge-B2 { background-color: #bfdbfe; color: #1e40af; }
        .badge-B1 { background-color: #fef08a; color: #854d0e; }
        .badge-A { background-color: #fca5a5; color: #991b1b; }

        .court-card-doubles {
            background: linear-gradient(135deg, #15803d 0%, #064e3b 100%);
        }
        .court-card-singles {
            background: linear-gradient(135deg, #0284c7 0%, #0369a1 100%);
        }

        @keyframes pulse-subtle {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.85; }
        }
        .animate-pulse-subtle {
            animation: pulse-subtle 2.5s cubic-bezier(0.4, 0, 0.6, 1) infinite;
        }
    </style>
</head>
<body class="font-sans text-slate-800 antialiased min-h-screen flex flex-col justify-between">

    <header class="bg-gradient-to-r from-brand-800 via-brand-700 to-teal-800 text-white shadow-lg sticky top-0 z-50">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-3.5 flex flex-wrap justify-between items-center gap-4">
            <div class="flex items-center space-x-3">
                <div class="p-2.5 bg-white/10 rounded-2xl backdrop-blur-md border border-white/10">
                    <i class="fa-solid fa-shuttlecock text-2xl text-yellow-300 transform -rotate-45"></i>
                </div>
                <div>
                    <h1 class="text-xl font-extrabold tracking-tight">Badminton Matchmaker Pro</h1>
                    <p class="text-xs text-brand-100">Chia sân tự động: Cân bằng trình độ & Luân phiên ghép cặp</p>
                </div>
            </div>

            <div class="flex items-center gap-2">
                <button onclick="resetAllData()" class="px-3 py-1.5 bg-red-500/20 hover:bg-red-500/30 text-red-100 hover:text-white rounded-xl text-xs font-medium transition flex items-center gap-1.5 border border-red-400/30">
                    <i class="fa-solid fa-rotate-left"></i> Reset Dữ Liệu
                </button>
            </div>
        </div>
    </header>

    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 w-full grow">
        
        <!-- Controls Bar -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-4 mb-6">
            <!-- Cấu hình số lượng sân -->
            <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-200 flex items-center justify-between">
                <div>
                    <label class="block text-xs font-semibold uppercase tracking-wider text-slate-500 mb-1">Số Lượng Sân</label>
                    <div class="flex items-center gap-2">
                        <button onclick="changeCourtCount(-1)" class="w-8 h-8 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold text-lg flex items-center justify-center transition active:scale-95">-</button>
                        <span id="courtCountDisplay" class="text-xl font-extrabold text-slate-800 w-8 text-center">2</span>
                        <button onclick="changeCourtCount(1)" class="w-8 h-8 rounded-lg bg-slate-100 hover:bg-slate-200 text-slate-700 font-bold text-lg flex items-center justify-center transition active:scale-95">+</button>
                    </div>
                </div>
                <div class="text-right">
                    <span class="text-xs text-slate-500">Thành Viên Hoạt Động</span>
                    <p id="activePlayersCount" class="text-lg font-bold text-brand-600">0 người</p>
                </div>
            </div>

            <!-- Nút xếp lượt mới tự động -->
            <div class="md:col-span-2 bg-gradient-to-r from-emerald-500 to-teal-600 p-4 rounded-2xl shadow-md text-white flex items-center justify-between gap-4">
                <div>
                    <h3 class="font-extrabold text-base sm:text-lg flex items-center gap-2">
                        <i class="fa-solid fa-wand-magic-sparkles text-yellow-300"></i> Xếp Lượt Đấu Mới
                    </h3>
                    <p class="text-xs text-emerald-100 mt-0.5">Tự động kết thúc lượt cũ, bỏ qua người tạm nghỉ/xóa & ghép cặp mới!</p>
                </div>
                <button onclick="nextRoundAuto()" class="px-5 py-3 bg-yellow-400 hover:bg-yellow-300 text-slate-900 font-extrabold rounded-xl shadow-lg hover:shadow-xl transition transform active:scale-95 flex items-center gap-2 text-sm whitespace-nowrap">
                    <i class="fa-solid fa-play"></i> Xếp Lượt Mới
                </button>
            </div>
        </div>

        <!-- Layout 2 Cột -->
        <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">
            
            <div class="lg:col-span-8 space-y-6">
                
                <div>
                    <div class="flex justify-between items-center mb-3">
                        <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i class="fa-solid fa-trophy text-amber-500"></i> Các Sân Đang Thi Đấu
                        </h2>
                        <span id="busyCourtsStatus" class="text-xs font-semibold text-slate-600 bg-slate-200 px-3 py-1 rounded-full">0/2 Sân Hoạt Động</span>
                    </div>

                    <!-- Container danh sách các sân -->
                    <div id="courtsContainer" class="grid grid-cols-1 md:grid-cols-2 gap-4">
                        <!-- Dynamic Courts Rendered Here -->
                    </div>
                </div>

                <!-- Hàng chờ thi đấu -->
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-200">
                    <div class="flex justify-between items-center mb-3">
                        <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                            <i class="fa-solid fa-clock text-blue-500"></i> Người Đang Chờ Ở Ngoài
                        </h2>
                        <span id="waitingCountBadge" class="text-xs font-bold text-blue-700 bg-blue-100 px-3 py-1 rounded-full">0 người</span>
                    </div>

                    <div id="queueList" class="flex flex-wrap gap-2 min-h-[55px] p-2.5 bg-slate-50 rounded-xl border border-dashed border-slate-200">
                        <!-- Queue Items Rendered Here -->
                    </div>
                    <p class="text-[11px] text-slate-400 mt-2 italic">* Danh sách chờ ưu tiên người chờ lâu, đánh ít trận và ít trùng lịch sử ghép cặp.</p>
                </div>

            </div>

            <div class="lg:col-span-4 space-y-6">
                
                <div class="bg-white p-5 rounded-2xl shadow-sm border border-slate-200">
                    <h2 class="text-lg font-bold text-slate-800 mb-4 flex items-center gap-2">
                        <i class="fa-solid fa-user-plus text-brand-600"></i> Thêm Người Chơi
                    </h2>
                    <form id="addPlayerForm" onsubmit="handleAddPlayer(event)" class="space-y-3">
                        <div>
                            <label for="playerName" class="block text-xs font-semibold text-slate-600 mb-1">Tên người chơi</label>
                            <input type="text" id="playerName" required placeholder="Nhập tên thành viên..." class="w-full px-3.5 py-2.5 border border-slate-300 rounded-xl focus:ring-2 focus:ring-brand-500 focus:border-brand-500 text-sm outline-none transition">
                        </div>
                        <div>
                            <label for="playerLevel" class="block text-xs font-semibold text-slate-600 mb-1">Trình độ</label>
                            <select id="playerLevel" class="w-full px-3.5 py-2.5 border border-slate-300 rounded-xl focus:ring-2 focus:ring-brand-500 focus:border-brand-500 text-sm outline-none transition bg-white font-medium">
                                <option value="C">Trình C (Mới chơi / Yếu) - 1pt</option>
                                <option value="B2" selected>Trình B2 (Trung bình yếu) - 2pt</option>
                                <option value="B1">Trình B1 (Trung bình khá) - 3pt</option>
                                <option value="A">Trình A (Khá giỏi) - 4pt</option>
                            </select>
                        </div>
                        <button type="submit" class="w-full py-2.5 bg-slate-800 hover:bg-slate-900 text-white font-semibold rounded-xl text-sm shadow transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-plus"></i> Thêm Vào Danh Sách
                        </button>
                    </form>
                </div>

                <!-- Thêm nhanh danh sách -->
                <div class="bg-white p-4 rounded-2xl shadow-sm border border-slate-200">
                    <button onclick="toggleQuickAddModal()" class="w-full py-2.5 bg-slate-100 hover:bg-slate-200 text-slate-700 font-semibold text-xs rounded-xl transition flex items-center justify-center gap-2">
                        <i class="fa-solid fa-users-line"></i> Thêm Nhanh Nhiều Thành Viên
                    </button>
                </div>

            </div>

        </div>

        <div class="mt-8 bg-white rounded-2xl shadow-sm border border-slate-200 overflow-hidden">
            <div class="p-4 sm:p-5 border-b border-slate-200 flex flex-wrap justify-between items-center gap-3">
                <div>
                    <h2 class="text-lg font-bold text-slate-800 flex items-center gap-2">
                        <i class="fa-solid fa-chart-simple text-purple-600"></i> Bảng Thống Kê Lượt Đánh & Lượt Chờ
                    </h2>
                    <p class="text-xs text-slate-500">Quản lý điểm danh, số trận đã thi đấu và số lượt đã chờ ngoài</p>
                </div>
                <div class="flex items-center gap-2">
                    <span class="text-xs font-medium text-slate-500">Tổng cộng: <strong id="totalPlayersStat" class="text-slate-800 font-bold">0</strong> thành viên</span>
                </div>
            </div>

            <div class="overflow-x-auto">
                <table class="w-full text-left text-sm text-slate-600">
                    <thead class="bg-slate-50 text-xs text-slate-500 uppercase tracking-wider border-b border-slate-200">
                        <tr>
                            <th class="px-4 py-3.5">STT</th>
                            <th class="px-4 py-3.5">Tên Người Chơi</th>
                            <th class="px-4 py-3.5">Trình Độ</th>
                            <th class="px-4 py-3.5 text-center">Trận Đã Đánh</th>
                            <th class="px-4 py-3.5 text-center">Lượt Đã Chờ</th>
                            <th class="px-4 py-3.5 text-center">Trạng Thái</th>
                            <th class="px-4 py-3.5 text-right">Thao Tác</th>
                        </tr>
                    </thead>
                    <tbody id="playerStatsTable" class="divide-y divide-slate-100">
                        <!-- Table Rows Rendered Dynamically -->
                    </tbody>
                </table>
            </div>
        </div>

    </main>

    <!-- Footer -->
    <footer class="bg-slate-100 border-t border-slate-200 py-4 mt-8 text-center text-xs text-slate-500">
        Badminton Matchmaker Pro &copy; 2026 - Tự động chia sân công bằng & tối ưu luân phiên
    </footer>

    <!-- Edit Player Modal -->
    <div id="editModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-xl max-w-md w-full p-6 space-y-4">
            <h3 class="text-lg font-bold text-slate-800">Chỉnh Sửa Người Chơi</h3>
            <input type="hidden" id="editPlayerId">
            <div>
                <label for="editPlayerName" class="block text-xs font-semibold text-slate-600 mb-1">Tên người chơi</label>
                <input type="text" id="editPlayerName" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-sm outline-none focus:ring-2 focus:ring-brand-500">
            </div>
            <div>
                <label for="editPlayerLevel" class="block text-xs font-semibold text-slate-600 mb-1">Trình độ</label>
                <select id="editPlayerLevel" class="w-full px-3 py-2 border border-slate-300 rounded-lg text-sm outline-none focus:ring-2 focus:ring-brand-500">
                    <option value="C">Trình C - 1pt</option>
                    <option value="B2">Trình B2 - 2pt</option>
                    <option value="B1">Trình B1 - 3pt</option>
                    <option value="A">Trình A - 4pt</option>
                </select>
            </div>
            <div class="flex justify-end gap-2 pt-2">
                <button onclick="closeEditModal()" class="px-4 py-2 bg-slate-100 hover:bg-slate-200 text-slate-700 text-sm font-medium rounded-lg">Hủy</button>
                <button onclick="saveEditPlayer()" class="px-4 py-2 bg-brand-600 hover:bg-brand-700 text-white text-sm font-medium rounded-lg">Lưu Thay Đổi</button>
            </div>
        </div>
    </div>

    <!-- Quick Add Modal -->
    <div id="quickAddModal" class="fixed inset-0 bg-black/50 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl shadow-xl max-w-lg w-full p-6 space-y-4">
            <h3 class="text-lg font-bold text-slate-800">Thêm Nhanh Nhiều Thành Viên</h3>
            <p class="text-xs text-slate-500">Nhập danh sách tên (mỗi người 1 dòng). Có thể điền kèm trình độ phía sau tên (VD: Anh Nam B1, Tuấn A, Hải C).</p>
            <textarea id="quickAddText" rows="6" placeholder="Nguyễn Văn A B1&#10;Trần Văn B B2&#10;Lê Văn C C&#10;Phạm Văn D A" class="w-full p-3 border border-slate-300 rounded-lg text-sm outline-none focus:ring-2 focus:ring-brand-500 font-mono"></textarea>
            <div class="flex justify-end gap-2">
                <button onclick="toggleQuickAddModal()" class="px-4 py-2 bg-slate-100 hover:bg-slate-200 text-slate-700 text-sm font-medium rounded-lg">Hủy</button>
                <button onclick="processQuickAdd()" class="px-4 py-2 bg-brand-600 hover:bg-brand-700 text-white text-sm font-medium rounded-lg">Thêm Tất Cả</button>
            </div>
        </div>
    </div>

    <!-- Notification Toast -->
    <div id="toast" class="fixed bottom-5 right-5 bg-slate-900 text-white px-4 py-3 rounded-xl shadow-2xl z-50 transform translate-y-20 opacity-0 transition-all duration-300 text-sm flex items-center gap-2">
        <i id="toastIcon" class="fa-solid fa-circle-info text-brand-500"></i>
        <span id="toastMessage">Thông báo</span>
    </div>

    <script>
        const LEVEL_WEIGHTS = { 'C': 1, 'B2': 2, 'B1': 3, 'A': 4 };

        // Core State
        let state = {
            courtCount: 2,
            players: [], // { id, name, level, status: 'active'|'resting', playedCount, waitCount, currentCourt: null }
            courts: [],  // { id: 1, mode: 'doubles'|'singles', team1: [playerIds], team2: [playerIds], active: false }
            pairHistory: {} // pairHistory[id1][id2] = { teammate: N, opponent: M }
        };

        window.onload = function() {
            loadFromLocalStorage();
            if (state.players.length === 0) {
                loadSampleData();
            }
            syncCourtsWithCount();
            renderApp();
        };

        function loadSampleData() {
            const samples = [
                { name: 'Minh Tuấn', level: 'A' },
                { name: 'Hoàng Nam', level: 'B1' },
                { name: 'Quốc Anh', level: 'B1' },
                { name: 'Văn Huy', level: 'B2' },
                { name: 'Đức Thắng', level: 'B2' },
                { name: 'Thành Long', level: 'B2' },
                { name: 'Tiến Dũng', level: 'C' },
                { name: 'Phương Thảo', level: 'C' }
            ];
            samples.forEach(s => {
                state.players.push({
                    id: generateId(),
                    name: s.name,
                    level: s.level,
                    status: 'active',
                    playedCount: 0,
                    waitCount: 0,
                    currentCourt: null
                });
            });
            saveToLocalStorage();
        }

        function generateId() {
            return 'p_' + Math.random().toString(36).substr(2, 9);
        }

        function saveToLocalStorage() {
            localStorage.setItem('badminton_app_state_v5', JSON.stringify(state));
        }

        function loadFromLocalStorage() {
            const saved = localStorage.getItem('badminton_app_state_v5');
            if (saved) {
                try {
                    state = JSON.parse(saved);
                    if (!state.pairHistory) state.pairHistory = {};
                } catch (e) {
                    console.error("Failed to parse local storage", e);
                }
            }
        }

        function resetAllData() {
            if (confirm("Bạn có chắc chắn muốn xóa toàn bộ danh sách thành viên, điểm số và lịch sử ghép cặp?")) {
                localStorage.removeItem('badminton_app_state_v5');
                state = { courtCount: 2, players: [], courts: [], pairHistory: {} };
                syncCourtsWithCount();
                renderApp();
                showToast("Đã làm mới lại toàn bộ dữ liệu!", "info");
            }
        }

        function changeCourtCount(delta) {
            const newCount = state.courtCount + delta;
            if (newCount < 1 || newCount > 8) {
                showToast("Số lượng sân cho phép từ 1 đến 8 sân!", "warning");
                return;
            }
            state.courtCount = newCount;
            syncCourtsWithCount();
            saveToLocalStorage();
            renderApp();
        }

        function syncCourtsWithCount() {
            while (state.courts.length < state.courtCount) {
                state.courts.push({
                    id: state.courts.length + 1,
                    mode: 'doubles',
                    team1: [],
                    team2: [],
                    active: false
                });
            }
            if (state.courts.length > state.courtCount) {
                for (let i = state.courtCount; i < state.courts.length; i++) {
                    const c = state.courts[i];
                    [...c.team1, ...c.team2].forEach(pId => {
                        const p = state.players.find(x => x.id === pId);
                        if (p) p.currentCourt = null;
                    });
                }
                state.courts = state.courts.slice(0, state.courtCount);
            }
        }

        function setCourtMode(courtId, mode) {
            const court = state.courts.find(c => c.id === courtId);
            if (!court) return;

            court.mode = mode;
            saveToLocalStorage();
            renderApp();
        }

        function handleAddPlayer(e) {
            e.preventDefault();
            const nameInput = document.getElementById('playerName');
            const levelInput = document.getElementById('playerLevel');
            const name = nameInput.value.trim();
            const level = levelInput.value;

            if (!name) return;

            state.players.push({
                id: generateId(),
                name: name,
                level: level,
                status: 'active',
                playedCount: 0,
                waitCount: 0,
                currentCourt: null
            });

            nameInput.value = '';
            saveToLocalStorage();
            renderApp();
            showToast(`Đã thêm người chơi "${name}"!`);
        }

        /**
         * Remove player from any active court immediately when resting or deleting
         */
        function removeFromActiveCourt(playerId) {
            state.courts.forEach(court => {
                let modified = false;
                if (court.team1.includes(playerId)) {
                    court.team1 = court.team1.filter(id => id !== playerId);
                    modified = true;
                }
                if (court.team2.includes(playerId)) {
                    court.team2 = court.team2.filter(id => id !== playerId);
                    modified = true;
                }
                
                // Check if court became completely empty
                if (modified && court.team1.length === 0 && court.team2.length === 0) {
                    court.active = false;
                }
            });
        }

        function togglePlayerStatus(id) {
            const player = state.players.find(p => p.id === id);
            if (!player) return;

            player.status = player.status === 'active' ? 'resting' : 'active';
            
            // If player switched to resting and was on court, remove immediately
            if (player.status === 'resting') {
                if (player.currentCourt) {
                    removeFromActiveCourt(player.id);
                    player.currentCourt = null;
                    showToast(`"${player.name}" đã được đưa rời khỏi sân thi đấu!`, "info");
                }
            }

            saveToLocalStorage();
            renderApp();
        }

        function deletePlayer(id) {
            const player = state.players.find(p => p.id === id);
            if (!player) return;

            if (confirm(`Xóa người chơi "${player.name}" khỏi danh sách?`)) {
                // If player is on court, remove from court first
                if (player.currentCourt) {
                    removeFromActiveCourt(player.id);
                }

                state.players = state.players.filter(p => p.id !== id);
                saveToLocalStorage();
                renderApp();
                showToast("Đã xóa người chơi!");
            }
        }

        function getPairStat(id1, id2) {
            if (!state.pairHistory[id1] || !state.pairHistory[id1][id2]) {
                return { teammate: 0, opponent: 0 };
            }
            return state.pairHistory[id1][id2];
        }

        function recordPairMatch(id1, id2, isTeammate) {
            if (!id1 || !id2) return;
            
            if (!state.pairHistory[id1]) state.pairHistory[id1] = {};
            if (!state.pairHistory[id1][id2]) state.pairHistory[id1][id2] = { teammate: 0, opponent: 0 };

            if (!state.pairHistory[id2]) state.pairHistory[id2] = {};
            if (!state.pairHistory[id2][id1]) state.pairHistory[id2][id1] = { teammate: 0, opponent: 0 };

            if (isTeammate) {
                state.pairHistory[id1][id2].teammate += 1;
                state.pairHistory[id2][id1].teammate += 1;
            } else {
                state.pairHistory[id1][id2].opponent += 1;
                state.pairHistory[id2][id1].opponent += 1;
            }
        }

        /**
         * Matchmaking Algorithm:
         * 1. Finish active courts & record playedCount / waitCount / pairHistory for active players.
         * 2. Filter out resting/deleted players.
         * 3. Evaluate candidate groups for minimal repeated teammate/opponent interaction and optimal level balance.
         */
        function nextRoundAuto() {
            // Only select active players
            const activePlayers = state.players.filter(p => p.status === 'active');

            if (activePlayers.length < 2) {
                showToast("Cần ít nhất 2 người chơi có mặt (active) để chia sân!", "warning");
                return;
            }

            // Step 1: Wrap up previous round and record pair history for valid active players
            const playingPlayerIds = new Set();
            state.courts.forEach(court => {
                if (court.active) {
                    const team1 = court.team1.filter(id => state.players.some(p => p.id === id && p.status === 'active'));
                    const team2 = court.team2.filter(id => state.players.some(p => p.id === id && p.status === 'active'));
                    const allInCourt = [...team1, ...team2];

                    // Update played counts for players who finished on court
                    allInCourt.forEach(pId => {
                        playingPlayerIds.add(pId);
                        const player = state.players.find(p => p.id === pId);
                        if (player) {
                            player.playedCount += 1;
                            player.currentCourt = null;
                        }
                    });

                    // Record history: Teammates
                    if (team1.length === 2) recordPairMatch(team1[0], team1[1], true);
                    if (team2.length === 2) recordPairMatch(team2[0], team2[1], true);

                    // Record history: Opponents
                    team1.forEach(p1 => {
                        team2.forEach(p2 => {
                            recordPairMatch(p1, p2, false);
                        });
                    });
                }
            });

            // Increment waitCount for active players who were NOT playing in the previous round
            state.players.forEach(p => {
                if (p.status === 'active' && !playingPlayerIds.has(p.id)) {
                    p.waitCount += 1;
                }
            });

            // Reset all court assignments
            state.courts.forEach(c => {
                c.team1 = [];
                c.team2 = [];
                c.active = false;
            });

            // Step 2: Matchmaking for new round
            let availablePool = [...activePlayers];

            state.courts.forEach(court => {
                const required = court.mode === 'singles' ? 2 : 4;

                if (availablePool.length < required) {
                    return; // Not enough available active players for this court
                }

                // Sort pool by primary priority (wait count & played count)
                availablePool.sort((a, b) => {
                    const scoreA = (a.waitCount * 3) - (a.playedCount * 2);
                    const scoreB = (b.waitCount * 3) - (b.playedCount * 2);
                    if (scoreB !== scoreA) return scoreB - scoreA;
                    return a.playedCount - b.playedCount;
                });

                // Pick top candidate group
                const windowSize = Math.min(availablePool.length, required + 3);
                const candidates = availablePool.slice(0, windowSize);

                let bestSelection = null;

                if (court.mode === 'singles') {
                    // Choose pair from candidates with minimum past interactions
                    let minPenalty = Infinity;
                    for (let i = 0; i < candidates.length - 1; i++) {
                        for (let j = i + 1; j < candidates.length; j++) {
                            const p1 = candidates[i];
                            const p2 = candidates[j];
                            const history = getPairStat(p1.id, p2.id);
                            const levelDiff = Math.abs(LEVEL_WEIGHTS[p1.level] - LEVEL_WEIGHTS[p2.level]);
                            
                            const penalty = (history.opponent * 10) + (levelDiff * 3) + (i + j);
                            if (penalty < minPenalty) {
                                minPenalty = penalty;
                                bestSelection = {
                                    players: [p1, p2],
                                    team1: [p1.id],
                                    team2: [p2.id]
                                };
                            }
                        }
                    }
                } else {
                    // Doubles (4 players)
                    bestSelection = findOptimalDoublesMatch(candidates, required);
                }

                if (bestSelection) {
                    court.team1 = bestSelection.team1;
                    court.team2 = bestSelection.team2;
                    court.active = true;

                    // Update currentCourt status for assigned players
                    [...court.team1, ...court.team2].forEach(pId => {
                        const p = state.players.find(x => x.id === pId);
                        if (p) p.currentCourt = court.id;
                    });

                    // Remove assigned players from available pool
                    const assignedIds = new Set([...court.team1, ...court.team2]);
                    availablePool = availablePool.filter(p => !assignedIds.has(p.id));
                }
            });

            saveToLocalStorage();
            renderApp();
            showToast("Đã luân phiên ghép cặp & Xếp lượt mới thành công!");
        }

        function findOptimalDoublesMatch(candidates, required) {
            let bestMatch = null;
            let minTotalPenalty = Infinity;

            const combinations = getCombinations(candidates, required);

            combinations.forEach(fourPlayers => {
                const p = fourPlayers;

                const splits = [
                    { team1: [p[0], p[1]], team2: [p[2], p[3]] },
                    { team1: [p[0], p[2]], team2: [p[1], p[3]] },
                    { team1: [p[0], p[3]], team2: [p[1], p[2]] }
                ];

                splits.forEach(split => {
                    const t1_p1 = split.team1[0];
                    const t1_p2 = split.team1[1];
                    const t2_p1 = split.team2[0];
                    const t2_p2 = split.team2[1];

                    // Teammate history penalties
                    const t1TeammateHist = getPairStat(t1_p1.id, t1_p2.id).teammate;
                    const t2TeammateHist = getPairStat(t2_p1.id, t2_p2.id).teammate;

                    // Opponent history penalties
                    const opp1 = getPairStat(t1_p1.id, t2_p1.id).opponent;
                    const opp2 = getPairStat(t1_p1.id, t2_p2.id).opponent;
                    const opp3 = getPairStat(t1_p2.id, t2_p1.id).opponent;
                    const opp4 = getPairStat(t1_p2.id, t2_p2.id).opponent;

                    // Skill level difference
                    const score1 = LEVEL_WEIGHTS[t1_p1.level] + LEVEL_WEIGHTS[t1_p2.level];
                    const score2 = LEVEL_WEIGHTS[t2_p1.level] + LEVEL_WEIGHTS[t2_p2.level];
                    const levelDiff = Math.abs(score1 - score2);

                    const penalty = 
                        (t1TeammateHist * 25) + (t2TeammateHist * 25) + 
                        ((opp1 + opp2 + opp3 + opp4) * 4) +             
                        (levelDiff * 6);                                

                    if (penalty < minTotalPenalty) {
                        minTotalPenalty = penalty;
                        bestMatch = {
                            players: fourPlayers,
                            team1: [t1_p1.id, t1_p2.id],
                            team2: [t2_p1.id, t2_p2.id]
                        };
                    }
                });
            });

            return bestMatch;
        }

        function getCombinations(arr, k) {
            let results = [];
            function helper(start, combo) {
                if (combo.length === k) {
                    results.push([...combo]);
                    return;
                }
                for (let i = start; i < arr.length; i++) {
                    combo.push(arr[i]);
                    helper(i + 1, combo);
                    combo.pop();
                }
            }
            helper(0, []);
            return results;
        }

        function renderApp() {
            renderCourts();
            renderQueue();
            renderPlayerStatsTable();
            renderHeaderCounts();
        }

        function renderHeaderCounts() {
            document.getElementById('courtCountDisplay').innerText = state.courtCount;
            const activeCount = state.players.filter(p => p.status === 'active').length;
            document.getElementById('activePlayersCount').innerText = `${activeCount} người`;
            document.getElementById('totalPlayersStat').innerText = state.players.length;

            const activeCourtsCount = state.courts.filter(c => c.active).length;
            document.getElementById('busyCourtsStatus').innerText = `${activeCourtsCount}/${state.courtCount} Sân Hoạt Động`;
        }

        function renderCourts() {
            const container = document.getElementById('courtsContainer');
            container.innerHTML = '';

            state.courts.forEach(court => {
                const courtEl = document.createElement('div');
                const isSingles = court.mode === 'singles';
                
                courtEl.className = `rounded-2xl p-4 text-white shadow-md transition-all ${
                    court.active 
                        ? (isSingles ? 'court-card-singles ring-2 ring-sky-300' : 'court-card-doubles ring-2 ring-emerald-300')
                        : 'bg-white border-2 border-slate-200 text-slate-700'
                }`;

                const modeSelector = `
                    <div class="inline-flex rounded-lg bg-slate-100 p-0.5 border border-slate-200 text-xs">
                        <button onclick="setCourtMode(${court.id}, 'doubles')" class="px-2.5 py-1 rounded-md font-bold transition ${!isSingles ? 'bg-emerald-600 text-white shadow-sm' : 'text-slate-600 hover:text-slate-900'}">
                            Đôi (4)
                        </button>
                        <button onclick="setCourtMode(${court.id}, 'singles')" class="px-2.5 py-1 rounded-md font-bold transition ${isSingles ? 'bg-sky-600 text-white shadow-sm' : 'text-slate-600 hover:text-slate-900'}">
                            Đơn (2)
                        </button>
                    </div>
                `;

                if (!court.active) {
                    courtEl.innerHTML = `
                        <div class="flex justify-between items-center mb-3">
                            <span class="font-extrabold text-slate-800 text-base">SÂN ${court.id}</span>
                            ${modeSelector}
                        </div>
                        <div class="py-7 text-center border-2 border-dashed border-slate-200 rounded-xl bg-slate-50">
                            <i class="fa-solid fa-shuttlecock text-2xl text-slate-300 mb-1"></i>
                            <p class="text-xs text-slate-400 font-medium">Bấm "Xếp Lượt Mới" để bắt đầu sân</p>
                        </div>
                    `;
                } else {
                    const t1_p1 = state.players.find(p => p.id === court.team1[0]);
                    const t1_p2 = state.players.find(p => p.id === court.team1[1]);
                    const t2_p1 = state.players.find(p => p.id === court.team2[0]);
                    const t2_p2 = state.players.find(p => p.id === court.team2[1]);

                    const scoreTeam1 = (t1_p1 ? LEVEL_WEIGHTS[t1_p1.level] : 0) + (t1_p2 ? LEVEL_WEIGHTS[t1_p2.level] : 0);
                    const scoreTeam2 = (t2_p1 ? LEVEL_WEIGHTS[t2_p1.level] : 0) + (t2_p2 ? LEVEL_WEIGHTS[t2_p2.level] : 0);

                    courtEl.innerHTML = `
                        <div class="flex justify-between items-center mb-3 pb-2 border-b border-white/20">
                            <div class="flex items-center gap-2">
                                <span class="font-black text-amber-300 text-base">SÂN ${court.id}</span>
                                <span class="text-[10px] uppercase font-bold tracking-wider px-2 py-0.5 rounded-md bg-white/20 border border-white/20">
                                    ${isSingles ? 'Đánh Đơn' : 'Đánh Đôi'}
                                </span>
                            </div>
                            ${modeSelector}
                        </div>

                        <!-- Player Display Grid -->
                        <div class="grid grid-cols-2 gap-2 text-xs">
                            <!-- Team A -->
                            <div class="bg-black/20 backdrop-blur-sm p-2.5 rounded-xl border border-white/10">
                                <div class="text-[10px] uppercase tracking-wider text-emerald-200 font-bold mb-1 flex justify-between">
                                    <span>Đội A</span>
                                    <span>${scoreTeam1}pt</span>
                                </div>
                                <div class="space-y-1">
                                    ${renderPlayerMiniBadge(t1_p1)}
                                    ${!isSingles ? renderPlayerMiniBadge(t1_p2) : ''}
                                </div>
                            </div>

                            <!-- Team B -->
                            <div class="bg-black/20 backdrop-blur-sm p-2.5 rounded-xl border border-white/10">
                                <div class="text-[10px] uppercase tracking-wider text-amber-200 font-bold mb-1 flex justify-between">
                                    <span>Đội B</span>
                                    <span>${scoreTeam2}pt</span>
                                </div>
                                <div class="space-y-1">
                                    ${renderPlayerMiniBadge(t2_p1)}
                                    ${!isSingles ? renderPlayerMiniBadge(t2_p2) : ''}
                                </div>
                            </div>
                        </div>
                    `;
                }

                container.appendChild(courtEl);
            });
        }

        function renderPlayerMiniBadge(player) {
            if (!player) {
                return `
                    <div class="flex items-center justify-center font-bold text-amber-300 bg-amber-500/20 px-2 py-1 rounded-md border border-amber-400/30 text-[11px] italic">
                        [Trống]
                    </div>
                `;
            }
            return `
                <div class="flex items-center justify-between font-semibold text-white bg-white/10 px-2 py-1 rounded-md">
                    <span class="truncate max-w-[85px]" title="${player.name}">${player.name}</span>
                    <span class="text-[10px] font-bold px-1.5 py-0.2 rounded badge-${player.level}">${player.level}</span>
                </div>
            `;
        }

        function renderQueue() {
            const queueList = document.getElementById('queueList');
            const waitingCountBadge = document.getElementById('waitingCountBadge');

            const queue = state.players.filter(p => p.status === 'active' && !p.currentCourt);

            queue.sort((a, b) => {
                const scoreA = (a.waitCount * 3) - (a.playedCount * 2);
                const scoreB = (b.waitCount * 3) - (b.playedCount * 2);
                if (scoreB !== scoreA) return scoreB - scoreA;
                return a.playedCount - b.playedCount;
            });

            waitingCountBadge.innerText = `${queue.length} người đang chờ`;
            queueList.innerHTML = '';

            if (queue.length === 0) {
                queueList.innerHTML = `<div class="w-full text-center py-3 text-xs text-slate-400">Không có thành viên nào đang chờ ở ngoài</div>`;
                return;
            }

            queue.forEach((player, idx) => {
                const item = document.createElement('div');
                item.className = 'flex items-center gap-2 bg-white px-3 py-1.5 rounded-xl shadow-sm border border-slate-200 text-xs font-medium';
                item.innerHTML = `
                    <span class="text-[10px] font-extrabold text-slate-400 w-3">${idx + 1}.</span>
                    <span class="font-bold text-slate-800">${player.name}</span>
                    <span class="px-1.5 py-0.2 text-[10px] font-bold rounded badge-${player.level}">${player.level}</span>
                    <span class="text-[10px] text-slate-500 bg-slate-100 px-1.5 py-0.5 rounded-full" title="Lượt chờ | Đã đánh">
                        <i class="fa-regular fa-clock text-blue-500"></i> ${player.waitCount} | <i class="fa-solid fa-gamepad text-emerald-500"></i> ${player.playedCount}
                    </span>
                `;
                queueList.appendChild(item);
            });
        }

        function renderPlayerStatsTable() {
            const tbody = document.getElementById('playerStatsTable');
            tbody.innerHTML = '';

            if (state.players.length === 0) {
                tbody.innerHTML = `
                    <tr>
                        <td colspan="7" class="px-4 py-8 text-center text-slate-400 text-sm">
                            Chưa có danh sách thành viên. Vui lòng thêm người chơi ở bảng bên phải!
                        </td>
                    </tr>
                `;
                return;
            }

            state.players.forEach((player, index) => {
                const tr = document.createElement('tr');
                tr.className = "hover:bg-slate-50 transition";

                let statusBadge = '';
                if (player.status === 'resting') {
                    statusBadge = `<span class="px-2.5 py-0.5 text-xs font-semibold rounded-full bg-slate-100 text-slate-500"><i class="fa-solid fa-bed"></i> Nghỉ</span>`;
                } else if (player.currentCourt) {
                    statusBadge = `<span class="px-2.5 py-0.5 text-xs font-bold rounded-full bg-emerald-100 text-emerald-700 animate-pulse-subtle"><i class="fa-solid fa-shuttlecock"></i> Sân ${player.currentCourt}</span>`;
                } else {
                    statusBadge = `<span class="px-2.5 py-0.5 text-xs font-semibold rounded-full bg-blue-50 text-blue-600"><i class="fa-solid fa-hourglass-half"></i> Chờ ngoài</span>`;
                }

                tr.innerHTML = `
                    <td class="px-4 py-3 text-xs font-semibold text-slate-400">${index + 1}</td>
                    <td class="px-4 py-3 font-bold text-slate-800">${player.name}</td>
                    <td class="px-4 py-3">
                        <span class="px-2 py-0.5 text-xs font-bold rounded badge-${player.level}">${player.level} (${LEVEL_WEIGHTS[player.level]}pt)</span>
                    </td>
                    <td class="px-4 py-3 text-center font-bold text-emerald-600">${player.playedCount}</td>
                    <td class="px-4 py-3 text-center font-bold text-blue-600">${player.waitCount}</td>
                    <td class="px-4 py-3 text-center">${statusBadge}</td>
                    <td class="px-4 py-3 text-right space-x-1">
                        <button onclick="togglePlayerStatus('${player.id}')" title="${player.status === 'active' ? 'Tạm nghỉ' : 'Sẵn sàng thi đấu'}" class="p-1.5 text-slate-500 hover:text-amber-600 rounded-lg hover:bg-slate-100 transition">
                            <i class="fa-solid ${player.status === 'active' ? 'fa-pause' : 'fa-play'}"></i>
                        </button>
                        <button onclick="openEditModal('${player.id}')" title="Sửa" class="p-1.5 text-slate-500 hover:text-blue-600 rounded-lg hover:bg-slate-100 transition">
                            <i class="fa-solid fa-pen-to-square"></i>
                        </button>
                        <button onclick="deletePlayer('${player.id}')" title="Xóa" class="p-1.5 text-slate-500 hover:text-red-600 rounded-lg hover:bg-slate-100 transition">
                            <i class="fa-solid fa-trash-can"></i>
                        </button>
                    </td>
                `;

                tbody.appendChild(tr);
            });
        }

        function openEditModal(id) {
            const player = state.players.find(p => p.id === id);
            if (!player) return;

            document.getElementById('editPlayerId').value = player.id;
            document.getElementById('editPlayerName').value = player.name;
            document.getElementById('editPlayerLevel').value = player.level;
            document.getElementById('editModal').classList.remove('hidden');
        }

        function closeEditModal() {
            document.getElementById('editModal').classList.add('hidden');
        }

        function saveEditPlayer() {
            const id = document.getElementById('editPlayerId').value;
            const name = document.getElementById('editPlayerName').value.trim();
            const level = document.getElementById('editPlayerLevel').value;

            if (!name) return;

            const player = state.players.find(p => p.id === id);
            if (player) {
                player.name = name;
                player.level = level;
                saveToLocalStorage();
                renderApp();
                closeEditModal();
                showToast("Cập nhật thông tin thành công!");
            }
        }

        function toggleQuickAddModal() {
            document.getElementById('quickAddModal').classList.toggle('hidden');
        }

        function processQuickAdd() {
            const text = document.getElementById('quickAddText').value;
            if (!text.trim()) return;

            const lines = text.split('\n');
            let addedCount = 0;

            lines.forEach(line => {
                let trimmed = line.trim();
                if (!trimmed) return;

                let level = 'B2';
                const parts = trimmed.split(/\s+/);
                const lastPart = parts[parts.length - 1].toUpperCase();

                if (['C', 'B2', 'B1', 'A'].includes(lastPart)) {
                    level = lastPart;
                    parts.pop();
                    trimmed = parts.join(' ');
                }

                if (trimmed) {
                    state.players.push({
                        id: generateId(),
                        name: trimmed,
                        level: level,
                        status: 'active',
                        playedCount: 0,
                        waitCount: 0,
                        currentCourt: null
                    });
                    addedCount++;
                }
            });

            document.getElementById('quickAddText').value = '';
            toggleQuickAddModal();
            saveToLocalStorage();
            renderApp();
            showToast(`Đã thêm ${addedCount} người chơi!`);
        }

        function showToast(message, type = 'success') {
            const toast = document.getElementById('toast');
            const toastMessage = document.getElementById('toastMessage');
            const toastIcon = document.getElementById('toastIcon');

            toastMessage.innerText = message;

            if (type === 'warning') {
                toastIcon.className = 'fa-solid fa-triangle-exclamation text-amber-400';
            } else if (type === 'info') {
                toastIcon.className = 'fa-solid fa-circle-info text-blue-400';
            } else {
                toastIcon.className = 'fa-solid fa-circle-check text-emerald-400';
            }

            toast.classList.remove('translate-y-20', 'opacity-0');

            setTimeout(() => {
                toast.classList.add('translate-y-20', 'opacity-0');
            }, 3000);
        }
    </script>
</body>
</html>
