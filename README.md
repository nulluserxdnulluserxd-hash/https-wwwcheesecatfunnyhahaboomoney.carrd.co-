<!DOCTYPE html>
<html lang="en" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>StreamShare Global - Media & Community Hub</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        darkbg: '#0b0f19',
                        cardbg: '#151c2c',
                        cardborder: '#232d42',
                        accent: '#6366f1',
                        accenthover: '#4f46e5'
                    }
                }
            }
        }
    </script>
    <style>
        @import url('https://fonts.googleapis.com/css2?family=Plus+Jakarta+Sans:wght@300;400;500;600;700;800&display=swap');
        body { font-family: 'Plus Jakarta Sans', sans-serif; }
        
        /* Custom scrollbars */
        ::-webkit-scrollbar {
            width: 6px;
            height: 6px;
        }
        ::-webkit-scrollbar-track {
            background: #0b0f19;
        }
        ::-webkit-scrollbar-thumb {
            background: #232d42;
            border-radius: 9999px;
        }
        ::-webkit-scrollbar-thumb:hover {
            background: #374151;
        }
    </style>
</head>
<body class="bg-darkbg text-slate-100 min-h-screen flex flex-col antialiased selection:bg-indigo-500 selection:text-white">

    <header class="bg-cardbg/90 backdrop-blur-md border-b border-cardborder sticky top-0 z-40 px-4 lg:px-6 py-3">
        <div class="max-w-7xl mx-auto flex items-center justify-between gap-4">
            
            <!-- Brand Logo -->
            <div class="flex items-center space-x-3 shrink-0">
                <div class="bg-gradient-to-tr from-indigo-600 via-purple-600 to-pink-500 p-2.5 rounded-2xl text-white shadow-lg shadow-indigo-500/20">
                    <i class="fa-solid fa-play text-lg"></i>
                </div>
                <div>
                    <div class="flex items-center gap-2">
                        <h1 class="font-extrabold text-lg tracking-tight bg-gradient-to-r from-white via-slate-200 to-slate-400 bg-clip-text text-transparent">
                            StreamShare
                        </h1>
                        <span class="text-xs font-semibold px-2 py-0.5 rounded-full bg-indigo-500/10 text-indigo-400 border border-indigo-500/20">
                            GLOBAL
                        </span>
                    </div>
                    <!-- Connection Status Badge -->
                    <div class="flex items-center gap-1.5 mt-0.5">
                        <span id="connection-dot" class="w-2 h-2 rounded-full bg-emerald-500 animate-pulse"></span>
                        <span id="connection-status" class="text-[11px] font-medium text-emerald-400">Connected to Global Relay</span>
                    </div>
                </div>
            </div>

            <!-- Central Navigation Tabs -->
            <nav class="hidden md:flex items-center gap-1 bg-slate-900/80 p-1 rounded-2xl border border-cardborder">
                <button onclick="switchTab('player-tab')" id="tab-btn-player" class="nav-tab-btn active px-4 py-2 rounded-xl text-xs font-semibold flex items-center gap-2 transition-all bg-indigo-600 text-white shadow-md">
                    <i class="fa-solid fa-tv"></i> Media Player
                </button>
                <button onclick="switchTab('user-uploads-tab')" id="tab-btn-user-uploads" class="nav-tab-btn px-4 py-2 rounded-xl text-xs font-semibold flex items-center gap-2 transition-all text-slate-400 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-cloud-arrow-up"></i> User Uploaded <span id="user-uploads-count" class="bg-indigo-500/20 text-indigo-300 text-[10px] px-1.5 py-0.5 rounded-full">0</span>
                </button>
                <button onclick="switchTab('lounge-tab')" id="tab-btn-lounge" class="nav-tab-btn px-4 py-2 rounded-xl text-xs font-semibold flex items-center gap-2 transition-all text-slate-400 hover:text-white hover:bg-slate-800">
                    <i class="fa-solid fa-comments"></i> Lounge & Call
                </button>
            </nav>

            <!-- Header Action Controls -->
            <div class="flex items-center gap-2 sm:gap-3 shrink-0">
                
                <!-- Ambient Music Synthesizer Toggle -->
                <button id="ambient-music-btn" onclick="toggleAmbientMusic()" class="bg-slate-900 hover:bg-slate-800 text-slate-300 border border-cardborder px-3 py-2 rounded-xl text-xs font-medium flex items-center gap-2 transition shadow-sm">
                    <i id="music-icon" class="fa-solid fa-music text-indigo-400"></i>
                    <span id="music-text" class="hidden sm:inline">Ambient Music: Off</span>
                </button>

                <!-- Data Save / Restore Dropdown -->
                <div class="relative group">
                    <button class="bg-slate-900 hover:bg-slate-800 text-slate-300 border border-cardborder px-3 py-2 rounded-xl text-xs font-medium flex items-center gap-2 transition">
                        <i class="fa-solid fa-floppy-disk text-emerald-400"></i>
                        <span class="hidden sm:inline">Data State</span>
                        <i class="fa-solid fa-chevron-down text-[10px] opacity-60"></i>
                    </button>
                    <div class="absolute right-0 mt-2 w-48 bg-cardbg border border-cardborder rounded-2xl shadow-2xl py-2 hidden group-hover:block z-50">
                        <button onclick="exportDataState()" class="w-full text-left px-4 py-2 text-xs hover:bg-slate-800 flex items-center gap-2 text-slate-200">
                            <i class="fa-solid fa-download text-indigo-400"></i> Export Save File (.json)
                        </button>
                        <label class="w-full text-left px-4 py-2 text-xs hover:bg-slate-800 flex items-center gap-2 text-slate-200 cursor-pointer">
                            <i class="fa-solid fa-upload text-emerald-400"></i> Import Save File
                            <input type="file" id="import-state-file" accept=".json" onchange="importDataState(event)" class="hidden">
                        </label>
                        <hr class="my-1 border-cardborder">
                        <button onclick="loadSampleData()" class="w-full text-left px-4 py-2 text-xs hover:bg-slate-800 flex items-center gap-2 text-amber-300">
                            <i class="fa-solid fa-flask"></i> Load Sample Data
                        </button>
                    </div>
                </div>

                <!-- User Session / Dev Toggle -->
                <button onclick="openDevAuthModal()" class="bg-indigo-600/10 hover:bg-indigo-600/20 text-indigo-300 border border-indigo-500/20 px-3 py-2 rounded-xl text-xs font-medium flex items-center gap-2 transition">
                    <i id="user-badge-icon" class="fa-solid fa-user"></i>
                    <span id="current-user-display" class="max-w-[100px] truncate">Guest User</span>
                </button>
            </div>
        </div>

        <!-- Mobile Navigation Row -->
        <div class="flex md:hidden items-center justify-around gap-1 mt-3 pt-2 border-t border-cardborder">
            <button onclick="switchTab('player-tab')" class="mobile-tab-btn active text-xs font-semibold px-3 py-1.5 rounded-lg text-indigo-400 bg-indigo-500/10 flex items-center gap-1.5">
                <i class="fa-solid fa-tv"></i> Player
            </button>
            <button onclick="switchTab('user-uploads-tab')" class="mobile-tab-btn text-xs font-semibold px-3 py-1.5 rounded-lg text-slate-400 flex items-center gap-1.5">
                <i class="fa-solid fa-cloud-arrow-up"></i> User Uploaded
            </button>
            <button onclick="switchTab('lounge-tab')" class="mobile-tab-btn text-xs font-semibold px-3 py-1.5 rounded-lg text-slate-400 flex items-center gap-1.5">
                <i class="fa-solid fa-comments"></i> Lounge
            </button>
        </div>
    </header>

    <main class="flex-1 max-w-7xl w-full mx-auto p-4 lg:p-6">

        <!-- TAB 1: MEDIA PLAYER SECTION -->
        <section id="player-tab" class="tab-content space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                
                <!-- Left Column: Video Stage & Controller -->
                <div class="lg:col-span-2 space-y-6">
                    
                    <!-- Player Box -->
                    <div class="bg-cardbg rounded-3xl border border-cardborder shadow-2xl overflow-hidden flex flex-col">
                        
                        <!-- Header info -->
                        <div class="p-4 border-b border-cardborder flex items-center justify-between gap-3">
                            <div class="flex items-center gap-3 overflow-hidden">
                                <span class="p-2 rounded-xl bg-indigo-500/10 text-indigo-400 border border-indigo-500/20 text-xs">
                                    <i class="fa-solid fa-circle-play"></i>
                                </span>
                                <div class="truncate">
                                    <h2 id="player-title" class="font-bold text-sm text-slate-100 truncate">No Media Active</h2>
                                    <p id="player-author" class="text-xs text-slate-400 truncate">Select a video from below or upload a new file.</p>
                                </div>
                            </div>
                            <span id="player-source-tag" class="text-[10px] font-bold uppercase tracking-wider bg-slate-900 border border-cardborder text-slate-400 px-3 py-1 rounded-full shrink-0">
                                Standby
                            </span>
                        </div>

                        <!-- Video Stage with Fullscreen Button -->
                        <div id="video-stage" class="relative aspect-video bg-black flex items-center justify-center overflow-hidden group">
                            
                            <!-- Placeholder display -->
                            <div id="player-placeholder" class="text-center p-8 space-y-3">
                                <div class="w-16 h-16 rounded-3xl bg-slate-900 border border-cardborder flex items-center justify-center mx-auto text-indigo-400 text-2xl shadow-inner">
                                    <i class="fa-solid fa-photo-film"></i>
                                </div>
                                <h3 class="font-bold text-slate-300 text-sm">Media Player Ready</h3>
                                <p class="text-xs text-slate-500 max-w-xs mx-auto">Upload video files up to 5 GB or paste direct URLs / YouTube links.</p>
                            </div>

                            <!-- Native HTML5 Video Element -->
                            <video id="native-player" class="w-full h-full object-contain hidden" controls crossorigin="anonymous"></video>

                            <!-- YouTube / Embed Iframe -->
                            <iframe id="iframe-player" class="w-full h-full hidden border-0" allowfullscreen allow="autoplay"></iframe>

                            <!-- Fullscreen Toggle Button Overlay -->
                            <button onclick="toggleFullscreen()" class="absolute bottom-4 right-4 bg-slate-900/80 hover:bg-slate-800 text-slate-200 border border-cardborder px-3 py-1.5 rounded-xl text-xs font-semibold flex items-center gap-1.5 backdrop-blur-sm transition shadow-lg z-20 opacity-0 group-hover:opacity-100 focus:opacity-100">
                                <i class="fa-solid fa-expand"></i> Fullscreen
                            </button>

                            <!-- Playback Error Fallback Notice -->
                            <div id="player-error-overlay" class="absolute inset-0 bg-slate-950/95 flex flex-col items-center justify-center p-6 text-center space-y-3 hidden">
                                <div class="w-12 h-12 rounded-full bg-amber-500/10 border border-amber-500/20 text-amber-400 flex items-center justify-center text-xl">
                                    <i class="fa-solid fa-triangle-exclamation"></i>
                                </div>
                                <h4 class="font-bold text-slate-200 text-sm">Playback Error</h4>
                                <p id="player-error-message" class="text-xs text-slate-400 max-w-md">The requested video stream could not be loaded directly.</p>
                                <button onclick="dismissPlayerError()" class="bg-slate-800 hover:bg-slate-700 text-slate-200 text-xs font-semibold px-4 py-2 rounded-xl border border-cardborder transition">
                                    Dismiss Warning
                                </button>
                            </div>
                        </div>

                        <!-- Video Quick Input & Upload Form -->
                        <div class="p-4 bg-slate-900/60 border-t border-cardborder flex flex-col sm:flex-row items-center gap-3">
                            <div class="w-full flex-1 flex gap-2">
                                <input type="text" id="direct-url-input" placeholder="Paste Direct Video URL or YouTube Link..." class="flex-1 bg-darkbg border border-cardborder rounded-xl px-4 py-2.5 text-xs text-slate-200 focus:outline-none focus:border-indigo-500 transition">
                                <button onclick="loadFromInputUrl()" class="bg-indigo-600 hover:bg-indigo-500 text-white px-4 py-2.5 rounded-xl text-xs font-semibold transition shrink-0 flex items-center gap-1.5">
                                    <i class="fa-solid fa-link"></i> Load
                                </button>
                            </div>
                            
                            <!-- File Upload Trigger (Up to 5GB) -->
                            <label class="w-full sm:w-auto bg-slate-800 hover:bg-slate-700 text-slate-200 border border-cardborder px-4 py-2.5 rounded-xl text-xs font-semibold cursor-pointer flex items-center justify-center gap-2 transition shrink-0">
                                <i class="fa-solid fa-upload text-indigo-400"></i>
                                <span>Upload Video (Max 5GB)</span>
                                <input type="file" id="media-file-input" accept="video/*" onchange="handleFileUpload(event)" class="hidden">
                            </label>
                        </div>
                    </div>

                    <!-- Quick Upload Info Alert -->
                    <div class="bg-cardbg rounded-2xl border border-cardborder p-4 flex items-center justify-between text-xs text-slate-400">
                        <div class="flex items-center gap-3">
                            <i class="fa-solid fa-shield-halved text-indigo-400 text-base"></i>
                            <span>All uploads automatically register under your current session (<strong id="session-author-tag" class="text-slate-200">Guest</strong>).</span>
                        </div>
                        <span class="text-[10px] bg-slate-900 border border-cardborder px-2.5 py-1 rounded-lg text-slate-400">Limit: 5 GB / File</span>
                    </div>

                </div>

                <!-- Right Column: Personal Playlist Side Panel -->
                <div class="space-y-6">
                    <div class="bg-cardbg rounded-3xl border border-cardborder p-5 space-y-4 shadow-xl flex flex-col h-full">
                        <div class="flex items-center justify-between border-b border-cardborder pb-3">
                            <div class="flex items-center gap-2">
                                <i class="fa-solid fa-list-ul text-indigo-400"></i>
                                <h3 class="font-bold text-sm text-slate-200">Current Session Playlist</h3>
                            </div>
                            <span id="playlist-count-badge" class="text-[10px] font-bold bg-slate-900 border border-cardborder text-slate-400 px-2 py-0.5 rounded-full">0 items</span>
                        </div>

                        <!-- Playlist Container -->
                        <div id="playlist-list" class="space-y-2.5 max-h-[460px] overflow-y-auto pr-1">
                            <!-- Items populated by JS -->
                        </div>
                    </div>
                </div>

            </div>
        </section>

        <section id="user-uploads-tab" class="tab-content hidden space-y-6">
            
            <!-- Tab Banner Header -->
            <div class="bg-gradient-to-r from-cardbg via-slate-900 to-cardbg rounded-3xl border border-cardborder p-6 flex flex-col md:flex-row items-start md:items-center justify-between gap-4 shadow-xl">
                <div>
                    <div class="flex items-center gap-2 text-indigo-400 font-bold text-xs uppercase tracking-wider mb-1">
                        <i class="fa-solid fa-globe"></i> Global Community Feed
                    </div>
                    <h2 class="text-xl font-extrabold text-slate-100">User Uploaded Videos</h2>
                    <p class="text-xs text-slate-400 mt-1 max-w-xl">
                        Explore content contributed by users worldwide. Direct ownership permissions ensure users can only modify or delete videos they personally uploaded.
                    </p>
                </div>
                
                <label class="bg-indigo-600 hover:bg-indigo-500 text-white font-semibold text-xs px-5 py-3 rounded-2xl shadow-lg shadow-indigo-600/20 flex items-center gap-2 cursor-pointer transition">
                    <i class="fa-solid fa-cloud-arrow-up"></i> Upload New Media
                    <input type="file" accept="video/*" onchange="handleFileUpload(event)" class="hidden">
                </label>
            </div>

            <!-- Uploads Grid -->
            <div id="user-uploads-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- User Upload Cards populated dynamically -->
            </div>

        </section>

        <section id="lounge-tab" class="tab-content hidden space-y-6">
            <div class="grid grid-cols-1 lg:grid-cols-3 gap-6">
                
                <!-- Chat Window -->
                <div class="lg:col-span-2 bg-cardbg rounded-3xl border border-cardborder shadow-xl flex flex-col h-[600px] overflow-hidden">
                    <div class="p-4 border-b border-cardborder flex items-center justify-between bg-slate-900/50">
                        <div class="flex items-center gap-3">
                            <div class="w-3 h-3 rounded-full bg-emerald-500 animate-pulse"></div>
                            <h3 class="font-bold text-sm text-slate-200">Global Peer Lounge</h3>
                        </div>
                        <button onclick="startCallSimulation()" class="bg-emerald-600/20 hover:bg-emerald-600/30 text-emerald-300 border border-emerald-500/30 text-xs px-3 py-1.5 rounded-xl flex items-center gap-1.5 transition">
                            <i class="fa-solid fa-phone"></i> Start Audio Call
                        </button>
                    </div>

                    <!-- Messages Box -->
                    <div id="chat-messages-box" class="flex-1 p-4 overflow-y-auto space-y-3 bg-slate-950/40">
                        <!-- Messages dynamically inserted -->
                    </div>

                    <!-- Input Bar -->
                    <form onsubmit="sendChatMessage(event)" class="p-3 border-t border-cardborder bg-slate-900/80 flex gap-2">
                        <input type="text" id="chat-input" placeholder="Type a message to the community..." class="flex-1 bg-darkbg border border-cardborder rounded-xl px-4 py-2.5 text-xs text-slate-200 focus:outline-none focus:border-indigo-500">
                        <button type="submit" class="bg-indigo-600 hover:bg-indigo-500 text-white px-5 py-2.5 rounded-xl text-xs font-semibold transition">
                            <i class="fa-solid fa-paper-plane"></i>
                        </button>
                    </form>
                </div>

                <!-- Call Status & Active Peer Sidebar -->
                <div class="space-y-6">
                    <div class="bg-cardbg rounded-3xl border border-cardborder p-5 space-y-4 shadow-xl">
                        <h4 class="font-bold text-xs uppercase text-slate-400 tracking-wider">Active Channel Status</h4>
                        <div id="call-status-card" class="bg-slate-900 border border-cardborder p-4 rounded-2xl space-y-3">
                            <div class="flex items-center justify-between">
                                <span class="text-xs text-slate-300">Audio Lounge:</span>
                                <span class="text-[10px] font-bold bg-slate-800 text-slate-400 px-2 py-0.5 rounded-full">Standby</span>
                            </div>
                            <p class="text-xs text-slate-500">Click 'Start Audio Call' to initiate peer audio session.</p>
                        </div>
                    </div>

                    <!-- Dev / Admin Panel Button inside sidebar -->
                    <div class="bg-cardbg rounded-3xl border border-amber-500/20 p-5 space-y-3 shadow-xl">
                        <div class="flex items-center gap-2 text-amber-400 font-bold text-xs">
                            <i class="fa-solid fa-terminal"></i> Developer & Moderation
                        </div>
                        <p class="text-xs text-slate-400">Access superuser administrative tools and global content purge.</p>
                        <button onclick="openDevAuthModal()" class="w-full bg-amber-500/10 hover:bg-amber-500/20 text-amber-300 border border-amber-500/30 text-xs py-2.5 rounded-xl font-semibold transition flex items-center justify-center gap-2">
                            <i class="fa-solid fa-key"></i> Open Sudo Console
                        </button>
                    </div>
                </div>

            </div>
        </section>

    </main>

    <!-- Audio Call Overlay Modal -->
    <div id="call-modal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-cardbg border border-cardborder rounded-3xl w-full max-w-sm p-6 text-center space-y-6 shadow-2xl">
            <div class="relative w-24 h-24 mx-auto">
                <div class="absolute inset-0 rounded-full bg-emerald-500/20 animate-ping"></div>
                <div class="relative w-24 h-24 rounded-full bg-slate-900 border-2 border-emerald-500 flex items-center justify-center text-3xl text-emerald-400 shadow-xl">
                    <i class="fa-solid fa-microphone"></i>
                </div>
            </div>

            <div>
                <h3 class="font-bold text-slate-100 text-base">Global Audio Lounge Active</h3>
                <p class="text-xs text-slate-400 mt-1">Live voice stream connected.</p>
            </div>

            <div class="flex items-center justify-center gap-4">
                <button onclick="toggleMuteCall()" id="call-mute-btn" class="w-12 h-12 rounded-full bg-slate-800 hover:bg-slate-700 text-slate-200 border border-cardborder flex items-center justify-center transition">
                    <i class="fa-solid fa-microphone"></i>
                </button>
                <button onclick="endCallSimulation()" class="w-12 h-12 rounded-full bg-rose-600 hover:bg-rose-500 text-white flex items-center justify-center shadow-lg shadow-rose-600/30 transition">
                    <i class="fa-solid fa-phone-slash"></i>
                </button>
            </div>
        </div>
    </div>

    <!-- Dev / Auth / Sudo Modal -->
    <div id="dev-modal" class="fixed inset-0 bg-slate-950/80 backdrop-blur-md z-50 flex items-center justify-center p-4 hidden">
        <div class="bg-cardbg border border-cardborder rounded-3xl w-full max-w-lg p-6 space-y-5 shadow-2xl">
            <div class="flex items-center justify-between border-b border-cardborder pb-3">
                <div class="flex items-center gap-2">
                    <i class="fa-solid fa-shield-cat text-amber-400 text-lg"></i>
                    <h3 class="font-bold text-slate-100 text-sm">User Session & Sudo Console</h3>
                </div>
                <button onclick="closeDevModal()" class="text-slate-400 hover:text-slate-200 text-sm">
                    <i class="fa-solid fa-xmark"></i>
                </button>
            </div>

            <div class="space-y-4">
                <div>
                    <label class="text-xs text-slate-400 mb-1 block">Set Your Display Name / Author Tag</label>
                    <div class="flex gap-2">
                        <input type="text" id="username-input" placeholder="e.g. Alex_Streamer" class="flex-1 bg-darkbg border border-cardborder rounded-xl px-4 py-2 text-xs text-slate-200 focus:outline-none focus:border-indigo-500">
                        <button onclick="updateUsername()" class="bg-indigo-600 hover:bg-indigo-500 text-white text-xs px-4 py-2 rounded-xl font-semibold transition">
                            Save Name
                        </button>
                    </div>
                </div>

                <hr class="border-cardborder">

                <div class="space-y-3">
                    <div class="flex items-center justify-between">
                        <h4 class="font-bold text-xs text-amber-400 uppercase tracking-wider">Sudo Administrator Key</h4>
                        <span id="sudo-status-badge" class="text-[10px] bg-slate-900 text-slate-500 border border-cardborder px-2 py-0.5 rounded-full">Standard User</span>
                    </div>

                    <div class="flex gap-2">
                        <input type="password" id="sudo-key-input" placeholder="Enter Sudo Passcode (Key: sudo123)..." class="flex-1 bg-darkbg border border-cardborder rounded-xl px-4 py-2 text-xs text-slate-200 focus:outline-none focus:border-amber-500">
                        <button onclick="authenticateSudo()" class="bg-amber-600 hover:bg-amber-500 text-white text-xs px-4 py-2 rounded-xl font-semibold transition">
                            Unlock Sudo
                        </button>
                    </div>
                </div>

                <!-- Admin Action Controls -->
                <div id="sudo-tools" class="hidden pt-2 space-y-2 border-t border-cardborder">
                    <p class="text-[11px] text-amber-300 font-medium">Sudo Privileges Active: Global content moderation enabled.</p>
                    <div class="flex gap-2">
                        <button onclick="sudoPurgeAllUploads()" class="bg-rose-600/20 hover:bg-rose-600/30 text-rose-300 border border-rose-500/30 text-xs px-3 py-2 rounded-xl w-full transition">
                            <i class="fa-solid fa-dumpster"></i> Purge All Uploads
                        </button>
                    </div>
                </div>
            </div>

            <div class="flex justify-end pt-2">
                <button onclick="closeDevModal()" class="bg-slate-800 hover:bg-slate-700 text-xs text-slate-300 px-4 py-2 rounded-xl font-semibold">
                    Close
                </button>
            </div>
        </div>
    </div>

    <!-- Notification / Custom Toast UI -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col gap-2 pointer-events-none"></div>

    <script>
        // Constants
        const MAX_FILE_SIZE = 5 * 1024 * 1024 * 1024; // 5 GB limit in bytes

        // Global Application State
        let appData = {
            currentUser: 'User_' + Math.floor(1000 + Math.random() * 9000),
            isSudo: false,
            playlist: [],
            userUploads: [],
            chatMessages: [
                { id: 1, sender: 'System', text: 'Welcome to StreamShare Global Lounge!', timestamp: '12:00 PM' }
            ]
        };

        // Web Audio Synthesizer State
        let audioCtx = null;
        let isAmbientPlaying = false;
        let ambientTimer = null;

        // Initialize application on load
        window.addEventListener('DOMContentLoaded', () => {
            // Retrieve session storage if available
            const savedState = localStorage.getItem('streamshare_state');
            if (savedState) {
                try {
                    const parsed = JSON.parse(savedState);
                    appData = { ...appData, ...parsed };
                } catch(e) { console.warn('Saved state parse error', e); }
            }

            updateUserDisplay();
            renderPlaylist();
            renderUserUploads();
            renderChat();

            showToast(`Connected as ${appData.currentUser}`, 'info');
        });

        // Save State Persist Helper
        function persistState() {
            try {
                // Strip Object URLs before saving to localStorage to prevent quota errors
                const cleanUploads = appData.userUploads.map(item => ({
                    ...item,
                    fileObjectUrl: item.isLocalFile ? null : item.url
                }));
                const stateToSave = { ...appData, userUploads: cleanUploads };
                localStorage.setItem('streamshare_state', JSON.stringify(stateToSave));
            } catch (e) {
                console.warn('LocalStorage save skipped due to size limits', e);
            }
        }

        // --- TAB SWITCHING ---
        function switchTab(tabId) {
            document.querySelectorAll('.tab-content').forEach(el => el.classList.add('hidden'));
            document.getElementById(tabId).classList.remove('hidden');

            document.querySelectorAll('.nav-tab-btn').forEach(btn => {
                btn.classList.remove('bg-indigo-600', 'text-white', 'shadow-md');
                btn.classList.add('text-slate-400');
            });

            const activeBtn = document.getElementById('tab-btn-' + tabId.replace('-tab', ''));
            if (activeBtn) {
                activeBtn.classList.add('bg-indigo-600', 'text-white', 'shadow-md');
                activeBtn.classList.remove('text-slate-400');
            }
        }


        // Handle Uploads up to 5GB
        function handleFileUpload(event) {
            const file = event.target.files[0];
            if (!file) return;

            // Validate Size Limit (5GB)
            if (file.size > MAX_FILE_SIZE) {
                showToast(`File size (${formatBytes(file.size)}) exceeds the maximum 5 GB limit.`, 'error');
                event.target.value = '';
                return;
            }

            // Create Memory-Efficient Object URL
            const objectUrl = URL.createObjectURL(file);

            const uploadItem = {
                id: 'up_' + Date.now() + '_' + Math.floor(Math.random()*1000),
                title: file.name,
                url: objectUrl,
                author: appData.currentUser,
                size: formatBytes(file.size),
                rawSize: file.size,
                type: 'local',
                isLocalFile: true,
                timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
            };

            // Add to User Uploads and Playlist
            appData.userUploads.unshift(uploadItem);
            appData.playlist.unshift(uploadItem);

            renderUserUploads();
            renderPlaylist();
            loadMediaItem(uploadItem);
            persistState();

            showToast(`Uploaded "${file.name}" (${uploadItem.size})`, 'success');
            event.target.value = '';
        }

        function loadFromInputUrl() {
            const input = document.getElementById('direct-url-input');
            const url = input.value.trim();
            if (!url) return;

            const newItem = {
                id: 'url_' + Date.now(),
                title: 'External Stream / Link',
                url: url,
                author: appData.currentUser,
                size: 'Web Stream',
                type: 'url',
                isLocalFile: false,
                timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
            };

            appData.userUploads.unshift(newItem);
            appData.playlist.unshift(newItem);

            renderUserUploads();
            renderPlaylist();
            loadMediaItem(newItem);
            persistState();

            input.value = '';
            showToast('External video stream loaded', 'info');
        }

        function loadMediaItem(item) {
            const nativePlayer = document.getElementById('native-player');
            const iframePlayer = document.getElementById('iframe-player');
            const placeholder = document.getElementById('player-placeholder');
            const errorOverlay = document.getElementById('player-error-overlay');
            const titleEl = document.getElementById('player-title');
            const authorEl = document.getElementById('player-author');
            const sourceTag = document.getElementById('player-source-tag');

            // Reset
            nativePlayer.pause();
            nativePlayer.src = '';
            nativePlayer.classList.add('hidden');
            iframePlayer.src = '';
            iframePlayer.classList.add('hidden');
            placeholder.classList.add('hidden');
            errorOverlay.classList.add('hidden');

            titleEl.innerText = item.title;
            authorEl.innerText = `Uploaded by @${item.author} • ${item.size}`;
            sourceTag.innerText = item.type.toUpperCase();

            // Check if YouTube
            const ytMatch = item.url.match(/(?:youtu\.be\/|youtube\.com\/(?:embed\/|v\/|watch\?v=|watch\?.+&v=))([\w-]{11})/);

            if (ytMatch && ytMatch[1]) {
                iframePlayer.src = `https://www.youtube.com/embed/${ytMatch[1]}?autoplay=1`;
                iframePlayer.classList.remove('hidden');
            } else {
                nativePlayer.src = item.url;
                nativePlayer.classList.remove('hidden');
                nativePlayer.play().catch(e => {
                    console.warn("Autoplay restricted or error", e);
                });
            }

            nativePlayer.onerror = function() {
                showPlayerError("The video file or stream could not be decoded.");
            };
        }

        // Fullscreen Toggle for Video Stage
        function toggleFullscreen() {
            const stage = document.getElementById('video-stage');
            if (!document.fullscreenElement) {
                if (stage.requestFullscreen) {
                    stage.requestFullscreen();
                } else if (stage.webkitRequestFullscreen) {
                    stage.webkitRequestFullscreen();
                } else if (stage.msRequestFullscreen) {
                    stage.msRequestFullscreen();
                }
            } else {
                if (document.exitFullscreen) {
                    document.exitFullscreen();
                } else if (document.webkitExitFullscreen) {
                    document.webkitExitFullscreen();
                }
            }
        }

        function showPlayerError(msg) {
            document.getElementById('player-error-message').innerText = msg;
            document.getElementById('player-error-overlay').classList.remove('hidden');
        }

        function dismissPlayerError() {
            document.getElementById('player-error-overlay').classList.add('hidden');
        }


        function renderUserUploads() {
            const container = document.getElementById('user-uploads-grid');
            const countBadge = document.getElementById('user-uploads-count');
            countBadge.innerText = appData.userUploads.length;

            if (appData.userUploads.length === 0) {
                container.innerHTML = `
                    <div class="col-span-full bg-cardbg rounded-3xl border border-cardborder p-12 text-center space-y-3">
                        <div class="w-12 h-12 rounded-2xl bg-slate-900 border border-cardborder text-slate-500 flex items-center justify-center mx-auto text-xl">
                            <i class="fa-solid fa-folder-open"></i>
                        </div>
                        <h3 class="font-bold text-slate-300 text-sm">No Community Uploads Yet</h3>
                        <p class="text-xs text-slate-500 max-w-sm mx-auto">Upload a video to start populating the global stream feed.</p>
                    </div>
                `;
                return;
            }

            container.innerHTML = appData.userUploads.map(item => {
                const isOwner = (item.author === appData.currentUser) || appData.isSudo;
                return `
                    <div class="bg-cardbg rounded-3xl border border-cardborder overflow-hidden shadow-xl flex flex-col group hover:border-indigo-500/50 transition">
                        <div class="aspect-video bg-slate-950 relative flex items-center justify-center overflow-hidden">
                            <i class="fa-solid fa-file-video text-3xl text-slate-700"></i>
                            <div class="absolute inset-0 bg-slate-950/40 opacity-0 group-hover:opacity-100 flex items-center justify-center transition gap-2">
                                <button onclick="playFromUploads('${item.id}')" class="bg-indigo-600 text-white w-10 h-10 rounded-full flex items-center justify-center text-sm shadow-lg hover:scale-110 transition">
                                    <i class="fa-solid fa-play"></i>
                                </button>
                            </div>
                            <span class="absolute bottom-2 right-2 text-[10px] bg-slate-900/80 backdrop-blur-sm text-slate-300 px-2 py-0.5 rounded-md font-mono border border-cardborder">
                                ${item.size}
                            </span>
                        </div>

                        <div class="p-4 flex-1 flex flex-col justify-between space-y-3">
                            <div>
                                <h4 class="font-bold text-xs text-slate-100 truncate">${item.title}</h4>
                                <div class="flex items-center gap-1.5 mt-1">
                                    <i class="fa-solid fa-user-circle text-indigo-400 text-xs"></i>
                                    <span class="text-[11px] text-slate-400 font-medium">@${item.author}</span>
                                    ${item.author === appData.currentUser ? '<span class="text-[9px] bg-indigo-500/20 text-indigo-300 px-1.5 py-0.2 rounded font-semibold">YOU</span>' : ''}
                                </div>
                            </div>

                            <div class="flex items-center justify-between border-t border-cardborder pt-3">
                                <span class="text-[10px] text-slate-500">${item.timestamp}</span>
                                
                                ${isOwner ? `
                                    <button onclick="deleteUploadItem('${item.id}')" class="text-xs text-slate-500 hover:text-rose-400 p-1 transition" title="Delete Upload">
                                        <i class="fa-solid fa-trash-can"></i>
                                    </button>
                                ` : `
                                    <span class="text-[10px] text-slate-600 flex items-center gap-1" title="Protected - Only owner can delete">
                                        <i class="fa-solid fa-lock text-[9px]"></i> Protected
                                    </span>
                                `}
                            </div>
                        </div>
                    </div>
                `;
            }).join('');
        }

        function playFromUploads(id) {
            const item = appData.userUploads.find(x => x.id === id);
            if (item) {
                switchTab('player-tab');
                loadMediaItem(item);
            }
        }

        function deleteUploadItem(id) {
            const item = appData.userUploads.find(x => x.id === id);
            if (!item) return;

            // Direct Ownership Permission Guard
            if (item.author !== appData.currentUser && !appData.isSudo) {
                showToast("Permission denied: You can only delete your own uploads.", "error");
                return;
            }

            appData.userUploads = appData.userUploads.filter(x => x.id !== id);
            appData.playlist = appData.playlist.filter(x => x.id !== id);

            renderUserUploads();
            renderPlaylist();
            persistState();
            showToast("Upload removed successfully.", "info");
        }

        function renderPlaylist() {
            const container = document.getElementById('playlist-list');
            const badge = document.getElementById('playlist-count-badge');
            badge.innerText = `${appData.playlist.length} items`;

            if (appData.playlist.length === 0) {
                container.innerHTML = `<p class="text-xs text-slate-500 text-center py-6">Playlist empty.</p>`;
                return;
            }

            container.innerHTML = appData.playlist.map(item => `
                <div onclick="loadMediaItemById('${item.id}')" class="bg-slate-900/80 hover:bg-slate-900 p-3 rounded-2xl border border-cardborder flex items-center justify-between gap-2 cursor-pointer transition group">
                    <div class="flex items-center gap-3 overflow-hidden">
                        <div class="w-8 h-8 rounded-xl bg-indigo-500/10 text-indigo-400 flex items-center justify-center shrink-0 group-hover:bg-indigo-600 group-hover:text-white transition">
                            <i class="fa-solid fa-play text-xs"></i>
                        </div>
                        <div class="truncate">
                            <h4 class="text-xs font-semibold text-slate-200 truncate">${item.title}</h4>
                            <p class="text-[10px] text-slate-500">@${item.author}</p>
                        </div>
                    </div>
                </div>
            `).join('');
        }

        function loadMediaItemById(id) {
            const item = appData.playlist.find(x => x.id === id);
            if (item) loadMediaItem(item);
        }


        function toggleAmbientMusic() {
            if (!isAmbientPlaying) {
                startAmbientMusic();
            } else {
                stopAmbientMusic();
            }
        }

        function startAmbientMusic() {
            try {
                audioCtx = new (window.AudioContext || window.webkitAudioContext)();
                isAmbientPlaying = true;

                document.getElementById('ambient-music-btn').classList.add('border-indigo-500', 'bg-indigo-950/40');
                document.getElementById('music-icon').classList.add('text-emerald-400', 'fa-spin');
                document.getElementById('music-text').innerText = 'Ambient Music: On';

                playPentatonicChord();
                ambientTimer = setInterval(playPentatonicChord, 7000);
                showToast("Ambient synth activated.", "info");
            } catch (e) {
                console.error(e);
                showToast("Web Audio initialized error.", "error");
            }
        }

        function stopAmbientMusic() {
            isAmbientPlaying = false;
            if (ambientTimer) clearInterval(ambientTimer);
            if (audioCtx) {
                audioCtx.close();
                audioCtx = null;
            }

            document.getElementById('ambient-music-btn').classList.remove('border-indigo-500', 'bg-indigo-950/40');
            document.getElementById('music-icon').classList.remove('text-emerald-400', 'fa-spin');
            document.getElementById('music-text').innerText = 'Ambient Music: Off';
            showToast("Ambient music stopped.", "info");
        }

        function playPentatonicChord() {
            if (!audioCtx || !isAmbientPlaying) return;
            const notes = [196.00, 220.00, 261.63, 293.66, 329.63, 392.00, 440.00];
            const baseFreq = notes[Math.floor(Math.random() * notes.length)];

            const osc = audioCtx.createOscillator();
            const gain = audioCtx.createGain();

            osc.type = 'sine';
            osc.frequency.setValueAtTime(baseFreq, audioCtx.currentTime);

            gain.gain.setValueAtTime(0, audioCtx.currentTime);
            gain.gain.linearRampToValueAtTime(0.06, audioCtx.currentTime + 2);
            gain.gain.linearRampToValueAtTime(0, audioCtx.currentTime + 6.5);

            osc.connect(gain);
            gain.connect(audioCtx.destination);

            osc.start();
            osc.stop(audioCtx.currentTime + 7);
        }


        // Fixed Blob-based Save File Download
        function exportDataState() {
            try {
                const dataStr = JSON.stringify(appData, null, 2);
                const blob = new Blob([dataStr], { type: 'application/json' });
                const url = URL.createObjectURL(blob);
                const downloadAnchor = document.createElement('a');
                downloadAnchor.href = url;
                downloadAnchor.download = `StreamShare_Save_${Date.now()}.json`;
                document.body.appendChild(downloadAnchor);
                downloadAnchor.click();
                document.body.removeChild(downloadAnchor);
                URL.revokeObjectURL(url);
                showToast("Save file exported successfully.", "success");
            } catch (err) {
                console.error(err);
                showToast("Failed to export save file.", "error");
            }
        }

        function importDataState(event) {
            const file = event.target.files[0];
            if (!file) return;

            const reader = new FileReader();
            reader.onload = function(e) {
                try {
                    const parsed = JSON.parse(e.target.result);
                    if (parsed.playlist && parsed.userUploads) {
                        appData = { ...appData, ...parsed };
                        renderPlaylist();
                        renderUserUploads();
                        renderChat();
                        updateUserDisplay();
                        persistState();
                        showToast("State restored from save file.", "success");
                    } else {
                        showToast("Invalid save file structure.", "error");
                    }
                } catch (err) {
                    showToast("Failed to parse save JSON.", "error");
                }
            };
            reader.readAsText(file);
            event.target.value = '';
        }

        function loadSampleData() {
            const sampleVideos = [
                {
                    id: 'sample_1',
                    title: 'Big Buck Bunny (Sample)',
                    url: 'https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/BigBuckBunny.mp4',
                    author: 'BlenderFoundation',
                    size: '158 MB',
                    type: 'url',
                    isLocalFile: false,
                    timestamp: '10:00 AM'
                },
                {
                    id: 'sample_2',
                    title: 'Elephants Dream (Sample)',
                    url: 'https://commondatastorage.googleapis.com/gtv-videos-bucket/sample/ElephantsDream.mp4',
                    author: 'OpenMediaNode',
                    size: '120 MB',
                    type: 'url',
                    isLocalFile: false,
                    timestamp: '10:15 AM'
                }
            ];

            appData.userUploads = [...sampleVideos, ...appData.userUploads];
            appData.playlist = [...sampleVideos, ...appData.playlist];
            renderUserUploads();
            renderPlaylist();
            showToast("Sample data loaded.", "info");
        }


        function renderChat() {
            const box = document.getElementById('chat-messages-box');
            box.innerHTML = appData.chatMessages.map(m => `
                <div class="bg-cardbg border border-cardborder p-3 rounded-2xl space-y-1">
                    <div class="flex items-center justify-between text-[10px]">
                        <span class="font-bold text-indigo-400">@${m.sender}</span>
                        <span class="text-slate-500">${m.timestamp}</span>
                    </div>
                    <p class="text-xs text-slate-200">${m.text}</p>
                </div>
            `).join('');
            box.scrollTop = box.scrollHeight;
        }

        function sendChatMessage(e) {
            e.preventDefault();
            const input = document.getElementById('chat-input');
            const txt = input.value.trim();
            if (!txt) return;

            appData.chatMessages.push({
                id: Date.now(),
                sender: appData.currentUser,
                text: txt,
                timestamp: new Date().toLocaleTimeString([], { hour: '2-digit', minute: '2-digit' })
            });

            renderChat();
            input.value = '';
            persistState();
        }

        function startCallSimulation() {
            document.getElementById('call-modal').classList.remove('hidden');
        }

        function endCallSimulation() {
            document.getElementById('call-modal').classList.add('hidden');
            showToast("Call ended.", "info");
        }

        function toggleMuteCall() {
            const btn = document.getElementById('call-mute-btn');
            btn.classList.toggle('bg-rose-600/20');
            btn.classList.toggle('text-rose-400');
        }


        function openDevAuthModal() {
            document.getElementById('dev-modal').classList.remove('hidden');
            document.getElementById('username-input').value = appData.currentUser;
        }

        function closeDevModal() {
            document.getElementById('dev-modal').classList.add('hidden');
        }

        function updateUsername() {
            const val = document.getElementById('username-input').value.trim();
            if (val) {
                appData.currentUser = val;
                updateUserDisplay();
                renderUserUploads();
                persistState();
                showToast(`Name updated to @${val}`, 'success');
            }
        }

        function updateUserDisplay() {
            document.getElementById('current-user-display').innerText = appData.currentUser;
            document.getElementById('session-author-tag').innerText = appData.currentUser;
        }

        function authenticateSudo() {
            const pass = document.getElementById('sudo-key-input').value.trim();
            if (pass === 'sudo123') {
                appData.isSudo = true;
                document.getElementById('sudo-status-badge').innerText = 'Superuser Sudo Active';
                document.getElementById('sudo-status-badge').className = 'text-[10px] bg-amber-500/20 text-amber-300 border border-amber-500/30 px-2 py-0.5 rounded-full font-bold';
                document.getElementById('sudo-tools').classList.remove('hidden');
                renderUserUploads();
                showToast("Sudo privileges granted.", "success");
            } else {
                showToast("Invalid Sudo Key.", "error");
            }
        }

        function sudoPurgeAllUploads() {
            appData.userUploads = [];
            appData.playlist = [];
            renderUserUploads();
            renderPlaylist();
            persistState();
            showToast("Sudo Purge executed.", "info");
        }

        // Helper: Bytes formatter
        function formatBytes(bytes) {
            if (bytes === 0) return '0 Bytes';
            const k = 1024;
            const sizes = ['Bytes', 'KB', 'MB', 'GB', 'TB'];
            const i = Math.floor(Math.log(bytes) / Math.log(k));
            return parseFloat((bytes / Math.pow(k, i)).toFixed(2)) + ' ' + sizes[i];
        }

        // Helper: Toast notification
        function showToast(msg, type = 'info') {
            const container = document.getElementById('toast-container');
            const toast = document.createElement('div');
            
            const colorClass = type === 'error' ? 'border-rose-500/40 bg-rose-950/90 text-rose-200' : 
                               type === 'success' ? 'border-emerald-500/40 bg-emerald-950/90 text-emerald-200' : 
                               'border-indigo-500/40 bg-slate-900/90 text-slate-200';

            toast.className = `px-4 py-2.5 rounded-2xl border ${colorClass} text-xs font-semibold shadow-2xl backdrop-blur-md transition-all duration-300 pointer-events-auto flex items-center gap-2`;
            toast.innerHTML = `<i class="fa-solid fa-circle-info"></i> ${msg}`;

            container.appendChild(toast);
            setTimeout(() => {
                toast.classList.add('opacity-0', 'translate-y-2');
                setTimeout(() => toast.remove(), 300);
            }, 3000);
        }
    </script>
</body>
</html>
