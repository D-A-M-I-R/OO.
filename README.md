<!DOCTYPE html>
<html lang="es">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Juego de la Oca: Camino a la Inclusión</title>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/qrcodejs/1.0.0/qrcode.min.js"></script>
    <script>var YT;</script>
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
        }

        body {
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100% );
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }

        .container {
            max-width: 1200px;
            width: 100%;
            background: white;
            border-radius: 20px;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            overflow: hidden;
        }

        .header {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            text-align: center;
        }

        .header h1 {
            font-size: 2.5em;
            margin-bottom: 10px;
        }

        .header p {
            font-size: 1.1em;
            opacity: 0.9;
        }

        .content {
            padding: 40px;
        }

        .screen {
            display: none;
        }

        .screen.active {
            display: block;
            animation: fadeIn 0.5s ease-in;
        }

        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }

        .start-screen {
            text-align: center;
        }

        .start-screen h2 {
            color: #333;
            margin-bottom: 20px;
            font-size: 2em;
        }

        .features {
            display: grid;
            grid-template-columns: repeat(auto-fit, minmax(200px, 1fr));
            gap: 20px;
            margin: 30px 0;
        }

        .feature-card {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            padding: 20px;
            border-radius: 15px;
            text-align: center;
        }

        .feature-card.green {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
        }

        .feature-card.purple {
            background: linear-gradient(135deg, #a8edea 0%, #fed6e3 100%);
            color: #333;
        }

        .feature-card h3 {
            font-size: 1.5em;
            margin-bottom: 10px;
        }

        .feature-card p {
            font-size: 0.9em;
        }

        .btn {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 40px;
            font-size: 1.1em;
            border-radius: 50px;
            cursor: pointer;
            margin: 10px;
            transition: transform 0.2s, box-shadow 0.2s;
        }

        .btn:hover {
            transform: translateY(-2px);
            box-shadow: 0 10px 20px rgba(102, 126, 234, 0.4);
        }

        .btn.secondary {
            background: #f0f0f0;
            color: #333;
        }

        .btn.secondary:hover {
            background: #e0e0e0;
        }

        .btn:disabled {
            opacity: 0.5;
            cursor: not-allowed;
        }

        .session-screen {
            text-align: center;
            max-width: 700px;
            margin: 0 auto;
        }

        .session-screen h2 {
            color: #333;
            margin-bottom: 30px;
            font-size: 2em;
        }

        .session-code {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            padding: 30px;
            border-radius: 15px;
            margin: 20px 0;
        }

        .session-code h3 {
            font-size: 1.2em;
            margin-bottom: 10px;
            opacity: 0.9;
        }

        .code-display {
            font-size: 3em;
            font-weight: bold;
            letter-spacing: 10px;
            font-family: 'Courier New', monospace;
            margin: 20px 0;
        }

        .code-display.highlight {
            animation: pulse 1s infinite;
        }

        @keyframes pulse {
            0%, 100% { transform: scale(1); }
            50% { transform: scale(1.05); }
        }

        .qr-container {
            background: white;
            padding: 20px;
            border-radius: 15px;
            margin: 20px 0;
            display: inline-block;
        }

        .qr-container h4 {
            color: #333;
            margin-bottom: 15px;
            font-size: 1.1em;
        }

        #qrcode {
            display: flex;
            justify-content: center;
        }

        .copy-btn {
            background: white;
            color: #667eea;
            padding: 10px 20px;
            border: none;
            border-radius: 25px;
            cursor: pointer;
            font-weight: bold;
            margin-top: 10px;
        }

        .copy-btn:hover {
            background: #f0f0f0;
        }

        .code-input-group {
            margin: 20px 0;
        }

        .code-input-group label {
            display: block;
            margin-bottom: 10px;
            color: #333;
            font-weight: 500;
        }

        .code-input-group input {
            width: 100%;
            padding: 15px;
            font-size: 1.5em;
            text-align: center;
            letter-spacing: 5px;
            border: 3px solid #ddd;
            border-radius: 10px;
            text-transform: uppercase;
        }

        .code-input-group input:focus {
            outline: none;
            border-color: #667eea;
        }

        .status-message {
            margin: 20px 0;
            padding: 15px;
            border-radius: 10px;
            font-weight: bold;
        }

        .status-message.success {
            background: #d4edda;
            color: #155724;
        }

        .status-message.error {
            background: #f8d7da;
            color: #721c24;
        }

        .status-message.waiting {
            background: #d1ecf1;
            color: #0c5460;
        }

        .setup-screen {
            max-width: 600px;
            margin: 0 auto;
        }

        .setup-screen h2 {
            color: #333;
            margin-bottom: 30px;
            text-align: center;
        }

        .player-setup {
            background: #f9f9f9;
            padding: 20px;
            margin-bottom: 20px;
            border-radius: 10px;
            border-left: 5px solid #667eea;
        }

        .player-setup h3 {
            color: #667eea;
            margin-bottom: 15px;
        }

        .form-group {
            margin-bottom: 15px;
        }

        .form-group label {
            display: block;
            margin-bottom: 5px;
            color: #333;
            font-weight: 500;
        }

        .form-group input {
            width: 100%;
            padding: 10px;
            border: 2px solid #ddd;
            border-radius: 5px;
            font-size: 1em;
        }

        .form-group input:focus {
            outline: none;
            border-color: #667eea;
        }

        .profiles {
            display: grid;
            grid-template-columns: repeat(2, 1fr);
            gap: 10px;
            margin-bottom: 15px;
        }

        .profile-btn {
            padding: 10px;
            border: 2px solid #ddd;
            background: white;
            border-radius: 8px;
            cursor: pointer;
            transition: all 0.2s;
            text-align: left;
            font-size: 0.9em;
        }

        .profile-btn:hover {
            border-color: #667eea;
            background: #f0f4ff;
        }

        .profile-btn.selected {
            border-color: #667eea;
            background: #667eea;
            color: white;
        }

        .profile-emoji {
            font-size: 1.5em;
            margin-right: 5px;
        }

        .game-screen {
            display: flex;
            flex-direction: column;
            gap: 20px;
        }

        .game-header {
            background: #f9f9f9;
            padding: 20px;
            border-radius: 10px;
            border-left: 5px solid #667eea;
        }

        .current-player {
            display: flex;
            align-items: center;
            gap: 15px;
            margin-bottom: 15px;
        }

        .player-avatar {
            width: 60px;
            height: 60px;
            border-radius: 50%;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 2em;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
        }

        .player-info h3 {
            color: #333;
            margin-bottom: 5px;
        }

        .player-info p {
            color: #666;
            font-size: 0.9em;
        }

        .dice-section {
            text-align: center;
            padding: 20px;
            background: #f0f4ff;
            border-radius: 10px;
        }

        .dice {
            width: 80px;
            height: 80px;
            background: white;
            border: 3px solid #667eea;
            border-radius: 10px;
            display: flex;
            align-items: center;
            justify-content: center;
            font-size: 3em;
            margin: 20px auto;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.1);
        }

        .dice.rolling {
            animation: diceRoll 0.6s ease-in-out;
        }

        @keyframes diceRoll {
            0%, 100% { transform: rotateX(0) rotateY(0) rotateZ(0); }
            25% { transform: rotateX(180deg) rotateY(180deg) rotateZ(90deg); }
            50% { transform: rotateX(360deg) rotateY(360deg) rotateZ(180deg); }
            75% { transform: rotateX(180deg) rotateY(180deg) rotateZ(270deg); }
        }

        .board {
            display: grid;
            grid-template-columns: repeat(9, 1fr);
            gap: 10px;
            background: #f9f9f9;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
        }

        .cell {
            aspect-ratio: 1;
            background: white;
            border: 2px solid #ddd;
            border-radius: 8px;
            display: flex;
            flex-direction: column;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            cursor: pointer;
            transition: all 0.2s;
            position: relative;
            min-height: 60px;
        }

        .cell:hover {
            transform: scale(1.05);
        }

        .cell.start {
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
        }

        .cell.penalty {
            background: linear-gradient(135deg, #f093fb 0%, #f5576c 100%);
            color: white;
            border: none;
        }

        .cell.advance {
            background: linear-gradient(135deg, #4facfe 0%, #00f2fe 100%);
            color: white;
            border: none;
        }

        .cell.finish {
            background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%);
            color: white;
            border: none;
            font-size: 1.5em;
        }

        .cell-number {
            font-size: 0.8em;
            opacity: 0.7;
        }

        .cell-icon {
            font-size: 1.5em;
        }

        .players-list {
            background: #f9f9f9;
            padding: 20px;
            border-radius: 10px;
            margin-top: 20px;
        }

        .players-list h3 {
            color: #333;
            margin-bottom: 15px;
        }

        .player-item {
            display: flex;
            align-items: center;
            justify-content: space-between;
            padding: 10px;
            background: white;
            margin-bottom: 10px;
            border-radius: 8px;
            border-left: 4px solid #667eea;
        }

        .player-item.current {
            background: #f0f4ff;
            border-left-color: #43e97b;
        }

        .player-name {
            display: flex;
            align-items: center;
            gap: 10px;
        }

        .player-position {
            color: #666;
            font-size: 0.9em;
        }

        .modal {
            display: none;
            position: fixed;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: rgba(0, 0, 0, 0.5);
            z-index: 1000;
            align-items: center;
            justify-content: center;
        }

        .modal.active {
            display: flex;
        }

        .modal-content {
            background: white;
            padding: 40px;
            border-radius: 20px;
            max-width: 500px;
            width: 90%;
            box-shadow: 0 20px 60px rgba(0, 0, 0, 0.3);
            animation: slideUp 0.3s ease-out;
        }

        @keyframes slideUp {
            from {
                transform: translateY(50px);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }

        .modal-content h2 {
            color: #667eea;
            margin-bottom: 20px;
            text-align: center;
        }

        .modal-content p {
            color: #333;
            line-height: 1.6;
            margin-bottom: 20px;
            font-size: 1.1em;
        }

        .answer-hint {
            background: #f0f4ff;
            padding: 15px;
            border-radius: 8px;
            margin-bottom: 20px;
            border-left: 4px solid #667eea;
            display: none;
        }

        .answer-hint.show {
            display: block;
        }

        .answer-hint strong {
            color: #667eea;
        }

        .victory-screen {
            text-align: center;
        }

        .victory-screen h2 {
            color: #43e97b;
            font-size: 2.5em;
            margin-bottom: 20px;
        }

        .victory-message {
            background: linear-gradient(135deg, #43e97b 0%, #38f9d7 100%);
            color: white;
            padding: 30px;
            border-radius: 15px;
            margin: 20px 0;
            font-size: 1.2em;
        }

        .button-group {
            display: flex;
            gap: 10px;
            justify-content: center;
            flex-wrap: wrap;
        }

        .waiting-players {
            background: #f0f4ff;
            padding: 20px;
            border-radius: 10px;
            margin: 20px 0;
        }

        .waiting-players h3 {
            color: #667eea;
            margin-bottom: 15px;
        }

        .player-badge {
            display: inline-block;
            background: #667eea;
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            margin: 5px;
            font-size: 0.9em;
        }

        .music-control {
            position: fixed;
            bottom: 20px;
            right: 20px;
            background: linear-gradient(135deg, #667eea 0%, #764ba2 100%);
            color: white;
            border: none;
            padding: 15px 20px;
            border-radius: 50px;
            cursor: pointer;
            font-size: 1.2em;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            transition: transform 0.2s;
            z-index: 999;
        }

        .music-control:hover {
            transform: scale(1.1);
        }

        @media (max-width: 768px) {
            .header h1 {
                font-size: 1.8em;
            }

            .board {
                grid-template-columns: repeat(6, 1fr);
            }

            .profiles {
                grid-template-columns: 1fr;
            }

            .content {
                padding: 20px;
            }

            .code-display {
                font-size: 2em;
                letter-spacing: 5px;
            }
        }
    </style>
</head>
<body>
    <!-- Control de música -->
    <button class="music-control" id="musicControl" onclick="toggleMusic()">🔊</button>
    <!-- YouTube Music Player (oculto) -->
    <div id="youtubePlayer" style="position: fixed; bottom: -100px; left: -100px; width: 1px; height: 1px; opacity: 0; pointer-events: none;"></div>

    <div class="container">
        <div class="header">
            <h1>🎲 Juego de la Oca</h1>
            <p>Camino a la Inclusión</p>
        </div>

        <div class="content">
            <!-- Pantalla de inicio -->
            <div id="startScreen" class="screen active">
                <div class="start-screen">
                    <h2>Bienvenido al Juego de la Oca</h2>
                    <p style="color: #666; margin-bottom: 30px; font-size: 1.1em;">
                        Un juego educativo sobre inclusión social y convivencia intercultural.
                    </p>
                    
                    <div class="features">
                        <div class="feature-card">
                            <div style="font-size: 2em; margin-bottom: 10px;">👥</div>
                            <h3>6 Perfiles</h3>
                            <p>Diferentes situaciones de vulnerabilidad</p>
                        </div>
                        <div class="feature-card green">
                            <div style="font-size: 2em; margin-bottom: 10px;">🎯</div>
                            <h3>63 Casillas</h3>
                            <p>Un camino lleno de desafíos</p>
                        </div>
                        <div class="feature-card purple">
                            <div style="font-size: 2em; margin-bottom: 10px;">💭</div>
                            <h3>Reflexión</h3>
                            <p>Preguntas para pensar y aprender</p>
                        </div>
                    </div>

                    <button class="btn" onclick="startGame()">¡Comenzar a Jugar! 🚀</button>
                </div>
            </div>

            <!-- Pantalla de sesión -->
            <div id="sessionScreen" class="screen">
                <div class="session-screen">
                    <h2>¡Bienvenido a la Sesión!</h2>
                    
                    <div class="session-code">
                        <h3>Código de Sesión</h3>
                        <div class="code-display highlight" id="sessionCode">ABC123</div>
                        <button class="copy-btn" onclick="copyCode()">📋 Copiar Código</button>
                        <p style="font-size: 0.9em; margin-top: 15px;">Comparte este código con tus compañeros</p>
                    </div>

                    <div style="background: #fff3cd; padding: 20px; border-radius: 10px; margin: 20px 0;">
                        <h4 style="color: #f57f17; margin-bottom: 15px;">✏️ O escribe el código manualmente</h4>
                        <p style="color: #666; font-size: 0.9em; margin-bottom: 15px;">Si no puedes escanear, escribe el código aquí:</p>
                        
                        <div class="code-input-group">
                            <label style="font-weight: bold; color: #333;">Código de sesión:</label>
                            <input type="text" id="sessionCodeInputManual" placeholder="Ejemplo: ABC123" maxlength="6" style="font-size: 1.3em; padding: 12px; text-align: center; letter-spacing: 3px; font-weight: bold; text-transform: uppercase;" onkeyup="checkCodeManual()">
                        </div>

                        <div id="statusMessageManual" style="margin-top: 15px;"></div>

                        <button class="btn" id="joinBtnManual" onclick="joinSessionManual()" style="display: none; margin-top: 15px; width: 100%;">
                            ✓ Unirse a la Sesión
                        </button>
                    </div>

                    <div class="qr-container">
                        <h4>📱 Código QR - Escanea para unirte</h4>
                        <div id="qrcode"></div>
                        <p style="color: #666; font-size: 0.9em; margin-top: 10px;">Tus compañeros pueden escanear este código</p>
                    </div>

                    <div class="waiting-players">
                        <h3>Jugadores Conectados</h3>
                        <div id="connectedPlayers" style="min-height: 50px;">
                            <p style="color: #666;">Esperando jugadores...</p>
                        </div>
                    </div>

                    <button class="btn" id="startGameBtn" onclick="startGameWithPlayers()" style="display: none;">
                        🎮 Comenzar Partida
                    </button>

                    <button class="btn secondary" onclick="goToStart()" style="margin-top: 20px;">← Atrás</button>
                </div>
            </div>

            <!-- Pantalla de unirse -->
            <div id="joinScreen" class="screen">
                <div class="setup-screen">
                    <h2>Unirse a una Sesión</h2>
                    <p style="color: #666; text-align: center; margin-bottom: 20px;">Tienes 2 opciones para unirte al juego</p>
                    
                    <h3 style="color: #667eea; margin-bottom: 20px; text-align: center;">Elige cómo unirte a la sesión</h3>

                    <div style="background: #e8f5e9; padding: 20px; border-radius: 10px; margin-bottom: 20px;">
                        <h4 style="color: #2e7d32; margin-bottom: 10px;">📱 Opción 1: Escanea el QR</h4>
                        <p style="color: #555; font-size: 0.95em; margin-bottom: 10px;">Pídele al anfitrión que te muestre el código QR</p>
                        <p style="color: #999; font-size: 0.85em;">El código se completará automáticamente (pero puedes editarlo después)</p>
                    </div>

                    <div style="background: #fff3cd; padding: 20px; border-radius: 10px;">
                        <h4 style="color: #f57f17; margin-bottom: 15px;">✏️ Opción 2: Escribe el código manualmente</h4>
                        <p style="color: #666; font-size: 0.9em; margin-bottom: 15px;">Pídele al anfitrión que comparta el código de 6 caracteres</p>
                        
                        <div class="code-input-group">
                            <label style="font-weight: bold; color: #333;">Ingresa el código de sesión:</label>
                            <input type="text" id="sessionCodeInput" placeholder="Ejemplo: ABC123" maxlength="6" style="font-size: 1.3em; padding: 12px; text-align: center; letter-spacing: 3px; font-weight: bold; text-transform: uppercase;" onkeyup="checkCode()">
                        </div>

                        <div id="statusMessage" style="margin-top: 15px;"></div>

                        <button class="btn" id="joinBtn" onclick="joinSession()" style="display: none; margin-top: 15px; width: 100%;">
                            ✓ Unirse a la Sesión
                        </button>
                    </div>

                    <button class="btn secondary" onclick="goToStart()" style="margin-top: 20px; width: 100%;">← Volver</button>
                </div>
            </div>

            <!-- Pantalla de configuración de jugador -->
            <div id="playerSetupScreen" class="screen">
                <div class="setup-screen">
                    <h2>Configuración de Jugador</h2>
                    
                    <div class="player-setup">
                        <h3>Tu Información</h3>
                        
                        <div class="form-group">
                            <label>Tu Nombre:</label>
                            <input type="text" id="playerNameInput" placeholder="Escribe tu nombre" oninput="checkPlayerSetupComplete()">
                        </div>

                        <div class="form-group">
                            <label>Selecciona tu Perfil:</label>
                            <div class="profiles" id="profilesContainer">
                                <!-- Los perfiles se generarán dinámicamente -->
                            </div>
                        </div>

                        <button class="btn" id="confirmPlayerBtn" onclick="confirmPlayerSetup()" disabled>
                            Confirmar
                        </button>
                    </div>
                </div>
            </div>

            <!-- Pantalla de juego -->
            <div id="gameScreen" class="screen">
                <div class="game-screen">
                    <div class="game-header">
                        <div class="current-player">
                            <div class="player-avatar" id="currentPlayerAvatar">👤</div>
                            <div class="player-info">
                                <h3>Turno de: <span id="currentPlayerName">Jugador</span></h3>
                                <p>Perfil: <span id="currentPlayerProfile">Refugiado</span></p>
                                <p>Casilla: <span id="currentPosition">1 / 63</span></p>
                            </div>
                        </div>
                        <div class="dice-section">
                            <div class="dice" id="diceDisplay">🎲</div>
                            <button class="btn" id="rollDiceBtn" onclick="rollDice()">Lanzar Dado</button>
                        </div>
                    </div>
                    <div class="board" id="gameBoard">
                        <!-- El tablero se generará dinámicamente -->
                    </div>
                    <div class="players-list">
                        <h3>Jugadores</h3>
                        <div id="playersList">
                            <!-- La lista de jugadores se generará dinámicamente -->
                        </div>
                    </div>
                </div>
            </div>

            <!-- Pantalla de victoria -->
            <div id="victoryScreen" class="screen">
                <div class="victory-screen">
                    <h2>🎉 ¡VICTORIA! 🎉</h2>
                    <div class="victory-message">
                        <p id="victoryMessage">¡El jugador ha llegado a la INCLUSIÓN! 🎉</p>
                    </div>
                    <p style="color: #666; margin-top: 20px;">¡Felicitaciones por completar el camino hacia la inclusión social!</p>
                    <button class="btn" onclick="goToStart()" style="margin-top: 30px;">Volver a Jugar</button>
                </div>
            </div>
        </div>
    </div>

    <!-- Modal de Reflexión -->
    <div id="reflectionModal" class="modal">
        <div class="modal-content">
            <h2>💭 Momento de Reflexión</h2>
            <p id="reflectionQuestion">¿Qué significa para ti la inclusión social?</p>
            
            <div class="answer-hint" id="answerHint">
                <strong>Respuesta:</strong>
                <p id="answerText"></p>
            </div>

            <div class="button-group">
                <button class="btn secondary" onclick="toggleAnswer()">Mostrar Respuesta</button>
                <button class="btn" onclick="closeReflectionModal()">Continuar</button>
            </div>
        </div>
    </div>

    <script>
        // Definiciones del juego
        const PROFILES = [
            { emoji: '👤', name: 'Refugiado', description: 'Persona que busca asilo y protección' },
            { emoji: '♿', name: 'Discapacitado', description: 'Persona con movilidad reducida' },
            { emoji: '👩‍🦱', name: 'Inmigrante', description: 'Persona que migra por oportunidades' },
            { emoji: '🏳️‍🌈', name: 'LGTBIQ+', description: 'Persona de la comunidad LGTBIQ+' },
            { emoji: '👴', name: 'Mayor', description: 'Persona mayor en situación de vulnerabilidad' }
        ];

        const REFLECTION_QUESTIONS = [
            {
                question: '¿Qué significa para ti la inclusión social?',
                answer: 'La inclusión social significa que todas las personas, sin importar su origen, condición o circunstancias, tienen las mismas oportunidades de participar plenamente en la sociedad.'
            },
            {
                question: '¿Cómo podemos ayudar a personas en situación de calle?',
                answer: 'Podemos ayudar mostrando empatía, apoyando organizaciones sociales, ofreciendo oportunidades de empleo y educación, y tratando a estas personas con dignidad y respeto.'
            },
            {
                question: '¿Qué desafíos enfrentan los refugiados al llegar a un nuevo país?',
                answer: 'Los refugiados enfrentan barreras idiomáticas, culturales, dificultades para validar estudios, encontrar empleo, y a menudo lidian con traumas y la separación de sus familias.'
            },
            {
                question: '¿Por qué es importante la diversidad cultural?',
                answer: 'La diversidad cultural enriquece nuestra sociedad, nos permite aprender de diferentes perspectivas, fomenta la creatividad y nos ayuda a construir comunidades más resilientes y comprensivas.'
            },
            {
                question: '¿Qué barreras enfrentan las personas con discapacidad?',
                answer: 'Enfrentan barreras físicas (accesibilidad), sociales (estigma), laborales (discriminación) y de comunicación. La sociedad debe adaptarse para garantizar su plena inclusión.'
            },
            {
                question: '¿Cómo podemos combatir los estereotipos y prejuicios?',
                answer: 'A través de la educación, el contacto directo con personas de diferentes grupos, cuestionando nuestras propias creencias y promoviendo historias que muestren la diversidad humana.'
            },
            {
                question: '¿Qué rol juega la educación en la inclusión?',
                answer: 'La educación es fundamental para romper ciclos de pobreza, desarrollar habilidades, fomentar el pensamiento crítico y crear oportunidades de movilidad social.'
            },
            {
                question: '¿Cómo podemos ser más empáticos con los demás?',
                answer: 'Escuchando activamente, poniéndonos en el lugar del otro, evitando juzgar, mostrando interés genuino por las experiencias ajenas y reconociendo nuestros propios privilegios.'
            },
            {
                question: '¿Qué significa ser un aliado en la inclusión?',
                answer: 'Ser aliado significa usar nuestros privilegios para apoyar a grupos marginados, amplificar sus voces, educarnos constantemente y actuar contra la injusticia cuando la presenciamos.'
            },
            {
                question: '¿Cómo podemos ayudar a jóvenes sin acceso a educación?',
                answer: 'Podemos apoyar a través de programas educativos, mentoría, becas o simplemente brindando oportunidades. La educación es clave para la inclusión social.'
            },
            {
                question: '¿Qué significa convivencia intercultural?',
                answer: 'Es la coexistencia respetuosa y enriquecedora de personas de diferentes culturas. Implica aprender unos de otros y valorar la diversidad.'
            },
            {
                question: '¿Cómo afecta la discriminación a las mujeres inmigrantes?',
                answer: 'Las mujeres inmigrantes enfrentan discriminación múltiple: por género, por origen. Esto limita sus oportunidades laborales y sociales.'
            },
            {
                question: '¿Qué recursos comunitarios conoces que ayuden a personas vulnerables?',
                answer: 'Pueden ser centros sociales, ONGs, programas de empleo, servicios de salud. Conocer estos recursos es importante para poder ayudar a otros.'
            },
            {
                question: '¿Cómo podemos crear una sociedad más inclusiva?',
                answer: 'A través de políticas inclusivas, educación, respeto a la diversidad, acceso igualitario a servicios y oportunidades, y solidaridad comunitaria.'
            },
            // Nuevos Acertijos/Preguntas de Reflexión
            {
                question: 'Acertijo: Tengo ciudades, pero no casas. Tengo montañas, pero no árboles. Tengo agua, pero no peces. ¿Qué soy?',
                answer: 'Un mapa. (Reflexión: La inclusión requiere ver el panorama completo, no solo las partes.)'
            },
            {
                question: '¿Qué es el "privilegio invisible" y cómo afecta la inclusión?',
                answer: 'El privilegio invisible son las ventajas no ganadas que una persona tiene por pertenecer a un grupo social dominante. Afecta la inclusión porque hace que las personas privilegiadas no perciban las barreras que enfrentan los demás.'
            },
            {
                question: '¿Cuál es la diferencia entre "igualdad" y "equidad" en el contexto de la inclusión?',
                answer: 'La igualdad significa dar a todos lo mismo. La equidad significa dar a cada uno lo que necesita para alcanzar el mismo resultado. La inclusión busca la equidad.'
            },
            {
                question: 'Acertijo: Si me tienes, quieres compartirme. Si me compartes, ya no me tienes. ¿Qué soy?',
                answer: 'Un secreto. (Reflexión: La inclusión requiere transparencia y confianza, no secretos.)'
            },
            {
                question: '¿Qué significa "microagresión" y por qué es importante reconocerla?',
                answer: 'Una microagresión es un comentario o acción sutil, a menudo involuntaria, que comunica prejuicios o actitudes negativas hacia un grupo marginado. Es importante reconocerlas porque su acumulación causa un daño significativo.'
            }
        ];

        const BOARD_CELLS = [
            { number: 1, type: 'start', icon: '🏁' },
            { number: 2, type: 'normal', icon: '🎯' },
            { number: 3, type: 'normal', icon: '🎯' },
            { number: 4, type: 'penalty', icon: '⬇️' }, // Nueva Penalización
            { number: 5, type: 'normal', icon: '🎯' },
            { number: 6, type: 'normal', icon: '🎯' },
            { number: 7, type: 'normal', icon: '🎯' },
            { number: 8, type: 'normal', icon: '🎯' },
            { number: 9, type: 'penalty', icon: '⬇️' }, // Nueva Penalización
            { number: 10, type: 'normal', icon: '🎯' },
            { number: 11, type: 'normal', icon: '🎯' },
            { number: 12, type: 'normal', icon: '🎯' },
            { number: 13, type: 'normal', icon: '🎯' },
            { number: 14, type: 'normal', icon: '🎯' },
            { number: 15, type: 'penalty', icon: '⬇️' }, // Nueva Penalización
            { number: 16, type: 'normal', icon: '🎯' },
            { number: 17, type: 'normal', icon: '🎯' },
            { number: 18, type: 'normal', icon: '🎯' },
            { number: 19, type: 'normal', icon: '🎯' },
            { number: 20, type: 'normal', icon: '🎯' },
            { number: 21, type: 'normal', icon: '🎯' },
            { number: 22, type: 'normal', icon: '🎯' },
            { number: 23, type: 'penalty', icon: '⬇️' }, // Nueva Penalización
            { number: 24, type: 'normal', icon: '🎯' },
            { number: 25, type: 'normal', icon: '🎯' },
            { number: 26, type: 'normal', icon: '🎯' },
            { number: 27, type: 'normal', icon: '🎯' },
            { number: 28, type: 'normal', icon: '🎯' },
            { number: 29, type: 'normal', icon: '🎯' },
            { number: 30, type: 'normal', icon: '🎯' },
            { number: 31, type: 'penalty', icon: '⬇️' }, // Penalización
            { number: 32, type: 'normal', icon: '🎯' },
            { number: 33, type: 'normal', icon: '🎯' },
            { number: 34, type: 'normal', icon: '🎯' },
            { number: 35, type: 'normal', icon: '🎯' },
            { number: 36, type: 'normal', icon: '🎯' },
            { number: 37, type: 'normal', icon: '🎯' },
            { number: 38, type: 'penalty', icon: '⬇️' }, // Nueva Penalización
            { number: 39, type: 'normal', icon: '🎯' },
            { number: 40, type: 'normal', icon: '🎯' },
            { number: 41, type: 'normal', icon: '🎯' },
            { number: 42, type: 'normal', icon: '🎯' },
            { number: 43, type: 'normal', icon: '🎯' },
            { number: 44, type: 'normal', icon: '🎯' },
            { number: 45, type: 'penalty', icon: '⬇️' }, // Nueva Penalización
            { number: 46, type: 'normal', icon: '🎯' },
            { number: 47, type: 'normal', icon: '🎯' },
            { number: 48, type: 'normal', icon: '🎯' },
            { number: 49, type: 'normal', icon: '🎯' },
            { number: 50, type: 'normal', icon: '🎯' },
            { number: 51, type: 'normal', icon: '🎯' },
            { number: 52, type: 'normal', icon: '🎯' },
            { number: 53, type: 'normal', icon: '🎯' },
            { number: 54, type: 'normal', icon: '🎯' },
            { number: 55, type: 'normal', icon: '🎯' },
            { number: 56, type: 'normal', icon: '🎯' },
            { number: 57, type: 'normal', icon: '🎯' },
            { number: 58, type: 'normal', icon: '🎯' },
            { number: 59, type: 'normal', icon: '🎯' },
            { number: 60, type: 'penalty', icon: '⬇️' }, // Penalización
            { number: 61, type: 'normal', icon: '🎯' },
            { number: 62, type: 'normal', icon: '🎯' },
            { number: 63, type: 'finish', icon: '🏆' }
        ];

        // Estado del juego
        let gameState = {
            sessionCode: null,
            players: [],
            currentPlayerIndex: 0,
            gameStarted: false,
            diceRolling: false,
            playerName: null,
            playerProfile: null,
            selectedProfileIdx: null,
            isHost: false
        };

        // Control de música con YouTube
        let musicPlaying = false;
        let player;
        
        // Playlist de música urbana/reggaeton (IDs de videos de YouTube)
        const urbanPlaylist = [
            'lQWOkauKYGE', // REGGAETON MIX 2024 - MYKE TOWERS, KAROL G, FEID, RAUW ALEJANDRO, BAD BUNNY
            'y2QW1jCA-SI', // REGGAETON MIX 2023 - Ozuna, Sech, Anuel AA, Bad Bunny, Jay Wheeler
            'G4SnI-zOb5w', // Mix TRAP - BAD BUNNY, ANUEL, OZUNA, ARCANGEL
            'xm7TJTIaKQM', // The Best of Reggaeton 2024 by OSOCITY
            'GGEMyt6mico'  // BEST REGGAETON MUSIC MIX 2024
        ];

        // Cargar API de YouTube
        function loadYouTubeAPI() {
            const tag = document.createElement('script');
            tag.src = 'https://www.youtube.com/iframe_api';
            const firstScriptTag = document.getElementsByTagName('script' )[0];
            firstScriptTag.parentNode.insertBefore(tag, firstScriptTag);
        }

        // Esta función se llama automáticamente cuando la API de YouTube está lista
        function onYouTubeIframeAPIReady() {
            player = new YT.Player('youtubePlayer', {
                height: '1',
                width: '1',
                videoId: urbanPlaylist[0],
                playerVars: {
                    'autoplay': 0,
                    'controls': 0,
                    'loop': 1,
                    'playlist': urbanPlaylist.join(','),
                    'playsinline': 1,
                    'enablejsapi': 1
                },
                events: {
                    'onReady': onPlayerReady,
                    'onStateChange': onPlayerStateChange
                }
            });
        }

        function onPlayerReady(event) {
            // Intentar reproducir automáticamente
            event.target.setVolume(30);
            event.target.playVideo();
        }

        function onPlayerStateChange(event) {
            if (event.data == YT.PlayerState.PLAYING) {
                musicPlaying = true;
                document.getElementById('musicControl').textContent = '🔊';
            } else if (event.data == YT.PlayerState.PAUSED) {
                musicPlaying = false;
                document.getElementById('musicControl').textContent = '🔇';
            }
        }

        function toggleMusic() {
            const musicControl = document.getElementById('musicControl');
            if (player && player.getPlayerState) {
                if (musicPlaying) {
                    player.pauseVideo();
                    musicControl.textContent = '🔇';
                    musicPlaying = false;
                } else {
                    player.playVideo();
                    musicControl.textContent = '🔊';
                    musicPlaying = true;
                }
            }
        }

        // Iniciar música automáticamente al cargar
        document.addEventListener('DOMContentLoaded', function() {
            loadYouTubeAPI();
            checkURLCode();
            const playerNameInput = document.getElementById('playerNameInput');
            if (playerNameInput) {
                playerNameInput.addEventListener('input', checkPlayerSetupComplete);
            }
        });

        // Funciones de navegación
        function generateRandomCode() {
            const characters = 'ABCDEFGHIJKLMNOPQRSTUVWXYZ0123456789';
            let result = '';
            for (let i = 0; i < 6; i++) {
                result += characters.charAt(Math.floor(Math.random() * characters.length));
            }
            return result;
        }

        function startGame() {
            gameState.sessionCode = generateRandomCode();
            gameState.isHost = true;
            document.getElementById('sessionCode').textContent = gameState.sessionCode;
            
            // Generar QR
            document.getElementById('qrcode').innerHTML = '';
            new QRCode(document.getElementById('qrcode'), {
                text: window.location.href.split('?')[0] + '?code=' + gameState.sessionCode,
                width: 180,
                height: 180,
                colorDark : "#000000",
                colorLight : "#ffffff",
                correctLevel : QRCode.CorrectLevel.H
            });

            showScreen('sessionScreen');
        }

        function copyCode() {
            const code = document.getElementById('sessionCode').textContent;
            navigator.clipboard.writeText(code).then(() => {
                alert('Código copiado: ' + code);
            }).catch(err => {
                console.error('Error al copiar: ', err);
            });
        }

        function checkCode() {
            const input = document.getElementById('sessionCodeInput').value.toUpperCase();
            const statusDiv = document.getElementById('statusMessage');
            const joinBtn = document.getElementById('joinBtn');

            if (input.length === 6) {
                // Validar que el código tenga el formato correcto
                statusDiv.innerHTML = '<div class="status-message success">✓ Código válido. ¡Puedes unirte!</div>';
                joinBtn.style.display = 'inline-block';
            } else {
                statusDiv.innerHTML = '';
                joinBtn.style.display = 'none';
            }
        }

        function joinSession() {
            const code = document.getElementById('sessionCodeInput').value.toUpperCase();
            if (code.length === 6) {
                gameState.sessionCode = code;
                gameState.isHost = false;
                renderPlayerProfileSetup();
                showScreen('playerSetupScreen');
            }
        }

        function checkCodeManual() {
            const input = document.getElementById('sessionCodeInputManual').value.toUpperCase();
            const statusDiv = document.getElementById('statusMessageManual');
            const joinBtn = document.getElementById('joinBtnManual');

            if (input.length === 6) {
                // Validar que el código tenga el formato correcto
                statusDiv.innerHTML = '<div class="status-message success">✓ Código válido. ¡Puedes unirte!</div>';
                joinBtn.style.display = 'inline-block';
            } else {
                statusDiv.innerHTML = '';
                joinBtn.style.display = 'none';
            }
        }

        function joinSessionManual() {
            const code = document.getElementById('sessionCodeInputManual').value.toUpperCase();
            if (code.length === 6) {
                gameState.sessionCode = code;
                gameState.isHost = false;
                renderPlayerProfileSetup();
                showScreen('playerSetupScreen');
            }
        }

        function renderPlayerProfileSetup() {
            const container = document.getElementById('profilesContainer');
            container.innerHTML = '';

            PROFILES.forEach((profile, idx) => {
                const btn = document.createElement('button');
                btn.className = 'profile-btn';
                btn.innerHTML = `<span class="profile-emoji">${profile.emoji}</span> ${profile.name}`;
                btn.onclick = () => selectPlayerProfile(idx);
                container.appendChild(btn);
            });
        }

        function selectPlayerProfile(idx) {
            gameState.selectedProfileIdx = idx;
            
            const buttons = document.querySelectorAll('#profilesContainer .profile-btn');
            buttons.forEach((btn, i) => {
                btn.classList.toggle('selected', i === idx);
            });

            checkPlayerSetupComplete();
        }

        function checkPlayerSetupComplete() {
            const nameInput = document.getElementById('playerNameInput').value.trim();
            const confirmBtn = document.getElementById('confirmPlayerBtn');
            
            if (nameInput && gameState.selectedProfileIdx !== null) {
                confirmBtn.disabled = false;
            } else {
                confirmBtn.disabled = true;
            }
        }

        function confirmPlayerSetup() {
            gameState.playerName = document.getElementById('playerNameInput').value.trim();
            gameState.playerProfile = PROFILES[gameState.selectedProfileIdx];

            // Simular unirse a la sesión
            const newPlayer = {
                id: gameState.players.length + 1,
                name: gameState.playerName,
                profile: gameState.playerProfile,
                position: 1
            };

            // Si es el anfitrión, se añade como primer jugador
            if (gameState.isHost) {
                gameState.players.push(newPlayer);
                showScreen('gameScreen');
                renderGame();
            } else {
                // Si es un jugador, se añade a la lista y espera
                gameState.players.push(newPlayer);
                
                // Simular que el anfitrión ve al jugador
                const connectedPlayersDiv = document.getElementById('connectedPlayers');
                connectedPlayersDiv.innerHTML = '';
                gameState.players.forEach(p => {
                    connectedPlayersDiv.innerHTML += `<span class="player-badge">${p.profile.emoji} ${p.name}</span>`;
                });

                // Mostrar pantalla de espera (simulada)
                const statusDiv = document.getElementById('statusMessage');
                if (statusDiv) {
                    statusDiv.innerHTML = '<div class="status-message waiting">✓ Te has unido a la sesión. Esperando a que el anfitrión inicie el juego...</div>';
                    document.getElementById('confirmPlayerBtn').style.display = 'none';
                } else {
                    const statusDiv = document.getElementById('statusMessageManual');
                    statusDiv.innerHTML = '<div class="status-message waiting">✓ Te has unido a la sesión. Esperando a que el anfitrión inicie el juego...</div>';
                    document.getElementById('joinBtnManual').style.display = 'none';
                }
                
                // Simular que el anfitrión puede iniciar
                if (gameState.players.length >= 1) {
                    document.getElementById('startGameBtn').style.display = 'inline-block';
                }
                
                showScreen('sessionScreen'); // Volver a la pantalla de sesión para ver la lista de jugadores
            }
        }

        function startGameWithPlayers() {
            if (gameState.players.length < 1) {
                alert('Se necesita al menos 1 jugador para comenzar');
                return;
            }

            gameState.currentPlayerIndex = 0;
            gameState.gameStarted = true;
            renderGame();
            showScreen('gameScreen');
        }

        function renderGame() {
            const boardContainer = document.getElementById('gameBoard');
            boardContainer.innerHTML = '';
            
            BOARD_CELLS.forEach(cell => {
                const cellDiv = document.createElement('div');
                cellDiv.className = `cell ${cell.type}`;
                cellDiv.innerHTML = `
                    <div class="cell-number">${cell.number}</div>
                    <div class="cell-icon">${cell.icon}</div>
                `;
                
                const playersOnCell = gameState.players.filter(p => p.position === cell.number);
                if (playersOnCell.length > 0) {
                    const avatarsDiv = document.createElement('div');
                    avatarsDiv.style.display = 'flex';
                    avatarsDiv.style.gap = '3px';
                    avatarsDiv.style.marginTop = '5px';
                    playersOnCell.forEach(player => {
                        const avatar = document.createElement('div');
                        avatar.style.width = '20px';
                        avatar.style.height = '20px';
                        avatar.style.borderRadius = '50%';
                        avatar.style.display = 'flex';
                        avatar.style.alignItems = 'center';
                        avatar.style.justifyContent = 'center';
                        avatar.style.fontSize = '0.8em';
                        avatar.style.background = 'rgba(255, 255, 255, 0.3)';
                        avatar.textContent = player.profile.emoji;
                        avatarsDiv.appendChild(avatar);
                    });
                    cellDiv.appendChild(avatarsDiv);
                }
                
                boardContainer.appendChild(cellDiv);
            });

            updateCurrentPlayerDisplay();
            updatePlayersList();
        }

        function updateCurrentPlayerDisplay() {
            const player = gameState.players[gameState.currentPlayerIndex];
            document.getElementById('currentPlayerAvatar').textContent = player.profile.emoji;
            document.getElementById('currentPlayerName').textContent = player.name;
            document.getElementById('currentPlayerProfile').textContent = player.profile.description;
            document.getElementById('currentPosition').textContent = `${player.position} / 63`;
        }

        function updatePlayersList() {
            const listContainer = document.getElementById('playersList');
            listContainer.innerHTML = '';
            
            gameState.players.forEach((player, idx) => {
                const playerDiv = document.createElement('div');
                playerDiv.className = `player-item ${idx === gameState.currentPlayerIndex ? 'current' : ''}`;
                playerDiv.innerHTML = `
                    <div class="player-name">
                        <span>${player.profile.emoji}</span>
                        <strong>${player.name}</strong>
                    </div>
                    <div class="player-position">Casilla ${player.position}</div>
                `;
                listContainer.appendChild(playerDiv);
            });
        }

        function rollDice() {
            if (gameState.diceRolling) return;
            
            gameState.diceRolling = true;
            const diceDisplay = document.getElementById('diceDisplay');
            const rollDiceBtn = document.getElementById('rollDiceBtn');
            
            rollDiceBtn.disabled = true;
            diceDisplay.classList.add('rolling');
            
            let rolls = 0;
            const rollInterval = setInterval(() => {
                const randomDice = Math.floor(Math.random() * 6) + 1;
                diceDisplay.textContent = randomDice;
                rolls++;
                
                if (rolls > 10) {
                    clearInterval(rollInterval);
                    const finalDice = Math.floor(Math.random() * 6) + 1;
                    diceDisplay.textContent = finalDice;
                    diceDisplay.classList.remove('rolling');
                    
                    setTimeout(() => {
                        movePlayer(finalDice);
                        gameState.diceRolling = false;
                        rollDiceBtn.disabled = false;
                    }, 300);
                }
            }, 50);
        }

        function movePlayer(diceValue) {
            const player = gameState.players[gameState.currentPlayerIndex];
            let newPosition = player.position + diceValue;
            
            if (newPosition > 63) {
                newPosition = 63 - (newPosition - 63);
            }
            
            player.position = newPosition;
            
            renderGame();
            
            const cellType = BOARD_CELLS[player.position - 1].type;
            
            if (cellType === 'penalty') {
                // Penalización: Retrocede 3 casillas
                const penaltyAmount = 3;
                player.position = Math.max(1, player.position - penaltyAmount);
                renderGame();
                alert(`¡Penalización! Has caído en una casilla de exclusión. Retrocedes ${penaltyAmount} casillas.`);
            } else if (cellType === 'advance') {
                // Avance: Avanza 2 casillas
                const advanceAmount = 2;
                player.position = Math.min(63, player.position + advanceAmount);
                renderGame();
                alert(`¡Premio! Has caído en una casilla de apoyo. Avanzas ${advanceAmount} casillas.`);
            }
            
            const randomQuestion = REFLECTION_QUESTIONS[Math.floor(Math.random() * REFLECTION_QUESTIONS.length)];
            showReflectionModal(randomQuestion);
        }

        function showReflectionModal(question) {
            document.getElementById('reflectionQuestion').textContent = question.question;
            document.getElementById('answerText').textContent = question.answer;
            document.getElementById('answerHint').classList.remove('show');
            document.getElementById('reflectionModal').classList.add('active');
        }

        function toggleAnswer() {
            document.getElementById('answerHint').classList.toggle('show');
        }

        function closeReflectionModal() {
            document.getElementById('reflectionModal').classList.remove('active');
            
            const player = gameState.players[gameState.currentPlayerIndex];
            
            if (player.position >= 63) {
                showVictoryScreen(player);
            } else {
                gameState.currentPlayerIndex = (gameState.currentPlayerIndex + 1) % gameState.players.length;
                updateCurrentPlayerDisplay();
                updatePlayersList();
            }
        }

        function showVictoryScreen(winner) {
            document.getElementById('victoryMessage').textContent = 
                `¡${winner.name} ha llegado a la INCLUSIÓN! 🎉`;
            showScreen('victoryScreen');
        }

        function showScreen(screenId) {
            document.querySelectorAll('.screen').forEach(screen => {
                screen.classList.remove('active');
            });
            document.getElementById(screenId).classList.add('active');
        }

        function goToStart() {
            gameState = {
                sessionCode: null,
                players: [],
                currentPlayerIndex: 0,
                gameStarted: false,
                diceRolling: false,
                playerName: null,
                playerProfile: null,
                selectedProfileIdx: null,
                isHost: false
            };
            showScreen('startScreen');
        }

        function goToJoinScreen() {
            showScreen('joinScreen');
        }

        // Verificar si hay un código en la URL
        function checkURLCode() {
            const params = new URLSearchParams(window.location.search);
            const code = params.get('code');
            if (code) {
                gameState.sessionCode = code;
                // Mostrar la pantalla de unirse pero permitir escribir el código
                showScreen('joinScreen');
                // Pre-llenar el código pero permitir edición
                document.getElementById('sessionCodeInput').value = code;
                checkCode();
            }
        }
    </script>
</body>
</html>
