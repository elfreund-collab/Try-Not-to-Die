# Try-Not-to-Die
A game of survival during the zombie apocalypse. Try to get to the safety of Cleveland Heights, Ohio. 
<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Try Not to Die</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/tone/14.7.77/Tone.js"></script>
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Special+Elite&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Special Elite', cursive;
            background-color: #1a1a1a;
            color: #e0e0e0;
        }
        .game-container {
            background-color: #2c2c2c;
            border: 2px solid #444;
            box-shadow: 0 0 20px rgba(0,0,0,0.7);
        }
        .progress-bar-container {
            background-color: #444;
        }
        .progress-bar {
            transition: width 0.5s ease-in-out;
        }
        .btn-choice {
            background-color: #3a3a3a;
            border: 2px solid #555;
            transition: all 0.2s ease;
        }
        .btn-choice:hover {
            background-color: #c8a000;
            border-color: #ffcc00;
            color: #1a1a1a;
        }
        .log-entry {
            border-bottom: 1px dashed #444;
        }
        .stat-icon {
            width: 24px;
            height: 24px;
            margin-right: 8px;
        }
        .start-screen, .end-screen {
            background: rgba(0,0,0,0.8);
            backdrop-filter: blur(5px);
        }
        #eventImageContainer img {
            width: 100%;
            height: 100%;
            object-fit: cover;
            border-radius: 0.375rem; /* rounded-md */
        }
    </style>
