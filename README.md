<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Employee Assessment Games</title>
    <style>
        /* General Styles */
        body {
            font-family: Arial, sans-serif;
            text-align: center;
            background-color: #f4f4f4;
        }
        h1, h2 {
            margin: 20px 0;
        }
        .game-container {
            margin: 20px auto;
            max-width: 600px;
        }
        /* Memory Matching Game Styles */
        #memory-grid {
            display: grid;
            grid-template-columns: repeat(4, 100px);
            grid-gap: 10px;
            justify-content: center;
            margin-bottom: 20px;
        }
        .memory-tile {
            width: 100px;
            height: 100px;
            background-color: #bbb;
            cursor: pointer;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 24px;
            color: white;
        }
        .hidden {
            background-color: #555;
        }
        .matched {
            background-color: #4CAF50;
        }
        /* Resource Allocation Styles */
        .project {
            margin: 15px 0;
            padding: 10px;
            border: 1px solid #ccc;
            background-color: #fff;
        }
        .resource-input {
            margin-bottom: 10px;
        }
        /* Crisis Management Styles */
        .crisis {
            margin: 15px 0;
            padding: 10px;
            border: 1px solid #ccc;
            background-color: #fff;
        }
    </style>
</head>
<body>
    <h1>Employee Assessment Games</h1>

    <!-- Memory Matching Game -->
    <div class="game-container" id="memory-matching-game">
        <h2>Memory Matching Game</h2>
        <div id="memory-grid"></div>
        <button onclick="startMemoryGame()">Start Memory Game</button>
        <p id="memory-timer">Time Left: 60s</p>
        <p id="memory-score">Score: 0</p>
    </div>

    <!-- Resource Allocation Simulation -->
    <div class="game-container" id="resource-allocation-game">
        <h2>Resource Allocation Simulation</h2>
        <p>You have 100 hours, $50,000, and 10 team members to allocate.</p>
        <div id="project-a" class="project">
            <h3>Project A</h3>
            <p>Requirements: 40 hours, $20,000, 3 team members</p>
            <input type="number" placeholder="Hours" class="resource-input" id="hours-a">
            <input type="number" placeholder="Budget" class="resource-input" id="budget-a">
            <input type="number" placeholder="Team members" class="resource-input" id="team-a">
        </div>
        <div id="project-b" class="project">
            <h3>Project B</h3>
            <p>Requirements: 30 hours, $15,000, 4 team members</p>
            <input type="number" placeholder="Hours" class="resource-input" id="hours-b">
            <input type="number" placeholder="Budget" class="resource-input" id="budget-b">
            <input type="number" placeholder="Team members" class="resource-input" id="team-b">
        </div>
        <div id="project-c" class="project">
            <h3>Project C</h3>
            <p>Requirements: 50 hours, $30,000, 5 team members</p>
            <input type="number" placeholder="Hours" class="resource-input" id="hours-c">
            <input type="number" placeholder="Budget" class="resource-input" id="budget-c">
            <input type="number" placeholder="Team members" class="resource-input" id="team-c">
        </div>
        <button onclick="startResourceGame()">Submit Resource Allocation</button>
        <p id="resource-result"></p>
    </div>

    <!-- Crisis Management Game -->
    <div class="game-container" id="crisis-management-game">
        <h2>Crisis Management</h2>
        <div id="crisis-a" class="crisis">
            <h3>Crisis A</h3>
            <p>Supplier failed to deliver on time. Choose a solution:</p>
            <button onclick="chooseCrisisOption(1, 'A')">Find new supplier</button>
            <button onclick="chooseCrisisOption(2, 'A')">Delay product launch</button>
            <button onclick="chooseCrisisOption(3, 'A')">Negotiate partial delivery</button>
        </div>
        <div id="crisis-b" class="crisis">
            <h3>Crisis B</h3>
            <p>Two team members sick. Choose a solution:</p>
            <button onclick="chooseCrisisOption(1, 'B')">Redistribute workload</button>
            <button onclick="chooseCrisisOption(2, 'B')">Bring in temp staff</button>
            <button onclick="chooseCrisisOption(3, 'B')">Delay non-essential tasks</button>
        </div>
        <div id="crisis-c" class="crisis">
            <h3>Crisis C</h3>
            <p>Website outages. Choose a solution:</p>
            <button onclick="chooseCrisisOption(1, 'C')">Fix immediately</button>
            <button onclick="chooseCrisisOption(2, 'C')">Apologize to customers</button>
            <button onclick="chooseCrisisOption(3, 'C')">Shift resources to IT</button>
        </div>
        <p id="crisis-result"></p>
    </div>

    <script>
        // Memory Matching Game Logic (already implemented)
        let memoryTiles = [];
        let firstTile = null;
        let secondTile = null;
        let matchedPairs = 0;
        let memoryTimeLeft = 60;
        let memoryTimerInterval;

        function startMemoryGame() {
            memoryTiles = [];
            matchedPairs = 0;
            document.getElementById('memory-score').textContent = 'Score: 0';
            memoryTimeLeft = 60;
            memoryTiles = generateMemoryTiles();
            displayMemoryGrid();
            memoryTimerInterval = setInterval(memoryGameTimer, 1000);
        }

        function generateMemoryTiles() {
            const shapes = ['Circle', 'Square', 'Triangle', 'Star', '1', '2', '3', '4'];
            const tilePairs = shapes.concat(shapes);
            return tilePairs.sort(() => 0.5 - Math.random());
        }

        function displayMemoryGrid() {
            const grid = document.getElementById('memory-grid');
            grid.innerHTML = '';
            memoryTiles.forEach((tile, index) => {
                const tileElement = document.createElement('div');
                tileElement.classList.add('memory-tile', 'hidden');
                tileElement.dataset.index = index;
                tileElement.onclick = () => revealTile(tileElement);
                grid.appendChild(tileElement);
            });
        }

        function revealTile(tileElement) {
            const index = tileElement.dataset.index;
            if (firstTile && secondTile) return; // Ignore if two tiles are already revealed
            tileElement.textContent = memoryTiles[index];
            tileElement.classList.remove('hidden');
            if (!firstTile) {
                firstTile = tileElement;
            } else if (firstTile && !secondTile) {
                secondTile = tileElement;
                checkMatch();
            }
        }

        function checkMatch() {
            if (firstTile.textContent === secondTile.textContent) {
                firstTile.classList.add('matched');
                secondTile.classList.add('matched');
                matchedPairs++;
                document.getElementById('memory-score').textContent = 'Score: ' + matchedPairs;
                firstTile = null;
                secondTile = null;
                if (matchedPairs === memoryTiles.length / 2) {
                    clearInterval(memoryTimerInterval);
                    alert('You won!');
                }
            } else {
                setTimeout(() => {
                    firstTile.textContent = '';
                    secondTile.textContent = '';
                    firstTile.classList.add('hidden');
                    secondTile.classList.add('hidden');
                    firstTile = null;
                    secondTile = null;
                }, 1000);
            }
        }

        function memoryGameTimer() {
            memoryTimeLeft--;
            document.getElementById('memory-timer').textContent = 'Time Left: ' + memoryTimeLeft + 's';
            if (memoryTimeLeft <= 0) {
                clearInterval(memoryTimerInterval);
                alert('Time is up!');
            }
        }

        // Resource Allocation Game Logic
        function startResourceGame() {
            const totalHours = 100;
            const totalBudget = 50000;
            const totalTeamMembers = 10;

            const hoursA = parseInt(document.getElementById('hours-a').value) || 0;
            const budgetA = parseInt(document.getElementById('budget-a').value) || 0;
            const teamA = parseInt(document.getElementById('team-a').value) || 0;

            const hoursB = parseInt(document.getElementById('hours-b').value) || 0;
            const budgetB = parseInt(document.getElementById('budget-b').value) || 0;
            const teamB = parseInt(document.getElementById('team-b').value) || 0;

            const hoursC = parseInt(document.getElementById('hours-c').value) || 0;
            const budgetC = parseInt(document.getElementById('budget-c').value) || 0;
            const teamC = parseInt(document.getElementById('team-c').value) || 0;

            const totalAllocatedHours = hoursA + hoursB + hoursC;
            const totalAllocatedBudget = budgetA + budgetB + budgetC;
            const totalAllocatedTeamMembers = teamA + teamB + teamC;

            if (totalAllocatedHours > totalHours || totalAllocatedBudget > totalBudget || totalAllocatedTeamMembers > totalTeamMembers) {
                document.getElementById('resource-result').textContent = "You have exceeded the available resources!";
            } else {
                let completedProjects = 0;
                if (hoursA >= 40 && budgetA >= 20000 && teamA >= 3) completedProjects++;
                if (hoursB >= 30 && budgetB >= 15000 && teamB >= 4) completedProjects++;
                if (hoursC >= 50 && budgetC >= 30000 && teamC >= 5) completedProjects++;

                document.getElementById('resource-result').textContent = `You have completed ${completedProjects} projects!`;
            }
        }

        // Crisis Management Game Logic
        const crisisResults = { A: null, B: null, C: null };

        function chooseCrisisOption(option, crisis) {
            crisisResults[crisis] = option;
            if (crisisResults.A && crisisResults.B && crisisResults.C) {
                document.getElementById('crisis-result').textContent = 'You made decisions for all crises!';
            }
        }
    </script>
</body>
</html>
