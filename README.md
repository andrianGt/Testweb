<!DOCTYPE html>
<html lang="id" class="dark">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CraftForge Studio | Minecraft Bedrock Add-ons & Patches</title>
    <!-- Tailwind CSS CDN -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome for Gaming Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts - Inter & Press Start 2P for gaming accents -->
    <link rel="preconnect" href="https://fonts.googleapis.com">
    <link rel="preconnect" href="https://fonts.gstatic.com" crossorigin>
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&family=Press+Start+2P&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                        gaming: ['"Press Start 2P"', 'monospace'],
                    },
                    colors: {
                        mcGreen: {
                            50: '#f2fbf4',
                            100: '#e2f7e7',
                            400: '#4ade80',
                            500: '#22c55e',
                            600: '#16a34a',
                            700: '#15803d',
                            800: '#166534',
                            900: '#14532d',
                        },
                        mcDark: {
                            900: '#0f172a', /* Slate 900 */
                            800: '#1e293b', /* Slate 800 */
                            700: '#334155', /* Slate 700 */
                        }
                    }
                }
            }
        }
    </script>
    <style>
        /* Custom Minecraft-like glow and styles */
        .glow-green {
            box-shadow: 0 0 15px rgba(34, 197, 94, 0.4);
        }
        .glow-orange {
            box-shadow: 0 0 15px rgba(249, 115, 22, 0.4);
        }
        .scrollbar-hide::-webkit-scrollbar {
            display: none;
        }
        .scrollbar-hide {
            -ms-overflow-style: none;
            scrollbar-width: none;
        }
        /* Custom pixel card accents */
        .pixel-border {
            border-image: linear-gradient(to right, #22c55e, #16a34a) 1;
        }
        .toast-animate {
            animation: slideInUp 0.3s ease-out forwards;
        }
        @keyframes slideInUp {
            from { transform: translateY(100px); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
    </style>
</head>
<body class="bg-slate-50 text-slate-900 dark:bg-slate-950 dark:text-slate-100 min-h-screen font-sans transition-colors duration-300">

    <!-- TOAST NOTIFICATION CONTAINER -->
    <div id="toast-container" class="fixed bottom-6 right-6 z-50 flex flex-col gap-3 max-w-sm w-full pointer-events-none"></div>

    <!-- MAIN NAVBAR -->
    <header class="sticky top-0 z-40 w-full backdrop-blur-md bg-white/80 dark:bg-slate-950/80 border-b border-slate-200 dark:border-slate-800 transition-all">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                <!-- Logo -->
                <div class="flex items-center space-x-3 cursor-pointer" onclick="navigateTo('home')">
                    <div class="w-10 h-10 bg-gradient-to-br from-mcGreen-500 to-mcGreen-700 rounded-lg flex items-center justify-center text-white glow-green">
                        <i class="fa-solid fa-cube text-xl"></i>
                    </div>
                    <div>
                        <span class="font-gaming text-xs sm:text-sm text-mcGreen-500 tracking-wider">CRAFTFORGE</span>
                        <p class="text-[10px] text-slate-500 dark:text-slate-400 font-bold tracking-widest uppercase">Addon Studio</p>
                    </div>
                </div>

                <!-- Desktop Navigation Items -->
                <nav class="hidden md:flex items-center space-x-1 lg:space-x-2">
                    <button onclick="navigateTo('home')" class="nav-link px-3 py-2 rounded-lg text-sm font-semibold transition-all hover:bg-slate-100 dark:hover:bg-slate-800 flex items-center gap-1.5 text-mcGreen-500"><i class="fa-solid fa-house"></i> Home</button>
                    <button onclick="navigateTo('addons')" class="nav-link px-3 py-2 rounded-lg text-sm font-semibold transition-all hover:bg-slate-100 dark:hover:bg-slate-800 flex items-center gap-1.5"><i class="fa-solid fa-box-open"></i> Add-ons</button>
                    <button onclick="navigateTo('patches')" class="nav-link px-3 py-2 rounded-lg text-sm font-semibold transition-all hover:bg-slate-100 dark:hover:bg-slate-800 flex items-center gap-1.5"><i class="fa-solid fa-toolbox"></i> Patches</button>
                    <button onclick="navigateTo('about')" class="nav-link px-3 py-2 rounded-lg text-sm font-semibold transition-all hover:bg-slate-100 dark:hover:bg-slate-800 flex items-center gap-1.5"><i class="fa-solid fa-user-gear"></i> About</button>
                </nav>

                <!-- Right Toolbar Actions -->
                <div class="flex items-center space-x-2">
                    <!-- Bug/Request shortcuts desktop -->
                    <button onclick="openModal('bug')" class="hidden sm:inline-flex items-center gap-1.5 bg-red-500/10 hover:bg-red-500/20 text-red-500 dark:text-red-400 px-3 py-1.5 rounded-lg text-xs font-semibold transition-all border border-red-500/20">
                        <i class="fa-solid fa-bug"></i> Report Bug
                    </button>
                    <button onclick="openModal('request')" class="hidden sm:inline-flex items-center gap-1.5 bg-mcGreen-500/10 hover:bg-mcGreen-500/20 text-mcGreen-600 dark:text-mcGreen-400 px-3 py-1.5 rounded-lg text-xs font-semibold transition-all border border-mcGreen-500/20">
                        <i class="fa-solid fa-lightbulb"></i> Request
                    </button>

                    <!-- Dark Mode Toggle -->
                    <button id="dark-mode-toggle" class="p-2.5 rounded-lg text-slate-500 hover:bg-slate-100 dark:text-slate-400 dark:hover:bg-slate-800 transition-colors focus:outline-none" aria-label="Toggle Theme">
                        <i class="fa-solid fa-moon text-lg dark:hidden"></i>
                        <i class="fa-solid fa-sun text-lg hidden dark:block text-amber-400"></i>
                    </button>

                    <!-- Mobile Menu Trigger -->
                    <button id="mobile-menu-btn" class="md:hidden p-2.5 rounded-lg text-slate-500 hover:bg-slate-100 dark:text-slate-400 dark:hover:bg-slate-800 transition-colors">
                        <i class="fa-solid fa-bars text-xl" id="menu-icon"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Menu Container -->
        <div id="mobile-menu" class="hidden md:hidden bg-white dark:bg-slate-900 border-t border-slate-200 dark:border-slate-800 py-3 px-4 flex flex-col space-y-2">
            <button onclick="navigateTo('home'); toggleMobileMenu();" class="mobile-nav-link text-left px-3 py-2.5 rounded-lg text-sm font-semibold hover:bg-slate-100 dark:hover:bg-slate-800 flex items-center gap-3"><i class="fa-solid fa-house w-5 text-mcGreen-500"></i> Home</button>
            <button onclick="navigateTo('addons'); toggleMobileMenu();" class="mobile-nav-link text-left px-3 py-2.5 rounded-lg text-sm font-semibold hover:bg-slate-100 dark:hover:bg-slate-800 flex items-center gap-3"><i class="fa-solid fa-box-open w-5 text-blue-500"></i> Add-ons</button>
            <button onclick="navigateTo('patches'); toggleMobileMenu();" class="mobile-nav-link text-left px-3 py-2.5 rounded-lg text-sm font-semibold hover:bg-slate-100 dark:hover:bg-slate-800 flex items-center gap-3"><i class="fa-solid fa-toolbox w-5 text-emerald-500"></i> Patches</button>
            <button onclick="navigateTo('about'); toggleMobileMenu();" class="mobile-nav-link text-left px-3 py-2.5 rounded-lg text-sm font-semibold hover:bg-slate-100 dark:hover:bg-slate-800 flex items-center gap-3"><i class="fa-solid fa-user-gear w-5 text-teal-500"></i> About</button>
            <div class="h-px bg-slate-200 dark:bg-slate-800 my-2"></div>
            <div class="flex items-center gap-2 pt-1">
                <button onclick="openModal('bug'); toggleMobileMenu();" class="flex-1 py-2 rounded-lg text-center font-bold text-xs text-red-500 bg-red-500/10 hover:bg-red-500/20">
                    <i class="fa-solid fa-bug"></i> Report Bug
                </button>
                <button onclick="openModal('request'); toggleMobileMenu();" class="flex-1 py-2 rounded-lg text-center font-bold text-xs text-mcGreen-500 bg-mcGreen-500/10 hover:bg-mcGreen-500/20">
                    <i class="fa-solid fa-lightbulb"></i> Request Feature
                </button>
            </div>
        </div>
    </header>

    <!-- HERO PROFILE HEADER BANNER -->
    <section class="relative bg-gradient-to-r from-slate-900 via-slate-900 to-mcGreen-950 text-white overflow-hidden py-12 md:py-20 border-b border-mcGreen-500/30">
        <!-- Floating Block Particles in Background -->
        <div class="absolute inset-0 opacity-15 pointer-events-none overflow-hidden select-none">
            <div class="absolute top-1/4 left-10 w-16 h-16 border-4 border-dashed border-white rounded animate-spin" style="animation-duration: 40s;"></div>
            <div class="absolute top-2/3 right-1/4 w-20 h-20 bg-mcGreen-500 rounded opacity-25"></div>
            <div class="absolute bottom-10 left-1/3 w-12 h-12 border-2 border-white rounded rotate-45"></div>
            <div class="absolute top-10 right-10 w-24 h-24 bg-gradient-to-br from-indigo-500 to-purple-500 rounded opacity-20"></div>
        </div>

        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 relative z-10">
            <div class="flex flex-col md:flex-row items-center md:items-start md:justify-between gap-8">
                <!-- Profile Avatar & Details -->
                <div class="flex flex-col sm:flex-row items-center sm:items-start gap-6 text-center sm:text-left">
                    <div class="relative group">
                        <!-- Hologram Border -->
                        <div class="absolute -inset-1.5 bg-gradient-to-r from-mcGreen-500 to-emerald-400 rounded-2xl blur opacity-75 group-hover:opacity-100 transition duration-1000 group-hover:duration-200 animate-pulse"></div>
                        
                        <!-- 
                          CARA MENGGANTI FOTO PROFIL:
                          Ganti link "https://placehold.co/150/1e293b/4ade80?text=Reza" di bawah 
                          dengan URL foto Anda sendiri (misalnya dari imgur, discord, atau file lokal seperti "img/profil.jpg")
                        -->
                        <div class="relative w-28 h-28 md:w-32 md:h-32 bg-slate-800 rounded-2xl overflow-hidden border-2 border-slate-700 flex items-center justify-center">
                            <img src="https://placehold.co/150/1e293b/4ade80?text=Reza" alt="Profile Picture" class="w-full h-full object-cover" onerror="this.onerror=null; this.src='https://placehold.co/150/1e293b/4ade80?text=Reza';">
                        </div>
                        <span class="absolute -bottom-1 -right-1 bg-mcGreen-500 text-slate-900 text-[10px] font-gaming px-2 py-0.5 rounded-full border border-slate-950 uppercase tracking-widest">Creator</span>
                    </div>

                    <div class="mt-2">
                        <div class="flex flex-wrap items-center justify-center sm:justify-start gap-2.5">
                            <h1 class="text-3xl md:text-4xl font-black tracking-tight text-white">RezaCraft Dev</h1>
                        </div>
                        <p class="text-mcGreen-400 text-sm font-gaming tracking-wide mt-1.5">@rezacraft_addons</p>
                        <p class="text-slate-300 mt-3 max-w-lg leading-relaxed text-sm sm:text-base">
                            "Saya membuat add-on Minecraft Bedrock gratis untuk komunitas."
                        </p>
                        <!-- Quick Socials -->
                        <div class="flex items-center justify-center sm:justify-start gap-4 mt-4 text-slate-400">
                            <a href="#" class="hover:text-white transition-colors"><i class="fa-brands fa-youtube text-lg"></i></a>
                            <a href="#" class="hover:text-white transition-colors"><i class="fa-brands fa-discord text-lg"></i></a>
                            <a href="#" class="hover:text-white transition-colors"><i class="fa-brands fa-github text-lg"></i></a>
                            <a href="#" class="hover:text-white transition-colors"><i class="fa-brands fa-x-twitter text-lg"></i></a>
                        </div>
                    </div>
                </div>

                <!-- Stats Quickboard -->
                <div id="profile-stats-quickboard" class="grid grid-cols-2 gap-3 sm:gap-4 w-full md:w-auto self-stretch md:self-center">
                    <div class="bg-slate-800/40 backdrop-blur border border-slate-700/50 rounded-xl p-4 flex flex-col items-center md:items-start justify-center text-center md:text-left">
                        <span class="text-mcGreen-400 text-2xl font-black font-gaming" id="stat-total-addons">3</span>
                        <span class="text-xs text-slate-400 font-medium uppercase tracking-wider mt-1">Add-ons Rilis</span>
                    </div>
                    <div class="bg-slate-800/40 backdrop-blur border border-slate-700/50 rounded-xl p-4 flex flex-col items-center md:items-start justify-center text-center md:text-left">
                        <span class="text-amber-400 text-2xl font-black font-gaming">4.8 ★</span>
                        <span class="text-xs text-slate-400 font-medium uppercase tracking-wider mt-1">Rating Komunitas</span>
                    </div>
                </div>
            </div>
        </div>
    </section>

    <!-- PAGE VIEW ROUTER WRAPPER -->
    <main class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-8">

        <!-- ================= HOME VIEW ================= -->
        <div id="view-home" class="page-view space-y-12">
            <!-- Latest Announcements -->
            <div class="bg-gradient-to-r from-mcGreen-950/20 to-slate-900/40 border border-mcGreen-500/20 rounded-2xl p-6 relative overflow-hidden">
                <div class="absolute -right-12 -top-12 opacity-5 pointer-events-none select-none">
                    <i class="fa-solid fa-bullhorn text-[180px]"></i>
                </div>
                <div class="flex flex-col sm:flex-row items-start gap-4">
                    <div class="p-3 bg-mcGreen-500/20 rounded-xl text-mcGreen-500">
                        <i class="fa-solid fa-bullhorn text-2xl"></i>
                    </div>
                    <div>
                        <span class="text-xs font-bold text-mcGreen-500 uppercase tracking-widest">Update Pengumuman</span>
                        <h2 class="text-xl font-bold mt-1 text-slate-900 dark:text-white">Dukung Minecraft Versi 1.21.0+!</h2>
                        <p class="text-slate-600 dark:text-slate-300 text-sm mt-2 leading-relaxed">
                            Semua add-on utama kami (Elemental Swords & DecoBlocks) saat ini telah diperbarui secara resmi untuk mendukung fungsionalitas scripting API terbaru di versi Minecraft Bedrock 1.21.0 ke atas. Jika menemukan bug, gunakan formulir pelapor bug di kanan atas!
                        </p>
                    </div>
                </div>
            </div>
        </div>

        <!-- ================= ADD-ONS CATALOG VIEW ================= -->
        <div id="view-addons" class="page-view hidden space-y-8">
            <div class="flex flex-col md:flex-row items-start md:items-center justify-between gap-4 border-b border-slate-200 dark:border-slate-800 pb-6">
                <div>
                    <h2 class="text-3xl font-black tracking-tight flex items-center gap-2.5"><i class="fa-solid fa-box-open text-mcGreen-500"></i> Semua Add-ons</h2>
                    <p class="text-slate-500 dark:text-slate-400 text-sm mt-1">Temukan berbagai modifikasi menarik untuk memperkaya pengalaman survival kamu.</p>
                </div>
                <!-- Filters -->
                <div class="flex flex-wrap items-center gap-3 w-full md:w-auto">
                    <select id="filter-version" class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-slate-700 dark:text-slate-300 py-2 px-3 text-sm rounded-xl focus:outline-none focus:border-mcGreen-500 transition-all cursor-pointer">
                        <option value="all">MC: Semua Versi</option>
                        <option value="1.21">MC 1.21+</option>
                        <option value="1.20">MC 1.20+</option>
                        <option value="1.19">MC 1.19+</option>
                    </select>
                    <select id="filter-status" class="bg-white dark:bg-slate-900 border border-slate-200 dark:border-slate-800 text-slate-700 dark:text-slate-300 py-2 px-3 text-sm rounded-xl focus:outline-none focus:border-mcGreen-500 transition-all cursor-pointer">
                        <option value="all">Status: Semua</option>
                        <option value="Aktif">Status: Aktif</option>
                        <option value="Beta">Status: Beta</option>
                        <option value="Deprecated">Status: Deprecated</option>
                    </select>
                </div>
            </div>

            <div id="addons-grid-container" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Dynamically loaded elements -->
            </div>
            
            <!-- Empty state -->
            <div id="addons-empty-state" class="hidden text-center py-20">
                <i class="fa-solid fa-box-open text-6xl text-slate-300 dark:text-slate-700 mb-4 animate-bounce"></i>
                <h3 class="text-xl font-bold">Add-on Tidak Ditemukan</h3>
                <p class="text-slate-500 dark:text-slate-400 text-sm mt-1">Coba gunakan kombinasi filter lain.</p>
            </div>
        </div>

        <!-- ================= MINECRAFT PATCHES VIEW ================= -->
        <div id="view-patches" class="page-view hidden space-y-8">
            <div class="border-b border-slate-200 dark:border-slate-800 pb-4">
                <h2 class="text-3xl font-black tracking-tight flex items-center gap-2.5 text-slate-900 dark:text-white">
                    <i class="fa-solid fa-toolbox text-mcGreen-500"></i> Unduhan Patch & Tools Minecraft
                </h2>
                <p class="text-slate-500 dark:text-slate-400 text-sm mt-1">
                    Unduh modifikasi optimasi grafis, perbaikan performa runtime RenderDragon, serta penyesuaian kompatibilitas untuk Minecraft Bedrock Anda.
                </p>
            </div>

            <!-- List in Announcement-like Banner Shapes -->
            <div class="space-y-6">
                <!-- Patch 1: FPS Booster -->
                <div class="bg-gradient-to-r from-indigo-950/20 via-slate-900/30 to-slate-900/40 border border-slate-200 dark:border-slate-800 rounded-2xl p-6 relative overflow-hidden flex flex-col md:flex-row justify-between items-start md:items-center gap-6 group hover:border-indigo-500/30 transition-all shadow-sm">
                    <div class="absolute -right-12 -top-12 opacity-5 pointer-events-none select-none">
                        <i class="fa-solid fa-bolt text-[180px]"></i>
                    </div>
                    <div class="flex flex-col sm:flex-row items-start gap-4">
  