</head>
<body class="p-4 md:p-8 flex items-center justify-center min-h-screen">

    <div id="gameContainer" class="game-container w-full max-w-4xl mx-auto p-4 md:p-6 rounded-lg">
        
        <!-- Game Title -->
        <h1 class="text-3xl md:text-4xl text-center mb-4 text-yellow-400 font-bold tracking-wider">Try Not to Die</h1>
        <p class="text-center text-sm text-gray-400 mb-6">NYC to Cleveland Heights | A Zombie Survival Journey</p>

        <!-- Stats Display -->
        <div class="grid grid-cols-2 md:grid-cols-4 gap-4 mb-6">
            <!-- Health Stat -->
            <div id="stat-health">
                <div class="flex items-center justify-between text-sm mb-1">
                    <span class="font-bold flex items-center">
                        <svg class="stat-icon text-red-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M4.318 6.318a4.5 4.5 0 000 6.364L12 20.364l7.682-7.682a4.5 4.5 0 00-6.364-6.364L12 7.636l-1.318-1.318a4.5 4.5 0 00-6.364 0z"></path></svg>
                        Health
                    </span>
                    <span id="healthValue">100</span>
                </div>
                <div class="progress-bar-container w-full rounded-full h-2.5"><div id="healthBar" class="bg-red-600 h-2.5 rounded-full" style="width: 100%"></div></div>
            </div>
            <!-- Strength Stat -->
            <div id="stat-strength">
                <div class="flex items-center justify-between text-sm mb-1">
                     <span class="font-bold flex items-center">
                        <svg class="stat-icon text-blue-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 10V3L4 14h7v7l9-11h-7z"></path></svg>
                        Strength
                     </span>
                    <span id="strengthValue">10</span>
                </div>
                <div class="progress-bar-container w-full rounded-full h-2.5"><div id="strengthBar" class="bg-blue-500 h-2.5 rounded-full" style="width: 10%"></div></div>
            </div>
            <!-- Trust Stat -->
            <div id="stat-trust">
                <div class="flex items-center justify-between text-sm mb-1">
                     <span class="font-bold flex items-center">
                        <svg class="stat-icon text-green-500" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17 20h5v-2a3 3 0 00-5.356-1.857M17 20H7m10 0v-2c0-.656-.126-1.283-.356-1.857M7 20H2v-2a3 3 0 015.356-1.857M7 20v-2c0-.656.126-1.283.356-1.857m0 0a5.002 5.002 0 019.288 0M15 7a3 3 0 11-6 0 3 3 0 016 0zm6 3a2 2 0 11-4 0 2 2 0 014 0zM7 10a2 2 0 11-4 0 2 2 0 014 0z"></path></svg>
                        Trust
                     </span>
                    <span id="trustValue">50</span>
                </div>
                <div class="progress-bar-container w-full rounded-full h-2.5"><div id="trustBar" class="bg-green-500 h-2.5 rounded-full" style="width: 50%"></div></div>
            </div>
             <!-- Distance Stat -->
            <div id="stat-distance">
                <div class="flex items-center justify-between text-sm mb-1">
                     <span class="font-bold flex items-center">
                        <svg class="stat-icon text-yellow-400" fill="none" stroke="currentColor" viewBox="0 0 24 24" xmlns="http://www.w3.org/2000/svg"><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M17.657 16.657L13.414 20.9a1.998 1.998 0 01-2.827 0l-4.244-4.243a8 8 0 1111.314 0z"></path><path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 11a3 3 0 11-6 0 3 3 0 016 0z"></path></svg>
                        Distance
                     </span>
                    <span id="distanceValue">460 mi</span>
                </div>
                <div class="progress-bar-container w-full rounded-full h-2.5"><div id="distanceBar" class="bg-yellow-400 h-2.5 rounded-full" style="width: 0%"></div></div>
            </div>
        </div>

        <!-- Main Game Content -->
        <div class="grid grid-cols-1 md:grid-cols-3 gap-6">
            <!-- Event Display -->
            <div class="md:col-span-2 bg-black bg-opacity-25 p-4 rounded-md">
                <div id="eventImageContainer" class="w-full h-48 md:h-64 bg-gray-800 rounded-md mb-4 flex items-center justify-center">
                    <!-- Image will go here -->
                </div>
                <h2 id="eventTitle" class="text-xl font-bold mb-2 text-yellow-300">The Journey Begins</h2>
                <p id="eventText" class="text-gray-300">You stand at the edge of a ruined New York City. The road to Cleveland Heights is long and treacherous. Your goal is simple: Try Not to Die.</p>
                <div id="choicesContainer" class="mt-4 grid grid-cols-1 gap-2">
                    <!-- Choice buttons will be injected here -->
                </div>
            </div>

            <!-- Log & Inventory -->
            <div class="bg-black bg-opacity-25 p-4 rounded-md">
                <h3 class="text-lg font-bold mb-2 text-yellow-300 border-b border-gray-600 pb-2">Inventory</h3>
                <ul id="inventoryList" class="text-sm space-y-1 mb-4">
                    <!-- Inventory items will be injected here -->
                </ul>
                <h3 class="text-lg font-bold mb-2 text-yellow-300 border-b border-gray-600 pb-2">Log</h3>
                <div id="logContainer" class="h-48 md:h-full max-h-80 overflow-y-auto text-sm space-y-2 pr-2">
                    <!-- Log entries will be injected here -->
                </div>
            </div>
        </div>
    </div>

    <!-- Start Screen -->
    <div id="startScreen" class="start-screen fixed inset-0 flex items-center justify-center z-50">
        <div class="text-center p-8 bg-gray-900 rounded-lg shadow-xl max-w-lg mx-4">
            <h1 class="text-4xl text-yellow-400 font-bold mb-4">Try Not to Die</h1>
            <p class="text-gray-300 mb-6">The undead have overrun the world. You must travel from the ruins of New York City to the rumored safe zone in Cleveland Heights, Ohio. Manage your health, strength, and trust in others to survive the 460-mile journey. Make wise choices, or become one of them.</p>
            <button id="startButton" class="btn-choice text-xl font-bold py-3 px-8 rounded-lg">Begin the Journey</button>
        </div>
    </div>

    <!-- End Screen -->
    <div id="endScreen" class="end-screen fixed inset-0 flex items-center justify-center z-50 hidden">
        <div class="text-center p-8 bg-gray-900 rounded-lg shadow-xl max-w-lg mx-4">
            <h1 id="endTitle" class="text-4xl font-bold mb-4"></h1>
            <p id="endMessage" class="text-gray-300 mb-6"></p>
            <button id="restartButton" class="btn-choice text-xl font-bold py-3 px-8 rounded-lg">Try Again</button>
        </div>
    </div>

    <script>
        document.addEventListener('DOMContentLoaded', () => {
            // --- DOM Elements ---
            const healthValue = document.getElementById('healthValue');
            const healthBar = document.getElementById('healthBar');
            const strengthValue = document.getElementById('strengthValue');
            const strengthBar = document.getElementById('strengthBar');
            const trustValue = document.getElementById('trustValue');
            const trustBar = document.getElementById('trustBar');
            const distanceValue = document.getElementById('distanceValue');
            const distanceBar = document.getElementById('distanceBar');

            const eventImageContainer = document.getElementById('eventImageContainer');
            const eventTitle = document.getElementById('eventTitle');
            const eventText = document.getElementById('eventText');
            const choicesContainer = document.getElementById('choicesContainer');
            const inventoryList = document.getElementById('inventoryList');
            const logContainer = document.getElementById('logContainer');

            const startScreen = document.getElementById('startScreen');
            const startButton = document.getElementById('startButton');
            const endScreen = document.getElementById('endScreen');
            const endTitle = document.getElementById('endTitle');
            const endMessage = document.getElementById('endMessage');
            const restartButton = document.getElementById('restartButton');

            // --- Game State ---
            let state;
            const TOTAL_DISTANCE = 460;

            // --- Sound Engine ---
            const sound = {
                isStarted: false,
                effects: {
                    click: new Tone.Synth({ oscillator: { type: 'sine' }, envelope: { attack: 0.005, decay: 0.1, sustain: 0.3, release: 0.1 } }).toDestination(),
                    success: new Tone.Synth({ oscillator: { type: 'triangle' }, envelope: { attack: 0.005, decay: 0.2, sustain: 0.1, release: 0.1 } }).toDestination(),
                    fail: new Tone.FMSynth({ harmonicity: 3, modulationIndex: 10, detune: 0, oscillator: { type: "sine" }, envelope: { attack: 0.01, decay: 0.2, sustain: 0.01, release: 0.2 }, modulation: { type: "square" }, modulationEnvelope: { attack: 0.01, decay: 0.5, sustain: 0, release: 0.5 } }).toDestination(),
                    gameOver: new Tone.MembraneSynth().toDestination(),
                    gameWin: new Tone.PolySynth(Tone.Synth).toDestination(),
                },
                play: function(effect) {
                    if (!this.isStarted) return;
                    switch(effect) {
                        case 'click': this.effects.click.triggerAttackRelease("C5", "8n"); break;
                        case 'success': this.effects.success.triggerAttackRelease("G5", "8n"); break;
                        case 'fail': this.effects.fail.triggerAttackRelease("C3", "4n"); break;
                        case 'gameOver': this.effects.gameOver.triggerAttackRelease("C2", "1n"); break;
                        case 'gameWin': this.effects.gameWin.triggerAttackRelease(["C4", "E4", "G4"], "1n"); break;
                    }
                },
                start: async function() {
                    if (!this.isStarted) {
                        await Tone.start();
                        this.isStarted = true;
                        console.log('Audio context started');
                    }
                }
            };

            // --- Game Asset Images ---
            const Images = {
                zombie: 'https://images.pexels.com/photos/2929227/pexels-photo-2929227.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1',
                store: 'https://images.pexels.com/photos/796607/pexels-photo-796607.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1',
                survivor: 'https://images.pexels.com/photos/3278215/pexels-photo-3278215.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1',
                road: 'https://images.pexels.com/photos/1032650/pexels-photo-1032650.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1',
                vehicle: 'https://images.pexels.com/photos/7317643/pexels-photo-7317643.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1',
                weapon: 'https://images.pexels.com/photos/38280/pexels-photo-38280.jpeg?auto=compress&cs=tinysrgb&w=1260&h=750&dpr=1',
            };
            
            const fallbackImages = {
                 zombie: 'https://placehold.co/600x400/4a0000/e0e0e0?text=Zombie+Ahead!',
                store: 'https://placehold.co/600x400/2a3b4a/e0e0e0?text=Abandoned+Store',
                survivor: 'https://placehold.co/600x400/4a4a2a/e0e0e0?text=Survivor',
                road: 'https://placehold.co/600x400/333333/e0e0e0?text=The+Lonesome+Road',
                vehicle: 'https://placehold.co/600x400/5a4a2a/e0e0e0?text=Wrecked+Vehicle',
                weapon: 'https://placehold.co/600x400/4a2a5a/e0e0e0?text=Weapon+Found',
            };

            // --- Game Logic Functions ---

            function initGame() {
                sound.start();
                state = {
                    health: 100, strength: 10, trust: 50, distance: TOTAL_DISTANCE,
                    inventory: { food: 3, healthKits: 1, weapons: ['Fists'], vehicle: null },
                    gameActive: true,
                };
                
                eventTitle.textContent = "The Journey Begins";
                eventText.textContent = "You stand at the edge of a ruined New York City. The road to Cleveland Heights is long and treacherous. Your goal is simple: Try Not to Die.";
                eventImageContainer.innerHTML = `<img src="${Images.road}" alt="An empty, desolate road stretching into the distance." onerror="this.onerror=null;this.src='${fallbackImages.road}';">`;
                choicesContainer.innerHTML = `<button id="beginJourneyButton" class="btn-choice p-3 rounded-md w-full">Start moving</button>`;
                document.getElementById('beginJourneyButton').addEventListener('click', () => {
                    sound.play('click');
                    nextTurn();
                });
                
                logContainer.innerHTML = '';
                addLog("Your journey begins.");
                updateUI();
                
                startScreen.classList.add('hidden');
                endScreen.classList.add('hidden');
            }

            function updateUI() {
                healthValue.textContent = state.health;
                healthBar.style.width = `${state.health}%`;
                strengthValue.textContent = state.strength;
                strengthBar.style.width = `${state.strength}%`;
                trustValue.textContent = state.trust;
                trustBar.style.width = `${state.trust}%`;
                distanceValue.textContent = `${state.distance} mi`;
                distanceBar.style.width = `${(TOTAL_DISTANCE - state.distance) / TOTAL_DISTANCE * 100}%`;

                inventoryList.innerHTML = `
                    <li>Food: ${state.inventory.food}</li>
                    <li>Health Kits: ${state.inventory.healthKits}</li>
                    <li>Weapon: ${state.inventory.weapons[state.inventory.weapons.length - 1]}</li>
                    <li>Vehicle: ${state.inventory.vehicle || 'None'}</li>
                `;
            }

            function addLog(message) {
                const logEntry = document.createElement('p');
                logEntry.className = 'log-entry pb-2';
                logEntry.textContent = message;
                logContainer.prepend(logEntry);
            }

            function showEvent(event) {
                eventTitle.textContent = event.title;
                eventText.textContent = event.text;
                const fallbackSrc = fallbackImages[event.image.split('/').pop().split('?')[0].split('-')[1]] || fallbackImages.road;
                eventImageContainer.innerHTML = `<img src="${event.image}" alt="${event.title}" onerror="this.onerror=null;this.src='${fallbackSrc}';">`;
                choicesContainer.innerHTML = '';

                event.choices.forEach(choice => {
                    const button = document.createElement('button');
                    button.textContent = choice.text;
                    button.className = 'btn-choice p-3 rounded-md w-full';
                    button.onclick = () => {
                        sound.play('click');
                        choice.action();
                        updateUI();
                        if(state.gameActive) {
                            // Delay next turn slightly to allow player to read log
                            setTimeout(nextTurn, 500);
                        }
                    };
                    choicesContainer.appendChild(button);
                });
            }

            function nextTurn() {
                if (!state.gameActive) return;

                let travelAmount = state.inventory.vehicle ? 40 + Math.floor(Math.random() * 20) : 15 + Math.floor(Math.random() * 10);
                state.distance -= travelAmount;
                if (state.distance < 0) state.distance = 0;
                addLog(`You traveled ${travelAmount} miles.`);

                if (state.distance <= 0) {
                    gameWin();
                    return;
                }

                const event = events[Math.floor(Math.random() * events.length)];
                showEvent(event);
                updateUI();
            }
            
            function gameOver(reason) {
                sound.play('gameOver');
                state.gameActive = false;
                endScreen.classList.remove('hidden');
                endTitle.textContent = "You Have Died";
                endTitle.className = "text-4xl font-bold mb-4 text-red-500";
                endMessage.textContent = reason;
            }

            function gameWin() {
                sound.play('gameWin');
                state.gameActive = false;
                endScreen.classList.remove('hidden');
                endTitle.textContent = "You Survived!";
                endTitle.className = "text-4xl font-bold mb-4 text-green-500";
                endMessage.textContent = "You've made it to the refuge of Cleveland Heights. Against all odds, you survived the journey. A new life awaits.";
            }
            
            function changeStat(stat, value) {
                state[stat] += value;
                if (state[stat] > 100) state[stat] = 100;
                if (state[stat] < 0) state[stat] = 0;
                
                if (value > 0 && (stat === 'health' || stat === 'strength')) sound.play('success');
                if (value < 0 && (stat === 'health' || stat === 'trust')) sound.play('fail');

                if (stat === 'health' && state.health <= 0) {
                    gameOver("Your health dropped to zero. The wasteland claims another victim.");
                }
            }

            // --- Event Definitions ---
            const events = [
                {
                    title: "A Lone Zombie", text: "A single, shambling zombie blocks your path. It hasn't seen you yet.", image: Images.zombie,
                    choices: [
                        { text: "Attack it head-on.", action: () => {
                            if (state.strength > 15) { addLog("You easily dispatched the zombie."); changeStat('strength', 2); } 
                            else { addLog("The struggle was tough, but you won. You took a small hit."); changeStat('health', -10); }
                        }},
                        { text: "Sneak around it.", action: () => { addLog("You quietly slipped past without being noticed."); }}
                    ]
                },
                {
                    title: "Abandoned Convenience Store", text: "You find a small, ransacked convenience store. It looks mostly empty, but there might be something left.", image: Images.store,
                    choices: [
                        { text: "Scavenge for supplies.", action: () => {
                            const roll = Math.random();
                            if (roll > 0.7) { addLog("Jackpot! You found a health kit and some food."); state.inventory.healthKits++; state.inventory.food++; sound.play('success'); } 
                            else if (roll > 0.3) { addLog("You found a can of stale beans."); state.inventory.food++; sound.play('success'); } 
                            else { addLog("A zombie was hiding inside! You fought it off but got injured."); changeStat('health', -20); }
                        }},
                        { text: "It's too risky. Move on.", action: () => { addLog("You decided to play it safe and continued on your way."); }}
                    ]
                },
                {
                    title: "A Fellow Survivor", text: "You see another person huddled by a fire. They look wary and are clutching a makeshift spear.", image: Images.survivor,
                    choices: [
                        { text: "Approach them cautiously.", action: () => {
                            if (state.trust > 40) { addLog("They see you mean no harm and share some food with you."); state.inventory.food++; changeStat('trust', 5); } 
                            else { addLog("They get spooked and run off into the darkness."); changeStat('trust', -10); }
                        }},
                        { text: "Threaten them for their supplies.", action: () => {
                            addLog("You chose a darker path. You scared them off and took their meager supplies."); state.inventory.food++; changeStat('trust', -25); changeStat('strength', 5);
                        }}
                    ]
                },
                {
                    title: "An Abandoned Armored Car", text: "You stumble upon a military armored car on the side of the road. The doors are ajar.", image: Images.vehicle,
                    choices: [
                        { text: "Try to get it working.", action: () => {
                            if (Math.random() > 0.5) { addLog("Success! The engine roars to life. This will speed up your journey significantly."); state.inventory.vehicle = 'Armored Car'; sound.play('success'); } 
                            else { addLog("No luck. The engine is dead, but you find a sturdy crowbar inside."); state.inventory.weapons.push('Crowbar'); changeStat('strength', 10); }
                        }},
                        { text: "Ignore it. It could be a trap.", action: () => { addLog("You leave the vehicle alone, not willing to risk it."); }}
                    ]
                },
                {
                    title: "A Quiet Day", text: "The road is eerily quiet today. The silence is unsettling, but nothing happens.", image: Images.road,
                    choices: [
                        { text: "Use the quiet to rest and eat.", action: () => {
                            if (state.inventory.food > 0) { addLog("You eat some food and regain some health."); state.inventory.food--; changeStat('health', 15); } 
                            else { addLog("You have no food to eat. The hunger gnaws at you."); changeStat('health', -5); }
                        }},
                        { text: "Push forward while it's safe.", action: () => { addLog("You take advantage of the calm to cover more ground."); state.distance -= 10; }}
                    ]
                },
            ];

            // --- Event Listeners ---
            startButton.addEventListener('click', initGame);
            restartButton.addEventListener('click', () => {
                sound.play('click');
                initGame();
            });
        });
    </script>
</body>
</html>
