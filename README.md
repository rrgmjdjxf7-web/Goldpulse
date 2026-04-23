<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>GOLDPULSE | Master Terminal</title>
    <script src="https://cdn.tailwindcss.com"></script>
    
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@300;400;700&family=JetBrains+Mono:wght@400;700&display=swap');
        
        :root {
            --gold: #D4AF37;
            --bg: #050505;
            --card-bg: rgba(15, 15, 15, 0.8);
        }

        body {
            background-color: var(--bg);
            color: #e5e5e5;
            font-family: 'Inter', sans-serif;
            margin: 0;
            overflow-x: hidden;
        }

        .mono { font-family: 'JetBrains Mono', monospace; }
        .gold-text { color: var(--gold); }
        
        /* Glassmorphism Effect */
        .glass-panel {
            background: var(--card-bg);
            backdrop-filter: blur(12px);
            -webkit-backdrop-filter: blur(12px);
            border: 1px solid rgba(255, 255, 255, 0.05);
        }

        /* Gold Pulse Animation for the Balance Card */
        .pulse-gold {
            animation: pulse-border 3s infinite ease-in-out;
        }

        @keyframes pulse-border {
            0% { border-color: rgba(212, 175, 55, 0.1); box-shadow: 0 0 0 0 rgba(212, 175, 55, 0); }
            50% { border-color: rgba(212, 175, 55, 0.4); box-shadow: 0 0 15px 0 rgba(212, 175, 55, 0.1); }
            100% { border-color: rgba(212, 175, 55, 0.1); box-shadow: 0 0 0 0 rgba(212, 175, 55, 0); }
        }

        /* Terminal Styling */
        #terminal::-webkit-scrollbar { width: 4px; }
        #terminal::-webkit-scrollbar-thumb { background: #222; border-radius: 10px; }
    </style>
