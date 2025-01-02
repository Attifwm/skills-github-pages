<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Daily Tracker</title>
    <style>
        /* Base Styles */
        body {
            font-family: 'Arial', sans-serif;
            margin: 0;
            padding: 0;
            background-color: #f4f4f9;
            color: #333;
            line-height: 1.6;
        }
        h1 {
            text-align: center;
            margin-top: 20px;
            color: #4a90e2;
        }
        p {
            text-align: center;
            margin: 0 20px 20px;
            color: #555;
        }

        /* Calendar Styles */
        .calendar-container {
            margin: 0 auto;
            max-width: 500px;
            padding: 20px;
            background: #ffffff;
            box-shadow: 0px 4px 10px rgba(0, 0, 0, 0.1);
            border-radius: 10px;
        }
        .calendar {
            display: grid;
            grid-template-columns: repeat(7, 1fr);
            gap: 10px;
            margin-top: 20px;
        }
        .day {
            width: 100%;
            aspect-ratio: 1; /* Ensures square shapes */
            border-radius: 5px;
            border: 1px solid #ddd;
            display: flex;
            justify-content: center;
            align-items: center;
            cursor: pointer;
            background-color: #f9f9f9;
            font-size: 16px;
            font-weight: bold;
        }
        .day.marked {
            background-color: #4caf50;
            color: #ffffff;
            border: none;
        }

        /* Controls */
        .controls {
            display: flex;
            justify-content: center;
            align-items: center;
            margin-bottom: 20px;
        }
        .controls select {
            padding: 10px;
            font-size: 16px;
            border: 1px solid #ddd;
            border-radius: 5px;
            background-color: #f9f9f9;
            margin-left: 10px;
        }

        /* Overview and Summary */
        .overview, .summary {
            text-align: center;
            margin: 20px 0;
        }
        .overview h2, .summary h2 {
            color: #4a90e2;
        }
        .marked-days, #yearlySavings {
            font-weight: bold;
            color: #4caf50;
        }
        .suggestion {
            margin-top: 10px;
            font-style: italic;
            color: #777;
        }

        /* Mobile-Friendly Adjustments */
        @media (max-width: 600px) {
            h1 {
                font-size: 1.5em;
            }
            .calendar {
                gap: 5px;
            }
            .day {
                font-size: 14px;
            }
            .controls select {
                font-size: 14px;
            }
        }
    </style>
</head>
<body>

<h1>Daily Tracker</h1>
<p>Select a month and click on a day to mark it as completed. Each marked day adds 30 DKK to your savings.</p>

<div class="calendar-container">
    <div class="controls">
        <label for="monthSelect">Select Month:</label>
        <select id="monthSelect">
            <option value="0">January</option>
            <option value="1">February</option>
            <option value="2">March</option>
            <option value="3">April</option>
            <option value="4">May</option>
            <option value="5">June</option>
            <option value="6">July</option>
            <option value="7">August</option>
            <option value="8">September</option>
            <option value="9">October</option>
            <option value="10">November</option>
            <option value="11">December</option>
        </select>
    </div>

    <div class="calendar" id="calendar"></div>
</div>

<div class="overview">
    <h2>Monthly Overview</h2>
    <p><span class="marked-days" id="markedDays">0</span> days marked this month.</p>
    <p>Savings: <span id="savings">0</span> DKK</p>
    <div class="suggestion" id="suggestion">You could save for a cup of coffee per day!</div>
</div>

<div class="summary">
    <h2>Yearly Summary</h2>
    <p>Total Savings for the Year: <span id="yearlySavings">0</span> DKK</p>
</div>

<script>
    const calendar = document.getElementById('calendar');
    const markedDaysElement = document.getElementById('markedDays');
    const savingsElement = document.getElementById('savings');
    const suggestionElement = document.getElementById('suggestion');
    const yearlySavingsElement = document.getElementById('yearlySavings');
    const monthSelect = document.getElementById('monthSelect');

    let markedDaysPerMonth = Array(12).fill(0);
    let totalPerMonth = Array(12).fill(0);

    const suggestions = [
        "Invest in some stocks for your future!",
        "Buy yourself a fancy dinner this month.",
        "Save up for a weekend getaway!",
        "Purchase that gadget you've been eyeing.",
        "Get a subscription to your favorite streaming service.",
        "Donate to a charity close to your heart.",
        "Plan a fun day trip with friends.",
        "Buy a new book or game to enjoy.",
        "Save for a bigger holiday at the end of the year.",
        "Treat yourself to a spa day or self-care items.",
        "Start building your emergency fund.",
        "Save for holiday gifts or celebrations!"
    ];

    function generateCalendar(month) {
        const year = 2025;
        const daysInMonth = new Date(year, month + 1, 0).getDate();

        calendar.innerHTML = '';
        markedDaysElement.textContent = markedDaysPerMonth[month];
        savingsElement.textContent = totalPerMonth[month];
        suggestionElement.textContent = suggestions[month];

        for (let i = 1; i <= daysInMonth; i++) {
            const day = document.createElement('div');
            day.className = 'day';
            day.textContent = i;

            day.addEventListener('click', () => {
                if (!day.classList.contains('marked')) {
                    day.classList.add('marked');
                    totalPerMonth[month] += 30;
                    markedDaysPerMonth[month]++;
                } else {
                    day.classList.remove('marked');
                    totalPerMonth[month] -= 30;
                    markedDaysPerMonth[month]--;
                }
                updateOverview(month);
                updateYearlySummary();
            });

            calendar.appendChild(day);
        }
    }

    function updateOverview(month) {
        markedDaysElement.textContent = markedDaysPerMonth[month];
        savingsElement.textContent = totalPerMonth[month];
        suggestionElement.textContent = suggestions[month];
    }

    function updateYearlySummary() {
        const yearlyTotal = totalPerMonth.reduce((sum, value) => sum + value, 0);
        yearlySavingsElement.textContent = yearlyTotal;
    }

    monthSelect.addEventListener('change', () => {
        const selectedMonth = parseInt(monthSelect.value, 10);
        generateCalendar(selectedMonth);
    });

    generateCalendar(0);
    updateYearlySummary();
</script>

</body>
</html>
