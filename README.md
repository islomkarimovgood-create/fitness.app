<!DOCTYPE html>
<html>
<head>
    <title>FitGenius - Персональный тренер</title>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }
        
        body { 
            font-family: 'Arial', sans-serif; 
            max-width: 400px; 
            margin: 0 auto; 
            padding: 20px; 
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            min-height: 100vh;
        }
        
        .container {
            background: white;
            border-radius: 20px;
            padding: 25px;
            box-shadow: 0 10px 30px rgba(0,0,0,0.2);
            margin-top: 20px;
        }
        
        .header {
            text-align: center;
            margin-bottom: 25px;
        }
        
        .header h1 {
            color: #333;
            font-size: 28px;
            margin-bottom: 5px;
            background: linear-gradient(45deg, #FF6B6B, #4ECDC4);
            -webkit-background-clip: text;
            -webkit-text-fill-color: transparent;
        }
        
        .header p {
            color: #666;
            font-size: 14px;
        }
        
        .subscription { 
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            padding: 25px; 
            border-radius: 15px; 
            margin: 20px 0; 
            color: white;
            text-align: center;
        }
        
        .subscription h3 {
            font-size: 20px;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
        }
        
        button { 
            background: linear-gradient(135deg, #4ECDC4 0%, #44A08D 100%);
            color: white; 
            padding: 15px; 
            border: none; 
            border-radius: 10px; 
            cursor: pointer; 
            margin: 8px 0;
            width: 100%;
            font-size: 16px;
            font-weight: bold;
            transition: all 0.3s ease;
            box-shadow: 0 4px 15px rgba(0,0,0,0.1);
        }
        
        button:hover {
            transform: translateY(-2px);
            box-shadow: 0 6px 20px rgba(0,0,0,0.15);
        }
        
        .premium { 
            background: linear-gradient(135deg, #FF6B6B 0%, #FF8E53 100%);
        }
        
        .premium:hover {
            background: linear-gradient(135deg, #FF8E53 0%, #FF6B6B 100%);
        }
        
        .status { 
            padding: 15px; 
            border-radius: 10px; 
            margin: 15px 0; 
            text-align: center;
            font-weight: bold;
        }
        
        .active { 
            background: linear-gradient(135deg, #a8e6cf 0%, #dcedc1 100%);
            color: #2d6a4f;
            border: 2px solid #74c69d;
        }
        
        .expired { 
            background: linear-gradient(135deg, #ffafbd 0%, #ffc3a0 100%);
            color: #9d0208;
            border: 2px solid #e63946;
        }
        
        .workout-card {
            background: linear-gradient(135deg, #a1c4fd 0%, #c2e9fb 100%);
            border-radius: 15px;
            padding: 20px;
            margin: 15px 0;
            border: 2px solid #90e0ef;
        }
        
        .workout-card h3 {
            color: #03045e;
            margin-bottom: 15px;
            display: flex;
            align-items: center;
            gap: 10px;
        }
        
        .progress-bar {
            background: #e9ecef;
            border-radius: 10px;
            height: 10px;
            margin: 10px 0;
            overflow: hidden;
        }
        
        .progress-fill {
            background: linear-gradient(135deg, #4ECDC4 0%, #44A08D 100%);
            height: 100%;
            border-radius: 10px;
            transition: width 0.5s ease;
        }
        
        .stats {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
            text-align: center;
        }
        
        .stat-item {
            background: white;
            padding: 15px;
            border-radius: 10px;
            box-shadow: 0 3px 10px rgba(0,0,0,0.1);
            flex: 1;
            margin: 0 5px;
        }
        
        .stat-number {
            font-size: 24px;
            font-weight: bold;
            color: #4ECDC4;
        }
        
        .stat-label {
            font-size: 12px;
            color: #666;
            margin-top: 5px;
        }
        
        @media (max-width: 480px) {
            body { 
                max-width: 100%; 
                padding: 15px; 
            }
            .container {
                padding: 20px;
            }
            button { 
                font-size: 16px;
                padding: 18px;
            }
        }
        
        .pulse {
            animation: pulse 2s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .icon {
            font-size: 20px;
        }
    </style>
</head>
<body>
    <div class="container">
        <div class="header">
            <h1>🏋️ FitGenius Pro</h1>
            <p>Твой персональный тренер с AI</p>
        </div>
        
        <div class="stats">
            <div class="stat-item">
                <div class="stat-number" id="workoutsCount">0</div>
                <div class="stat-label">Тренировок</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="daysLeft">30</div>
                <div class="stat-label">Дней осталось</div>
            </div>
            <div class="stat-item">
                <div class="stat-number" id="level">1</div>
                <div class="stat-label">Уровень</div>
            </div>
        </div>
        
        <div id="userStatus" class="status"></div>
        <div id="workoutPlan"></div>
        
        <div class="subscription pulse">
            <h3>💎 Премиум подписка</h3>
            <p>Пробный период: 30 дней бесплатно!</p>
            <div class="progress-bar">
                <div class="progress-fill" id="trialProgress" style="width: 0%"></div>
            </div>
            
            <button onclick="startTrial()">🎁 Начать пробный месяц</button>
            <button onclick="generateWorkout()">📋 Сгенерировать тренировку</button>
            <button onclick="generateCustomWorkout()">🎯 Персональная тренировка</button>
            <button onclick="saveProgress()">✅ Завершить тренировку</button>
            <button onclick="subscribe()" class="premium">💳 Оформить подписку - 299₽/мес</button>
        </div>
    </div>

    <script>
        function startTrial() {
            const trialData = {
                startDate: new Date().toISOString(),
                endDate: new Date(Date.now() + 30 * 24 * 60 * 60 * 1000).toISOString(),
                used: true
            };
            localStorage.setItem('fitnessTrial', JSON.stringify(trialData));
            
            // Устанавливаем счетчик тренировок
            localStorage.setItem('workoutsCompleted', '0');
            localStorage.setItem('userLevel', '1');
            
            showNotification('🎉 Пробный месяц активирован! Теперь ты можешь получать планы тренировок!', 'success');
            updateStatus();
            updateStats();
        }

        function subscribe() {
            showNotification('💳 Премиум функции скоро будут доступны! Следи за обновлениями!', 'info');
        }

        function generateWorkout() {
            const trialData = JSON.parse(localStorage.getItem('fitnessTrial'));
            const workoutPlan = document.getElementById('workoutPlan');
            
            if (!trialData || !trialData.used) {
                showNotification('❌ Сначала активируй пробный период!', 'error');
                return;
            }

            const endDate = new Date(trialData.endDate);
            const now = new Date();

            if (now > endDate) {
                workoutPlan.innerHTML = '<div class="status expired">❌ Пробный период закончился. Оформи подписку!</div>';
                return;
            }

            const workouts = [
                {
                    title: "💪 Силовая тренировка",
                    exercises: "• Приседания: 4х12<br>• Жим лежа: 4х10<br>• Тяга штанги: 4х10<br>• Планка: 3х60 сек<br>• Подъемы на бицепс: 3х12",
                    calories: "🔥 450-550 ккал"
                },
                {
                    title: "🔥 Кардио тренировка", 
                    exercises: "• Берпи: 5х10<br>• Прыжки на скакалке: 5х50<br>• Бег на месте: 5х2 мин<br>• Альпинист: 4х30 сек<br>• Прыжки в планке: 4х20",
                    calories: "🔥 500-600 ккал"
                },
                {
                    title: "🏃 ВИИТ тренировка",
                    exercises: "• Спринты: 10х30 сек<br>• Отжимания: 4х15<br>• Приседания с прыжком: 4х15<br>• Скручивания: 4х20<br>• Бёрпи: 4х12",
                    calories: "🔥 550-650 ккал"
                },
                {
                    title: "🧘 Функциональная тренировка",
                    exercises: "• Выпады: 4х12 на ногу<br>• Подтягивания: 4х8<br>• Пресс: 4х25<br>• Бёрпи: 4х10<br>• Планка-паук: 3х10",
                    calories: "🔥 400-500 ккал"
                }
            ];

            const randomWorkout = workouts[Math.floor(Math.random() * workouts.length)];
            const daysLeft = Math.ceil((endDate - now) / (1000 * 60 * 60 * 24));

            workoutPlan.innerHTML = `
                <div class="workout-card">
                    <h3>${randomWorkout.title}</h3>
                    <div style="background: rgba(255,255,255,0.7); padding: 15px; border-radius: 10px; margin: 10px 0;">
                        ${randomWorkout.exercises}
                    </div>
                    <div style="text-align: center; color: #03045e; font-weight: bold; margin: 10px 0;">
                        ${randomWorkout.calories}
                    </div>
                    <p style="text-align: center; color: #666; font-size: 12px; margin-top: 10px;">
                        ⭐ Пробная версия • Осталось ${daysLeft} дней
                    </p>
                </div>
            `;
        }

        function generateCustomWorkout() {
            const trialData = JSON.parse(localStorage.getItem('fitnessTrial'));
            
            if (!trialData || !trialData.used) {
                showNotification('❌ Сначала активируй пробный период!', 'error');
                return;
            }

            const level = prompt("🏆 Выбери уровень:\n1 - Начинающий 👶\n2 - Средний 💪\n3 - Продвинутый 🦍");
            const goal = prompt("🎯 Выбери цель:\n1 - Похудение 🏃\n2 - Набор массы 🏋️\n3 - Поддержание формы ✅");
            
            let workout = "";
            let title = "";
            let calories = "";

            if (level == "1") {
                title = "💪 Начинающий уровень";
                workout = "• Приседания: 3х15<br>• Отжимания с колен: 3х10<br>• Планка: 3х30 сек<br>• Скручивания: 3х15<br>• Подъемы ног: 3х12";
                calories = "🔥 300-400 ккал";
            } else if (level == "2") {
                title = "🔥 Средний уровень";
                workout = "• Берпи: 4х10<br>• Подтягивания: 3х8<br>• Выпады: 4х12<br>• Пресс: 4х20<br>• Отжимания: 4х12";
                calories = "🔥 450-550 ккал";
            } else {
                title = "🚀 Продвинутый уровень";
                workout = "• Становая тяга: 4х8<br>• Жим штанги: 4х6<br>• Подтягивания с весом: 4х6<br>• Кардио: 20 мин<br>• Бёрпи: 5х15";
                calories = "🔥 600-700 ккал";
            }
            
            document.getElementById('workoutPlan').innerHTML = `
                <div class="workout-card">
                    <h3>${title}</h3>
                    <div style="background: rgba(255,255,255,0.7); padding: 15px; border-radius: 10px; margin: 10px 0;">
                        ${workout}
                    </div>
                    <div style="text-align: center; color: #03045e; font-weight: bold; margin: 10px 0;">
                        ${calories}
                    </div>
                    <p style="text-align: center; color: #666; font-size: 12px;">
                        🎯 Персональная программа • Подходит для твоих целей
                    </p>
                </div>
            `;
        }

        function saveProgress() {
            const currentWorkouts = parseInt(localStorage.getItem('workoutsCompleted') || 0);
            const newWorkouts = currentWorkouts + 1;
            
            localStorage.setItem('workoutsCompleted', newWorkouts.toString());
            
            // Повышаем уровень каждые 5 тренировок
            let userLevel = Math.floor(newWorkouts / 5) + 1;
            if (userLevel > 3) userLevel = 3;
            localStorage.setItem('userLevel', userLevel.toString());
            
            showNotification(`🎊 Отлично! Ты завершил ${newWorkouts} тренировок! Твой уровень: ${userLevel}`, 'success');
            updateStats();
        }

        function updateStatus() {
            const statusElement = document.getElementById('userStatus');
            const trialData = JSON.parse(localStorage.getItem('fitnessTrial'));
            
            if (!trialData || !trialData.used) {
                statusElement.innerHTML = '<div class="status expired">❌ Пробный период не активирован</div>';
                return;
            }

            const endDate = new Date(trialData.endDate);
            const now = new Date();
            const daysLeft = Math.ceil((endDate - now) / (1000 * 60 * 60 * 24));
            const totalDays = 30;
            const progress = ((totalDays - daysLeft) / totalDays) * 100;

            document.getElementById('trialProgress').style.width = progress + '%';

            if (now > endDate) {
                statusElement.innerHTML = '<div class="status expired">❌ Пробный период закончился</div>';
            } else {
                statusElement.innerHTML = `<div class="status active">✅ Пробный период активен • Осталось ${daysLeft} дней</div>`;
            }
        }

        function updateStats() {
            const workoutsCompleted = parseInt(localStorage.getItem('workoutsCompleted') || 0);
            const userLevel = parseInt(localStorage.getItem('userLevel') || 1);
            
            const trialData = JSON.parse(localStorage.getItem('fitnessTrial'));
            let daysLeft = 30;
            if (trialData && trialData.used) {
                const endDate = new Date(trialData.endDate);
                const now = new Date();
                daysLeft = Math.ceil((endDate - now) / (1000 * 60 * 60 * 24));
                if (daysLeft < 0) daysLeft = 0;
            }
            
            document.getElementById('workoutsCount').textContent = workoutsCompleted;
            document.getElementById('daysLeft').textContent = daysLeft;
            document.getElementById('level').textContent = userLevel;
        }

        function showNotification(message, type) {
            alert(message);
        }

        // Запускаем при загрузке страницы
        updateStatus();
        updateStats();
    </script>
</body>
</html>
