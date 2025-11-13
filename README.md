<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Retro Game Hub</title>
    <!-- Imports Tailwind CSS for styling -->
    <script src="https://cdn.tailwindcss.com"></script>
    <style>
        /* Custom styles for the games */
        body {
            font-family: 'Inter', sans-serif;
            overscroll-behavior: none; /* Prevents pull-to-refresh on mobile */
        }
        
        /* Ensures the canvas is sharp on all displays */
        canvas {
            image-rendering: -moz-crisp-edges;
            image-rendering: -webkit-crisp-edges;
            image-rendering: pixelated;
            image-rendering: crisp-edges;
            background-color: #000;
            border: 2px solid #475569;
            border-radius: 0.5rem;
        }

        /* 2048 Game-specific styles */
        #game-2048-board {
            display: grid;
            grid-template-rows: repeat(4, 1fr);
            grid-template-columns: repeat(4, 1fr);
            gap: 0.75rem; /* 12px */
            background-color: #475569; /* slate-600 */
            border-radius: 0.5rem; /* rounded-lg */
            padding: 0.75rem;
            width: 100%;
            /* Aspect ratio 1:1 */
            aspect-ratio: 1 / 1;
            max-width: 400px;
            margin: auto;
            touch-action: none; /* Disable pan/zoom gestures */
        }

        .game-2048-tile {
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 1.5rem; /* 24px */
            font-weight: bold;
            border-radius: 0.25rem; /* rounded */
            background-color: #334155; /* slate-700 */
            color: #f1f5f9; /* slate-100 */
            transition: all 0.1s ease-in-out;
        }

        /* Color mapping for 2048 tiles */
        .game-2048-tile[data-value="2"] { background-color: #7dd3fc; color: #0c4a6e; }
        .game-2048-tile[data-value="4"] { background-color: #67e8f9; color: #155e75; }
        .game-2048-tile[data-value="8"] { background-color: #f87171; color: #7f1d1d; }
        .game-2048-tile[data-value="16"] { background-color: #fb923c; color: #7c2d12; }
        .game-2048-tile[data-value="32"] { background-color: #facc15; color: #713f12; }
        .game-2048-tile[data-value="64"] { background-color: #a3e635; color: #3f6212; }
        .game-2048-tile[data-value="128"] { background-color: #4ade80; color: #166534; }
        .game-2048-tile[data-value="256"] { background-color: #34d399; color: #065f46; }
        .game-2048-tile[data-value="512"] { background-color: #2dd4bf; color: #042f2e; }
        .game-2048-tile[data-value="1024"] { background-color: #60a5fa; color: #1e3a8a; }
        .game-2048-tile[data-value="2048"] { background-color: #a78bfa; color: #4c1d95; }
        .game-2048-tile[data-value="4096"] { background-color: #f472b6; color: #831843; }
    </style>
</head>
<body class="bg-gray-900 text-slate-200 overscroll-none">

    <!-- Header -->
    <header class="border-b border-gray-700">
        <div class="container mx-auto px-4 py-5">
            <h1 class="text-3xl font-bold text-center text-cyan-400">
                Retro Game Hub
            </h1>
        </div>
    </header>

    <!-- Game Gallery -->
    <main id="game-gallery" class="container mx-auto p-4 md:p-8">
        <h2 class="text-2xl font-semibold mb-6 text-center">Select a Game</h2>
        <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">

            <!-- Game Card: Snake -->
            <div onclick="showGame('snake')" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20">
                <img src="https://placehold.co/600x400/0f172a/67e8f9?text=SNAKE" alt="Snake Game" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold">Snake</h3>
                    <p class="text-gray-400 mt-2">The classic arcade game. Eat the dots, avoid your tail!</p>
                </div>
            </div>

            <!-- Game Card: Tetris -->
            <div onclick="showGame('tetris')" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20">
                <img src="https://placehold.co/600x400/0f172a/f472b6?text=TETRIS" alt="Tetris Game" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold">Tetris</h3>
                    <p class="text-gray-400 mt-2">The timeless puzzle game. Clear lines to score points.</p>
                </div>
            </div>

            <!-- Game Card: 2048 -->
            <div onclick="showGame('2048')" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20">
                <img src="https://placehold.co/600x400/0f172a/facc15?text=2048" alt="2048 Game" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold">2048</h3>
                    <p class="text-gray-400 mt-2">Slide and merge tiles to reach the 2048 tile.</p>
                </div>
            </div>

            <!-- Game Card: Pong -->
            <div onclick="showGame('pong')" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20">
                <img src="https://placehold.co/600x400/0f172a/fafafa?text=PONG" alt="Pong Game" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold">Pong</h3>
                    <p class="text-gray-400 mt-2">The original paddle game. W/S for left, Arrows for right.</p>
                </div>
            </div>

            <!-- Game Card: Breakout -->
            <div onclick="showGame('breakout')" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20">
                <img src="https://placehold.co/600x400/0f172a/f97316?text=BREAKOUT" alt="Breakout Game" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold">Breakout</h3>
                    <p class="text-gray-400 mt-2">Destroy all the bricks with your paddle and ball.</p>
                </div>
            </div>

            <!-- Game Card: Roblox (External) -->
            <a href="https://www.roblox.com" target="_blank" rel="noopener noreferrer" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20 block">
                <img src="https://placehold.co/600x400/0f172a/ef4444?text=ROBLOX" alt="Roblox" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold flex justify-between items-center">
                        <span>Roblox</span>
                        <!-- External link icon -->
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                        </svg>
                    </h3>
                    <p class="text-gray-400 mt-2">Explore millions of 3D worlds. (Opens in new tab)</p>
                </div>
            </a>

            <!-- === COOL MATH GAMES LINKS === -->

            <!-- Game Card: Run 3 (External) -->
            <a href="https://www.coolmathgames.com/0-run-3" target="_blank" rel="noopener noreferrer" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20 block">
                <img src="https://placehold.co/600x400/0f172a/a78bfa?text=RUN+3" alt="Run 3" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold flex justify-between items-center">
                        <span>Run 3</span>
                        <!-- External link icon -->
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                        </svg>
                    </h3>
                    <p class="text-gray-400 mt-2">The classic gravity-defying runner. (Opens Cool Math)</p>
                </div>
            </a>

            <!-- Game Card: Fireboy and Watergirl (External) -->
            <a href="https://www.coolmathgames.com/0-fireboy-and-watergirl-in-the-forest-temple" target="_blank" rel="noopener noreferrer" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20 block">
                <img src="https://placehold.co/600x400/0f172a/f87171?text=FIREBOY+%26+WATERGIRL" alt="Fireboy and Watergirl" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold flex justify-between items-center">
                        <span>Fireboy & Watergirl</span>
                        <!-- External link icon -->
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                        </svg>
                    </h3>
                    <p class="text-gray-400 mt-2">Solve puzzles in the Forest Temple. (Opens Cool Math)</p>
                </div>
            </a>

            <!-- Game Card: Bloons Tower Defense (External) -->
            <a href="https://www.coolmathgames.com/0-bloons-tower-defense-5" target="_blank" rel="noopener noreferrer" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20 block">
                <img src="https://placehold.co/600x400/0f172a/4ade80?text=BLOONS+TD" alt="Bloons Tower Defense" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold flex justify-between items-center">
                        <span>Bloons Tower Defense</span>
                        <!-- External link icon -->
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                        </svg>
                    </h3>
                    <p class="text-gray-400 mt-2">Pop all the balloons! (Opens Cool Math)</p>
                </div>
            </a>

            <!-- === SPOOKY-THEMED GAMES === -->

            <!-- Game Card: Moto X3M Spooky Land (External) -->
            <a href="https://www.coolmathgames.com/0-moto-x3m-spooky-land" target="_blank" rel="noopener noreferrer" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20 block">
                <img src="https://placehold.co/600x400/0f172a/f97316?text=MOTO+X3M+SPOOKY" alt="Moto X3M Spooky Land" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold flex justify-between items-center">
                        <span>Moto X3M Spooky Land</span>
                        <!-- External link icon -->
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                        </svg>
                    </h3>
                    <p class="text-gray-400 mt-2">A fun bike game with a spooky Halloween theme. (Opens Cool Math)</p>
                </div>
            </a>

            <!-- Game Card: Ghost House (External) -->
            <a href="https://www.coolmathgames.com/0-ghost-house" target="_blank" rel="noopener noreferrer" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20 block">
                <img src="https://placehold.co/600x400/0f172a/a78bfa?text=GHOST+HOUSE" alt="Ghost House" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold flex justify-between items-center">
                        <span>Ghost House</span>
                        <!-- External link icon -->
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                        </svg>
                    </h3>
                    <p class="text-gray-400 mt-2">A fun puzzle game to find all the ghosts. (Opens Cool Math)</p>
                </div>
            </a>

            <!-- Game Card: Catch the Candy Halloween (External) -->
            <a href="https://www.coolmathgames.com/0-catch-the-candy-halloween" target="_blank" rel="noopener noreferrer" class="bg-gray-800 rounded-lg shadow-lg overflow-hidden cursor-pointer transition-transform duration-300 hover:scale-105 hover:shadow-cyan-500/20 block">
                <img src="https://placehold.co/600x400/0f172a/eab308?text=CATCH+THE+CANDY" alt="Catch the Candy Halloween" class="w-full h-48 object-cover">
                <div class="p-5">
                    <h3 class="text-xl font-bold flex justify-between items-center">
                        <span>Catch the Candy Halloween</span>
                        <!-- External link icon -->
                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5 text-gray-400" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                            <path stroke-linecap="round" stroke-linejoin="round" d="M10 6H6a2 2 0 00-2 2v10a2 2 0 002 2h10a2 2 0 002-2v-4M14 4h6m0 0v6m0-6L10 14" />
                        </svg>
                    </h3>
                    <p class="text-gray-400 mt-2">A spooky-themed puzzle game to get the candy. (Opens Cool Math)</p>
                </div>
            </a>

        </div>
    </main>

    <!-- Game Modal -->
    <div id="game-modal" class="fixed inset-0 bg-gray-900/90 backdrop-blur-sm flex items-center justify-center p-4 hidden z-50">
        <div class="bg-gray-800 rounded-lg shadow-2xl w-full max-w-3xl flex flex-col overflow-hidden">
            <!-- Modal Header -->
            <div class="flex justify-between items-center p-4 border-b border-gray-700">
                <div class="flex items-center space-x-4">
                    <button onclick="restartGame()" class="px-3 py-1.5 bg-cyan-600 text-white rounded-md hover:bg-cyan-500 transition-colors text-sm font-medium">
                        Restart
                    </button>
                    <div class="text-lg font-semibold">
                        Score: <span id="game-score" class="text-cyan-400">0</span>
                    </div>
                </div>
                <h2 id="game-title" class="text-xl font-bold text-center absolute left-1/2 -translate-x-1/2">Game Title</h2>
                <button onclick="closeGame()" class="text-gray-400 hover:text-white">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-6 w-6" fill="none" viewBox="0 0 24 24" stroke="currentColor" stroke-width="2">
                        <path stroke-linecap="round" stroke-linejoin="round" d="M6 18L18 6M6 6l12 12" />
                    </svg>
                </button>
            </div>
            
            <!-- Game Container -->
            <div id="game-container-wrapper" class="p-4 bg-gray-900 aspect-video flex items-center justify-center">
                <!-- Game Canvas or 2048 board will be injected here -->
                <div id="game-container" class="relative flex items-center justify-center w-full h-full">
                    <!-- This div is the primary container for game elements -->
                </div>
            </div>

            <!-- Game Instructions -->
            <div class="p-3 bg-gray-700 text-center text-gray-300 text-sm">
                <span id="game-instructions">Use ARROW KEYS to play.</span>
                <div id="game-over-message" class="absolute inset-0 bg-black/70 items-center justify-center text-3xl font-bold text-red-500 hidden">
                    GAME OVER
                </div>
            </div>
        </div>
    </div>

    <!-- === CORE SCRIPT (Modal, Game Loop) === -->
    <script>
        // DOM Elements
        const gameModal = document.getElementById('game-modal');
        const gameContainer = document.getElementById('game-container');
        const gameScoreDisplay = document.getElementById('game-score');
        const gameTitle = document.getElementById('game-title');
        const gameInstructions = document.getElementById('game-instructions');
        const gameOverMessage = document.getElementById('game-over-message');
        const gameGallery = document.getElementById('game-gallery');

        // Game State
        let activeGameId = null;
        let activeGameLoopId = null;
        let activeGameKeydownListener = null;
        let activeGameRestartFunction = null;

        /**
         * Shows the game modal and starts a selected game
         */
        function showGame(gameId) {
            activeGameId = gameId;
            gameGallery.classList.add('hidden');
            gameModal.classList.remove('hidden');
            
            // Set title and instructions
            switch (gameId) {
                case 'snake':
                    gameTitle.innerText = 'Snake';
                    gameInstructions.innerText = "Use ARROW KEYS to move.";
                    activeGameRestartFunction = initSnakeGame;
                    break;
                case 'tetris':
                    gameTitle.innerText = 'Tetris';
                    gameInstructions.innerText = "ARROW KEYS to move & rotate. DOWN to drop.";
                    activeGameRestartFunction = initTetrisGame;
                    break;
                case '2048':
                    gameTitle.innerText = '2048';
                    gameInstructions.innerText = "Use ARROW KEYS or SWIPE to merge tiles.";
                    activeGameRestartFunction = init2048Game;
                    break;
                case 'pong':
                    gameTitle.innerText = 'Pong';
                    gameInstructions.innerText = "W/S for Left Paddle, ↑/↓ ARROWS for Right Paddle.";
                    activeGameRestartFunction = initPongGame;
                    break;
                case 'breakout':
                    gameTitle.innerText = 'Breakout';
                    gameInstructions.innerText = "Use ←/→ ARROWS to move the paddle.";
                    activeGameRestartFunction = initBreakoutGame;
                    break;
            }
            restartGame(); // Start the selected game
        }

        /**
         * Cleans up all game-related resources
         */
        function clearGame() {
            // Stop game loop
            if (activeGameLoopId) {
                cancelAnimationFrame(activeGameLoopId);
                activeGameLoopId = null;
            }
            
            // Stop 2048-specific loop (if any, though it's event-driven)
            // (No specific loop to clear for 2048)

            // Remove event listeners
            if (activeGameKeydownListener) {
                document.removeEventListener('keydown', activeGameKeydownListener);
                activeGameKeydownListener = null;
            }
            
            // Clean up 2048 touch listeners
            gameContainer.removeEventListener('touchstart', handleTouchStart);
            gameContainer.removeEventListener('touchend', handleTouchEnd);

            // Clear game container
            gameContainer.innerHTML = '';
            
            // Reset state
            activeGameId = null;
            activeGameRestartFunction = null;
            gameOverMessage.classList.add('hidden'); // Hide game over msg
            gameScoreDisplay.innerText = '0';
        }

        /**
         * Closes the game modal and cleans up
         */
        function closeGame() {
            clearGame();
            gameModal.classList.add('hidden');
            gameGallery.classList.remove('hidden');
        }

        /**
         * Restarts the currently active game
         */
        function restartGame() {
            clearGame();
            if (activeGameRestartFunction) {
                activeGameRestartFunction();
            }
        }
        
        /**
         * Shows the Game Over message
         */
         function showGameOver() {
             gameOverMessage.classList.remove('hidden');
             if (activeGameLoopId) {
                cancelAnimationFrame(activeGameLoopId);
                activeGameLoopId = null;
             }
         }

    </script>

    <!-- === GAME 1 SCRIPT: SNAKE === -->
    <script>
        function initSnakeGame() {
            // --- Canvas Setup ---
            const canvas = document.createElement('canvas');
            canvas.width = 400;
            canvas.height = 400;
            gameContainer.appendChild(canvas);
            const ctx = canvas.getContext('2d');
            const gridSize = 20;
            let score = 0;

            // --- Game State ---
            let snake = [{ x: 10, y: 10 }]; // Snake starts in the middle
            let food = {};
            let direction = { x: 0, y: 0 };
            let lastDirection = { x: 0, y: 0 }; // To prevent moving backwards
            let speed = 10; // Frames per second
            let lastPaintTime = 0;

            function placeFood() {
                food = {
                    x: Math.floor(Math.random() * (canvas.width / gridSize)),
                    y: Math.floor(Math.random() * (canvas.height / gridSize))
                };
                // Ensure food doesn't spawn on the snake
                for (let part of snake) {
                    if (part.x === food.x && part.y === food.y) {
                        placeFood();
                        break;
                    }
                }
            }

            function snakeKeyHandler(e) {
                e.preventDefault(); // Prevent page scrolling
                switch (e.key) {
                    case "ArrowUp":
                        if (lastDirection.y === 0) direction = { x: 0, y: -1 };
                        break;
                    case "ArrowDown":
                        if (lastDirection.y === 0) direction = { x: 0, y: 1 };
                        break;
                    case "ArrowLeft":
                        if (lastDirection.x === 0) direction = { x: -1, y: 0 };
                        break;
                    case "ArrowRight":
                        if (lastDirection.x === 0) direction = { x: 1, y: 0 };
                        break;
                }
            }

            function gameLoop(currentTime) {
                if (!activeGameId || activeGameId !== 'snake') return;
                
                activeGameLoopId = requestAnimationFrame(gameLoop);
                
                const secondsSinceLastPaint = (currentTime - lastPaintTime) / 1000;
                if (secondsSinceLastPaint < 1 / speed) {
                    return; // Not time to update yet
                }
                lastPaintTime = currentTime;

                // --- Update Game Logic ---
                const head = { x: snake[0].x + direction.x, y: snake[0].y + direction.y };
                lastDirection = direction; // Save last move

                // Check for Game Over (Collision)
                if (head.x < 0 || head.x * gridSize >= canvas.width || 
                    head.y < 0 || head.y * gridSize >= canvas.height ||
                    snake.some(part => part.x === head.x && part.y === head.y)) {
                    showGameOver();
                    return;
                }

                snake.unshift(head); // Add new head

                // Check for Food
                if (head.x === food.x && head.y === food.y) {
                    score++;
                    gameScoreDisplay.innerText = score;
                    speed += 0.5; // Increase speed
                    placeFood();
                } else {
                    snake.pop(); // Remove tail
                }

                // --- Draw Game ---
                ctx.fillStyle = '#000'; // Black background
                ctx.fillRect(0, 0, canvas.width, canvas.height);

                // Draw snake
                ctx.fillStyle = '#67e8f9'; // cyan-300
                snake.forEach(part => {
                    ctx.fillRect(part.x * gridSize, part.y * gridSize, gridSize - 1, gridSize - 1);
                });

                // Draw food
                ctx.fillStyle = '#f87171'; // red-400
                ctx.fillRect(food.x * gridSize, food.y * gridSize, gridSize - 1, gridSize - 1);
            }

            // --- Start Game ---
            placeFood();
            activeGameKeydownListener = snakeKeyHandler;
            document.addEventListener('keydown', activeGameKeydownListener);
            gameScoreDisplay.innerText = score;
            requestAnimationFrame(gameLoop);
        }
    </script>

    <!-- === GAME 2 SCRIPT: TETRIS === -->
    <script>
        function initTetrisGame() {
            // --- Canvas Setup ---
            const canvas = document.createElement('canvas');
            canvas.width = 300; // 10 blocks wide
            canvas.height = 600; // 20 blocks high
            gameContainer.appendChild(canvas);
            const ctx = canvas.getContext('2d');
            const blockSize = 30;
            const rows = 20;
            const cols = 10;

            // --- Game State ---
            let board = Array.from({ length: rows }, () => Array(cols).fill(0));
            let score = 0;
            let dropCounter = 0;
            let dropInterval = 1000; // Milliseconds
            let lastTime = 0;

            const colors = [
                null,
                '#f472b6', // T (pink)
                '#facc15', // O (yellow)
                '#67e8f9', // I (cyan)
                '#f87171', // Z (red)
                '#4ade80', // S (green)
                '#fb923c', // L (orange)
                '#a78bfa', // J (purple)
            ];

            const shapes = {
                'T': [[1, 1, 1], [0, 1, 0]],
                'O': [[2, 2], [2, 2]],
                'I': [[3, 3, 3, 3]],
                'Z': [[4, 4, 0], [0, 4, 4]],
                'S': [[0, 5, 5], [5, 5, 0]],
                'L': [[6, 0, 0], [6, 6, 6]],
                'J': [[0, 0, 7], [7, 7, 7]],
            };
            const shapeKeys = 'TOIZSLJ';

            let player = {
                pos: { x: 0, y: 0 },
                matrix: null,
            };

            function playerReset() {
                const type = shapeKeys[Math.floor(Math.random() * shapeKeys.length)];
                player.matrix = shapes[type];
                player.pos.y = 0;
                player.pos.x = Math.floor(cols / 2) - Math.floor(player.matrix[0].length / 2);
                
                // Game Over check
                if (collide(board, player)) {
                    showGameOver();
                    board.forEach(row => row.fill(0));
                }
            }

            function playerDrop() {
                player.pos.y++;
                if (collide(board, player)) {
                    player.pos.y--;
                    merge(board, player);
                    playerReset();
                    sweepBoard();
                }
                dropCounter = 0;
            }

            function playerMove(dir) {
                player.pos.x += dir;
                if (collide(board, player)) {
                    player.pos.x -= dir;
                }
            }

            function playerRotate() {
                const originalMatrix = player.matrix;
                const rotated = rotate(originalMatrix);
                player.matrix = rotated;
                
                // Check for collision after rotation and "kick" if needed
                let offset = 1;
                while (collide(board, player)) {
                    player.pos.x += offset;
                    offset = -(offset + (offset > 0 ? 1 : -1));
                    if (offset > player.matrix[0].length) {
                        // Can't rotate, revert
                        player.matrix = originalMatrix;
                        return;
                    }
                }
            }

            function rotate(matrix) {
                const newMatrix = [];
                for (let y = 0; y < matrix[0].length; y++) {
                    newMatrix.push([]);
                    for (let x = matrix.length - 1; x >= 0; x--) {
                        newMatrix[y].push(matrix[x][y]);
                    }
                }
                return newMatrix;
            }

            function collide(board, player) {
                for (let y = 0; y < player.matrix.length; y++) {
                    for (let x = 0; x < player.matrix[y].length; x++) {
                        if (player.matrix[y][x] !== 0 &&
                            (board[y + player.pos.y] && board[y + player.pos.y][x + player.pos.x]) !== 0) {
                            return true;
                        }
                    }
                }
                return false;
            }

            function merge(board, player) {
                player.matrix.forEach((row, y) => {
                    row.forEach((value, x) => {
                        if (value !== 0) {
                            board[y + player.pos.y][x + player.pos.x] = value;
                        }
                    });
                });
            }
            
            function sweepBoard() {
                let rowCount = 0;
                outer: for (let y = board.length - 1; y >= 0; y--) {
                    for (let x = 0; x < board[y].length; x++) {
                        if (board[y][x] === 0) {
                            continue outer;
                        }
                    }
                    // Row is full
                    const row = board.splice(y, 1)[0].fill(0);
                    board.unshift(row);
                    y++; // Re-check the same row index
                    rowCount++;
                }
                // Update score
                if (rowCount > 0) {
                    score += rowCount * 10; // Simple score
                    gameScoreDisplay.innerText = score;
                    dropInterval *= 0.95; // Speed up
                }
            }

            function drawMatrix(matrix, offset) {
                matrix.forEach((row, y) => {
                    row.forEach((value, x) => {
                        if (value !== 0) {
                            ctx.fillStyle = colors[value];
                            ctx.fillRect((x + offset.x) * blockSize,
                                         (y + offset.y) * blockSize,
                                         blockSize, blockSize);
                            ctx.strokeStyle = '#000';
                            ctx.strokeRect((x + offset.x) * blockSize,
                                         (y + offset.y) * blockSize,
                                         blockSize, blockSize);
                        }
                    });
                });
            }

            function draw() {
                ctx.fillStyle = '#000';
                ctx.fillRect(0, 0, canvas.width, canvas.height);
                drawMatrix(board, { x: 0, y: 0 });
                drawMatrix(player.matrix, player.pos);
            }

            function gameLoop(time = 0) {
                if (!activeGameId || activeGameId !== 'tetris') return;
                
                const deltaTime = time - lastTime;
                lastTime = time;
                dropCounter += deltaTime;

                if (dropCounter > dropInterval) {
                    playerDrop();
                }

                draw();
                activeGameLoopId = requestAnimationFrame(gameLoop);
            }

            function tetrisKeyHandler(e) {
                e.preventDefault();
                switch (e.key) {
                    case "ArrowLeft":
                        playerMove(-1);
                        break;
                    case "ArrowRight":
                        playerMove(1);
                        break;
                    case "ArrowDown":
                        playerDrop();
                        break;
                    case "ArrowUp": // Rotate
                        playerRotate();
                        break;
                }
            }

            // --- Start Game ---
            playerReset();
            gameScoreDisplay.innerText = score;
            activeGameKeydownListener = tetrisKeyHandler;
            document.addEventListener('keydown', activeGameKeydownListener);
            gameLoop();
        }
    </script>

    <!-- === GAME 3 SCRIPT: 2048 === -->
    <script>
        let board2048 = [];
        let score2048 = 0;
        let touchStartX = 0;
        let touchStartY = 0;
        let touchEndX = 0;
        let touchEndY = 0;

        function init2048Game() {
            // --- DOM Setup ---
            const boardElement = document.createElement('div');
            boardElement.id = 'game-2048-board';
            gameContainer.appendChild(boardElement);
            
            // --- Game State ---
            board2048 = [
                [0, 0, 0, 0],
                [0, 0, 0, 0],
                [0, 0, 0, 0],
                [0, 0, 0, 0]
            ];
            score2048 = 0;
            gameScoreDisplay.innerText = '0';

            // Start game
            addRandomTile();
            addRandomTile();
            drawBoard();

            // --- Event Listeners ---
            activeGameKeydownListener = handle2048Key;
            document.addEventListener('keydown', activeGameKeydownListener);
            
            gameContainer.addEventListener('touchstart', handleTouchStart, { passive: false });
            gameContainer.addEventListener('touchend', handleTouchEnd, { passive: false });
        }
        
        function handleTouchStart(e) {
            e.preventDefault();
            touchStartX = e.changedTouches[0].screenX;
            touchStartY = e.changedTouches[0].screenY;
        }

        function handleTouchEnd(e) {
            e.preventDefault();
            touchEndX = e.changedTouches[0].screenX;
            touchEndY = e.changedTouches[0].screenY;
            handleSwipe();
        }
        
        function handleSwipe() {
            const dx = touchEndX - touchStartX;
            const dy = touchEndY - touchStartY;
            const absDx = Math.abs(dx);
            const absDy = Math.abs(dy);

            if (Math.max(absDx, absDy) < 50) return; // Not a swipe

            let moved = false;
            if (absDx > absDy) {
                // Horizontal swipe
                if (dx > 0) moved = moveRight();
                else moved = moveLeft();
            } else {
                // Vertical swipe
                if (dy > 0) moved = moveDown();
                else moved = moveUp();
            }
            
            if (moved) {
                addRandomTile();
                drawBoard();
                checkGameOver2048();
            }
        }

        function drawBoard() {
            const boardElement = document.getElementById('game-2048-board');
            if (!boardElement) return;
            boardElement.innerHTML = ''; // Clear board
            
            for (let r = 0; r < 4; r++) {
                for (let c = 0; c < 4; c++) {
                    const tile = document.createElement('div');
                    tile.className = 'game-2048-tile';
                    const value = board2048[r][c];
                    tile.dataset.value = value;
                    if (value > 0) {
                        tile.innerText = value;
                    }
                    boardElement.appendChild(tile);
                }
            }
            gameScoreDisplay.innerText = score2048;
        }

        function addRandomTile() {
            let emptyTiles = [];
            for (let r = 0; r < 4; r++) {
                for (let c = 0; c < 4; c++) {
                    if (board2048[r][c] === 0) {
                        emptyTiles.push({ r, c });
                    }
                }
            }
            if (emptyTiles.length > 0) {
                const { r, c } = emptyTiles[Math.floor(Math.random() * emptyTiles.length)];
                board2048[r][c] = Math.random() < 0.9 ? 2 : 4;
            }
        }

        function handle2048Key(e) {
            if (!activeGameId || activeGameId !== '2048') return;
            e.preventDefault();
            let moved = false;
            switch (e.key) {
                case "ArrowUp": moved = moveUp(); break;
                case "ArrowDown": moved = moveDown(); break;
                case "ArrowLeft": moved = moveLeft(); break;
                case "ArrowRight": moved = moveRight(); break;
                default: return; // Do nothing for other keys
            }
            
            if (moved) {
                addRandomTile();
                drawBoard();
                checkGameOver2048();
            }
        }

        // --- 2048 Move Logic ---
        function slide(row) {
            let arr = row.filter(val => val);
            let missing = 4 - arr.length;
            let zeros = Array(missing).fill(0);
            return arr.concat(zeros);
        }

        function combine(row) {
            let moved = false;
            for (let i = 0; i < 3; i++) {
                if (row[i] !== 0 && row[i] === row[i + 1]) {
                    row[i] *= 2;
                    row[i + 1] = 0;
                    score2048 += row[i];
                    moved = true;
                }
            }
            return { row, moved };
        }
        
        function operate(row) {
            let slidedRow = slide(row);
            let { row: combinedRow, moved } = combine(slidedRow);
            let finalRow = slide(combinedRow);
            
            // Check if anything actually changed
            let changed = false;
            for(let i=0; i<4; i++) {
                if(row[i] !== finalRow[i]) {
                    changed = true;
                    break;
                }
            }
            return { row: finalRow, moved: changed || moved };
        }

        function moveUp() {
            let boardMoved = false;
            for (let c = 0; c < 4; c++) {
                let col = [board2048[0][c], board2048[1][c], board2048[2][c], board2048[3][c]];
                let { row: newCol, moved } = operate(col);
                if (moved) boardMoved = true;
                for (let r = 0; r < 4; r++) {
                    board2048[r][c] = newCol[r];
                }
            }
            return boardMoved;
        }

        function moveDown() {
            let boardMoved = false;
            for (let c = 0; c < 4; c++) {
                let col = [board2048[3][c], board2048[2][c], board2048[1][c], board2048[0][c]];
                let { row: newCol, moved } = operate(col);
                if (moved) boardMoved = true;
                for (let r = 0; r < 4; r++) {
                    board2048[3 - r][c] = newCol[r];
                }
            }
            return boardMoved;
        }

        function moveLeft() {
            let boardMoved = false;
            for (let r = 0; r < 4; r++) {
                let row = board2048[r];
                let { row: newRow, moved } = operate(row);
                if (moved) boardMoved = true;
                board2048[r] = newRow;
            }
            return boardMoved;
        }

        function moveRight() {
            let boardMoved = false;
            for (let r = 0; r < 4; r++) {
                let row = board2048[r].slice().reverse();
                let { row: newRow, moved } = operate(row);
                if (moved) boardMoved = true;
                board2048[r] = newRow.reverse();
            }
            return boardMoved;
        }
        
        function canMove() {
            for (let r = 0; r < 4; r++) {
                for (let c = 0; c < 4; c++) {
                    if (board2048[r][c] === 0) return true; // Empty space
                    if (r < 3 && board2048[r][c] === board2048[r + 1][c]) return true; // Vertical merge
                    if (c < 3 && board2048[r][c] === board2048[r][c + 1]) return true; // Horizontal merge
                }
            }
            return false;
        }
        
        function checkGameOver2048() {
            if (!canMove()) {
                showGameOver();
            }
        }
    </script>

    <!-- === GAME 4: PONG === -->
    <script>
        function initPongGame() {
            const canvas = document.createElement('canvas');
            canvas.width = 600;
            canvas.height = 400;
            gameContainer.appendChild(canvas);
            const ctx = canvas.getContext('2d');

            let ball = {
                x: canvas.width / 2,
                y: canvas.height / 2,
                radius: 10,
                dx: 5,
                dy: -5
            };

            let leftPaddle = {
                x: 10,
                y: canvas.height / 2 - 50,
                width: 15,
                height: 100,
                dy: 0,
                score: 0
            };

            let rightPaddle = {
                x: canvas.width - 25,
                y: canvas.height / 2 - 50,
                width: 15,
                height: 100,
                dy: 0,
                score: 0
            };

            const paddleSpeed = 6;
            let keysPressed = {};
            const maxScore = 5;

            function drawBall() {
                ctx.beginPath();
                ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
                ctx.fillStyle = '#f1f5f9'; // slate-100
                ctx.fill();
                ctx.closePath();
            }

            function drawPaddle(paddle) {
                ctx.fillStyle = '#67e8f9'; // cyan-300
                ctx.fillRect(paddle.x, paddle.y, paddle.width, paddle.height);
            }
            
            function drawScores() {
                gameScoreDisplay.innerText = `${leftPaddle.score} - ${rightPaddle.score}`;
            }

            function movePaddles() {
                // Left paddle (W/S)
                if (keysPressed['w'] && leftPaddle.y > 0) {
                    leftPaddle.y -= paddleSpeed;
                }
                if (keysPressed['s'] && leftPaddle.y + leftPaddle.height < canvas.height) {
                    leftPaddle.y += paddleSpeed;
                }
                // Right paddle (Up/Down)
                if (keysPressed['ArrowUp'] && rightPaddle.y > 0) {
                    rightPaddle.y -= paddleSpeed;
                }
                if (keysPressed['ArrowDown'] && rightPaddle.y + rightPaddle.height < canvas.height) {
                    rightPaddle.y += paddleSpeed;
                }
            }

            function moveBall() {
                ball.x += ball.dx;
                ball.y += ball.dy;

                // Wall collision (top/bottom)
                if (ball.y + ball.radius > canvas.height || ball.y - ball.radius < 0) {
                    ball.dy *= -1;
                }

                // Paddle collision
                // Left paddle
                if (ball.x - ball.radius < leftPaddle.x + leftPaddle.width &&
                    ball.y > leftPaddle.y && ball.y < leftPaddle.y + leftPaddle.height &&
                    ball.dx < 0) {
                    ball.dx *= -1;
                    // Increase speed slightly
                    ball.dx *= 1.1;
                    ball.dy *= 1.1;
                }
                // Right paddle
                if (ball.x + ball.radius > rightPaddle.x &&
                    ball.y > rightPaddle.y && ball.y < rightPaddle.y + rightPaddle.height &&
                    ball.dx > 0) {
                    ball.dx *= -1;
                    ball.dx *= 1.1;
                    ball.dy *= 1.1;
                }

                // Score point
                if (ball.x + ball.radius < 0) { // Right paddle scores
                    rightPaddle.score++;
                    resetBall();
                } else if (ball.x - ball.radius > canvas.width) { // Left paddle scores
                    leftPaddle.score++;
                    resetBall();
                }
                
                drawScores();
            }
            
            function resetBall() {
                if (leftPaddle.score >= maxScore || rightPaddle.score >= maxScore) {
                    showGameOver();
                    return;
                }
                ball.x = canvas.width / 2;
                ball.y = canvas.height / 2;
                ball.dx = (Math.random() > 0.5 ? 1 : -1) * 5;
                ball.dy = (Math.random() > 0.5 ? 1 : -1) * 5;
            }

            function draw() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                // Draw middle line
                ctx.strokeStyle = '#475569'; // slate-600
                ctx.beginPath();
                ctx.setLineDash([5, 15]);
                ctx.moveTo(canvas.width / 2, 0);
                ctx.lineTo(canvas.width / 2, canvas.height);
                ctx.stroke();
                ctx.setLineDash([]);

                drawBall();
                drawPaddle(leftPaddle);
                drawPaddle(rightPaddle);
            }

            function gameLoop() {
                if (!activeGameId || activeGameId !== 'pong') return;
                movePaddles();
                moveBall();
                draw();
                activeGameLoopId = requestAnimationFrame(gameLoop);
            }
            
            function pongKeyHandler(e) {
                e.preventDefault();
                if (e.type === 'keydown') {
                    keysPressed[e.key] = true;
                } else if (e.type === 'keyup') {
                    keysPressed[e.key] = false;
                }
            }

            activeGameKeydownListener = pongKeyHandler;
            document.addEventListener('keydown', activeGameKeydownListener);
            document.addEventListener('keyup', activeGameKeydownListener); // Need keyup for pong
            
            // Need to adjust cleanup function
            const originalCloseGame = window.closeGame;
            window.closeGame = function() {
                document.removeEventListener('keyup', activeGameKeydownListener);
                originalCloseGame();
                window.closeGame = originalCloseGame; // Reset to original
            }
            const originalRestartGame = window.restartGame;
            window.restartGame = function() {
                document.removeEventListener('keyup', activeGameKeydownListener);
                originalRestartGame();
                window.restartGame = originalRestartGame; // Reset to original
            }

            drawScores();
            gameLoop();
        }
    </script>
    
    <!-- === GAME 5: BREAKOUT === -->
    <script>
        function initBreakoutGame() {
            const canvas = document.createElement('canvas');
            canvas.width = 480;
            canvas.height = 360;
            gameContainer.appendChild(canvas);
            const ctx = canvas.getContext('2d');

            let score = 0;
            let lives = 3;

            let ball = {
                x: canvas.width / 2,
                y: canvas.height - 30,
                radius: 8,
                dx: 3,
                dy: -3
            };

            let paddle = {
                height: 12,
                width: 75,
                x: (canvas.width - 75) / 2
            };

            let keys = { right: false, left: false };

            // Bricks setup
            const brickRowCount = 5;
            const brickColumnCount = 7;
            const brickWidth = 60;
            const brickHeight = 20;
            const brickPadding = 5;
            const brickOffsetTop = 30;
            const brickOffsetLeft = 30;
            const brickColors = ['#f87171', '#fb923c', '#facc15', '#4ade80', '#38bdf8']; // red, orange, yellow, green, blue
            
            let bricks = [];
            for(let c = 0; c < brickColumnCount; c++) {
                bricks[c] = [];
                for(let r = 0; r < brickRowCount; r++) {
                    bricks[c][r] = { x: 0, y: 0, status: 1, color: brickColors[r] };
                }
            }

            function drawBall() {
                ctx.beginPath();
                ctx.arc(ball.x, ball.y, ball.radius, 0, Math.PI * 2);
                ctx.fillStyle = "#f1f5f9"; // slate-100
                ctx.fill();
                ctx.closePath();
            }

            function drawPaddle() {
                ctx.beginPath();
                ctx.rect(paddle.x, canvas.height - paddle.height, paddle.width, paddle.height);
                ctx.fillStyle = "#67e8f9"; // cyan-300
                ctx.fill();
                ctx.closePath();
            }

            function drawBricks() {
                for(let c = 0; c < brickColumnCount; c++) {
                    for(let r = 0; r < brickRowCount; r++) {
                        if(bricks[c][r].status == 1) {
                            let brickX = (c * (brickWidth + brickPadding)) + brickOffsetLeft;
                            let brickY = (r * (brickHeight + brickPadding)) + brickOffsetTop;
                            bricks[c][r].x = brickX;
                            bricks[c][r].y = brickY;
                            ctx.beginPath();
                            ctx.rect(brickX, brickY, brickWidth, brickHeight);
                            ctx.fillStyle = bricks[c][r].color;
                            ctx.fill();
                            ctx.closePath();
                        }
                    }
                }
            }
            
            function drawScoreAndLives() {
                gameScoreDisplay.innerText = `${score} | Lives: ${lives}`;
            }

            function collisionDetection() {
                for(let c = 0; c < brickColumnCount; c++) {
                    for(let r = 0; r < brickRowCount; r++) {
                        let b = bricks[c][r];
                        if(b.status == 1) {
                            if(ball.x > b.x && ball.x < b.x + brickWidth && ball.y > b.y && ball.y < b.y + brickHeight) {
                                ball.dy = -ball.dy;
                                b.status = 0;
                                score++;
                                if(score == brickRowCount * brickColumnCount) {
                                    // YOU WIN!
                                    gameInstructions.innerText = "YOU WIN! CONGRATS!";
                                    showGameOver();
                                }
                            }
                        }
                    }
                }
            }

            function moveBall() {
                ball.x += ball.dx;
                ball.y += ball.dy;

                // Wall collision (left/right)
                if(ball.x + ball.radius > canvas.width || ball.x - ball.radius < 0) {
                    ball.dx = -ball.dx;
                }

                // Wall collision (top)
                if(ball.y - ball.radius < 0) {
                    ball.dy = -ball.dy;
                } 
                // Wall collision (bottom)
                else if(ball.y + ball.radius > canvas.height) {
                    // Paddle collision
                    if(ball.x > paddle.x && ball.x < paddle.x + paddle.width) {
                        ball.dy = -ball.dy;
                        // Add some spin
                        let deltaX = ball.x - (paddle.x + paddle.width / 2);
                        ball.dx = deltaX * 0.2;
                    } else {
                        // You miss
                        lives--;
                        if(lives < 0) {
                            showGameOver();
                        } else {
                            ball.x = canvas.width / 2;
                            ball.y = canvas.height - 30;
                            ball.dx = 3;
                            ball.dy = -3;
                            paddle.x = (canvas.width - paddle.width) / 2;
                        }
                    }
                }
            }

            function movePaddle() {
                if(keys.right && paddle.x < canvas.width - paddle.width) {
                    paddle.x += 7;
                }
                else if(keys.left && paddle.x > 0) {
                    paddle.x -= 7;
                }
            }
            
            function drawAll() {
                ctx.clearRect(0, 0, canvas.width, canvas.height);
                drawBricks();
                drawBall();
                drawPaddle();
                drawScoreAndLives();
                collisionDetection();
            }

            function gameLoop() {
                if (!activeGameId || activeGameId !== 'breakout') return;
                
                drawAll();
                moveBall();
                movePaddle();
                
                activeGameLoopId = requestAnimationFrame(gameLoop);
            }
            
            function breakoutKeyHandler(e) {
                e.preventDefault();
                if(e.type === 'keydown') {
                    if(e.key == "Right" || e.key == "ArrowRight") keys.right = true;
                    else if(e.key == "Left" || e.key == "ArrowLeft") keys.left = true;
                }
                else if (e.type === 'keyup') {
                    if(e.key == "Right" || e.key == "ArrowRight") keys.right = false;
                    else if(e.key == "Left" || e.key == "ArrowLeft") keys.left = false;
                }
            }
            
            activeGameKeydownListener = breakoutKeyHandler;
            document.addEventListener('keydown', activeGameKeydownListener);
            document.addEventListener('keyup', activeGameKeydownListener); // Need keyup
            
            // Need to adjust cleanup function
            const originalCloseGame = window.closeGame;
            window.closeGame = function() {
                document.removeEventListener('keyup', activeGameKeydownListener);
                originalCloseGame();
                window.closeGame = originalCloseGame; // Reset to original
            }
            const originalRestartGame = window.restartGame;
            window.restartGame = function() {
                document.removeEventListener('keyup', activeGameKeydownListener);
                originalRestartGame();
                window.restartGame = originalRestartGame; // Reset to original
            }

            drawScoreAndLives();
            gameLoop();
        }
    </script>
</body>
</html>