</head>
<body class="p-4 md:p-8">

    <nav class="flex justify-between items-center mb-10 border-b border-zinc-800 pb-6">
        <div>
            <h1 class="text-3xl font-bold tracking-tighter gold-text uppercase">GoldPulse<span class="text-white font-light">Tech</span></h1>
            <p class="text-[10px] uppercase tracking-[0.3em] text-zinc-500">Master Core Engine v1.1</p>
        </div>
        <div class="text-right">
            <p id="live-clock" class="text-sm text-zinc-400 mono">00:00:00 UTC</p>
            <p class="text-[10px] text-green-500 mono tracking-tighter uppercase font-bold">● System Active</p>
        </div>
    </nav>

    <div class="grid grid-cols-1 md:grid-cols-4 gap-4 mb-8">
        <div class="glass-panel p-6 rounded-xl pulse-gold">
            <p class="text-xs text-zinc-500 uppercase tracking-widest font-bold">Account Equity</p>
            <p id="balance" class="text-3xl font-bold mono mt-1">$10.00</p>
            <p class="text-[10px] text-green-500 mt-1 uppercase">+0.00% Today</p>
        </div>
        <div class="glass-panel p-6 rounded-xl">
            <p class="text-xs text-zinc-500 uppercase tracking-widest font-bold">Trading Pair</p>
            <p class="text-3xl font-bold mono mt-1">EURUSD</p>
            <p class="text-[10px] text-zinc-500 mt-1 uppercase">Standard Lot: 0.01</p>
        </div>
        <div class="glass-panel p-6 rounded-xl">
            <p class="text-xs text-zinc-500 uppercase tracking-widest font-bold">Live Spread</p>
            <p id="spread" class="text-3xl font-bold mono mt-1 text-blue-400">1.2</p>
            <p class="text-[10px] text-zinc-500 mt-1 uppercase">Threshold: < 1.5</p>
        </div>
        <div class="glass-panel p-6 rounded-xl">
            <p class="text-xs text-zinc-500 uppercase tracking-widest font-bold">Logic Mode</p>
            <p class="text-3xl font-bold mono mt-1 text-yellow-600">SNIPER</p>
            <p class="text-[10px] text-zinc-500 mt-1 uppercase">RSI + EMA Filter</p>
        </div>
    </div>

    <div class="glass-panel rounded-xl overflow-hidden shadow-2xl">
        <div class="bg-zinc-900/80 p-3 border-b border-zinc-800 flex justify-between items-center">
            <span class="text-[10px] font-bold tracking-widest text-zinc-400 uppercase">Master Execution Logs</span>
            <div class="flex gap-1.5">
                <div class="w-2.5 h-2.5 rounded-full bg-zinc-800"></div>
                <div class="w-2.5 h-2.5 rounded-full bg-zinc-800"></div>
                <div class="w-2.5 h-2.5 rounded-full bg-zinc-800"></div>
            </div>
        </div>
        <div id="terminal" class="p-6 h-80 overflow-y-auto mono text-[12px] space-y-1.5 bg-black/40">
            <p class="text-blue-500">> [SYSTEM] INITIALIZING GOLDPULSE MASTER CORE...</p>
            <p class="text-zinc-600">> [BOOT] Loading EURUSD Historical Data (M5)...</p>
            <p class="text-zinc-600">> [BOOT] EMA 50 & RSI 14 Modules: ONLINE</p>
        </div>
    </div>

    <script>
        const terminal = document.getElementById('terminal');
        const spreadEl = document.getElementById('spread');
        const clockEl = document.getElementById('live-clock');
        const balanceEl = document.getElementById('balance');

        let currentBalance = 10.00;

        // 1. Live UTC Clock
        function updateClock() {
            const now = new Date();
            clockEl.innerText = now.toISOString().split('T')[1].split('.')[0] + " UTC";
        }
        setInterval(updateClock, 1000);

        // 2. Bot Logic Simulation
        const logTemplates = [
            "Scanning Market Liquidity...",
            "Price above EMA 50 - Trend: BULLISH",
            "Price below EMA 50 - Trend: BEARISH",
            "RSI Level: {rsi} - Status: Neutral",
            "Checking Spread... {spread} Pips - OK",
            "Analyzing Volatility (ATR Filter)... Safe",
            "Waiting for Sniper Entry Signal...",
            "Liquidity Gap Detected - Waiting for Fill"
        ];

        function addLog() {
            const time = new Date().toLocaleTimeString([], { hour12: false });
            
            // Generate random values for logs
            const rsiVal = (Math.random() * (65 - 35) + 35).toFixed(2);
            const spreadVal = (Math.random() * (1.4 - 0.9) + 0.9).toFixed(1);
            
            let rawLog = logTemplates[Math.floor(Math.random() * logTemplates.length)];
            let finalLog = rawLog.replace('{rsi}', rsiVal).replace('{spread}', spreadVal);

            // Update UI Spread
            spreadEl.innerText = spreadVal;

            // Random "Trade Execution" Event (Rare)
            if (Math.random() > 0.92) {
                const profit = (Math.random() * (0.80 - 0.20) + 0.20).toFixed(2);
                currentBalance += parseFloat(profit);
                balanceEl.innerText = `$${currentBalance.toFixed(2)}`;
                
                const tradeLog = document.createElement('p');
                tradeLog.innerHTML = `<span class="text-zinc-700">[${time}]</span> <span class="text-green-500 font-bold">EXECUTED: BUY EURUSD | PROFIT: +$${profit}</span>`;
                terminal.appendChild(tradeLog);
            } else {
                const p = document.createElement('p');
                p.innerHTML = `<span class="text-zinc-700">[${time}]</span> <span class="text-zinc-400">${finalLog}</span>`;
                terminal.appendChild(p);
            }
            
            // Auto-scroll
            terminal.scrollTop = terminal.scrollHeight;
        }

        // Run simulation loop
        setInterval(addLog, 2500);
        updateClock();
    </script>
</body>
</html>
