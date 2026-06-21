<!DOCTYPE html>
<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>U.I.T • Unité d'Instruction Tactique</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.6.0/css/all.min.css">
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Inter:wght@400;500;600;700&family=Space+Grotesk:wght@500;600;700&display=swap');
        
        :root {
            --glass: rgba(15, 23, 42, 0.85);
        }
        
        body {
            font-family: 'Inter', system_ui, sans-serif;
        }
        
        .title-font {
            font-family: 'Space Grotesk', sans-serif;
        }

        .glass {
            background: var(--glass);
            backdrop-filter: blur(24px);
            -webkit-backdrop-filter: blur(24px);
            border: 1px solid rgba(148, 163, 184, 0.2);
        }

        .military-bg {
            background: linear-gradient(135deg, #0f172a 0%, #1e2937 100%);
        }

        .scanline {
            position: relative;
        }
        
        .scanline::after {
            content: '';
            position: absolute;
            top: 0;
            left: 0;
            width: 100%;
            height: 100%;
            background: linear-gradient(
                to bottom,
                transparent 50%,
                rgba(34, 211, 238, 0.08) 50%
            );
            background-size: 100% 4px;
            pointer-events: none;
            animation: scan 4s linear infinite;
        }

        @keyframes scan {
            0% { background-position: 0 0; }
            100% { background-position: 0 400px; }
        }

        .status-valid {
            animation: pulse-valid 2s infinite;
        }

        @keyframes pulse-valid {
            0%, 100% { opacity: 1; }
            50% { opacity: 0.85; }
        }

        .modal {
            animation: modalPop 0.4s cubic-bezier(0.34, 1.56, 0.64, 1);
        }
        
        @keyframes modalPop {
            0% { transform: scale(0.7); opacity: 0; }
            100% { transform: scale(1); opacity: 1; }
        }
    </style>
</head>
<body class="military-bg text-slate-200 min-h-screen overflow-hidden">
    <div class="flex h-screen">
        <!-- Sidebar -->
        <div class="w-72 glass border-r border-slate-700 flex flex-col">
            <div class="p-6 border-b border-slate-700">
                <div class="flex items-center gap-3">
                    <div class="w-10 h-10 bg-cyan-500 rounded-2xl flex items-center justify-center text-black font-bold text-2xl">U</div>
                    <div>
                        <h1 class="title-font text-3xl font-bold tracking-tighter">U.I.T</h1>
                        <p class="text-xs text-cyan-400 tracking-[2px]">UNITE D'INSTRUCTION TACTIQUE</p>
                    </div>
                </div>
                <div class="mt-2 text-[10px] text-slate-500">GLENDALE ONE STATE • ARMY</div>
            </div>

            <div class="flex-1 p-4 space-y-1">
                <a onclick="navigateTo('dashboard')" class="nav-item flex items-center gap-3 px-4 py-3 rounded-3xl hover:bg-white/10 transition-all cursor-pointer active">
                    <i class="fa-solid fa-gauge-high w-5"></i>
                    <span>Tableau de bord</span>
                </a>
                <a onclick="navigateTo('soldiers')" class="nav-item flex items-center gap-3 px-4 py-3 rounded-3xl hover:bg-white/10 transition-all cursor-pointer">
                    <i class="fa-solid fa-users w-5"></i>
                    <span>Effectifs</span>
                </a>
                <a onclick="navigateTo('formation')" class="nav-item flex items-center gap-3 px-4 py-3 rounded-3xl hover:bg-white/10 transition-all cursor-pointer">
                    <i class="fa-solid fa-graduation-cap w-5"></i>
                    <span>Formations</span>
                </a>
                <a onclick="navigateTo('reports')" class="nav-item flex items-center gap-3 px-4 py-3 rounded-3xl hover:bg-white/10 transition-all cursor-pointer">
                    <i class="fa-solid fa-chart-column w-5"></i>
                    <span>Rapports</span>
                </a>
            </div>

            <div class="p-4 border-t border-slate-700">
                <div onclick="showAdminModal()" class="flex items-center gap-3 px-4 py-3 rounded-3xl hover:bg-amber-500/10 text-amber-400 cursor-pointer transition-all">
                    <i class="fa-solid fa-shield-halved"></i>
                    <span class="font-medium">Accès Administratif</span>
                </div>
                <div class="text-[10px] text-center mt-6 text-slate-500">v27.4.1 • CONFIDENTIEL</div>
            </div>
        </div>

        <!-- Main Content -->
        <div class="flex-1 flex flex-col">
            <!-- Top Bar -->
            <div class="h-14 glass border-b border-slate-700 flex items-center px-6 justify-between">
                <div class="flex items-center gap-8">
                    <div class="flex items-center gap-2 text-sm">
                        <div class="w-2 h-2 bg-emerald-500 rounded-full animate-pulse"></div>
                        <span class="text-emerald-400">OPÉRATIONNEL</span>
                    </div>
                    <div class="text-slate-400 text-sm">Glendale One State RP • Secteur 7</div>
                </div>
                
                <div class="flex items-center gap-6">
                    <div class="text-xs px-3 py-1.5 bg-slate-800 rounded-2xl flex items-center gap-2">
                        <i class="fa-solid fa-clock"></i>
                        <span id="current-time"></span>
                    </div>
                    <div onclick="logout()" class="flex items-center gap-2 text-red-400 hover:text-red-500 cursor-pointer">
                        <i class="fa-solid fa-right-from-bracket"></i>
                        <span class="text-sm">Déconnexion</span>
                    </div>
                </div>
            </div>

            <!-- Dashboard -->
            <div id="dashboard" class="flex-1 p-8 overflow-auto">
                <h2 class="title-font text-5xl font-bold mb-8 tracking-tighter">Centre de Commandement</h2>
                
                <div class="grid grid-cols-12 gap-6">
                    <!-- Stats -->
                    <div class="col-span-12 lg:col-span-8 grid grid-cols-2 gap-6">
                        <div class="glass rounded-3xl p-8 scanline">
                            <div class="flex justify-between items-start">
                                <div>
                                    <div class="text-cyan-400 text-sm tracking-widest">RANG 0</div>
                                    <div id="rank0-count" class="text-7xl font-bold title-font mt-2">47</div>
                                    <div class="text-emerald-400 text-lg flex items-center gap-2">
                                        <span>FORMÉS</span>
                                        <i class="fa-solid fa-arrow-trend-up"></i>
                                    </div>
                                </div>
                                <i class="fa-solid fa-user-shield text-6xl text-slate-700"></i>
                            </div>
                        </div>
                        
                        <div class="glass rounded-3xl p-8 scanline">
                            <div class="flex justify-between items-start">
                                <div>
                                    <div class="text-violet-400 text-sm tracking-widest">RANG 1</div>
                                    <div id="rank1-count" class="text-7xl font-bold title-font mt-2">19</div>
                                    <div class="text-emerald-400 text-lg flex items-center gap-2">
                                        <span>FORMÉS</span>
                                        <i class="fa-solid fa-arrow-trend-up"></i>
                                    </div>
                                </div>
                                <i class="fa-solid fa-user-ninja text-6xl text-slate-700"></i>
                            </div>
                        </div>
                    </div>

                    <div class="col-span-12 lg:col-span-4 glass rounded-3xl p-8 flex flex-col justify-between">
                        <div>
                            <div class="text-amber-400 flex items-center gap-2 mb-4">
                                <i class="fa-solid fa-bell"></i>
                                <span class="font-medium">ALERTES</span>
                            </div>
                            <div class="space-y-4">
                                <div class="flex gap-3 items-center">
                                    <div class="text-2xl">🔴</div>
                                    <div class="flex-1">
                                        <div class="font-medium">3 recrues en retard de formation</div>
                                        <div class="text-xs text-slate-400">Dernière mise à jour il y a 2h</div>
                                    </div>
                                </div>
                            </div>
                        </div>
                        <button onclick="navigateTo('formation')" 
                                class="mt-6 w-full bg-white text-black py-4 rounded-2xl font-semibold hover:scale-105 transition-all">
                            GÉRER LES FORMATIONS
                        </button>
                    </div>

                    <!-- Recent Soldiers -->
                    <div class="col-span-12 glass rounded-3xl p-6">
                        <div class="flex justify-between items-center mb-6">
                            <h3 class="font-semibold text-xl">Dernières recrues formées</h3>
                            <button onclick="navigateTo('soldiers')" class="text-cyan-400 text-sm flex items-center gap-2 hover:underline">
                                Voir tout <i class="fa-solid fa-arrow-right"></i>
                            </button>
                        </div>
                        <div id="recent-soldiers" class="grid grid-cols-1 md:grid-cols-3 gap-4"></div>
                    </div>
                </div>
            </div>

            <!-- Soldiers Page -->
            <div id="soldiers" class="flex-1 p-8 overflow-auto hidden">
                <div class="flex justify-between items-end mb-8">
                    <h2 class="title-font text-5xl font-bold tracking-tighter">Effectifs</h2>
                    <input id="search-input" 
                           onkeyup="searchSoldiers()"
                           type="text" 
                           placeholder="Rechercher par nom ou code..." 
                           class="glass px-6 py-4 rounded-3xl w-96 focus:outline-none">
                </div>
                
                <div class="glass rounded-3xl overflow-hidden">
                    <table class="w-full">
                        <thead>
                            <tr class="border-b border-slate-700">
                                <th class="text-left p-6">Code</th>
                                <th class="text-left p-6">Nom</th>
                                <th class="text-left p-6">Rang</th>
                                <th class="text-left p-6">Formation</th>
                                <th class="text-left p-6">Date</th>
                                <th class="w-32"></th>
                            </tr>
                        </thead>
                        <tbody id="soldiers-table" class="text-sm"></tbody>
                    </table>
                </div>
            </div>

            <!-- Formation Page -->
            <div id="formation" class="flex-1 p-8 overflow-auto hidden">
                <h2 class="title-font text-5xl font-bold tracking-tighter mb-8">Validation des Formations</h2>
                
                <div class="max-w-md mx-auto glass rounded-3xl p-10">
                    <div class="text-center mb-8">
                        <i class="fa-solid fa-id-card text-6xl text-cyan-400"></i>
                    </div>
                    <input id="check-code" 
                           type="text" 
                           placeholder="Entrer le code soldat (ex: KASSANO-47)"
                           class="w-full glass border border-slate-600 focus:border-cyan-400 rounded-3xl px-6 py-6 text-center text-lg focus:outline-none">
                    <button onclick="checkFormation()" 
                            class="mt-6 w-full bg-gradient-to-r from-cyan-500 to-violet-500 py-6 rounded-3xl font-bold text-xl hover:brightness-110 transition-all">
                        VÉRIFIER FORMATION
                    </button>
                    <div id="formation-result" class="mt-8 min-h-[120px] text-center"></div>
                </div>
            </div>

            <!-- Reports -->
            <div id="reports" class="flex-1 p-8 overflow-auto hidden">
                <h2 class="title-font text-5xl font-bold tracking-tighter mb-8">Rapports d'Instruction</h2>
                <div class="glass rounded-3xl p-10">
                    <div class="grid grid-cols-2 gap-8">
                        <div>
                            <h4 class="text-cyan-400 mb-4">Statistiques globales</h4>
                            <div class="space-y-6">
                                <div class="flex justify-between">
                                    <span>Total soldats formés</span>
                                    <span class="font-mono text-3xl" id="total-formed">66</span>
                                </div>
                                <div class="h-px bg-slate-700"></div>
                                <div class="flex justify-between">
                                    <span>Taux de réussite</span>
                                    <span class="text-emerald-400 font-mono text-3xl">94%</span>
                                </div>
                            </div>
                        </div>
                        <div class="border-l border-slate-700 pl-8">
                            <h4 class="text-cyan-400 mb-4">Prochaines sessions</h4>
                            <ul class="space-y-4 text-sm">
                                <li class="flex justify-between"><span>Session Rang 1 - Section Alpha</span><span class="text-amber-400">Demain 08:00</span></li>
                                <li class="flex justify-between"><span>Exercice de tir tactique</span><span class="text-amber-400">Mer 14:00</span></li>
                            </ul>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Admin Modal -->
    <div id="admin-modal" class="hidden fixed inset-0 bg-black/80 flex items-center justify-center z-50">
        <div class="glass modal rounded-3xl w-full max-w-lg p-10">
            <h3 class="text-2xl font-bold mb-6">Accès Administratif</h3>
            <p class="text-slate-400 mb-8">Code d'autorisation requis</p>
            
            <input id="admin-code" 
                   type="password" 
                   placeholder="Code d'accès"
                   class="w-full glass px-6 py-5 rounded-2xl text-lg mb-6 focus:outline-none">
            
            <div class="flex gap-4">
                <button onclick="hideAdminModal()" 
                        class="flex-1 py-5 border border-slate-600 rounded-2xl hover:bg-white/5">ANNULER</button>
                <button onclick="validateAdminCode()" 
                        class="flex-1 py-5 bg-white text-black rounded-2xl font-semibold">ACCÉDER</button>
            </div>
        </div>
    </div>

    <!-- Add Soldier Modal (Admin only) -->
    <div id="add-soldier-modal" class="hidden fixed inset-0 bg-black/80 flex items-center justify-center z-50">
        <div class="glass modal rounded-3xl w-full max-w-md p-10">
            <h3 class="text-2xl font-bold mb-8">Ajouter un Soldat</h3>
            
            <div class="space-y-6">
                <div>
                    <label class="text-xs tracking-widest block mb-2">CODE SOLDAT</label>
                    <input id="new-code" type="text" class="w-full glass px-5 py-4 rounded-2xl" placeholder="KASSANO-10">
                </div>
                <div>
                    <label class="text-xs tracking-widest block mb-2">NOM COMPLET</label>
                    <input id="new-name" type="text" class="w-full glass px-5 py-4 rounded-2xl" placeholder="John Kassano">
                </div>
                <div>
                    <label class="text-xs tracking-widest block mb-2">RANG</label>
                    <select id="new-rank" class="w-full glass px-5 py-4 rounded-2xl">
                        <option value="0">Rang 0 - Recrue</option>
                        <option value="1">Rang 1 - Soldat Confirmé</option>
                    </select>
                </div>
            </div>
            
            <div class="flex gap-4 mt-10">
                <button onclick="hideAddModal()" class="flex-1 py-5 border border-slate-600 rounded-2xl">ANNULER</button>
                <button onclick="addSoldier()" class="flex-1 py-5 bg-emerald-500 text-black rounded-2xl font-bold">AJOUTER SOLDAT</button>
            </div>
        </div>
    </div>

    <script>
        // Tailwind script
        tailwind.config = {
            content: [],
            theme: {
                extend: {}
            }
        }

        let soldiers = [
            { code: "KASSANO-10", name: "John Kassano", rank: 1, formation: true, date: "2026-06-18" },
            { code: "RIVERA-47", name: "Maria Rivera", rank: 0, formation: true, date: "2026-06-20" },
            { code: "THORNE-22", name: "Alex Thorne", rank: 0, formation: false, date: "2026-06-15" },
            { code: "VALDEZ-09", name: "Samuel Valdez", rank: 1, formation: true, date: "2026-06-19" }
        ]

        function updateTime() {
            const timeEl = document.getElementById('current-time')
            setInterval(() => {
                const now = new Date()
                timeEl.textContent = now.toLocaleTimeString('fr-FR', { hour: '2-digit', minute: '2-digit' })
            }, 1000)
        }

        function renderSoldiers(filteredSoldiers = soldiers) {
            const tbody = document.getElementById('soldiers-table')
            tbody.innerHTML = ''
            
            filteredSoldiers.forEach(s => {
                const tr = document.createElement('tr')
                tr.className = 'border-b border-slate-700 hover:bg-white/5 transition-all'
                tr.innerHTML = `
                    <td class="p-6 font-mono">${s.code}</td>
                    <td class="p-6">${s.name}</td>
                    <td class="p-6"><span class="px-4 py-1 rounded-full ${s.rank === 1 ? 'bg-violet-500/20 text-violet-300' : 'bg-slate-500/20 text-slate-300'}">${s.rank === 0 ? 'R0' : 'R1'}</span></td>
                    <td class="p-6">
                        <span onclick="toggleFormation('${s.code}')" class="cursor-pointer inline-flex items-center gap-2 px-5 py-1 rounded-3xl ${s.formation ? 'bg-emerald-500/20 text-emerald-400' : 'bg-red-500/20 text-red-400'}">
                            ${s.formation ? '✅ FORMATION VALIDÉE' : '❌ NON VALIDÉE'}
                        </span>
                    </td>
                    <td class="p-6 text-slate-400">${s.date}</td>
                    <td class="p-6">
                        <button onclick="deleteSoldier('${s.code}')" class="text-red-400 hover:text-red-500">
                            <i class="fa-solid fa-trash"></i>
                        </button>
                    </td>
                `
                tbody.appendChild(tr)
            })
        }

        function renderRecent() {
            const container = document.getElementById('recent-soldiers')
            container.innerHTML = ''
            
            soldiers.slice(0, 3).forEach(s => {
                const div = document.createElement('div')
                div.className = 'glass rounded-3xl p-5'
                div.innerHTML = `
                    <div class="font-mono text-xs mb-2">${s.code}</div>
                    <div class="font-medium">${s.name}</div>
                    <div class="mt-4 flex justify-between items-end">
                        <div class="${s.formation ? 'text-emerald-400' : 'text-red-400'}">
                            ${s.formation ? '✅ Validé' : '❌ En cours'}
                        </div>
                        <div class="text-xs text-slate-500">Rang ${s.rank}</div>
                    </div>
                `
                container.appendChild(div)
            })
        }

        function updateStats() {
            const rank0 = soldiers.filter(s => s.rank === 0).length
            const rank1 = soldiers.filter(s => s.rank === 1).length
            
            document.getElementById('rank0-count').textContent = rank0 + 12
            document.getElementById('rank1-count').textContent = rank1 + 7
            document.getElementById('total-formed').textContent = soldiers.length + 45
        }

        function navigateTo(page) {
            document.querySelectorAll('#dashboard, #soldiers, #formation, #reports').forEach(el => {
                el.classList.add('hidden')
            })
            document.getElementById(page).classList.remove('hidden')
            
            document.querySelectorAll('.nav-item').forEach(el => el.classList.remove('active'))
        }

        function showAdminModal() {
            document.getElementById('admin-modal').classList.remove('hidden')
            document.getElementById('admin-code').focus()
        }

        function hideAdminModal() {
            document.getElementById('admin-modal').classList.add('hidden')
        }

        function validateAdminCode() {
            const code = document.getElementById('admin-code').value.trim()
            if (code === "Kassano @10" || code === "kassano@10") {
                hideAdminModal()
                document.getElementById('add-soldier-modal').classList.remove('hidden')
                document.getElementById('new-code').focus()
            } else {
                alert("Code d'accès invalide. Accès refusé.")
            }
        }

        function hideAddModal() {
            document.getElementById('add-soldier-modal').classList.add('hidden')
        }

        function addSoldier() {
            const code = document.getElementById('new-code').value.trim()
            const name = document.getElementById('new-name').value.trim()
            const rank = parseInt(document.getElementById('new-rank').value)
            
            if (!code || !name) {
                alert("Veuillez remplir tous les champs.")
                return
            }
            
            soldiers.unshift({
                code: code.toUpperCase(),
                name: name,
                rank: rank,
                formation: true,
                date: "2026-06-21"
            })
            
            hideAddModal()
            renderSoldiers()
            renderRecent()
            updateStats()
            
            alert(`Soldat ${name} ajouté avec succès.`)
        }

        function checkFormation() {
            const codeInput = document.getElementById('check-code').value.trim().toUpperCase()
            const resultDiv = document.getElementById('formation-result')
            
            if (!codeInput) {
                resultDiv.innerHTML = `<p class="text-red-400">Veuillez entrer un code soldat.</p>`
                return
            }
            
            const soldier = soldiers.find(s => s.code === codeInput)
            
            if (soldier) {
                resultDiv.innerHTML = `
                    <div class="text-6xl mb-4 ${soldier.formation ? 'status-valid' : ''}">
                        ${soldier.formation ? '✅' : '❌'}
                    </div>
                    <div class="text-2xl font-bold ${soldier.formation ? 'text-emerald-400' : 'text-red-400'}">
                        ${soldier.formation ? 'FORMATION VALIDÉE' : 'FORMATION NON VALIDÉE'}
                    </div>
                    <div class="mt-3 text-slate-400">${soldier.name} • Rang ${soldier.rank}</div>
                `
            } else {
                resultDiv.innerHTML = `
                    <div class="text-amber-400 text-6xl mb-4">⚠️</div>
                    <div class="text-xl font-medium">Soldat non trouvé dans la base U.I.T</div>
                `
            }
        }

        function toggleFormation(code) {
            const soldier = soldiers.find(s => s.code === code)
            if (soldier) {
                soldier.formation = !soldier.formation
                renderSoldiers()
                updateStats()
            }
        }

        function deleteSoldier(code) {
            if (confirm("Supprimer ce soldat ? Cette action est irréversible.")) {
                soldiers = soldiers.filter(s => s.code !== code)
                renderSoldiers()
                renderRecent()
                updateStats()
            }
        }

        function searchSoldiers() {
            const term = document.getElementById('search-input').value.toLowerCase().trim()
            const filtered = soldiers.filter(s => 
                s.name.toLowerCase().includes(term) || 
                s.code.toLowerCase().includes(term)
            )
            renderSoldiers(filtered)
        }

        function logout() {
            if (confirm("Déconnexion de la plateforme U.I.T ?")) {
                window.location.reload()
            }
        }

        // Initialize
        window.onload = function() {
            updateTime()
            renderSoldiers()
            renderRecent()
            updateStats()
            
            // Keyboard shortcuts
            document.addEventListener('keydown', function(e) {
                if (e.key === "/" && document.getElementById('soldiers').classList.contains('hidden') === false) {
                    document.getElementById('search-input').focus()
                    e.preventDefault()
                }
            })
            
            // Make nav items nice
            document.querySelectorAll('.nav-item').forEach(item => {
                item.addEventListener('click', () => {
                    document.querySelectorAll('.nav-item').forEach(i => i.classList.remove('active'))
                    item.classList.add('active')
                })
            })
            
            console.log('%cU.I.T v27.4.1 - Plateforme confidentielle chargée avec succès', 'color:#22d3ee; font-family:monospace')
        }
    </script>
</body>
</html>
