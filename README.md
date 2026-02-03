<!DOCTYPE html>
<html lang="ar" dir="rtl">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>تطبيق ترتيب عادتي اليومية - Pro</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #0f172a 0%, #1e3a8a 100%);
            min-height: 100vh;
            padding: 20px;
            direction: rtl;
        }

        .container {
            max-width: 1100px;
            margin: 0 auto;
            background: rgba(255, 255, 255, 0.95);
            backdrop-filter: blur(12px);
            border-radius: 25px;
            box-shadow: 0 30px 60px rgba(0, 0, 0, 0.3);
            padding: 40px;
            border: 1px solid rgba(255, 255, 255, 0.2);
        }

        h1 {
            text-align: center;
            color: #1e3a8a;
            margin-bottom: 15px;
            font-size: 3em;
            font-weight: 800;
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }

        .quote {
            text-align: center;
            color: #4a5568;
            font-style: italic;
            margin-bottom: 35px;
            padding: 20px;
            background: rgba(240, 248, 255, 0.8);
            border-radius: 15px;
            border-right: 6px solid #1e3a8a;
            font-size: 1.2em;
            font-weight: 500;
        }

        /* لوحة الإحصائيات والتقدم */
        .stats-dashboard {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin-bottom: 35px;
        }

        .stat-card {
            background: white;
            padding: 25px;
            border-radius: 18px;
            text-align: center;
            box-shadow: 0 8px 25px rgba(0, 0, 0, 0.1);
            transition: all 0.3s;
            position: relative;
            overflow: hidden;
        }

        .stat-card:hover {
            transform: translateY(-8px);
            box-shadow: 0 15px 35px rgba(0, 0, 0, 0.15);
        }

        .stat-card::before {
            content: '';
            position: absolute;
            top: 0;
            right: 0;
            width: 6px;
            height: 100%;
            background: linear-gradient(to bottom, #3b82f6, #1e3a8a);
        }

        .stat-emoji {
            font-size: 3em;
            margin-bottom: 10px;
            display: block;
        }

        .stat-number {
            font-size: 2.5em;
            font-weight: bold;
            color: #1e3a8a;
            margin-bottom: 5px;
        }

        .stat-label {
            color: #718096;
            font-size: 1em;
            font-weight: 500;
        }

        /* شريط التقدم المرئي */
        .progress-section {
            margin-bottom: 35px;
        }

        .progress-bar-container {
            background: #e2e8f0;
            border-radius: 50px;
            height: 35px;
            overflow: hidden;
            position: relative;
            box-shadow: inset 0 2px 5px rgba(0, 0, 0, 0.1);
        }

        .progress-bar-fill {
            background: linear-gradient(90deg, #3b82f6, #8b5cf6, #10b981);
            height: 100%;
            border-radius: 50px;
            transition: width 0.8s cubic-bezier(0.4, 0, 0.2, 1);
            display: flex;
            align-items: center;
            justify-content: flex-end;
            padding-right: 20px;
            color: white;
            font-weight: bold;
            font-size: 1.1em;
            box-shadow: 0 0 20px rgba(59, 130, 246, 0.4);
        }

        .progress-text {
            text-align: center;
            margin-top: 15px;
            font-size: 1.2em;
            color: #4a5568;
            font-weight: 500;
        }

        /* سجل التقدم اليومي */
        .progress-history {
            background: rgba(240, 248, 255, 0.9);
            padding: 25px;
            border-radius: 18px;
            margin-bottom: 35px;
            border-right: 6px solid #1e3a8a;
        }

        .history-grid {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(40px, 1fr));
            gap: 8px;
            margin-top: 15px;
        }

        .history-day {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            font-size: 0.9em;
            transition: all 0.3s;
            cursor: pointer;
        }

        .history-day.completed {
            background: linear-gradient(135deg, #10b981, #059669);
            color: white;
            transform: scale(1.1);
            box-shadow: 0 4px 10px rgba(16, 185, 129, 0.4);
        }

        .history-day.pending {
            background: #e2e8f0;
            color: #718096;
        }

        .history-day:hover {
            transform: scale(1.15);
        }

        /* إشعار التحفيز الإيموجي */
        .motivation-popup {
            position: fixed;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%) scale(0);
            background: white;
            padding: 40px;
            border-radius: 25px;
            box-shadow: 0 25px 50px rgba(0, 0, 0, 0.3);
            z-index: 1000;
            text-align: center;
            display: none;
            transition: transform 0.5s cubic-bezier(0.68, -0.55, 0.265, 1.55);
        }

        .motivation-popup.show {
            display: block;
            transform: translate(-50%, -50%) scale(1);
        }

        .motivation-emoji {
            font-size: 5em;
            margin-bottom: 20px;
            animation: bounce 1s infinite;
        }

        @keyframes bounce {
            0%, 100% { transform: translateY(0); }
            50% { transform: translateY(-20px); }
        }

        .motivation-text {
            font-size: 1.5em;
            color: #1e3a8a;
            font-weight: 700;
            margin-bottom: 15px;
        }

        .overlay {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 999;
            display: none;
        }

        .overlay.show {
            display: block;
        }

        .add-habit-form {
            background: rgba(240, 248, 255, 0.9);
            padding: 25px;
            border-radius: 15px;
            margin-bottom: 35px;
            border-right: 6px solid #1e3a8a;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .form-group {
            display: flex;
            gap: 15px;
            margin-bottom: 20px;
            flex-wrap: wrap;
        }

        input[type="text"], select {
            padding: 14px;
            border: 2px solid #cbd5e0;
            border-radius: 10px;
            font-size: 16px;
            flex: 1;
            min-width: 180px;
            background: white;
        }

        button {
            padding: 14px 30px;
            background: linear-gradient(135deg, #1e3a8a, #3b82f6);
            color: white;
            border: none;
            border-radius: 10px;
            font-size: 17px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 600;
            width: 100%;
        }

        .habits-list {
            margin-top: 25px;
        }

        .habit-item {
            display: flex;
            align-items: center;
            padding: 18px;
            margin-bottom: 12px;
            background: white;
            border-radius: 12px;
            transition: all 0.3s;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.07);
            border-right: 4px solid #e2e8f0;
        }

        .habit-checkbox {
            width: 28px;
            height: 28px;
            margin-left: 20px;
            cursor: pointer;
            accent-color: #1e3a8a;
        }

        .habit-name {
            flex: 1;
            font-size: 19px;
            color: #2d3748;
            font-weight: 600;
        }

        .habit-name.completed {
            text-decoration: line-through;
            color: #718096;
        }

        .delete-btn {
            background: #e53e3e;
            color: white;
            padding: 10px 18px;
            border-radius: 8px;
            font-size: 15px;
            cursor: pointer;
            transition: all 0.3s;
            font-weight: 500;
        }
    </style>
</head>
<body>
    <div class="container">
        <h1>📋 تطبيق ترتيب عادتي اليومية</h1>
        
        <div class="quote" id="dailyQuote"></div>

        <!-- لوحة الإحصائيات والتقدم -->
        <div class="stats-dashboard">
            <div class="stat-card">
                <span class="stat-emoji">🎯</span>
                <div class="stat-number" id="dailyGoal">0%</div>
                <div class="stat-label">هدف اليوم</div>
            </div>
            <div class="stat-card">
                <span class="stat-emoji">🔥</span>
                <div class="stat-number" id="bestStreak">0</div>
                <div class="stat-label">أطول سلسلة</div>
            </div>
            <div class="stat-card">
                <span class="stat-emoji">📅</span>
                <div class="stat-number" id="monthlyAverage">0%</div>
                <div class="stat-label">متوسط الشهر</div>
            </div>
            <div class="stat-card">
                <span class="stat-emoji">🏆</span>
                <div class="stat-number" id="totalPoints">0</div>
                <div class="stat-label">نقاطك</div>
            </div>
        </div>

        <!-- شريط التقدم المرئي -->
        <div class="progress-section">
            <h3 style="color: #1e3a8a; margin-bottom: 10px;">📊 تقدمك اليومي</h3>
            <div class="progress-bar-container">
                <div class="progress-bar-fill" id="progressBarFill">0%</div>
            </div>
            <div class="progress-text" id="progressText">أكمل 0 من 0 عادات اليوم</div>
        </div>

        <!-- سجل التقدم -->
        <div class="progress-history">
            <h3 style="color: #1e3a8a; margin-bottom: 10px;">📈 سجل إنجازات الـ30 يوم الأخيرة</h3>
            <div class="history-grid" id="historyGrid"></div>
            <div style="text-align: center; margin-top: 15px; color: #718096;">
                <span style="display: inline-block; margin: 0 10px;">🟢 مكتمل</span>
                <span style="display: inline-block; margin: 0 10px;">⚪ قيد الانتظار</span>
            </div>
        </div>

        <!-- إضافة عادة -->
        <div class="add-habit-form">
            <h3>➕ إضافة عادة جديدة</h3>
            <div class="form-group">
                <input type="text" id="habitName" placeholder="اسم العادة (مثال: ممارسة الرياضة)">
                <select id="habitPriority">
                    <option value="2">متوسطة الأهمية</option>
                    <option value="1">عالية الأهمية</option>
                    <option value="3">منخفضة الأهمية</option>
                </select>
            </div>
            <button onclick="addHabit()">إضافة العادة</button>
        </div>

        <div class="habits-list" id="habitsList"></div>
    </div>

    <!-- إشعار التحفيز الإيموجي -->
    <div class="overlay" id="overlay"></div>
    <div class="motivation-popup" id="motivationPopup">
        <div class="motivation-emoji" id="motivationEmoji"></div>
        <div class="motivation-text" id="motivationText"></div>
        <div style="color: #718096; margin-bottom: 20px;">استمر في العمل الجيد! 💪</div>
        <button onclick="closeMotivation()" style="width: auto; padding: 12px 30px;">رائع!</button>
    </div>

    <script>
        let habits = JSON.parse(localStorage.getItem('habits')) || [];
        const quotes = [
            "العادة ليست شيئاً تفعله، بل هي ما أنت عليه 💫",
            "النجاح هو مجموعة من العادات الصغيرة المت repeated يومياً 🌟",
            "لا تنتظر الحافز، ابدأ بالعمل والحافز سيلحق بك 🚀",
            "أفضل وقت لزرع شجرة كان قبل 20 سنة، ثاني أفضل وقت هو الآن 🌱"
        ];

        // **رسائل التحفيز الإيموجي**
        const motivationMessages = [
            { emoji: "🎉", text: "إنجاز رائع! أنت مذهل!" },
            { emoji: "🔥", text: "سلسلة الإنجازات تستمر!" },
            { emoji: "💪", text: "قوة الإرادة لديك لا تقهر!" },
            { emoji: "⭐", text: "نجمة اليوم أنت!" },
            { emoji: "🌟", text: "تألقك مستمر!" },
            { emoji: "🏆", text: "إنجاز جديد إلى مجموعتك!" },
            { emoji: "🚀", text: "أنت في طريقك إلى القمة!" },
            { emoji: "🌈", text: "كل يوم ألوان جديدة من النجاح!" }
        ];

        // **طلب إذن الإشعارات**
        if ('Notification' in window && Notification.permission === 'default') {
            Notification.requestPermission();
        }

        function init() {
            document.getElementById('dailyQuote').textContent = quotes[Math.floor(Math.random() * quotes.length)];
            renderHabits();
            updateStats();
            renderHistory();
            setupDailyReminder();
        }

        function addHabit() {
            const name = document.getElementById('habitName').value.trim();
            const priority = document.getElementById('habitPriority').value;

            if (!name) {
                alert('⚠️ الرجاء كتابة اسم العادة!');
                return;
            }

            const newHabit = {
                id: Date.now(),
                name: name,
                priority: priority,
                createdAt: new Date().toISOString(),
                completedDates: []
            };

            habits.push(newHabit);
            localStorage.setItem('habits', JSON.stringify(habits));
            
            document.getElementById('habitName').value = '';
            
            renderHabits();
            updateStats();
            showMotivation("🎯", "عادة جديدة أضيفت! استمر في التميز!");
            
            // **إشعار تأكيد**
            if (Notification.permission === 'granted') {
                new Notification('✅ تمت إضافة عادة جديدة', {
                    body: `"${name}" أصبحت جزءاً من روتينك اليومي`,
                    icon: '📋'
                });
            }
        }

        function deleteHabit(id) {
            if (confirm('هل أنت متأكد من حذف هذه العادة؟')) {
                const habit = habits.find(h => h.id === id);
                habits = habits.filter(h => h.id !== id);
                localStorage.setItem('habits', JSON.stringify(habits));
                renderHabits();
                updateStats();
                renderHistory();
                
                // **إشعار الحذف**
                if (Notification.permission === 'granted') {
                    new Notification('🗑️ تم حذف العادة', {
                        body: `"${habit.name}" تمت إزالتها`,
                        icon: '📋'
                    });
                }
            }
        }

        function toggleHabit(id) {
            const habit = habits.find(h => h.id === id);
            const today = new Date().toDateString();
            
            if (habit.completedDates.includes(today)) {
                habit.completedDates = habit.completedDates.filter(d => d !== today);
                localStorage.setItem('habits', JSON.stringify(habits));
                renderHabits();
                updateStats();
                renderHistory();
            } else {
                habit.completedDates.push(today);
                localStorage.setItem('habits', JSON.stringify(habits));
                renderHabits();
                updateStats();
                renderHistory();
                
                // **إشعار إنجاز + رسالة تحفيز**
                const randomMsg = motivationMessages[Math.floor(Math.random() * motivationMessages.length)];
                showMotivation(randomMsg.emoji, randomMsg.text);
                
                if (Notification.permission === 'granted') {
                    new Notification('🎉 إنجاز رائع!', {
                        body: `لقد أكملت: "${habit.name}"`,
                        icon: '✅'
                    });
                }
            }
        }

        // **إعداد تذكير يومي تلقائي**
        function setupDailyReminder() {
            if ('Notification' in window && Notification.permission === 'granted') {
                setInterval(() => {
                    const now = new Date();
                    // تذكير الساعة 8 صباحاً
                    if (now.getHours() === 8 && now.getMinutes() === 0) {
                        const completedToday = habits.filter(h => h.completedDates.includes(now.toDateString())).length;
                        if (completedToday < habits.length) {
                            new Notification('📅 صباح الخير! وقت العادات', {
                                body: `لديك ${habits.length - completedToday} عادات اليوم. لنبدأ!`,
                                icon: '🌅'
                            });
                        }
                    }
                }, 60000); // تحديث كل دقيقة
            }
        }

        // **تحديث جميع الإحصائيات**
        function updateStats() {
            const today = new Date().toDateString();
            const totalHabits = habits.length;
            const completedToday = habits.filter(h => h.completedDates.includes(today)).length;
            const dailyProgress = totalHabits > 0 ? Math.round((completedToday / totalHabits) * 100) : 0;
            
            const bestStreak = Math.max(...habits.map(h => calculateStreak(h)), 0);
            const monthlyAverage = calculateMonthlyAverage();
            const totalPoints = habits.reduce((sum, h) => sum + (h.completedDates.length * 10), 0);

            document.getElementById('dailyGoal').textContent = dailyProgress + '%';
            document.getElementById('bestStreak').textContent = bestStreak;
            document.getElementById('monthlyAverage').textContent = monthlyAverage + '%';
            document.getElementById('totalPoints').textContent = totalPoints;

            document.getElementById('progressBarFill').style.width = dailyProgress + '%';
            document.getElementById('progressBarFill').textContent = dailyProgress + '%';
            document.getElementById('progressText').textContent = `أكمل ${completedToday} من ${totalHabits} عادات اليوم ${getProgressEmoji(dailyProgress)}`;
        }

        function calculateMonthlyAverage() {
            if (habits.length === 0) return 0;
            
            const days = 30;
            let totalDaysCompleted = 0;
            
            for (let i = 0; i < days; i++) {
                const date = new Date();
                date.setDate(date.getDate() - i);
                const dateString = date.toDateString();
                
                const completedHabits = habits.filter(h => h.completedDates.includes(dateString)).length;
                if (completedHabits > 0) totalDaysCompleted++;
            }
            
            return Math.round((totalDaysCompleted / days) * 100);
        }

        function renderHistory() {
            const historyGrid = document.getElementById('historyGrid');
            historyGrid.innerHTML = '';
            
            const today = new Date();
            
            for (let i = 29; i >= 0; i--) {
                const date = new Date(today);
                date.setDate(today.getDate() - i);
                const dateString = date.toDateString();
                const dayNumber = date.getDate();
                
                const completedHabits = habits.filter(h => h.completedDates.includes(dateString)).length;
                const isCompleted = completedHabits > 0;
                
                const dayElement = document.createElement('div');
                dayElement.className = `history-day ${isCompleted ? 'completed' : 'pending'}`;
                dayElement.textContent = dayNumber;
                dayElement.title = `${date.toLocaleDateString('ar-SA')}: ${completedHabits} عادات`;
                
                historyGrid.appendChild(dayElement);
            }
        }

        function getProgressEmoji(progress) {
            if (progress >= 100) return "🏆🎉";
            if (progress >= 75) return "🔥💪";
            if (progress >= 50) return "⭐👍";
            if (progress >= 25) return "😊✨";
            if (progress > 0) return "🌟🚀";
            return "📌";
        }

        function showMotivation(emoji, text) {
            const popup = document.getElementById('motivationPopup');
            const overlay = document.getElementById('overlay');
            
            document.getElementById('motivationEmoji').textContent = emoji;
            document.getElementById('motivationText').textContent = text;
            
            overlay.classList.add('show');
            popup.classList.add('show');
            
            setTimeout(closeMotivation, 3000);
        }

        function closeMotivation() {
            document.getElementById('motivationPopup').classList.remove('show');
            document.getElementById('overlay').classList.remove('show');
        }

        function calculateStreak(habit) {
            let streak = 0;
            const today = new Date();
            
            for (let i = 0; i < 365; i++) {
                const checkDate = new Date(today);
                checkDate.setDate(today.getDate() - i);
                const dateString = checkDate.toDateString();
                if (habit.completedDates.includes(dateString)) {
                    streak++;
                } else if (i > 0) break;
            }
            return streak;
        }

        function renderHabits() {
            const listContainer = document.getElementById('habitsList');
            const today = new Date().toDateString();
            
            listContainer.innerHTML = '';

            if (habits.length === 0) {
                listContainer.innerHTML = '<p style="text-align: center; color: #718096; padding: 40px; font-size: 1.2em;">📝 لا توجد عادات بعد. أضف أول عادة لك!</p>';
                return;
            }

            const sortedHabits = [...habits].sort((a, b) => a.priority - b.priority);

            sortedHabits.forEach(habit => {
                const streak = calculateStreak(habit);
                const isCompleted = habit.completedDates.includes(today);
                
                const habitElement = document.createElement('div');
                habitElement.className = 'habit-item';
                habitElement.innerHTML = `
                    <input type="checkbox" class="habit-checkbox" 
                           ${isCompleted ? 'checked' : ''} 
                           onchange="toggleHabit(${habit.id})">
                    <span class="habit-name ${isCompleted ? 'completed' : ''}">${habit.name}</span>
                    ${streak > 0 ? `<span style="background: linear-gradient(135deg, #1e3a8a, #3b82f6); color: white; padding: 8px 18px; border-radius: 25px; font-size: 14px; font-weight: 600;">🔥 ${streak} أيام</span>` : ''}
                    <span class="delete-btn" onclick="deleteHabit(${habit.id})">حذف</span>
                `;
                listContainer.appendChild(habitElement);
            });
        }

        window.onload = init;
    </script>
</body>
</html>
