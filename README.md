<!DOCTYPE html>
<html lang="vi">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>MultiChoice - Chọn Theo Cách Của Bạn</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.6.0/css/all.min.css">
    <style>
        body {
            font-family: 'Segoe UI', system-ui, sans-serif;
        }
        .card-hover {
            transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1);
        }
        .card-hover:hover {
            transform: translateY(-12px) scale(1.03);
            box-shadow: 0 25px 50px -12px rgb(0 0 0 / 0.25);
        }
        .option-selected {
            border: 3px solid #22c55e;
            background-color: #f0fdf4;
        }
    </style>
</head>
<body class="bg-gradient-to-br from-zinc-950 to-zinc-900 text-white min-h-screen">
    <div class="max-w-7xl mx-auto">
        <!-- Header -->
        <header class="border-b border-zinc-800 bg-black/50 backdrop-blur-lg sticky top-0 z-50">
            <div class="px-8 py-5 flex justify-between items-center">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-green-500 rounded-2xl flex items-center justify-center text-2xl font-bold">M</div>
                    <h1 class="text-3xl font-bold tracking-tight">MultiChoice</h1>
                </div>
                <nav class="flex gap-8 text-lg">
                    <a href="#" onclick="switchTab(0)" class="tab-link active hover:text-green-400 transition-colors">Trang chủ</a>
                    <a href="#" onclick="switchTab(1)" class="tab-link hover:text-green-400 transition-colors">Lựa chọn thẻ</a>
                    <a href="#" onclick="switchTab(2)" class="tab-link hover:text-green-400 transition-colors">Quiz</a>
                    <a href="#" onclick="switchTab(3)" class="tab-link hover:text-green-400 transition-colors">Bộ lọc</a>
                </nav>
                <div class="flex items-center gap-4">
                    <button onclick="resetAll()" 
                            class="px-5 py-2.5 bg-zinc-800 hover:bg-zinc-700 rounded-2xl flex items-center gap-2 transition-colors">
                        <i class="fas fa-redo"></i>
                        <span>Reset</span>
                    </button>
                </div>
            </div>
        </header>

        <!-- Tab Content -->
        <!-- Tab 0: Trang chủ -->
        <div id="tab-0" class="tab-content p-8">
            <div class="text-center py-16">
                <h1 class="text-6xl font-bold mb-6">Bạn muốn chọn gì hôm nay?</h1>
                <p class="text-2xl text-zinc-400 max-w-2xl mx-auto">
                    Trang web demo với rất nhiều kiểu lựa chọn khác nhau
                </p>
                <div class="mt-10 flex justify-center gap-6">
                    <button onclick="switchTab(1)" 
                            class="px-10 py-5 bg-green-500 hover:bg-green-600 text-black font-semibold text-xl rounded-3xl transition-all flex items-center gap-3">
                        <i class="fas fa-th-large"></i>
                        Xem các lựa chọn thẻ
                    </button>
                </div>
            </div>
        </div>

        <!-- Tab 1: Card Selection -->
        <div id="tab-1" class="tab-content hidden p-8">
            <h2 class="text-4xl font-bold mb-8 flex items-center gap-3">
                <i class="fas fa-grip-vertical text-green-500"></i>
                Chọn nhiều thẻ (Multi Select)
            </h2>
            
            <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 xl:grid-cols-4 gap-6" id="card-container">
                <!-- Cards sẽ được JS tạo -->
            </div>

            <div class="mt-12 bg-zinc-900 rounded-3xl p-8">
                <h3 class="text-2xl font-semibold mb-6">Bạn đã chọn (<span id="selected-count" class="text-green-500">0</span>)</h3>
                <div id="selected-items" class="flex flex-wrap gap-4 min-h-[60px]"></div>
            </div>
        </div>

        <!-- Tab 2: Quiz -->
        <div id="tab-2" class="tab-content hidden p-8">
            <h2 class="text-4xl font-bold mb-8">Multiple Choice Quiz</h2>
            <div class="max-w-2xl mx-auto bg-zinc-900 rounded-3xl p-10">
                <div id="quiz-question" class="text-2xl mb-10"></div>
                
                <div id="quiz-options" class="space-y-4">
                    <!-- JS sẽ render -->
                </div>

                <div class="flex justify-between mt-12">
                    <button onclick="prevQuestion()" 
                            class="px-8 py-4 bg-zinc-800 hover:bg-zinc-700 rounded-2xl">Câu trước</button>
                    <button onclick="nextQuestion()" 
                            class="px-8 py-4 bg-green-500 hover:bg-green-600 text-black rounded-2xl font-semibold">Câu sau</button>
                </div>
            </div>
        </div>

        <!-- Tab 3: Advanced Filter -->
        <div id="tab-3" class="tab-content hidden p-8">
            <h2 class="text-4xl font-bold mb-8">Bộ lọc nâng cao</h2>
            
            <div class="grid grid-cols-1 lg:grid-cols-4 gap-8">
                <!-- Sidebar filters -->
                <div class="lg:col-span-1 space-y-8">
                    <div>
                        <h3 class="text-xl font-semibold mb-4">Giá tiền</h3>
                        <input type="range" min="0" max="100" value="50" 
                               class="w-full accent-green-500" id="price-range"
                               oninput="updatePrice(this.value)">
                        <div class="flex justify-between text-sm text-zinc-400 mt-2">
                            <span>0đ</span>
                            <span id="price-value">50 triệu</span>
                        </div>
                    </div>

                    <div>
                        <h3 class="text-xl font-semibold mb-4">Danh mục</h3>
                        <div class="space-y-3" id="category-filters">
                            <!-- JS render -->
                        </div>
                    </div>
                </div>

                <!-- Results -->
                <div class="lg:col-span-3">
                    <div class="grid grid-cols-1 md:grid-cols-2 gap-6" id="filtered-results">
                        <!-- JS render -->
                    </div>
                </div>
            </div>
        </div>
    </div>

    <script>
        // Tailwind script đã có

        // Data cho Card Selection
        const options = [
            { id: 1, title: "iPhone 16 Pro", desc: "Màu Titan", price: "28.990.000", icon: "📱" },
            { id: 2, title: "MacBook Air M3", desc: "16GB - 512GB", price: "32.990.000", icon: "💻" },
            { id: 3, title: "Sony WH-1000XM5", desc: "Tai nghe chống ồn", price: "7.490.000", icon: "🎧" },
            { id: 4, title: "Samsung Galaxy Watch 7", desc: "44mm", price: "8.990.000", icon: "⌚" },
            { id: 5, title: "DJI Mini 4 Pro", desc: "Flycam", price: "22.500.000", icon: "🚁" },
            { id: 6, title: "PS5 Slim", desc: "Digital Edition", price: "11.990.000", icon: "🎮" },
        ];

        let selectedCards = new Set();

        function createCards() {
            const container = document.getElementById('card-container');
            container.innerHTML = '';
            
            options.forEach(opt => {
                const card = document.createElement('div');
                card.className = `bg-zinc-900 rounded-3xl p-8 card-hover cursor-pointer border-2 border-transparent`;
                card.innerHTML = `
                    <div class="text-6xl mb-6">${opt.icon}</div>
                    <h3 class="text-2xl font-semibold mb-2">${opt.title}</h3>
                    <p class="text-zinc-400 mb-4">${opt.desc}</p>
                    <p class="text-green-400 font-mono text-xl">${opt.price}đ</p>
                `;
                card.onclick = () => toggleCardSelection(card, opt.id);
                container.appendChild(card);
            });
        }

        function toggleCardSelection(cardElement, id) {
            if (selectedCards.has(id)) {
                selectedCards.delete(id);
                cardElement.classList.remove('option-selected');
            } else {
                selectedCards.add(id);
                cardElement.classList.add('option-selected');
            }
            updateSelectedDisplay();
        }

        function updateSelectedDisplay() {
            const countEl = document.getElementById('selected-count');
            const container = document.getElementById('selected-items');
            
            countEl.textContent = selectedCards.size;
            container.innerHTML = '';
            
            Array.from(selectedCards).forEach(id => {
                const item = options.find(o => o.id === id);
                if (!item) return;
                
                const badge = document.createElement('div');
                badge.className = "bg-zinc-800 text-white px-5 py-3 rounded-2xl flex items-center gap-3";
                badge.innerHTML = `
                    <span>${item.icon}</span>
                    <span>${item.title}</span>
                    <button onclick="removeSelected(${id}); event.stopImmediatePropagation();" 
                            class="ml-2 text-red-400 hover:text-red-500">
                        ✕
                    </button>
                `;
                container.appendChild(badge);
            });
        }

        function removeSelected(id) {
            selectedCards.delete(id);
            updateSelectedDisplay();
            // Refresh cards
            createCards();
        }

        // Quiz Data
        let currentQuestion = 0;
        const quizData = [
            {
                q: "Ngôn ngữ lập trình nào phổ biến nhất năm 2025?",
                a: ["Python", "JavaScript", "Java", "C++"],
                correct: 1
            },
            {
                q: "Framework nào dùng để build UI nhanh nhất?",
                a: ["React", "Vue", "Svelte", "Angular"],
                correct: 2
            },
            {
                q: "Bạn thích kiểu thiết kế website nào?",
                a: ["Minimal", "Glassmorphism", "Neubrutalism", "3D"],
                correct: 0
            }
        ];

        function loadQuiz() {
            document.getElementById('quiz-question').textContent = quizData[currentQuestion].q;
            const optionsDiv = document.getElementById('quiz-options');
            optionsDiv.innerHTML = '';
            
            quizData[currentQuestion].a.forEach((ans, index) => {
                const btn = document.createElement('button');
                btn.className = "w-full text-left px-8 py-6 bg-zinc-800 hover:bg-zinc-700 rounded-2xl text-xl transition-all";
                btn.textContent = ans;
                btn.onclick = () => selectAnswer(index);
                optionsDiv.appendChild(btn);
            });
        }

        function selectAnswer(index) {
            alert(`Bạn chọn: ${quizData[currentQuestion].a[index]}\nĐáp án đúng là: ${quizData[currentQuestion].a[quizData[currentQuestion].correct]}`);
        }

        function nextQuestion() {
            currentQuestion = (currentQuestion + 1) % quizData.length;
            loadQuiz();
        }

        function prevQuestion() {
            currentQuestion = (currentQuestion - 1 + quizData.length) % quizData.length;
            loadQuiz();
        }

        // Filter Demo
        function createFilters() {
            const categories = ["Điện thoại", "Laptop", "Tai nghe", "Đồng hồ", "Flycam"];
            const container = document.getElementById('category-filters');
            
            categories.forEach(cat => {
                const div = document.createElement('label');
                div.className = "flex items-center gap-3 cursor-pointer";
                div.innerHTML = `
                    <input type="checkbox" class="w-5 h-5 accent-green-500" onchange="filterResults()">
                    <span class="text-lg">${cat}</span>
                `;
                container.appendChild(div);
            });
        }

        function updatePrice(value) {
            document.getElementById('price-value').textContent = value + " triệu";
            filterResults();
        }

        function filterResults() {
            const container = document.getElementById('filtered-results');
            container.innerHTML = `
                <div class="bg-zinc-900 rounded-3xl p-8 text-center">
                    <p class="text-3xl mb-4">🎉</p>
                    <p class="text-2xl">Kết quả lọc sẽ hiển thị ở đây</p>
                    <p class="text-zinc-500 mt-4">Bạn có thể mở rộng chức năng này dễ dàng</p>
                </div>
            `;
        }

        // Tab management
        function switchTab(tabIndex) {
            document.querySelectorAll('.tab-content').forEach(tab => tab.classList.add('hidden'));
            document.getElementById(`tab-${tabIndex}`).classList.remove('hidden');
            
            // Active link
            document.querySelectorAll('.tab-link').forEach(link => link.classList.remove('active', 'text-green-400'));
            document.querySelectorAll('.tab-link')[tabIndex].classList.add('active', 'text-green-400');
        }

        function resetAll() {
            selectedCards.clear();
            createCards();
            updateSelectedDisplay();
            alert("Đã reset tất cả lựa chọn!");
        }

        // Khởi tạo
        window.onload = () => {
            createCards();
            loadQuiz();
            createFilters();
            filterResults();
            
            // Mặc định mở tab 1
            switchTab(1);
        };
    </script>
</body>
</html>
