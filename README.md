<!DOCTYPE html>
<html lang="zh-CN">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>经期记录助手</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', 'PingFang SC', 'Microsoft YaHei', sans-serif;
            -webkit-tap-highlight-color: transparent;
        }
        
        :root {
            --primary: #ff6b8b;
            --primary-light: #ff8e9e;
            --primary-dark: #e55a7a;
            --secondary: #5a3d5c;
            --light: #fff6f8;
            --dark: #333333;
            --success: #4cd97b;
            --warning: #ffb259;
            --text: #333333;
            --gray: #f0f0f0;
            --border: #e0e0e0;
            --card-shadow: 0 4px 20px rgba(0, 0, 0, 0.08);
        }
        
        body {
            background: linear-gradient(135deg, #fff6f8 0%, #fef0f3 100%);
            min-height: 100vh;
            padding: 15px;
            color: var(--text);
            position: relative;
            overflow-x: hidden;
        }
        
        .container {
            max-width: 500px;
            margin: 0 auto;
            padding-bottom: 80px;
        }
        
        header {
            text-align: center;
            padding: 20px 0 15px;
            position: relative;
        }
        
        .logo {
            width: 70px;
            height: 70px;
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            margin: 0 auto 10px;
            box-shadow: var(--card-shadow);
        }
        
        .logo i {
            font-size: 32px;
            color: white;
        }
        
        h1 {
            font-size: 22px;
            font-weight: 600;
            color: var(--secondary);
            margin-bottom: 5px;
        }
        
        .subtitle {
            color: var(--primary);
            font-size: 14px;
            font-weight: 500;
        }
        
        .card {
            background: white;
            border-radius: 20px;
            padding: 20px;
            margin-bottom: 15px;
            box-shadow: var(--card-shadow);
            position: relative;
            overflow: hidden;
        }
        
        .card-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
            padding-bottom: 12px;
            border-bottom: 1px solid var(--border);
        }
        
        .card-title {
            font-size: 18px;
            font-weight: 600;
            color: var(--secondary);
        }
        
        .status-card {
            text-align: center;
            background: linear-gradient(135deg, var(--primary) 0%, var(--primary-light) 100%);
            color: white;
        }
        
        .status-card .card-header {
            border-bottom: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .status-card .card-title {
            color: white;
        }
        
        .cycle-info {
            display: flex;
            justify-content: space-around;
            margin: 20px 0;
        }
        
        .cycle-item {
            text-align: center;
        }
        
        .cycle-value {
            font-size: 26px;
            font-weight: 700;
            margin-bottom: 5px;
        }
        
        .cycle-label {
            font-size: 13px;
            opacity: 0.9;
        }
        
        .cycle-phase {
            margin-top: 12px;
            font-size: 14px;
            font-weight: 500;
            background: rgba(255, 255, 255, 0.2);
            display: inline-block;
            padding: 6px 15px;
            border-radius: 30px;
        }
        
        .btn {
            display: block;
            width: 100%;
            padding: 15px;
            border: none;
            border-radius: 12px;
            font-size: 16px;
            font-weight: 600;
            cursor: pointer;
            transition: all 0.3s ease;
            text-align: center;
        }
        
        .btn-primary {
            background: var(--primary);
            color: white;
            box-shadow: 0 4px 12px rgba(255, 107, 139, 0.3);
        }
        
        .btn-primary:active {
            background: var(--primary-dark);
            transform: translateY(2px);
        }
        
        .btn-outline {
            background: transparent;
            color: var(--primary);
            border: 2px solid var(--primary);
            margin-top: 12px;
        }
        
        .btn-outline:active {
            background: rgba(255, 107, 139, 0.1);
        }
        
        .period-indicator {
            display: flex;
            flex-wrap: wrap;
            justify-content: center;
            gap: 5px;
            margin: 20px 0;
        }
        
        .day {
            width: 30px;
            height: 30px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 50%;
            font-size: 13px;
            font-weight: 500;
            background: rgba(255, 255, 255, 0.2);
        }
        
        .day.current {
            background: white;
            color: var(--primary);
            font-weight: 700;
            box-shadow: 0 0 0 2px white;
        }
        
        .day.period {
            background: rgba(255, 255, 255, 0.5);
            color: white;
        }
        
        .day.period.current {
            background: white;
            color: var(--primary);
            font-weight: 700;
        }
        
        .day.ovulation {
            background: rgba(76, 217, 123, 0.4);
            color: white;
        }
        
        .history-item {
            display: flex;
            justify-content: space-between;
            padding: 14px 0;
            border-bottom: 1px solid var(--border);
        }
        
        .history-item:last-child {
            border-bottom: none;
        }
        
        .history-date {
            font-weight: 500;
        }
        
        .history-duration {
            color: var(--primary);
            font-weight: 600;
        }
        
        .stats {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 12px;
            margin-top: 15px;
        }
        
        .stat-card {
            background: var(--light);
            border-radius: 15px;
            padding: 15px;
            text-align: center;
        }
        
        .stat-value {
            font-size: 22px;
            font-weight: 700;
            color: var(--primary);
            margin-bottom: 5px;
        }
        
        .stat-label {
            font-size: 13px;
            color: var(--secondary);
        }
        
        .tabs {
            display: flex;
            background: white;
            border-radius: 15px;
            padding: 5px;
            margin-bottom: 15px;
            box-shadow: var(--card-shadow);
        }
        
        .tab {
            flex: 1;
            text-align: center;
            padding: 12px;
            border-radius: 12px;
            font-weight: 500;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 14px;
        }
        
        .tab.active {
            background: var(--primary);
            color: white;
        }
        
        .tab-content {
            display: none;
        }
        
        .tab-content.active {
            display: block;
        }
        
        .modal {
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.7);
            display: flex;
            align-items: center;
            justify-content: center;
            z-index: 1000;
            padding: 20px;
            box-sizing: border-box;
            display: none;
        }
        
        .modal-content {
            background: white;
            border-radius: 20px;
            padding: 25px;
            width: 100%;
            max-width: 400px;
            position: relative;
        }
        
        .modal-header {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 20px;
        }
        
        .modal-title {
            font-size: 20px;
            font-weight: 600;
            color: var(--secondary);
        }
        
        .close-modal {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            background: var(--gray);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--dark);
        }
        
        .date-selector {
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .date-input-group {
            display: flex;
            flex-direction: column;
            gap: 8px;
        }
        
        .date-input-group label {
            font-weight: 500;
            color: var(--dark);
        }
        
        .date-input {
            padding: 12px 15px;
            border: 2px solid var(--border);
            border-radius: 12px;
            font-size: 16px;
            background: white;
        }
        
        .date-input:focus {
            border-color: var(--primary);
            outline: none;
        }
        
        .calendar {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 5px;
            margin-top: 15px;
        }
        
        .calendar-header {
            text-align: center;
            font-weight: 600;
            padding: 8px 0;
            color: var(--secondary);
            font-size: 14px;
        }
        
        .calendar-day {
            height: 40px;
            display: flex;
            align-items: center;
            justify-content: center;
            border-radius: 10px;
            font-size: 14px;
            cursor: pointer;
            transition: all 0.2s ease;
        }
        
        .calendar-day:hover {
            background: var(--light);
        }
        
        .calendar-day.selected {
            background: var(--primary);
            color: white;
            font-weight: 600;
        }
        
        .calendar-day.other-month {
            color: #ccc;
        }
        
        .calendar-nav {
            display: flex;
            justify-content: space-between;
            align-items: center;
            margin-bottom: 15px;
        }
        
        .calendar-month {
            font-size: 18px;
            font-weight: 600;
            color: var(--secondary);
        }
        
        .calendar-btn {
            width: 40px;
            height: 40px;
            border-radius: 50%;
            background: var(--light);
            display: flex;
            align-items: center;
            justify-content: center;
            cursor: pointer;
            color: var(--primary);
            font-size: 18px;
        }
        
        .modal-actions {
            display: flex;
            gap: 10px;
            margin-top: 20px;
        }
        
        .modal-btn {
            flex: 1;
            padding: 14px;
            border-radius: 12px;
            font-weight: 600;
            cursor: pointer;
            border: none;
        }
        
        .modal-btn.primary {
            background: var(--primary);
            color: white;
        }
        
        .modal-btn.secondary {
            background: var(--gray);
            color: var(--dark);
        }
        
        .data-warning {
            background: #fff9e6;
            border-left: 4px solid var(--warning);
            padding: 12px 15px;
            border-radius: 0 10px 10px 0;
            margin: 15px 0;
            font-size: 14px;
        }
        
        footer {
            text-align: center;
            padding: 25px 0 15px;
            color: #999;
            font-size: 12px;
            line-height: 1.6;
        }
        
        .selected-date-display {
            background: rgba(255, 255, 255, 0.2);
            padding: 8px 15px;
            border-radius: 20px;
            margin-top: 10px;
            display: inline-block;
            font-size: 14px;
        }
        
        @media (max-width: 480px) {
            .card {
                padding: 18px;
            }
            
            .cycle-value {
                font-size: 22px;
            }
            
            .day {
                width: 26px;
                height: 26px;
                font-size: 12px;
            }
            
            .modal-content {
                padding: 20px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <div class="logo">
                <i class="fas fa-female"></i>
            </div>
            <h1>经期记录助手</h1>
            <div class="subtitle">记录、追踪、预测您的月经周期</div>
        </header>
        
        <div class="tabs">
            <div class="tab active" data-tab="home">首页</div>
            <div class="tab" data-tab="history">历史记录</div>
            <div class="tab" data-tab="stats">统计</div>
        </div>
        
        <div class="tab-content active" id="home-tab">
            <div class="card status-card">
                <div class="card-header">
                    <div class="card-title">当前状态</div>
                    <div class="cycle-phase">经期第<span id="period-day">3</span>天</div>
                </div>
                
                <div class="cycle-info">
                    <div class="cycle-item">
                        <div class="cycle-value">28</div>
                        <div class="cycle-label">平均周期</div>
                    </div>
                    <div class="cycle-item">
                        <div class="cycle-value">5</div>
                        <div class="cycle-label">经期天数</div>
                    </div>
                    <div class="cycle-item">
                        <div class="cycle-value" id="end-date">8.15</div>
                        <div class="cycle-label">预计结束</div>
                    </div>
                </div>
                
                <div class="period-indicator" id="period-indicator">
                    <!-- 日历日期将由JS生成 -->
                </div>
                
                <div class="card-title" style="margin-top: 20px;">下次经期预测</div>
                <div style="margin-top: 10px; font-size: 18px; font-weight: 600;" id="next-period">8月24日</div>
            </div>
            
            <div class="card">
                <div class="card-header">
                    <div class="card-title">记录经期</div>
                    <i class="fas fa-info-circle" style="color: var(--primary);" id="info-icon"></i>
                </div>
                
                <button class="btn btn-primary" id="record-btn">
                    <i class="fas fa-plus-circle"></i> 记录经期开始
                </button>
                
                <button class="btn btn-outline" id="end-btn">
                    结束本次经期
                </button>
                
                <div class="selected-date-display" id="selected-date-display">
                    未选择日期，默认使用今天
                </div>
                
                <div class="data-warning">
                    所有数据仅存储在您的设备中，清除浏览器缓存会丢失数据
                </div>
            </div>
        </div>
        
        <div class="tab-content" id="history-tab">
            <div class="card">
                <div class="card-header">
                    <div class="card-title">历史记录</div>
                </div>
                
                <div class="history-list" id="history-list">
                    <div class="history-item">
                        <div class="history-date">2025年7月27日 - 7月31日</div>
                        <div class="history-duration">5天</div>
                    </div>
                    <div class="history-item">
                        <div class="history-date">2025年6月30日 - 7月4日</div>
                        <div class="history-duration">5天</div>
                    </div>
                    <div class="history-item">
                        <div class="history-date">2025年6月2日 - 6月6日</div>
                        <div class="history-duration">5天</div>
                    </div>
                </div>
            </div>
        </div>
        
        <div class="tab-content" id="stats-tab">
            <div class="card">
                <div class="card-header">
                    <div class="card-title">周期统计</div>
                </div>
                
                <div class="stats">
                    <div class="stat-card">
                        <div class="stat-value">28</div>
                        <div class="stat-label">平均周期天数</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value">5</div>
                        <div class="stat-label">平均经期天数</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value">28</div>
                        <div class="stat-label">最短周期</div>
                    </div>
                    <div class="stat-card">
                        <div class="stat-value">29</div>
                        <div class="stat-label">最长周期</div>
                    </div>
                </div>
            </div>
        </div>
        
        <footer>
            数据仅存储在您的设备中，保护您的隐私安全<br>
            长按屏幕选择"添加到主屏幕"可创建快捷方式<br>
            © 2025 经期记录助手
        </footer>
    </div>

    <!-- 日期选择模态框 -->
    <div class="modal" id="date-modal">
        <div class="modal-content">
            <div class="modal-header">
                <div class="modal-title">选择经期开始日期</div>
                <div class="close-modal" id="close-modal">
                    <i class="fas fa-times"></i>
                </div>
            </div>
            
            <div class="date-selector">
                <div class="date-input-group">
                    <label>选择日期：</label>
                    <input type="date" id="date-input" class="date-input">
                </div>
                
                <div class="calendar-nav">
                    <div class="calendar-btn" id="prev-month">
                        <i class="fas fa-chevron-left"></i>
                    </div>
                    <div class="calendar-month" id="current-month">2025年8月</div>
                    <div class="calendar-btn" id="next-month">
                        <i class="fas fa-chevron-right"></i>
                    </div>
                </div>
                
                <div class="calendar">
                    <div class="calendar-header">日</div>
                    <div class="calendar-header">一</div>
                    <div class="calendar-header">二</div>
                    <div class="calendar-header">三</div>
                    <div class="calendar-header">四</div>
                    <div class="calendar-header">五</div>
                    <div class="calendar-header">六</div>
                    
                    <!-- 日历日期将由JS生成 -->
                    <div id="calendar-days"></div>
                </div>
                
                <div class="modal-actions">
                    <button class="modal-btn secondary" id="use-today">使用今天</button>
                    <button class="modal-btn primary" id="confirm-date">确认日期</button>
                </div>
            </div>
        </div>
    </div>

    <script>
        // 当前选中的经期开始日期
        let selectedStartDate = null;
        
        // DOM元素
        const dateModal = document.getElementById('date-modal');
        const closeModal = document.getElementById('close-modal');
        const recordBtn = document.getElementById('record-btn');
        const confirmDateBtn = document.getElementById('confirm-date');
        const useTodayBtn = document.getElementById('use-today');
        const dateInput = document.getElementById('date-input');
        const selectedDateDisplay = document.getElementById('selected-date-display');
        const periodDayElement = document.getElementById('period-day');
        const endDateElement = document.getElementById('end-date');
        const nextPeriodElement = document.getElementById('next-period');
        const calendarDays = document.getElementById('calendar-days');
        const currentMonthElement = document.getElementById('current-month');
        const prevMonthBtn = document.getElementById('prev-month');
        const nextMonthBtn = document.getElementById('next-month');
        const periodIndicator = document.getElementById('period-indicator');
        
        // 当前显示的日历月份
        let currentCalendarDate = new Date();
        
        // 初始化日期输入为今天
        function initDate() {
            const today = new Date();
            dateInput.valueAsDate = today;
            selectedStartDate = today;
            updateSelectedDateDisplay();
        }
        
        // 更新选中的日期显示
        function updateSelectedDateDisplay() {
            if (selectedStartDate) {
                const dateStr = formatDate(selectedStartDate);
                selectedDateDisplay.textContent = `已选择: ${dateStr}`;
                
                // 计算经期天数（从开始日期到今天）
                const today = new Date();
                const diffTime = Math.abs(today - selectedStartDate);
                const diffDays = Math.ceil(diffTime / (1000 * 60 * 60 * 24)) + 1;
                periodDayElement.textContent = diffDays;
                
                // 更新预计结束日期（假设经期持续5天）
                const endDate = new Date(selectedStartDate);
                endDate.setDate(endDate.getDate() + 5);
                endDateElement.textContent = formatShortDate(endDate);
                
                // 更新下次经期预测（假设周期28天）
                const nextPeriod = new Date(selectedStartDate);
                nextPeriod.setDate(nextPeriod.getDate() + 28);
                nextPeriodElement.textContent = formatMonthDate(nextPeriod);
                
                // 更新日历指示器
                updatePeriodIndicator();
            }
        }
        
        // 日期格式化
        function formatDate(date) {
            return date.toLocaleDateString('zh-CN', {
                year: 'numeric',
                month: 'long',
                day: 'numeric'
            });
        }
        
        function formatShortDate(date) {
            return `${date.getMonth() + 1}.${date.getDate()}`;
        }
        
        function formatMonthDate(date) {
            return `${date.getMonth() + 1}月${date.getDate()}日`;
        }
        
        // 生成日历
        function generateCalendar(date) {
            const year = date.getFullYear();
            const month = date.getMonth();
            
            // 更新当前月份显示
            currentMonthElement.textContent = `${year}年${month + 1}月`;
            
            // 获取当月第一天
            const firstDay = new Date(year, month, 1);
            // 获取当月最后一天
            const lastDay = new Date(year, month + 1, 0);
            // 获取当月第一天是星期几（0为星期日）
            const firstDayOfWeek = firstDay.getDay();
            // 获取当月天数
            const daysInMonth = lastDay.getDate();
            
            // 清空日历
            calendarDays.innerHTML = '';
            
            // 添加上个月的天数
            const prevMonthLastDay = new Date(year, month, 0).getDate();
            for (let i = firstDayOfWeek - 1; i >= 0; i--) {
                const day = document.createElement('div');
                day.className = 'calendar-day other-month';
                day.textContent = prevMonthLastDay - i;
                calendarDays.appendChild(day);
            }
            
            // 添加当月的天数
            for (let i = 1; i <= daysInMonth; i++) {
                const day = document.createElement('div');
                day.className = 'calendar-day';
                day.textContent = i;
                
                // 检查是否是选中的日期
                if (selectedStartDate && 
                    selectedStartDate.getFullYear() === year &&
                    selectedStartDate.getMonth() === month &&
                    selectedStartDate.getDate() === i) {
                    day.classList.add('selected');
                }
                
                day.addEventListener('click', () => {
                    // 移除之前选中的日期
                    document.querySelectorAll('.calendar-day.selected').forEach(d => {
                        d.classList.remove('selected');
                    });
                    
                    // 选中当前日期
                    day.classList.add('selected');
                    
                    // 更新选中的日期
                    selectedStartDate = new Date(year, month, i);
                    dateInput.valueAsDate = selectedStartDate;
                });
                
                calendarDays.appendChild(day);
            }
            
            // 添加下个月的天数（补齐42个格子）
            const totalCells = 42; // 6行 * 7列
            const daysSoFar = firstDayOfWeek + daysInMonth;
            const daysNextMonth = totalCells - daysSoFar;
            
            for (let i = 1; i <= daysNextMonth; i++) {
                const day = document.createElement('div');
                day.className = 'calendar-day other-month';
                day.textContent = i;
                calendarDays.appendChild(day);
            }
        }
        
        // 更新经期指示器
        function updatePeriodIndicator() {
            if (!selectedStartDate) return;
            
            periodIndicator.innerHTML = '';
            
            const today = new Date();
            const startDate = new Date(selectedStartDate);
            const endDate = new Date(startDate);
            endDate.setDate(endDate.getDate() + 5); // 假设经期持续5天
            
            // 排卵日（经期开始后14天）
            const ovulationDate = new Date(startDate);
            ovulationDate.setDate(ovulationDate.getDate() + 14);
            
            // 显示最近30天的日历
            for (let i = -15; i <= 15; i++) {
                const date = new Date();
                date.setDate(date.getDate() + i);
                
                const day = document.createElement('div');
                day.className = 'day';
                day.textContent = date.getDate();
                
                // 标记今天
                if (date.toDateString() === today.toDateString()) {
                    day.classList.add('current');
                }
                
                // 标记经期
                if (date >= startDate && date <= endDate) {
                    day.classList.add('period');
                }
                
                // 标记排卵日
                if (date.toDateString() === ovulationDate.toDateString()) {
                    day.classList.add('ovulation');
                }
                
                periodIndicator.appendChild(day);
            }
        }
        
        // 初始化
        document.addEventListener('DOMContentLoaded', () => {
            initDate();
            generateCalendar(currentCalendarDate);
            updatePeriodIndicator();
            
            // 事件监听
            recordBtn.addEventListener('click', () => {
                dateModal.style.display = 'flex';
            });
            
            closeModal.addEventListener('click', () => {
                dateModal.style.display = 'none';
            });
            
            confirmDateBtn.addEventListener('click', () => {
                // 使用日期输入框的值
                selectedStartDate = new Date(dateInput.value);
                dateModal.style.display = 'none';
                updateSelectedDateDisplay();
            });
            
            useTodayBtn.addEventListener('click', () => {
                selectedStartDate = new Date();
                dateInput.valueAsDate = selectedStartDate;
                updateSelectedDateDisplay();
                dateModal.style.display = 'none';
            });
            
            dateInput.addEventListener('change', () => {
                selectedStartDate = new Date(dateInput.value);
                generateCalendar(currentCalendarDate);
            });
            
            prevMonthBtn.addEventListener('click', () => {
                currentCalendarDate.setMonth(currentCalendarDate.getMonth() - 1);
                generateCalendar(currentCalendarDate);
            });
            
            nextMonthBtn.addEventListener('click', () => {
                currentCalendarDate.setMonth(currentCalendarDate.getMonth() + 1);
                generateCalendar(currentCalendarDate);
            });
            
            // 标签切换功能
            document.querySelectorAll('.tab').forEach(tab => {
                tab.addEventListener('click', () => {
                    // 移除所有活动标签
                    document.querySelectorAll('.tab').forEach(t => t.classList.remove('active'));
                    document.querySelectorAll('.tab-content').forEach(c => c.classList.remove('active'));
                    
                    // 添加当前活动标签
                    tab.classList.add('active');
                    const tabId = `${tab.dataset.tab}-tab`;
                    document.getElementById(tabId).classList.add('active');
                });
            });
        });
    </script>
</body>
</html>
