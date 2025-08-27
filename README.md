<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>English-German Vocabulary Game</title>
    <style>
        * {
            box-sizing: border-box;
            margin: 0;
            padding: 0;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #1a2a6c, #b21f1f, #fdbb2d);
            min-height: 100vh;
            display: flex;
            flex-direction: column;
            align-items: center;
            padding: 20px;
            color: #333;
        }
        
        .container {
            background-color: rgba(255, 255, 255, 0.95);
            border-radius: 15px;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.2);
            width: 90%;
            max-width: 800px;
            padding: 30px;
            margin-top: 20px;
        }
        
        header {
            text-align: center;
            margin-bottom: 25px;
        }
        
        h1 {
            color: #1a2a6c;
            font-size: 2.5rem;
            margin-bottom: 10px;
        }
        
        .instructions {
            color: #555;
            font-size: 1.1rem;
            margin-bottom: 20px;
            line-height: 1.5;
        }
        
        .score-board {
            display: flex;
            justify-content: space-between;
            margin-bottom: 20px;
            padding: 15px;
            background-color: #f8f9fa;
            border-radius: 10px;
            font-weight: bold;
            font-size: 1.2rem;
        }
        
        .game-board {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 15px;
            margin-bottom: 25px;
        }
        
        .column {
            display: flex;
            flex-direction: column;
            gap: 12px;
        }
        
        .column-title {
            text-align: center;
            padding: 10px;
            background-color: #1a2a6c;
            color: white;
            border-radius: 8px;
            font-weight: bold;
        }
        
        .card {
            padding: 20px;
            background-color: #e9ecef;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.2rem;
            text-align: center;
            transition: all 0.3s ease;
            min-height: 80px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .card:hover {
            background-color: #d0ebff;
            transform: translateY(-3px);
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }
        
        .card.selected {
            background-color: #4dabf7;
            color: white;
            transform: scale(1.03);
        }
        
        .card.correct {
            background-color: #40c057;
            color: white;
        }
        
        .card.incorrect {
            background-color: #fa5252;
            color: white;
        }
        
        .card.matched {
            background-color: #20c997;
            color: white;
            transform: scale(0.95);
            cursor: default;
        }
        
        .controls {
            display: flex;
            justify-content: center;
            gap: 15px;
            margin-top: 20px;
        }
        
        button {
            padding: 15px 30px;
            background-color: #3b5bdb;
            color: white;
            border: none;
            border-radius: 8px;
            cursor: pointer;
            font-size: 1.1rem;
            font-weight: bold;
            transition: background-color 0.3s;
            box-shadow: 0 4px 6px rgba(0, 0, 0, 0.1);
        }
        
        button:hover {
            background-color: #364fc7;
            transform: translateY(-2px);
            box-shadow: 0 6px 8px rgba(0, 0, 0, 0.15);
        }
        
        button:active {
            transform: translateY(0);
        }
        
        button:disabled {
            background-color: #ccd5e0;
            cursor: not-allowed;
            transform: none;
            box-shadow: none;
        }
        
        .results {
            text-align: center;
            padding: 30px;
        }
        
        .results h2 {
            margin-bottom: 20px;
            color: #3b5bdb;
        }
        
        .progress-bar {
            height: 12px;
            background-color: #e9ecef;
            border-radius: 6px;
            margin-bottom: 20px;
            overflow: hidden;
        }
        
        .progress {
            height: 100%;
            background: linear-gradient(90deg, #4dabf7, #3b5bdb);
            transition: width 0.3s ease;
        }
        
        .feedback {
            text-align: center;
            margin: 15px 0;
            font-weight: bold;
            min-height: 24px;
            font-size: 1.2rem;
        }
        
        .success {
            color: #40c057;
        }
        
        .error {
            color: #fa5252;
        }
        
        .match-animation {
            position: fixed;
            pointer-events: none;
            font-size: 2rem;
            font-weight: bold;
            color: #20c997;
            animation: floatUp 1s ease-in-out;
            z-index: 100;
        }
        
        @keyframes floatUp {
            0% {
                opacity: 1;
                transform: translateY(0);
            }
            100% {
                opacity: 0;
                transform: translateY(-100px);
            }
        }
        
        @media (max-width: 600px) {
            .game-board {
                grid-template-columns: 1fr;
            }
            
            h1 {
                font-size: 2rem;
            }
            
            .card {
                padding: 15px;
                font-size: 1.1rem;
                min-height: 70px;
            }
        }
    </style>
</head>
<body>
    <div class="container">
        <header>
            <h1>English-German Vocabulary Game</h1>
            <p class="instructions">Match each English word with its German translation by selecting pairs of cards. Improve your vocabulary while having fun!</p>
        </header>
        
        <div class="score-board">
            <div>Score: <span id="score">0</span></div>
            <div>Matches: <span id="matches">0</span>/<span id="total">10</span></div>
            <div>Time: <span id="timer">60</span>s</div>
        </div>
        
        <div class="progress-bar">
            <div class="progress" id="progress"></div>
        </div>
        
        <div class="feedback" id="feedback"></div>
        
        <div class="game-board" id="game-board">
            <div class="column">
                <div class="column-title">English Words</div>
                <div id="english-cards"></div>
            </div>
            <div class="column">
                <div class="column-title">German Translations</div>
                <div id="german-cards"></div>
            </div>
        </div>
        
        <div class="controls">
            <button id="reset">Reset Game</button>
            <button id="hint">Give Hint</button>
        </div>
    </div>

    <script>
        // Vocabulary pairs (English to German)
        const vocabulary = [
            { english: "loud", german: "laut" },
            { english: "quiet", german: "ruhig" },
            { english: "impossible", german: "unmöglich" },
            { english: "harbour", german: "Hafen" },
            { english: "volcanic eruption", german: "Vulkanausbruch" },
            { english: "volcano", german: "Vulkan" },
            { english: "natural disaster", german: "Naturkatastrophe" },
            { english: "drought", german: "Dürre" },
            { english: "flood", german: "Flut" },
            { english: "lightning", german: "Blitz" },
            { english: "thunder", german: "Donner" },
            { english: "sunshine", german: "Sonnenschein" },
            { english: "worse", german: "schlimmer" },
            { english: "cause", german: "verursachen" },
            { english: "cover", german: "bedecken" },
            { english: "be afraid", german: "Angst haben" },
            { english: "erupt", german: "ausbrechen" },
            { english: "escape", german: "fliehen" }
        ];

        // Game state
        let selectedEnglish = null;
        let selectedGerman = null;
        let matchedPairs = 0;
        let score = 0;
        let timeLeft = 60;
        let timer;
        let gameActive = false;
        
        // DOM elements
        const englishCardsElement = document.getElementById('english-cards');
        const germanCardsElement = document.getElementById('german-cards');
        const scoreElement = document.getElementById('score');
        const matchesElement = document.getElementById('matches');
        const totalElement = document.getElementById('total');
        const timerElement = document.getElementById('timer');
        const progressElement = document.getElementById('progress');
        const feedbackElement = document.getElementById('feedback');
        const resetButton = document.getElementById('reset');
        const hintButton = document.getElementById('hint');
        
        // Initialize game
        totalElement.textContent = vocabulary.length;
        
        function initializeGame() {
            // Clear previous game state
            englishCardsElement.innerHTML = '';
            germanCardsElement.innerHTML = '';
            selectedEnglish = null;
            selectedGerman = null;
            matchedPairs = 0;
            score = 0;
            timeLeft = 60;
            clearInterval(timer);
            
            // Update UI
            scoreElement.textContent = score;
            matchesElement.textContent = matchedPairs;
            timerElement.textContent = timeLeft;
            progressElement.style.width = '100%';
            feedbackElement.textContent = '';
            feedbackElement.className = 'feedback';
            
            // Shuffle vocabulary
            const shuffledVocabulary = [...vocabulary].sort(() => Math.random() - 0.5);
            
            // Create English cards
            shuffledVocabulary.forEach((word, index) => {
                const card = document.createElement('div');
                card.className = 'card';
                card.textContent = word.english;
                card.dataset.index = index;
                card.addEventListener('click', () => selectEnglishCard(card, index));
                englishCardsElement.appendChild(card);
            });
            
            // Create German cards (shuffled)
            const shuffledGerman = [...shuffledVocabulary]
                .map((word, index) => ({ german: word.german, originalIndex: index }))
                .sort(() => Math.random() - 0.5);
            
            shuffledGerman.forEach((word, index) => {
                const card = document.createElement('div');
                card.className = 'card';
                card.textContent = word.german;
                card.dataset.originalIndex = word.originalIndex;
                card.addEventListener('click', () => selectGermanCard(card, word.originalIndex));
                germanCardsElement.appendChild(card);
            });
            
            // Start timer
            gameActive = true;
            timer = setInterval(updateTimer, 1000);
        }
        
        function selectEnglishCard(card, index) {
            if (!gameActive || card.classList.contains('matched')) return;
            
            // Deselect previous selection
            if (selectedEnglish) {
                selectedEnglish.classList.remove('selected');
            }
            
            // Select new card
            card.classList.add('selected');
            selectedEnglish = card;
            selectedEnglishIndex = index;
            
            checkMatch();
        }
        
        function selectGermanCard(card, originalIndex) {
            if (!gameActive || card.classList.contains('matched')) return;
            
            // Deselect previous selection
            if (selectedGerman) {
                selectedGerman.classList.remove('selected');
            }
            
            // Select new card
            card.classList.add('selected');
            selectedGerman = card;
            selectedGermanIndex = originalIndex;
            
            checkMatch();
        }
        
        function checkMatch() {
            if (selectedEnglish && selectedGerman) {
                const englishIndex = parseInt(selectedEnglish.dataset.index);
                const germanOriginalIndex = parseInt(selectedGerman.dataset.originalIndex);
                
                if (englishIndex === germanOriginalIndex) {
                    // Correct match
                    selectedEnglish.classList.remove('selected');
                    selectedGerman.classList.remove('selected');
                    selectedEnglish.classList.add('correct');
                    selectedGerman.classList.add('correct');
                    
                    setTimeout(() => {
                        selectedEnglish.classList.remove('correct');
                        selectedGerman.classList.remove('correct');
                        selectedEnglish.classList.add('matched');
                        selectedGerman.classList.add('matched');
                        
                        selectedEnglish = null;
                        selectedGerman = null;
                        
                        matchedPairs++;
                        score += 10;
                        scoreElement.textContent = score;
                        matchesElement.textContent = matchedPairs;
                        
                        // Show match animation
                        showMatchAnimation('Correct! +10 points');
                        
                        // Check for game completion
                        if (matchedPairs === vocabulary.length) {
                            endGame(true);
                        }
                    }, 500);
                } else {
                    // Incorrect match
                    selectedEnglish.classList.add('incorrect');
                    selectedGerman.classList.add('incorrect');
                    
                    setTimeout(() => {
                        selectedEnglish.classList.remove('selected', 'incorrect');
                        selectedGerman.classList.remove('selected', 'incorrect');
                        
                        selectedEnglish = null;
                        selectedGerman = null;
                        
                        score = Math.max(0, score - 2);
                        scoreElement.textContent = score;
                        
                        feedbackElement.textContent = 'Try again!';
                        feedbackElement.className = 'feedback error';
                    }, 1000);
                }
            }
        }
        
        function updateTimer() {
            timeLeft--;
            timerElement.textContent = timeLeft;
            
            // Update progress bar
            progressElement.style.width = `${(timeLeft / 60) * 100}%`;
            
            if (timeLeft <= 0) {
                endGame(false);
            }
        }
        
        function endGame(isWin) {
            clearInterval(timer);
            gameActive = false;
            
            if (isWin) {
                feedbackElement.textContent = `Congratulations! You matched all words with a score of ${score}!`;
                feedbackElement.className = 'feedback success';
            } else {
                feedbackElement.textContent = `Time's up! Your final score is ${score}.`;
                feedbackElement.className = 'feedback error';
            }
        }
        
        function showMatchAnimation(text) {
            const animation = document.createElement('div');
            animation.className = 'match-animation';
            animation.textContent = text;
            animation.style.left = `${window.innerWidth / 2 - 100}px`;
            animation.style.top = `${window.innerHeight / 2}px`;
            
            document.body.appendChild(animation);
            
            setTimeout(() => {
                document.body.removeChild(animation);
            }, 1000);
        }
        
        function giveHint() {
            if (!gameActive || matchedPairs === vocabulary.length) return;
            
            // Find the first unmatched English card
            const englishCards = document.querySelectorAll('#english-cards .card:not(.matched)');
            if (englishCards.length === 0) return;
            
            const randomEnglishCard = englishCards[Math.floor(Math.random() * englishCards.length)];
            const englishIndex = parseInt(randomEnglishCard.dataset.index);
            
            // Find the matching German card
            const germanCards = document.querySelectorAll('#german-cards .card');
            let matchingGermanCard = null;
            
            for (const card of germanCards) {
                if (parseInt(card.dataset.originalIndex) === englishIndex && !card.classList.contains('matched')) {
                    matchingGermanCard = card;
                    break;
                }
            }
            
            if (matchingGermanCard) {
                // Highlight the matching pair
                randomEnglishCard.classList.add('correct');
                matchingGermanCard.classList.add('correct');
                
                setTimeout(() => {
                    randomEnglishCard.classList.remove('correct');
                    matchingGermanCard.classList.remove('correct');
                }, 1000);
                
                // Deduct points for using hint
                score = Math.max(0, score - 5);
                scoreElement.textContent = score;
                
                feedbackElement.textContent = 'Hint used! -5 points';
                feedbackElement.className = 'feedback error';
            }
        }
        
        // Event listeners
        resetButton.addEventListener('click', initializeGame);
        hintButton.addEventListener('click', giveHint);
        
        // Start the game
        initializeGame();
    </script>
</body>
</html>
