<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Vortex - Premium Ludo Gambling</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Poppins:wght@300;400;600;700;900&display=swap');
        
        body {
            font-family: 'Poppins', sans-serif;
            background: radial-gradient(circle at center, #1a1a2e 0%, #16213e 100%);
            color: white;
            min-height: 100vh;
        }
        
        .game-board {
            width: 600px;
            height: 600px;
            background-color: #0f3460;
            border-radius: 20px;
            box-shadow: 0 0 30px rgba(106, 90, 205, 0.6);
            position: relative;
            overflow: hidden;
        }
        
        .home-base {
            width: 120px;
            height: 120px;
            border-radius: 20px;
            position: absolute;
        }
        
        .red-home { background-color: #ff6b6b; top: 15px; left: 15px; }
        .blue-home { background-color: #74b9ff; top: 15px; right: 15px; }
        .green-home { background-color: #55efc4; bottom: 15px; left: 15px; }
        .yellow-home { background-color: #ffeaa7; bottom: 15px; right: 15px; }
        
        .path {
            position: absolute;
            background-color: #e2e2e2;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #333;
            cursor: pointer;
        }
        
        .piece {
            width: 30px;
            height: 30px;
            border-radius: 50%;
            position: absolute;
            cursor: pointer;
            transition: all 0.3s ease;
            z-index: 10;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            box-shadow: 0 3px 6px rgba(0,0,0,0.3);
        }
        
        .piece:hover {
            transform: scale(1.1);
        }
        
        .red-piece { background-color: #ff4757; color: white; }
        .blue-piece { background-color: #1e90ff; color: white; }
        .green-piece { background-color: #2ed573; color: white; }
        .yellow-piece { background-color: #ffa502; color: white; }
        
        .dice {
            width: 70px;
            height: 70px;
            background-color: white;
            border-radius: 15px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 32px;
            font-weight: bold;
            color: #333;
            box-shadow: 0 5px 15px rgba(0,0,0,0.2);
            cursor: pointer;
            user-select: none;
        }
        
        .dice-container {
            background-color: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 30px;
            display: flex;
            flex-direction: column;
            align-items: center;
            gap: 20px;
        }
        
        .bet-controls {
            background-color: rgba(0,0,0,0.3);
            border-radius: 20px;
            padding: 20px;
            display: flex;
            flex-direction: column;
            gap: 15px;
        }
        
        .bet-chip {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            box-shadow: 0 3px 6px rgba(0,0,0,0.3);
        }
        
        .bet-chip:hover {
            transform: translateY(-5px);
        }
        
        .chip-10 { background: linear-gradient(135deg, #fdcb6e, #e17055); }
        .chip-50 { background: linear-gradient(135deg, #00b894, #0984e3); }
        .chip-100 { background: linear-gradient(135deg, #6c5ce7, #fd79a8); }
        .chip-500 { background: linear-gradient(135deg, #e84393, #6c5ce7); }
        
        .highlight {
            animation: pulse 1s infinite;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,255,255,0.7); }
            70% { transform: scale(1.05); box-shadow: 0 0 0 10px rgba(255,255,255,0); }
            100% { transform: scale(1); box-shadow: 0 0 0 0 rgba(255,255,255,0); }
        }
        
        .token-count {
            position: absolute;
            font-size: 10px;
            bottom: 2px;
            right: 2px;
            background-color: rgba(0,0,0,0.3);
            border-radius: 50%;
            width: 16px;
            height: 16px;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .winner-animation {
            animation: celebratory 1.5s ease infinite;
        }
        
        @keyframes celebratory {
            0% { transform: scale(1); }
            50% { transform: scale(1.1); }
            100% { transform: scale(1); }
        }
    </style>
</head>
<body class="flex flex-col items-center py-10">
    <!-- Header Section -->
    <header class="w-full max-w-6xl px-6 mb-8">
        <div class="flex justify-between items-center">
            <div class="flex items-center">
                <div class="w-12 h-12 rounded-full bg-gradient-to-r from-purple-600 to-blue-500 flex items-center justify-center mr-3">
                    <i class="fas fa-dice text-white text-xl"></i>
                </div>
                <h1 class="text-3xl font-bold bg-gradient-to-r from-purple-400 to-blue-500 bg-clip-text text-transparent">
                    VORTEX
                </h1>
            </div>
            <div class="flex items-center space-x-4">
                <div class="bg-gradient-to-r from-blue-900 to-purple-800 px-5 py-2 rounded-full flex items-center">
                    <i class="fas fa-coins text-yellow-300 mr-2"></i>
                    <span id="balance">1000</span>
                    <span class="ml-1 text-yellow-300">₮</span>
                </div>
                <button class="bg-gradient-to-r from-purple-500 to-blue-500 px-4 py-2 rounded-full hover:opacity-90 transition">
                    Deposit
                </button>
            </div>
        </div>
    </header>

    <div class="flex flex-col lg:flex-row gap-8 w-full max-w-6xl px-6">
        <!-- Left Panel - Game Controls -->
        <div class="flex flex-col space-y-6 w-full lg:w-1/3">
            <!-- Game Info -->
            <div class="bg-gray-900 bg-opacity-50 rounded-2xl p-6 backdrop-blur-sm">
                <h2 class="text-xl font-bold mb-4">Ludo Gambling</h2>
                <div class="flex justify-between mb-2">
                    <span class="text-gray-300">Players:</span>
                    <span id="player-count">1/4</span>
                </div>
                <div class="flex justify-between mb-2">
                    <span class="text-gray-300">Current Bet:</span>
                    <span id="current-bet">0 ₮</span>
                </div>
                <div class="flex justify-between mb-4">
                    <span class="text-gray-300">Pot:</span>
                    <span id="total-pot">0 ₮</span>
                </div>
                <div class="flex space-x-3">
                    <button id="join-game" class="flex-1 bg-green-600 hover:bg-green-700 py-2 rounded-lg transition">
                        Join Game (10₮)
                    </button>
                    <button id="leave-game" class="flex-1 bg-red-600 hover:bg-red-700 py-2 rounded-lg transition" disabled>
                        Leave
                    </button>
                </div>
            </div>

            <!-- Bet Controls -->
            <div class="bet-controls">
                <h3 class="text-lg font-semibold mb-2">Bet Amount</h3>
                <div class="grid grid-cols-4 gap-3">
                    <div class="bet-chip chip-10" data-value="10">10</div>
                    <div class="bet-chip chip-50" data-value="50">50</div>
                    <div class="bet-chip chip-100" data-value="100">100</div>
                    <div class="bet-chip chip-500" data-value="500">500</div>
                </div>
                <div class="flex items-center mt-3">
                    <button id="place-bet" class="flex-1 bg-purple-600 hover:bg-purple-700 py-2 rounded-lg transition" disabled>
                        Place Bet
                    </button>
                </div>
            </div>

            <!-- Player Stats -->
            <div class="bg-gray-900 bg-opacity-50 rounded-2xl p-6 backdrop-blur-sm">
                <h3 class="text-lg font-semibold mb-4">Players</h3>
                <div class="space-y-3">
                    <div class="flex items-center justify-between">
                        <div class="flex items-center">
                            <div class="w-8 h-8 rounded-full bg-red-500 mr-2"></div>
                            <span>You</span>
                        </div>
                        <span class="font-bold" id="player-bet">0 ₮</span>
                    </div>
                    <div class="flex items-center justify-between" id="ai-player-1">
                        <div class="flex items-center">
                            <div class="w-8 h-8 rounded-full bg-blue-500 mr-2"></div>
                            <span class="text-gray-300">AI Player 1</span>
                        </div>
                        <span class="font-bold text-gray-300" id="ai-bet-1">0 ₮</span>
                    </div>
                    <div class="flex items-center justify-between" id="ai-player-2">
                        <div class="flex items-center">
                            <div class="w-8 h-8 rounded-full bg-green-500 mr-2"></div>
                            <span class="text-gray-300">AI Player 2</span>
                        </div>
                        <span class="font-bold text-gray-300" id="ai-bet-2">0 ₮</span>
                    </div>
                    <div class="flex items-center justify-between" id="ai-player-3">
                        <div class="flex items-center">
                            <div class="w-8 h-8 rounded-full bg-yellow-500 mr-2"></div>
                            <span class="text-gray-300">AI Player 3</span>
                        </div>
                        <span class="font-bold text-gray-300" id="ai-bet-3">0 ₮</span>
                    </div>
                </div>
            </div>
        </div>

        <!-- Center Panel - Game Board -->
        <div class="flex-1 flex flex-col items-center">
            <div class="game-board mb-6" id="ludo-board">
                <!-- Home bases -->
                <div class="red-home" id="red-home"></div>
                <div class="blue-home" id="blue-home"></div>
                <div class="green-home" id="green-home"></div>
                <div class="yellow-home" id="yellow-home"></div>
                
                <!-- Path cells will be generated by JavaScript -->
            </div>

            <!-- Dice and Status -->
            <div class="dice-container">
                <div class="text-xl font-bold mb-2" id="current-turn">
                    Your turn! Roll the dice
                </div>
                <div class="dice highlight" id="dice">
                    6
                </div>
                <button id="roll-dice" class="bg-gradient-to-r from-purple-500 to-blue-500 px-6 py-2 rounded-full hover:opacity-90 transition">
                    Roll Dice
                </button>
            </div>
        </div>

        <!-- Right Panel - Game Log and Chat -->
        <div class="flex flex-col space-y-6 w-full lg:w-1/3">
            <!-- Game Log -->
            <div class="bg-gray-900 bg-opacity-50 rounded-2xl p-6 h-64 backdrop-blur-sm overflow-hidden">
                <h3 class="text-lg font-semibold mb-3">Game Log</h3>
                <div class="h-48 overflow-y-auto" id="game-log">
                    <div class="text-green-400 mb-1">Welcome to Vortex Ludo!</div>
                    <div class="text-gray-400 mb-1">Place a bet and join the game.</div>
                # Vortex-
