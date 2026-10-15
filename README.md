<!DOCTYPE html>
<html lang="en">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>CampusLearn - Interactive Learning Hub</title>
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <!-- FontAwesome Icons -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <!-- Google Fonts Inter -->
    <link href="https://fonts.googleapis.com/css2?family=Inter:wght@300;400;500;600;700;800&display=swap" rel="stylesheet">
    
    <script>
        tailwind.config = {
            theme: {
                extend: {
                    colors: {
                        brand: {
                            dark: '#0d233a',
                            primary: '#1b365d',
                            accent: '#1e88e5',
                            hover: '#1565c0',
                            light: '#f4f7fa',
                            bg: '#eef2f6'
                        }
                    },
                    fontFamily: {
                        sans: ['Inter', 'sans-serif'],
                    }
                }
            }
        }
    </script>
    
    <style>
        body {
            font-family: 'Inter', sans-serif;
            background-color: #eef2f6;
            color: #334155;
        }
        .nav-link-active {
            border-bottom: 2px solid #ffffff;
            color: #ffffff !important;
            font-weight: 600;
        }
        .side-nav-active {
            background-color: #1e88e5;
            color: #ffffff !important;
            font-weight: 600;
        }
        .status-badge {
            display: inline-block;
            padding: 0.25rem 0.65rem;
            border-radius: 0.375rem;
            font-size: 0.725rem;
            font-weight: 600;
            text-align: center;
        }
        .status-completed { background-color: #2e7d32; color: #ffffff; }
        .status-in-progress { background-color: #f59e0b; color: #ffffff; }
        .status-not-started { background-color: #94a3b8; color: #ffffff; }

        .site-intro-overlay {
            position: fixed;
            inset: 0;
            z-index: 100;
            display: grid;
            place-items: center;
            overflow: hidden;
            color: #ffffff;
            background:
                radial-gradient(circle at 50% 45%, rgba(30, 136, 229, 0.24), transparent 34rem),
                linear-gradient(135deg, #0d233a 0%, #102d4c 55%, #0b1b2c 100%);
            opacity: 1;
            visibility: visible;
            transition: opacity 0.55s ease, visibility 0.55s ease;
        }
        .site-intro-open { overflow: hidden; }
        .site-intro-overlay.is-dismissed {
            opacity: 0;
            visibility: hidden;
            pointer-events: none;
        }
        .site-intro-orb {
            position: absolute;
            width: 18rem;
            height: 18rem;
            border-radius: 9999px;
            background: rgba(30, 136, 229, 0.16);
            filter: blur(70px);
            animation: siteIntroDrift 3.8s ease-in-out infinite alternate;
        }
        .site-intro-orb-left {
            left: -7rem;
            bottom: -8rem;
        }
        .site-intro-orb-right {
            right: -8rem;
            top: -7rem;
            background: rgba(96, 165, 250, 0.16);
            animation-delay: 0.5s;
        }
        .site-intro-content {
            position: relative;
            z-index: 1;
            padding: 2rem;
            text-align: center;
        }
        .site-intro-mark {
            display: grid;
            place-items: center;
            width: 5rem;
            height: 5rem;
            margin: 0 auto 1.5rem;
            border: 1px solid rgba(255, 255, 255, 0.24);
            border-radius: 1.35rem;
            color: #bfdbfe;
            background: linear-gradient(145deg, rgba(255, 255, 255, 0.18), rgba(255, 255, 255, 0.06));
            box-shadow: 0 18px 55px rgba(0, 0, 0, 0.22);
            font-size: 2rem;
            font-weight: 800;

            animation: siteIntroMarkIn 0.75s cubic-bezier(0.2, 0.8, 0.2, 1) both;
        }
        .site-intro-eyebrow {
            margin-bottom: 0.5rem;
            color: #bfdbfe;
            font-size: 0.7rem;
            font-weight: 700;
            letter-spacing: 0.28em;
            text-transform: uppercase;
            animation: siteIntroFadeUp 0.7s ease 0.18s both;
        }
        .site-intro-title {
            margin: 0;
            font-size: clamp(3rem, 9vw, 5.8rem);
            font-weight: 800;
            line-height: 1;
            letter-spacing: -0.065em;
            animation: siteIntroFadeUp 0.8s cubic-bezier(0.2, 0.8, 0.2, 1) 0.32s both;
        }
        .site-intro-title span {
            color: #60a5fa;
        }
        .site-intro-tagline {
            margin-top: 1rem;
            color: rgba(219, 234, 254, 0.88);
            font-size: 0.8rem;
            font-weight: 600;
            letter-spacing: 0.24em;
            text-transform: uppercase;
            animation: siteIntroFadeUp 0.7s ease 0.58s both;
        }
        .site-intro-progress {
            width: min(13rem, 58vw);
            height: 2px;
            margin: 2rem auto 0;
            overflow: hidden;
            border-radius: 9999px;
            background: rgba(255, 255, 255, 0.18);
        }
        .site-intro-progress span {
            display: block;
            width: 100%;
            height: 100%;
            border-radius: inherit;
            background: #60a5fa;
            transform: scaleX(0);
            transform-origin: left center;
            animation: siteIntroSweep 3.1s cubic-bezier(0.4, 0, 0.2, 1) 0.15s forwards;
        }
        @keyframes siteIntroFadeUp {
            from { opacity: 0; transform: translateY(18px); }
            to { opacity: 1; transform: translateY(0); }
        }
        @keyframes siteIntroMarkIn {
            from { opacity: 0; transform: scale(0.72) rotate(-8deg); }
            to { opacity: 1; transform: scale(1) rotate(0); }
        }
        @keyframes siteIntroDrift {
            from { transform: translate3d(-10px, 8px, 0) scale(0.94); }
            to { transform: translate3d(16px, -14px, 0) scale(1.08); }
        }
        @keyframes siteIntroSweep {
            to { transform: scaleX(1); }
        }
        @media (prefers-reduced-motion: reduce) {
            .site-intro-orb,
            .site-intro-mark,
            .site-intro-eyebrow,
            .site-intro-title,
            .site-intro-tagline,
            .site-intro-progress span {
                animation: none !important;
                opacity: 1;
                transform: none;
            }
            .site-intro-overlay {
                transition-duration: 0.15s;
            }
        }
        .modal-fade-in {
            animation: fadeIn 0.25s ease-out forwards;
        }
        @keyframes fadeIn {
            from { opacity: 0; transform: scale(0.96); }
            to { opacity: 1; transform: scale(1); }
        }

        .toast-slide-up {
            animation: slideUp 0.3s cubic-bezier(0.16, 1, 0.3, 1) forwards;
        }
        @keyframes slideUp {
            from { transform: translateY(100%); opacity: 0; }
            to { transform: translateY(0); opacity: 1; }
        }
    </style>
</head>
<body class="min-h-screen flex flex-col antialiased site-intro-open">
    <div id="site-intro" class="site-intro-overlay" role="dialog" aria-modal="true" aria-labelledby="site-intro-title">
        <div class="site-intro-orb site-intro-orb-left" aria-hidden="true"></div>
        <div class="site-intro-orb site-intro-orb-right" aria-hidden="true"></div>
        <div class="site-intro-content">
            <div class="site-intro-mark" aria-hidden="true"><i class="fa-solid fa-graduation-cap"></i></div>
            <p class="site-intro-eyebrow">Welcome to</p>
            <h1 id="site-intro-title" class="site-intro-title">Campus<span>Learn</span></h1>
            <p class="site-intro-tagline">Learn · Build · Grow</p>
            <div class="site-intro-progress" aria-hidden="true"><span></span></div>
        </div>
    </div>

    <!-- Header -->
    <header class="bg-[#1b365d] text-white shadow-lg sticky top-0 z-40">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex items-center justify-between h-16">
                
                <!-- Logo -->
                <button onclick="navigateTo('home')" class="flex items-center space-x-3 text-left focus:outline-none group">
                    <div class="p-2 bg-white/10 rounded-xl group-hover:bg-white/20 transition">
                        <i class="fa-solid fa-graduation-cap text-2xl text-blue-300"></i>
                    </div>
                    <div>
                        <span class="text-xl font-extrabold tracking-tight block leading-tight">CampusLearn</span>
                        <span class="text-[10px] text-blue-200 uppercase tracking-widest block font-medium">Learn · Build · Grow</span>
                    </div>
                </button>

                <!-- Navigation Links -->
                <nav class="hidden md:flex items-center space-x-8 text-sm font-medium">
                    <button id="nav-home" onclick="navigateTo('home')" class="nav-link-active py-1 text-blue-100 hover:text-white transition">Home</button>
                    <button id="nav-about" onclick="navigateTo('about')" class="py-1 text-blue-100 hover:text-white transition">About</button>
                    <button id="nav-courses" onclick="navigateTo('courses')" class="py-1 text-blue-100 hover:text-white transition">Courses</button>
                    <button id="nav-resources" onclick="navigateTo('resources')" class="py-1 text-blue-100 hover:text-white transition">Resources</button>
                    <button id="nav-contact" onclick="navigateTo('contact')" class="py-1 text-blue-100 hover:text-white transition">Contact</button>
                    
                    <button onclick="toggleQuickSearch()" class="p-2 text-blue-200 hover:text-white hover:bg-white/10 rounded-full transition">
                        <i class="fa-solid fa-magnifying-glass text-lg"></i>
                    </button>
                </nav>

                <!-- Mobile Menu Trigger -->
                <div class="md:hidden flex items-center space-x-2">
                    <button onclick="toggleQuickSearch()" class="p-2 text-blue-200 hover:text-white">
                        <i class="fa-solid fa-magnifying-glass text-lg"></i>
                    </button>
                    <button onclick="document.getElementById('mobile-menu').classList.toggle('hidden')" class="text-white p-2 focus:outline-none">
                        <i class="fa-solid fa-bars text-2xl"></i>
                    </button>
                </div>
            </div>
        </div>

        <!-- Mobile Menu -->
        <div id="mobile-menu" class="hidden md:hidden bg-[#0d233a] px-4 pt-3 pb-5 space-y-2 text-sm border-t border-blue-900/50 shadow-inner">
            <button onclick="navigateTo('home')" class="block w-full text-left text-white font-medium py-2 px-3 rounded hover:bg-white/10">Home</button>
            <button onclick="navigateTo('about')" class="block w-full text-left text-blue-100 py-2 px-3 rounded hover:bg-white/10">About</button>
            <button onclick="navigateTo('courses')" class="block w-full text-left text-blue-100 py-2 px-3 rounded hover:bg-white/10">Courses</button>
            <button onclick="navigateTo('resources')" class="block w-full text-left text-blue-100 py-2 px-3 rounded hover:bg-white/10">Resources</button>
            <button onclick="navigateTo('contact')" class="block w-full text-left text-blue-100 py-2 px-3 rounded hover:bg-white/10">Contact</button>
        </div>

        <!-- Search Bar Dropdown -->
        <div id="quick-search-bar" class="hidden bg-[#0d233a] border-t border-blue-800 p-4 shadow-xl">
            <div class="max-w-3xl mx-auto flex items-center bg-white rounded-lg px-3 py-2 shadow-inner">
                <i class="fa-solid fa-magnifying-glass text-slate-400 mr-2"></i>
                <input id="global-search-input" type="text" placeholder="Search courses, resources, syllabus, topics..." class="w-full text-sm text-slate-800 focus:outline-none" onkeyup="handleGlobalSearch(event)">
                <button onclick="toggleQuickSearch()" class="text-slate-400 hover:text-slate-600 ml-2">
                    <i class="fa-solid fa-xmark text-lg"></i>
                </button>
            </div>
        </div>
    </header>

    <div id="main-content" class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8 py-6 flex-grow w-full">

        <!-- ================= PAGE 1: HOME PAGE ================= -->
        <div id="page-home" class="page-section">
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-6">

                <!-- Left Sidebar -->
                <aside class="lg:col-span-3 space-y-6">

                    <div class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
                        <nav class="flex flex-col text-sm text-slate-700 font-medium">
                            <button onclick="navigateTo('home')" class="flex items-center px-4 py-3 side-nav-active transition border-b border-slate-100 text-left">
                                <i class="fa-solid fa-house w-6 text-center mr-2"></i> Home
                            </button>
                            <button onclick="scrollToSection('hero-banner')" class="flex items-center px-4 py-3 hover:bg-slate-50 transition border-b border-slate-100 text-left">
                                <i class="fa-solid fa-book-open w-6 text-center mr-2 text-slate-500"></i> Course Overview
                            </button>
                            <button onclick="scrollToSection('modules-section')" class="flex items-center px-4 py-3 hover:bg-slate-50 transition border-b border-slate-100 text-left">
                                <i class="fa-solid fa-list-check w-6 text-center mr-2 text-slate-500"></i> Syllabus
                            </button>
                            <button onclick="navigateTo('resources')" class="flex items-center px-4 py-3 hover:bg-slate-50 transition border-b border-slate-100 text-left">
                                <i class="fa-solid fa-folder-open w-6 text-center mr-2 text-slate-500"></i> Study Materials
                            </button>
                            <button onclick="scrollToSection('assignments-section')" class="flex items-center px-4 py-3 hover:bg-slate-50 transition border-b border-slate-100 text-left">
                                <i class="fa-solid fa-pen-to-square w-6 text-center mr-2 text-slate-500"></i> Assignments
                            </button>
                            <button onclick="scrollToSection('links-section')" class="flex items-center px-4 py-3 hover:bg-slate-50 transition border-b border-slate-100 text-left">
                                <i class="fa-solid fa-link w-6 text-center mr-2 text-slate-500"></i> Useful Links
                            </button>
                            <button onclick="navigateTo('contact')" class="flex items-center px-4 py-3 hover:bg-slate-50 transition text-left">
                                <i class="fa-solid fa-circle-question w-6 text-center mr-2 text-slate-500"></i> FAQ
                            </button>
                        </nav>
                    </div>

                    <!-- Motto Card -->
                    <div class="bg-gradient-to-br from-blue-50 to-indigo-50 border border-blue-100 rounded-xl p-6 text-center shadow-sm relative overflow-hidden">
                        <div class="flex justify-center mb-3">
                            <div class="w-12 h-12 bg-blue-100 rounded-full flex items-center justify-center text-[#1b365d]">
                                <i class="fa-solid fa-graduation-cap text-2xl"></i>
                            </div>
                        </div>
                        <p class="text-blue-950 font-semibold italic text-sm leading-relaxed">
                            "Small steps today, big dreams tomorrow!"
                        </p>
                        <p class="text-[11px] text-blue-600 mt-2 font-medium">— CampusLearn Student Motto</p>
                    </div>
                </aside>

                <!-- Center Content -->
                <main class="lg:col-span-6 space-y-6">
                    
                    <!-- Featured course carousel -->
                    <div id="hero-banner" class="relative h-72 rounded-xl overflow-hidden shadow-md group" role="region" aria-roledescription="carousel" aria-label="Featured courses">
                        <img id="featured-course-image" src="https://images.unsplash.com/photo-1517694712202-14dd9538aa97?auto=format&fit=crop&w=1200&q=80" alt="Computer Science Fundamentals" class="w-full h-full object-cover" onerror="this.onerror=null;this.src='https://placehold.co/1200x600/1b365d/ffffff?text=Featured+Course'">
                        <div class="absolute inset-0 bg-gradient-to-r from-slate-950/90 via-slate-900/75 to-slate-900/20 flex flex-col justify-between p-5 sm:p-6 text-white">
                            <div class="max-w-xl pr-3">
                                <span class="inline-block bg-blue-600 text-white text-[10px] font-bold uppercase tracking-wider px-2.5 py-1 rounded-full mb-2">Featured Course</span>
                                <h1 id="featured-course-title" class="text-2xl sm:text-3xl font-bold tracking-tight mb-2">Computer Science Fundamentals</h1>
                                <p id="featured-course-description" class="text-blue-100 text-sm leading-relaxed mb-2 max-w-md">Learn the basics. Build the future. Master coding, algorithms, logic, and web foundations.</p>
                                <p id="featured-course-meta" class="text-[11px] font-semibold text-blue-200">Beginner · 6 Weeks · 4.9 ★</p>
                            </div>
                            <div class="flex flex-wrap items-center justify-between gap-3">
                                <button id="featured-course-cta" type="button" onclick="openCourseModal(1)" class="inline-flex items-center bg-[#1e88e5] hover:bg-blue-600 text-white font-medium text-sm px-4 py-2.5 rounded-lg transition shadow-md">
                                    Explore course <i class="fa-solid fa-arrow-right ml-2 text-xs"></i>
                                </button>
                                <div class="flex items-center gap-2" aria-label="Featured course navigation">
                                    <button type="button" onclick="changeFeaturedCourse(-1)" aria-label="Previous featured course" class="grid h-8 w-8 place-items-center rounded-full bg-slate-950/40 text-white transition hover:bg-slate-950/70 focus:outline-none focus:ring-2 focus:ring-white">
                                        <i class="fa-solid fa-chevron-left text-xs" aria-hidden="true"></i>
                                    </button>
                                    <div id="featured-course-dots" class="flex items-center gap-1.5" role="group" aria-label="Choose a featured course"></div>
                                    <button type="button" onclick="changeFeaturedCourse(1)" aria-label="Next featured course" class="grid h-8 w-8 place-items-center rounded-full bg-slate-950/40 text-white transition hover:bg-slate-950/70 focus:outline-none focus:ring-2 focus:ring-white">
                                        <i class="fa-solid fa-chevron-right text-xs" aria-hidden="true"></i>
                                    </button>
                                </div>
                                <span id="featured-course-count" class="sr-only" aria-live="polite">Slide 1 of 3</span>
                            </div>
                        </div>
                    </div>
                    <!-- Welcome Card -->
                    <section class="bg-white rounded-xl p-6 shadow-sm border border-slate-200">
                        <h2 class="text-xl font-bold text-[#1b365d] mb-3">Welcome to Computer Science Fundamentals</h2>
                        <p class="text-slate-600 text-sm leading-relaxed">
                            This course introduces the basic concepts of computer science, including problem solving, programming, data structures, and web technologies. It is designed for first-year students and aspiring developers who are new to the subject.
                        </p>
                    </section>

                    <!-- Interactive Learning Dashboard -->
                    <section class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <article class="relative overflow-hidden rounded-xl bg-gradient-to-br from-[#0d233a] via-[#1b365d] to-[#1e88e5] p-5 text-white shadow-md">
                            <div class="absolute -right-8 -top-10 h-36 w-36 rounded-full bg-white/10 blur-2xl" aria-hidden="true"></div>
                            <div class="relative">
                                <div class="mb-4 flex items-center justify-between">
                                    <span class="text-[10px] font-bold uppercase tracking-[0.2em] text-blue-200">Your learning</span>
                                    <span class="rounded-full border border-white/20 bg-white/10 px-2.5 py-1 text-[10px] font-semibold">Keep it up</span>
                                </div>
                                <h2 class="text-lg font-bold">Pick up where you left off</h2>
                                <p id="learning-next-step" class="mt-1 min-h-8 text-xs leading-relaxed text-blue-100">Loading your next step...</p>
                                <div class="mt-4">
                                    <div class="mb-1.5 flex items-center justify-between text-[10px] font-semibold text-blue-100">
                                        <span>Course progress</span><span id="learning-progress-copy">0 of 0 steps complete</span>
                                    </div>
                                    <div id="learning-progress-track" class="h-2 overflow-hidden rounded-full bg-white/20" role="progressbar" aria-label="Overall course progress" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0">
                                        <div id="learning-progress-bar" class="h-full rounded-full bg-cyan-300 transition-all duration-500" style="width: 0%"></div>
                                    </div>
                                </div>
                                <button type="button" onclick="resumeLearning()" class="mt-4 inline-flex items-center rounded-lg bg-white px-4 py-2 text-xs font-bold text-[#1b365d] shadow transition hover:bg-blue-50 focus:outline-none focus:ring-2 focus:ring-white focus:ring-offset-2 focus:ring-offset-[#1b365d]">
                                    Resume learning <i class="fa-solid fa-arrow-right ml-2 text-[10px]"></i>
                                </button>
                            </div>
                        </article>
                        <article id="daily-quiz-panel" class="rounded-xl border border-blue-100 bg-gradient-to-br from-white to-blue-50 p-5 shadow-sm">
                            <div class="mb-3 flex items-center justify-between">
                                <span class="text-[10px] font-bold uppercase tracking-[0.18em] text-blue-600">Daily coding quiz</span>
                                <span id="daily-quiz-points" class="rounded-full bg-amber-100 px-2.5 py-1 text-[10px] font-bold text-amber-800">0 XP</span>
                            </div>
                            <div class="mb-1 flex items-center justify-between gap-2">
                                <h2 id="daily-quiz-title" class="text-base font-bold text-[#1b365d]">Quick check</h2>
                                <span id="daily-quiz-count" class="text-[10px] font-semibold text-slate-500">Question 1 of 5</span>
                            </div>
                            <p id="daily-quiz-prompt" class="mt-1 text-xs leading-relaxed text-slate-600">Loading today's quiz...</p>
                            <fieldset id="daily-quiz-options" class="mt-3 space-y-2" aria-describedby="daily-quiz-prompt"></fieldset>
                            <div class="mt-3 flex flex-wrap items-center justify-between gap-2">
                                <p id="daily-quiz-feedback" class="text-[11px] font-medium text-slate-500" aria-live="polite">Correct: +10 XP · Incorrect: -5 XP.</p>
                                <button id="daily-quiz-submit" type="button" onclick="checkDailyQuiz()" class="rounded-lg bg-[#1e88e5] px-3.5 py-2 text-xs font-semibold text-white shadow-sm transition hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-blue-300">Check answer</button>
                                <button id="daily-quiz-retry" type="button" onclick="retryDailyQuiz()" class="hidden rounded-lg border border-slate-300 px-3.5 py-2 text-xs font-semibold text-slate-600 transition hover:bg-slate-100 focus:outline-none focus:ring-2 focus:ring-blue-300">Retry today's quiz</button>
                            </div>
                        </article>
                    </section>

                    <!-- Core Pillars -->
                    <section class="grid grid-cols-2 sm:grid-cols-4 gap-4">
                        <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 text-center flex flex-col items-center hover:shadow-md transition">
                            <div class="w-12 h-12 rounded-full bg-blue-50 text-[#1e88e5] flex items-center justify-center text-xl mb-2">
                                <i class="fa-solid fa-code"></i>
                            </div>
                            <h3 class="font-bold text-slate-800 text-sm mb-1">Learn</h3>
                            <p class="text-xs text-slate-500 leading-tight">Understand core concepts & terminology.</p>
                        </div>

                        <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 text-center flex flex-col items-center hover:shadow-md transition">
                            <div class="w-12 h-12 rounded-full bg-blue-50 text-[#1e88e5] flex items-center justify-center text-xl mb-2">
                                <i class="fa-solid fa-laptop-code"></i>
                            </div>
                            <h3 class="font-bold text-slate-800 text-sm mb-1">Practice</h3>
                            <p class="text-xs text-slate-500 leading-tight">Solve real problems with simple code.</p>
                        </div>

                        <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 text-center flex flex-col items-center hover:shadow-md transition">
                            <div class="w-12 h-12 rounded-full bg-blue-50 text-[#1e88e5] flex items-center justify-center text-xl mb-2">
                                <i class="fa-regular fa-lightbulb"></i>
                            </div>
                            <h3 class="font-bold text-slate-800 text-sm mb-1">Build</h3>
                            <p class="text-xs text-slate-500 leading-tight">Create mini projects for your portfolio.</p>
                        </div>

                        <div class="bg-white p-4 rounded-xl shadow-sm border border-slate-200 text-center flex flex-col items-center hover:shadow-md transition">
                            <div class="w-12 h-12 rounded-full bg-blue-50 text-[#1e88e5] flex items-center justify-center text-xl mb-2">
                                <i class="fa-solid fa-user-group"></i>
                            </div>
                            <h3 class="font-bold text-slate-800 text-sm mb-1">Grow</h3>
                            <p class="text-xs text-slate-500 leading-tight">Prepare for advanced career pathways.</p>
                        </div>
                    </section>

                    <!-- Course Modules Table with Integrated Progress Bars -->
                    <section id="modules-section" class="bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
                        <div class="p-4 border-b border-slate-100 flex items-center justify-between">
                            <div class="flex items-center space-x-2">
                                <i class="fa-solid fa-table-cells text-[#1b365d]"></i>
                                <h2 class="font-bold text-slate-800 text-base">Course Modules</h2>
                            </div>
                            <span class="text-xs text-slate-500">4 Modules Active</span>
                        </div>
                        <div class="overflow-x-auto">
                            <table class="w-full text-left text-xs sm:text-sm">
                                <thead class="bg-[#1b365d] text-white font-semibold">
                                    <tr>
                                        <th class="py-3 px-4 text-center">Module</th>
                                        <th class="py-3 px-4">Topic</th>
                                        <th class="py-3 px-4">Module Completion Progress</th>
                                        <th class="py-3 px-4 text-center">Status</th>
                                    </tr>
                                </thead>
                                <tbody class="divide-y divide-slate-100 text-slate-700">
                                    
                                    <!-- Module 1 -->
                                    <tr class="hover:bg-slate-50 transition">
                                        <td class="py-3.5 px-4 font-bold text-center">1</td>
                                        <td class="py-3.5 px-4">
                                            <div class="text-blue-600 font-semibold">Introduction to Computers & OS</div>
                                            <div class="text-[11px] text-slate-400">Week 1 · Hardware & OS Concepts</div>
                                        </td>
                                        <td class="py-3.5 px-4 w-48">
                                            <div class="flex items-center justify-between text-[11px] font-semibold text-slate-600 mb-1">
                                                <span>Progress</span>
                                                <span id="module1-percent" aria-live="polite" class="text-emerald-600 font-extrabold">100%</span>
                                            </div>
                                            <div id="module1-progress-track" class="w-full bg-slate-100 rounded-full h-2 overflow-hidden border border-slate-200" role="progressbar" aria-label="Module 1 progress" aria-valuemin="0" aria-valuemax="100" aria-valuenow="100">
                                                <div id="module1-bar" class="bg-emerald-500 h-full rounded-full transition-all duration-500" style="width: 100%"></div>
                                            </div>
                                            <button type="button" id="module1-activity-toggle" onclick="toggleModuleActivities(1)" aria-controls="module1-activities-row" aria-expanded="false" class="mt-2 flex items-center gap-1 text-[10px] font-semibold text-blue-600 hover:underline">
                                                <span id="module1-toggle-label">View activities</span><span aria-hidden="true">·</span><span id="module1-step-summary">4/4 steps</span>
                                            </button>
                                        </td>
                                        <td class="py-3.5 px-4 text-center">
                                            <span id="module1-badge" class="status-badge status-completed">Completed</span>
                                        </td>
                                    </tr>
                                    <tr id="module1-activities-row" class="hidden">
                                        <td colspan="4" class="px-4 pb-4 bg-slate-50">
                                            <div class="rounded-lg border border-slate-200 bg-white p-4">
                                                <div class="flex flex-wrap items-center justify-between gap-2 mb-1">
                                                    <h3 class="text-xs font-bold text-slate-800">Module 1 activities</h3>
                                                    <span id="module1-activity-summary" class="text-[10px] font-semibold text-slate-500" aria-live="polite">4 of 4 complete</span>
                                                </div>
                                                <p class="text-[11px] text-slate-500 mb-3">Check off each step as you finish it. Progress updates automatically.</p>
                                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="1" checked onchange="updateModuleProgressFromSteps(1)">Identify the main computer hardware components</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="1" checked onchange="updateModuleProgressFromSteps(1)">Explain what an operating system does</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="1" checked onchange="updateModuleProgressFromSteps(1)">Compare files, applications, and processes</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="1" checked onchange="updateModuleProgressFromSteps(1)">Finish the computer fundamentals review</label>
                                                </div>
                                            </div>
                                        </td>
                                    </tr>

                                    <!-- Module 2 -->
                                    <tr class="hover:bg-slate-50 transition">
                                        <td class="py-3.5 px-4 font-bold text-center">2</td>
                                        <td class="py-3.5 px-4">
                                            <div class="text-blue-600 font-semibold">Programming Basics & Logic</div>
                                            <div class="text-[11px] text-slate-400">Week 2 – 3 · Syntax & Flow Control</div>
                                        </td>
                                        <td class="py-3.5 px-4 w-48">
                                            <div class="flex items-center justify-between text-[11px] font-semibold text-slate-600 mb-1">
                                                <span>Progress</span>
                                                <span id="module2-percent" aria-live="polite" class="text-amber-600 font-extrabold">50%</span>
                                            </div>
                                            <div id="module2-progress-track" class="w-full bg-slate-100 rounded-full h-2 overflow-hidden border border-slate-200" role="progressbar" aria-label="Module 2 progress" aria-valuemin="0" aria-valuemax="100" aria-valuenow="50">
                                                <div id="module2-bar" class="bg-amber-500 h-full rounded-full transition-all duration-500" style="width: 50%"></div>
                                            </div>
                                            <button type="button" id="module2-activity-toggle" onclick="toggleModuleActivities(2)" aria-controls="module2-activities-row" aria-expanded="false" class="mt-2 flex items-center gap-1 text-[10px] font-semibold text-blue-600 hover:underline">
                                                <span id="module2-toggle-label">View activities</span><span aria-hidden="true">·</span><span id="module2-step-summary">2/4 steps</span>
                                            </button>
                                        </td>
                                        <td class="py-3.5 px-4 text-center">
                                            <span id="module2-badge" class="status-badge status-in-progress">In Progress</span>
                                        </td>
                                    </tr>
                                    <tr id="module2-activities-row" class="hidden">
                                        <td colspan="4" class="px-4 pb-4 bg-slate-50">
                                            <div class="rounded-lg border border-slate-200 bg-white p-4">
                                                <div class="flex flex-wrap items-center justify-between gap-2 mb-1">
                                                    <h3 class="text-xs font-bold text-slate-800">Module 2 activities</h3>
                                                    <span id="module2-activity-summary" class="text-[10px] font-semibold text-slate-500" aria-live="polite">2 of 4 complete</span>
                                                </div>
                                                <p class="text-[11px] text-slate-500 mb-3">Check off each step as you finish it. Progress updates automatically.</p>
                                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="2" checked onchange="updateModuleProgressFromSteps(2)">Explore variables and data types</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="2" checked onchange="updateModuleProgressFromSteps(2)">Write a basic input and output program</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="2" onchange="updateModuleProgressFromSteps(2)">Use conditions to control a program</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="2" onchange="updateModuleProgressFromSteps(2)">Practice loops and flow control</label>
                                                </div>
                                            </div>
                                        </td>
                                    </tr>

                                    <!-- Module 3 -->
                                    <tr class="hover:bg-slate-50 transition">
                                        <td class="py-3.5 px-4 font-bold text-center">3</td>
                                        <td class="py-3.5 px-4">
                                            <div class="text-blue-600 font-semibold">Web Technologies (HTML & CSS)</div>
                                            <div class="text-[11px] text-slate-400">Week 4 – 5 · Layouts & DOM Structuring</div>
                                        </td>
                                        <td class="py-3.5 px-4 w-48">
                                            <div class="flex items-center justify-between text-[11px] font-semibold text-slate-600 mb-1">
                                                <span>Progress</span>
                                                <span id="module3-percent" aria-live="polite" class="text-slate-400 font-extrabold">0%</span>
                                            </div>
                                            <div id="module3-progress-track" class="w-full bg-slate-100 rounded-full h-2 overflow-hidden border border-slate-200" role="progressbar" aria-label="Module 3 progress" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0">
                                                <div id="module3-bar" class="bg-slate-300 h-full rounded-full transition-all duration-500" style="width: 0%"></div>
                                            </div>
                                            <button type="button" id="module3-activity-toggle" onclick="toggleModuleActivities(3)" aria-controls="module3-activities-row" aria-expanded="false" class="mt-2 flex items-center gap-1 text-[10px] font-semibold text-blue-600 hover:underline">
                                                <span id="module3-toggle-label">View activities</span><span aria-hidden="true">·</span><span id="module3-step-summary">0/4 steps</span>
                                            </button>
                                        </td>
                                        <td class="py-3.5 px-4 text-center">
                                            <span id="module3-badge" class="status-badge status-not-started">Not Started</span>
                                        </td>
                                    </tr>
                                    <tr id="module3-activities-row" class="hidden">
                                        <td colspan="4" class="px-4 pb-4 bg-slate-50">
                                            <div class="rounded-lg border border-slate-200 bg-white p-4">
                                                <div class="flex flex-wrap items-center justify-between gap-2 mb-1">
                                                    <h3 class="text-xs font-bold text-slate-800">Module 3 activities</h3>
                                                    <span id="module3-activity-summary" class="text-[10px] font-semibold text-slate-500" aria-live="polite">0 of 4 complete</span>
                                                </div>
                                                <p class="text-[11px] text-slate-500 mb-3">Check off each step as you finish it. Progress updates automatically.</p>
                                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="3" onchange="updateModuleProgressFromSteps(3)">Build an HTML document structure</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="3" onchange="updateModuleProgressFromSteps(3)">Style content with CSS</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="3" onchange="updateModuleProgressFromSteps(3)">Create a responsive page section</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="3" onchange="updateModuleProgressFromSteps(3)">Complete a DOM basics exercise</label>
                                                </div>
                                            </div>
                                        </td>
                                    </tr>

                                    <!-- Module 4 -->
                                    <tr class="hover:bg-slate-50 transition">
                                        <td class="py-3.5 px-4 font-bold text-center">4</td>
                                        <td class="py-3.5 px-4">
                                            <div class="text-blue-600 font-semibold">Problem Solving & Algorithms</div>
                                            <div class="text-[11px] text-slate-400">Week 6 · Logic Patterns & Search</div>
                                        </td>
                                        <td class="py-3.5 px-4 w-48">
                                            <div class="flex items-center justify-between text-[11px] font-semibold text-slate-600 mb-1">
                                                <span>Progress</span>
                                                <span id="module4-percent" aria-live="polite" class="text-slate-400 font-extrabold">0%</span>
                                            </div>
                                            <div id="module4-progress-track" class="w-full bg-slate-100 rounded-full h-2 overflow-hidden border border-slate-200" role="progressbar" aria-label="Module 4 progress" aria-valuemin="0" aria-valuemax="100" aria-valuenow="0">
                                                <div id="module4-bar" class="bg-slate-300 h-full rounded-full transition-all duration-500" style="width: 0%"></div>
                                            </div>
                                            <button type="button" id="module4-activity-toggle" onclick="toggleModuleActivities(4)" aria-controls="module4-activities-row" aria-expanded="false" class="mt-2 flex items-center gap-1 text-[10px] font-semibold text-blue-600 hover:underline">
                                                <span id="module4-toggle-label">View activities</span><span aria-hidden="true">·</span><span id="module4-step-summary">0/4 steps</span>
                                            </button>
                                        </td>
                                        <td class="py-3.5 px-4 text-center">
                                            <span id="module4-badge" class="status-badge status-not-started">Not Started</span>
                                        </td>
                                    </tr>
                                    <tr id="module4-activities-row" class="hidden">
                                        <td colspan="4" class="px-4 pb-4 bg-slate-50">
                                            <div class="rounded-lg border border-slate-200 bg-white p-4">
                                                <div class="flex flex-wrap items-center justify-between gap-2 mb-1">
                                                    <h3 class="text-xs font-bold text-slate-800">Module 4 activities</h3>
                                                    <span id="module4-activity-summary" class="text-[10px] font-semibold text-slate-500" aria-live="polite">0 of 4 complete</span>
                                                </div>
                                                <p class="text-[11px] text-slate-500 mb-3">Check off each step as you finish it. Progress updates automatically.</p>
                                                <div class="grid grid-cols-1 sm:grid-cols-2 gap-2">
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="4" onchange="updateModuleProgressFromSteps(4)">Break a problem into smaller steps</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="4" onchange="updateModuleProgressFromSteps(4)">Write a solution in pseudocode</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="4" onchange="updateModuleProgressFromSteps(4)">Apply a search or sort pattern</label>
                                                    <label class="flex items-start gap-2 rounded-md border border-slate-200 px-3 py-2 text-xs text-slate-700 cursor-pointer hover:border-blue-300"><input type="checkbox" class="module-step-checkbox mt-0.5 accent-blue-600" data-module="4" onchange="updateModuleProgressFromSteps(4)">Solve an algorithms practice problem</label>
                                                </div>
                                            </div>
                                        </td>
                                    </tr>
                                </tbody>
                            </table>
                        </div>
                    </section>

                    <!-- Assignments -->
                    <section id="assignments-section" class="scroll-mt-24 bg-white rounded-xl shadow-sm border border-slate-200 overflow-hidden">
                        <div class="p-4 sm:p-5 border-b border-slate-100 flex flex-wrap items-center justify-between gap-3">
                            <div>
                                <div class="flex items-center gap-2 mb-1">
                                    <i class="fa-solid fa-pen-to-square text-[#1b365d]"></i>
                                    <h2 class="font-bold text-slate-800 text-base">Assignments</h2>
                                </div>
                                <p class="text-xs text-slate-500">Choose an assignment to see its full task and what to submit.</p>
                            </div>
                            <span class="text-[11px] font-semibold text-blue-700 bg-blue-50 rounded-full px-3 py-1">Course work</span>
                        </div>
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-3 p-4 sm:p-5">
                            <article class="rounded-lg border border-slate-200 p-4 hover:border-blue-300 transition">
                                <div class="flex items-start justify-between gap-3 mb-2">
                                    <div>
                                        <p class="text-[10px] font-bold uppercase tracking-wide text-blue-600">Module 1</p>
                                        <h3 class="text-sm font-bold text-slate-800 mt-1">Computer Systems Overview</h3>
                                    </div>
                                    <i class="fa-solid fa-desktop text-slate-400 mt-1" aria-hidden="true"></i>
                                </div>
                                <p class="text-xs leading-5 text-slate-600 mb-4">Explain how computer hardware and the operating system work together.</p>
                                <button type="button" id="assignment-button-1" onclick="toggleAssignmentDetails(1)" aria-controls="assignment-details-1" aria-expanded="false" class="inline-flex items-center gap-2 rounded-md bg-[#1b365d] px-3 py-2 text-xs font-semibold text-white hover:bg-blue-900 transition">
                                    <span data-assignment-label>Open assignment</span><i class="fa-solid fa-arrow-right" aria-hidden="true"></i>
                                </button>
                                <div id="assignment-details-1" class="hidden mt-4 rounded-lg border border-blue-100 bg-blue-50/60 p-4">
                                    <h4 class="text-xs font-bold text-slate-800 mb-2">Your task</h4>
                                    <p class="text-xs leading-5 text-slate-600 mb-3">Write a short report explaining what happens inside a computer when someone opens an application, edits a document, and saves it.</p>
                                    <p class="text-xs font-semibold text-slate-700 mb-1">Include these points:</p>
                                    <ul class="list-disc pl-5 text-xs leading-5 text-slate-600 space-y-1">
                                        <li>Describe the role of the CPU, memory, storage, and input/output devices.</li>
                                        <li>Explain how the operating system coordinates the application and hardware.</li>
                                        <li>Trace how the document moves from being edited in memory to being saved in storage.</li>
                                    </ul>
                                    <div class="mt-3 border-t border-blue-100 pt-3">
                                        <p class="text-xs font-semibold text-slate-700">Submit</p>
                                        <p class="text-xs leading-5 text-slate-600">A 500–700 word report with a simple labeled diagram. Use your own words and label each computer component clearly.</p>
                                    </div>
                                    <form class="mt-4 border-t border-blue-100 pt-4" onsubmit="saveAssignmentDraft(event, 1)">
                                        <label for="assignment-response-1" class="block text-xs font-bold text-slate-800 mb-1">Submission area</label>
                                        <textarea id="assignment-response-1" rows="4" class="w-full rounded-md border border-slate-300 bg-white p-2.5 text-xs text-slate-700 focus:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-100" placeholder="Write or paste your report here..."></textarea>
                                        <div class="mt-3 flex flex-wrap items-center justify-between gap-2">
                                            <label class="inline-flex cursor-pointer items-center gap-2 rounded-md border border-slate-300 bg-white px-3 py-2 text-xs font-semibold text-slate-700 hover:border-blue-400">
                                                <i class="fa-solid fa-paperclip" aria-hidden="true"></i> Attach a file
                                                <input id="assignment-file-1" type="file" accept=".pdf,.doc,.docx,.txt,.png,.jpg,.jpeg" class="sr-only" onchange="showAssignmentFileName(1)">
                                            </label>
                                            <button type="submit" class="rounded-md bg-[#1e88e5] px-3 py-2 text-xs font-semibold text-white hover:bg-blue-700 transition">Save submission</button>
                                        </div>
                                        <p id="assignment-file-name-1" class="mt-2 text-[11px] text-slate-500" aria-live="polite">No file selected</p>
                                        <p class="mt-1 text-[10px] leading-4 text-slate-500">Drafts are saved in this browser. File attachments are not uploaded by this demo.</p>
                                        <p id="assignment-submit-status-1" class="mt-2 text-xs font-medium text-emerald-700" role="status" aria-live="polite"></p>
                                    </form>
                                </div>
                            </article>
                            <article class="rounded-lg border border-slate-200 p-4 hover:border-blue-300 transition">
                                <div class="flex items-start justify-between gap-3 mb-2">
                                    <div>
                                        <p class="text-[10px] font-bold uppercase tracking-wide text-blue-600">Module 2</p>
                                        <h3 class="text-sm font-bold text-slate-800 mt-1">Programming Logic Challenge</h3>
                                    </div>
                                    <i class="fa-solid fa-code text-slate-400 mt-1" aria-hidden="true"></i>
                                </div>
                                <p class="text-xs leading-5 text-slate-600 mb-4">Plan a small program with variables, a decision, and a loop.</p>
                                <button type="button" id="assignment-button-2" onclick="toggleAssignmentDetails(2)" aria-controls="assignment-details-2" aria-expanded="false" class="inline-flex items-center gap-2 rounded-md bg-[#1b365d] px-3 py-2 text-xs font-semibold text-white hover:bg-blue-900 transition">
                                    <span data-assignment-label>Open assignment</span><i class="fa-solid fa-arrow-right" aria-hidden="true"></i>
                                </button>
                                <div id="assignment-details-2" class="hidden mt-4 rounded-lg border border-blue-100 bg-blue-50/60 p-4">
                                    <h4 class="text-xs font-bold text-slate-800 mb-2">Your task</h4>
                                    <p class="text-xs leading-5 text-slate-600 mb-3">Design a study-time tracker that asks for the number of minutes studied on each of five days, then reports the weekly total, daily average, and whether the student reached a six-hour goal.</p>
                                    <p class="text-xs font-semibold text-slate-700 mb-1">Your solution should:</p>
                                    <ul class="list-disc pl-5 text-xs leading-5 text-slate-600 space-y-1">
                                        <li>Use variables to store daily minutes and the running total.</li>
                                        <li>Use a loop to collect all five days and a decision to check the goal.</li>
                                        <li>Show a trace using 60, 90, 45, 120, and 75 minutes as sample inputs.</li>
                                    </ul>
                                    <div class="mt-3 border-t border-blue-100 pt-3">
                                        <p class="text-xs font-semibold text-slate-700">Submit</p>
                                        <p class="text-xs leading-5 text-slate-600">Pseudocode, a short explanation of your variables and logic, and a trace table with the final total, average, and goal result.</p>
                                    </div>
                                    <form class="mt-4 border-t border-blue-100 pt-4" onsubmit="saveAssignmentDraft(event, 2)">
                                        <label for="assignment-response-2" class="block text-xs font-bold text-slate-800 mb-1">Submission area</label>
                                        <textarea id="assignment-response-2" rows="4" class="w-full rounded-md border border-slate-300 bg-white p-2.5 text-xs text-slate-700 focus:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-100" placeholder="Write or paste your pseudocode and trace here..."></textarea>
                                        <div class="mt-3 flex flex-wrap items-center justify-between gap-2">
                                            <label class="inline-flex cursor-pointer items-center gap-2 rounded-md border border-slate-300 bg-white px-3 py-2 text-xs font-semibold text-slate-700 hover:border-blue-400">
                                                <i class="fa-solid fa-paperclip" aria-hidden="true"></i> Attach a file
                                                <input id="assignment-file-2" type="file" accept=".pdf,.doc,.docx,.txt,.png,.jpg,.jpeg" class="sr-only" onchange="showAssignmentFileName(2)">
                                            </label>
                                            <button type="submit" class="rounded-md bg-[#1e88e5] px-3 py-2 text-xs font-semibold text-white hover:bg-blue-700 transition">Save submission</button>
                                        </div>
                                        <p id="assignment-file-name-2" class="mt-2 text-[11px] text-slate-500" aria-live="polite">No file selected</p>
                                        <p class="mt-1 text-[10px] leading-4 text-slate-500">Drafts are saved in this browser. File attachments are not uploaded by this demo.</p>
                                        <p id="assignment-submit-status-2" class="mt-2 text-xs font-medium text-emerald-700" role="status" aria-live="polite"></p>
                                    </form>
                                </div>
                            </article>
                        </div>
                        <div id="assignment-calendar" class="scroll-mt-24 border-t border-slate-100 p-4 sm:p-5">
                            <div class="mb-4 flex flex-wrap items-center justify-between gap-3">
                                <div>
                                    <h3 class="flex items-center gap-2 text-sm font-bold text-slate-800"><i class="fa-regular fa-calendar-days text-[#1b365d]" aria-hidden="true"></i> Assignment calendar</h3>
                                    <p class="mt-1 text-xs text-slate-500">Add due dates and select a day to see what is due.</p>
                                </div>
                                <div class="flex items-center gap-2">
                                    <button type="button" onclick="changeAssignmentCalendarMonth(-1)" aria-label="Previous month" class="grid h-8 w-8 place-items-center rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50 focus:outline-none focus:ring-2 focus:ring-blue-200"><i class="fa-solid fa-chevron-left text-xs" aria-hidden="true"></i></button>
                                    <span id="assignment-calendar-month" class="min-w-28 text-center text-xs font-bold text-slate-700" aria-live="polite"></span>
                                    <button type="button" onclick="changeAssignmentCalendarMonth(1)" aria-label="Next month" class="grid h-8 w-8 place-items-center rounded-lg border border-slate-200 text-slate-600 hover:bg-slate-50 focus:outline-none focus:ring-2 focus:ring-blue-200"><i class="fa-solid fa-chevron-right text-xs" aria-hidden="true"></i></button>
                                    <button type="button" onclick="goToAssignmentCalendarToday()" class="rounded-lg border border-blue-200 px-2.5 py-2 text-[11px] font-semibold text-blue-700 hover:bg-blue-50">Today</button>
                                </div>
                            </div>
                            <div class="grid grid-cols-1 gap-4 lg:grid-cols-5">
                                <div class="lg:col-span-3">
                                    <div class="mb-1 grid grid-cols-7 gap-1 text-center text-[10px] font-bold uppercase tracking-wide text-slate-400" aria-hidden="true">
                                        <span>Sun</span><span>Mon</span><span>Tue</span><span>Wed</span><span>Thu</span><span>Fri</span><span>Sat</span>
                                    </div>
                                    <div id="assignment-calendar-grid" class="grid grid-cols-7 gap-1" role="group" aria-label="Assignment calendar dates"></div>
                                    <p class="mt-2 text-[10px] text-slate-400">A blue marker means at least one assignment is due that day.</p>
                                </div>
                                <div class="rounded-xl border border-slate-200 bg-slate-50/70 p-3 sm:p-4 lg:col-span-2">
                                    <h4 id="assignment-calendar-selected-date" class="text-xs font-bold text-slate-800"></h4>
                                    <div id="assignment-calendar-day-list" class="mt-2 space-y-2" aria-live="polite"></div>
                                    <form class="mt-4 border-t border-slate-200 pt-3" onsubmit="addAssignmentCalendarEntry(event)">
                                        <label for="assignment-calendar-title" class="mb-1 block text-[11px] font-semibold text-slate-700">Add an assignment</label>
                                        <input id="assignment-calendar-title" type="text" list="assignment-calendar-suggestions" maxlength="80" required placeholder="Assignment name" class="w-full rounded-md border border-slate-300 bg-white px-3 py-2 text-xs text-slate-700 focus:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-100">
                                        <datalist id="assignment-calendar-suggestions">
                                            <option value="Computer Systems Overview"></option>
                                            <option value="Programming Logic Challenge"></option>
                                        </datalist>
                                        <label for="assignment-calendar-date" class="mb-1 mt-2 block text-[11px] font-semibold text-slate-700">Due date</label>
                                        <input id="assignment-calendar-date" type="date" required class="w-full rounded-md border border-slate-300 bg-white px-3 py-2 text-xs text-slate-700 focus:border-blue-500 focus:outline-none focus:ring-2 focus:ring-blue-100">
                                        <button type="submit" class="mt-2 w-full rounded-md bg-[#1b365d] px-3 py-2 text-xs font-semibold text-white transition hover:bg-blue-900"><i class="fa-solid fa-plus mr-1" aria-hidden="true"></i> Add to calendar</button>
                                        <p id="assignment-calendar-status" class="mt-2 text-[10px] text-slate-500" role="status" aria-live="polite"></p>
                                    </form>
                                </div>
                            </div>
                        </div>
                    </section>
                    <!-- Gallery -->
                    <section class="bg-white rounded-xl p-4 shadow-sm border border-slate-200">
                        <div class="flex items-center space-x-2 mb-4">
                            <i class="fa-regular fa-image text-[#1b365d]"></i>
                            <h2 class="font-bold text-slate-800 text-base">Student Gallery</h2>
                        </div>
                        <div class="grid grid-cols-1 sm:grid-cols-3 gap-4">
                            <div class="group cursor-pointer" onclick="showToast('Gallery Preview: Learning to Code in Lab 3')">
                                <div class="overflow-hidden rounded-lg mb-2 shadow-sm border border-slate-100">
                                    <img src="https://images.unsplash.com/photo-1555066931-4365d14bab8c?auto=format&fit=crop&w=400&q=80" 
                                         alt="Learning to Code" 
                                         class="w-full h-32 object-cover group-hover:scale-105 transition duration-300">
                                </div>
                                <p class="text-xs font-semibold text-slate-700 text-center">Learning to Code</p>
                            </div>
                            <div class="group cursor-pointer" onclick="showToast('Gallery Preview: Digital & Print Study Materials')">
                                <div class="overflow-hidden rounded-lg mb-2 shadow-sm border border-slate-100">
                                    <img src="https://images.unsplash.com/photo-1497633762265-9d179a990aa6?auto=format&fit=crop&w=400&q=80" 
                                         alt="Study Materials" 
                                         class="w-full h-32 object-cover group-hover:scale-105 transition duration-300">
                                </div>
                                <p class="text-xs font-semibold text-slate-700 text-center">Study Materials</p>
                            </div>
                            <div class="group cursor-pointer" onclick="showToast('Gallery Preview: Collaborative Team Workshops')">
                                <div class="overflow-hidden rounded-lg mb-2 shadow-sm border border-slate-100">
                                    <img src="https://images.unsplash.com/photo-1522071820081-009f0129c71c?auto=format&fit=crop&w=400&q=80" 
                                         alt="Team Learning" 
                                         class="w-full h-32 object-cover group-hover:scale-105 transition duration-300">
                                </div>
                                <p class="text-xs font-semibold text-slate-700 text-center">Team Learning</p>
                            </div>
                        </div>
                    </section>

                    <!-- Key Topics & Links -->
                    <div id="links-section" class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                        <div class="bg-white rounded-xl p-4 shadow-sm border border-slate-200">
                            <div class="flex items-center space-x-2 mb-3">
                                <i class="fa-solid fa-list-ul text-[#1b365d]"></i>
                                <h3 class="font-bold text-slate-800 text-sm">Key Topics Covered</h3>
                            </div>
                            <ul class="text-xs text-slate-600 space-y-2 list-disc list-inside">
                                <li>What is a Computer? (Architecture & CPU)</li>
                                <li>Operating Systems & Process Management</li>
                                <li>Programming Languages Overview</li>
                                <li>Web Development (HTML, CSS, JavaScript)</li>
                                <li>Algorithms & Problem Solving Techniques</li>
                            </ul>
                        </div>

                        <div class="bg-white rounded-xl p-4 shadow-sm border border-slate-200">
                            <div class="flex items-center space-x-2 mb-3">
                                <i class="fa-solid fa-star text-[#1b365d]"></i>
                                <h3 class="font-bold text-slate-800 text-sm">Useful Links & Docs</h3>
                            </div>
                            <ul class="text-xs text-slate-600 space-y-2">
                                <li><a href="https://www.w3schools.com" target="_blank" class="text-blue-600 hover:underline"><i class="fa-solid fa-arrow-up-right-from-square text-[10px] mr-1"></i> W3Schools (HTML & CSS)</a></li>
                                <li><a href="https://javascript.info" target="_blank" class="text-blue-600 hover:underline"><i class="fa-solid fa-arrow-up-right-from-square text-[10px] mr-1"></i> Modern JavaScript Tutorial</a></li>
                                <li><a href="https://github.com" target="_blank" class="text-blue-600 hover:underline"><i class="fa-solid fa-arrow-up-right-from-square text-[10px] mr-1"></i> GitHub Student Developer Pack</a></li>
                                <li><a href="https://developer.mozilla.org" target="_blank" class="text-blue-600 hover:underline"><i class="fa-solid fa-arrow-up-right-from-square text-[10px] mr-1"></i> MDN Web Docs Reference</a></li>
                                <li><a href="https://www.coursera.org" target="_blank" class="text-blue-600 hover:underline"><i class="fa-solid fa-arrow-up-right-from-square text-[10px] mr-1"></i> Coursera Computer Science</a></li>
                            </ul>
                        </div>
                    </div>

                    <!-- Bottom CTA Banner -->
                    <div class="bg-blue-50 border border-blue-100 rounded-xl p-5 flex flex-col sm:flex-row items-center justify-between gap-4 shadow-sm">
                        <div class="flex items-center space-x-3">
                            <div class="p-3 bg-[#1e88e5] text-white rounded-xl flex-shrink-0">
                                <i class="fa-solid fa-laptop-code text-xl"></i>
                            </div>
                            <div>
                                <h4 class="font-bold text-slate-800 text-sm">Ready to take the next step?</h4>
                                <p class="text-xs text-slate-500">Explore all course materials and start your digital learning journey today!</p>
                            </div>
                        </div>
                        <button onclick="navigateTo('courses')" class="w-full sm:w-auto whitespace-nowrap bg-[#1b365d] hover:bg-blue-900 text-white font-medium text-xs px-5 py-2.5 rounded-lg transition shadow-sm">
                            Browse All Courses <i class="fa-solid fa-arrow-right ml-1.5 text-[10px]"></i>
                        </button>
                    </div>

                </main>

                <!-- Right Sidebar -->
                <aside class="lg:col-span-3 space-y-6">
                    <div class="bg-white rounded-xl overflow-hidden shadow-sm border border-slate-200">
                        <img src="https://images.unsplash.com/photo-1562774053-701939374585?auto=format&fit=crop&w=600&q=80" 
                             alt="Campus Building" 
                             class="w-full h-40 object-cover">
                    </div>

                    <div class="bg-white rounded-xl p-4 shadow-sm border border-slate-200">
                        <div class="flex items-center space-x-2 mb-3 pb-2 border-b border-slate-100">
                            <i class="fa-solid fa-link text-[#1b365d]"></i>
                            <h3 class="font-bold text-slate-800 text-sm">Quick Links</h3>
                        </div>
                        <ul class="space-y-2 text-xs">
                            <li><button onclick="openCourseModal(1)" class="text-blue-600 hover:text-blue-800 flex items-center group text-left"><i class="fa-solid fa-arrow-right text-[10px] mr-2 text-blue-500 group-hover:translate-x-1 transition-transform"></i> Course Syllabus</button></li>
                            <li><button onclick="navigateTo('resources')" class="text-blue-600 hover:text-blue-800 flex items-center group text-left"><i class="fa-solid fa-arrow-right text-[10px] mr-2 text-blue-500 group-hover:translate-x-1 transition-transform"></i> Download Notes (PDF)</button></li>
                            <li><button onclick="navigateTo('resources')" class="text-blue-600 hover:text-blue-800 flex items-center group text-left"><i class="fa-solid fa-arrow-right text-[10px] mr-2 text-blue-500 group-hover:translate-x-1 transition-transform"></i> Programming Resources</button></li>
                            <li><button onclick="openForumModal()" class="text-blue-600 hover:text-blue-800 flex items-center group text-left"><i class="fa-solid fa-arrow-right text-[10px] mr-2 text-blue-500 group-hover:translate-x-1 transition-transform"></i> Student Forum</button></li>
                            <li><button onclick="navigateTo('about')" class="text-blue-600 hover:text-blue-800 flex items-center group text-left"><i class="fa-solid fa-arrow-right text-[10px] mr-2 text-blue-500 group-hover:translate-x-1 transition-transform"></i> External References</button></li>
                        </ul>
                    </div>

                    <div id="events-section" class="bg-white rounded-xl p-4 shadow-sm border border-slate-200">
                        <div class="flex items-center space-x-2 mb-3 pb-2 border-b border-slate-100">
                            <i class="fa-regular fa-calendar-days text-[#1b365d]"></i>
                            <h3 class="font-bold text-slate-800 text-sm">Upcoming Assignment Deadlines</h3>
                        </div>
                        <div id="upcoming-assignment-due-list" class="space-y-3 text-xs">
                            <p class="text-slate-500 text-[11px]">No assignment deadlines scheduled.</p>
                        </div>
                        <button type="button" onclick="scrollToSection('assignment-calendar')" class="mt-3 text-[11px] font-semibold text-blue-600 hover:text-blue-800">Open assignment calendar <i class="fa-solid fa-arrow-right ml-1 text-[9px]" aria-hidden="true"></i></button>
                    </div>
                    <!-- Sidebar Quick Contact -->
                    <div class="bg-white rounded-xl p-4 shadow-sm border border-slate-200">
                        <div class="flex items-center space-x-2 mb-3 pb-2 border-b border-slate-100">
                            <i class="fa-regular fa-envelope text-[#1b365d]"></i>
                            <h3 class="font-bold text-slate-800 text-sm">Quick Inquiry</h3>
                        </div>

                        <form onsubmit="handleQuickContact(event)" class="space-y-3 text-xs">
                            <div>
                                <label class="block text-slate-700 font-medium mb-1">Name <span class="text-red-500">*</span></label>
                                <input required type="text" name="name" placeholder="Enter your name" class="w-full px-3 py-2 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs">
                            </div>
                            <div>
                                <label class="block text-slate-700 font-medium mb-1">Email <span class="text-red-500">*</span></label>
                                <input required type="email" name="email" placeholder="Enter your email" class="w-full px-3 py-2 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs">
                            </div>
                            <div>
                                <label class="block text-slate-700 font-medium mb-1">Message <span class="text-red-500">*</span></label>
                                <textarea required name="message" rows="2" placeholder="Your query..." class="w-full px-3 py-2 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs resize-none"></textarea>
                            </div>
                            <button type="submit" class="w-full bg-[#1e88e5] hover:bg-blue-600 text-white font-medium py-2 rounded-lg transition shadow-sm">
                                Send Message
                            </button>
                        </form>
                    </div>
                </aside>

            </div>
        </div>


        <!-- ================= PAGE 2: UPDATED ABOUT PAGE ================= -->
        <div id="page-about" class="page-section hidden space-y-8">
            
            <!-- Hero Header -->
            <div class="bg-gradient-to-r from-[#1b365d] via-[#1565c0] to-[#1e88e5] text-white rounded-2xl p-8 sm:p-12 shadow-xl relative overflow-hidden">
                <div class="max-w-2xl relative z-10">
                    <span class="bg-white/20 text-blue-100 text-xs font-bold uppercase tracking-wider px-3 py-1 rounded-full mb-3 inline-block backdrop-blur-sm">Empowering Global Innovation</span>
                    <h1 class="text-3xl sm:text-5xl font-extrabold tracking-tight mb-4 leading-tight">Bridging Academic Vision & Future Tech Skills</h1>
                    <p class="text-blue-100 text-sm sm:text-base leading-relaxed mb-6">
                        CampusLearn was created to reinvent online higher education. We merge academic rigor with modern, industry-first coding environments, providing students with accessible paths to real-world software engineering careers.
                    </p>
                    <div class="flex flex-wrap gap-3">
                        <button onclick="navigateTo('courses')" class="bg-white text-[#1b365d] font-bold text-xs px-5 py-2.5 rounded-lg hover:bg-blue-50 transition shadow">
                            Explore Curriculum
                        </button>
                        <button onclick="navigateTo('contact')" class="bg-blue-600/40 hover:bg-blue-600/60 text-white font-bold text-xs px-5 py-2.5 rounded-lg transition backdrop-blur-sm border border-white/20">
                            Partner With Us
                        </button>
                    </div>
                </div>
                <div class="absolute -right-10 -bottom-10 opacity-10 text-white text-[220px] pointer-events-none">
                    <i class="fa-solid fa-compass-drafting"></i>
                </div>
            </div>

            <!-- Interactive Timeline Journey -->
            <div class="bg-white rounded-2xl p-6 sm:p-8 border border-slate-200 shadow-sm">
                <div class="text-center max-w-lg mx-auto mb-8">
                    <h2 class="text-2xl font-extrabold text-[#1b365d]">Our Growth Journey</h2>
                    <p class="text-xs text-slate-500 mt-1">From a humble computer science lab project to a global learning network.</p>
                </div>

                <div class="grid grid-cols-1 md:grid-cols-4 gap-4 relative">
                    <div class="p-4 bg-slate-50 rounded-xl border border-slate-200 hover:border-blue-500 transition group">
                        <span class="text-xs font-bold text-blue-600 bg-blue-100 px-2.5 py-0.5 rounded-full">2021</span>
                        <h4 class="font-bold text-slate-800 text-sm mt-3 mb-1">Campus Genesis</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Launched as an open-source syllabus portal for 200 introductory computer science students.</p>
                    </div>

                    <div class="p-4 bg-slate-50 rounded-xl border border-slate-200 hover:border-blue-500 transition group">
                        <span class="text-xs font-bold text-blue-600 bg-blue-100 px-2.5 py-0.5 rounded-full">2023</span>
                        <h4 class="font-bold text-slate-800 text-sm mt-3 mb-1">Interactive Labs</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Integrated real-time code sandboxes and online assignment auto-graders.</p>
                    </div>

                    <div class="p-4 bg-slate-50 rounded-xl border border-slate-200 hover:border-blue-500 transition group">
                        <span class="text-xs font-bold text-blue-600 bg-blue-100 px-2.5 py-0.5 rounded-full">2025</span>
                        <h4 class="font-bold text-blue-600 bg-blue-100 px-2.5 py-0.5 rounded-full">University Network</h4>
                        <p class="text-xs text-slate-500 leading-relaxed mt-3">Partnered with over 50 universities worldwide to share interactive study materials.</p>
                    </div>

                    <div class="p-4 bg-blue-50/50 rounded-xl border border-blue-200 hover:border-blue-600 transition group">
                        <span class="text-xs font-bold text-white bg-blue-600 px-2.5 py-0.5 rounded-full">2026 Today</span>
                        <h4 class="font-bold text-slate-800 text-sm mt-3 mb-1">Global Platform</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Empowering over 25,000 active learners with accredited course tracks.</p>
                    </div>
                </div>
            </div>

            <!-- Dynamic Interactive Core Pillars -->
            <div>
                <h2 class="text-2xl font-bold text-[#1b365d] mb-6 text-center">Core Educational Principles</h2>
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                    <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm hover:shadow-md transition text-center group cursor-pointer" onclick="showToast('Principle: Academic Rigor - Standards vetted by industry veterans.')">
                        <div class="w-14 h-14 bg-blue-50 text-[#1e88e5] rounded-2xl flex items-center justify-center text-2xl mx-auto mb-4 group-hover:bg-[#1e88e5] group-hover:text-white transition duration-300">
                            <i class="fa-solid fa-award"></i>
                        </div>
                        <h4 class="font-bold text-slate-800 mb-1">Academic Rigor</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Designed by computer science department chairs and senior architects.</p>
                    </div>

                    <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm hover:shadow-md transition text-center group cursor-pointer" onclick="showToast('Principle: Open Accessibility - Zero cost barriers to student foundations.')">
                        <div class="w-14 h-14 bg-blue-50 text-[#1e88e5] rounded-2xl flex items-center justify-center text-2xl mx-auto mb-4 group-hover:bg-[#1e88e5] group-hover:text-white transition duration-300">
                            <i class="fa-solid fa-earth-americas"></i>
                        </div>
                        <h4 class="font-bold text-slate-800 mb-1">Open Access</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Free introductory modules and open download resources for all students.</p>
                    </div>

                    <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm hover:shadow-md transition text-center group cursor-pointer" onclick="showToast('Principle: Active Building - Learn by completing portfolio projects.')">
                        <div class="w-14 h-14 bg-blue-50 text-[#1e88e5] rounded-2xl flex items-center justify-center text-2xl mx-auto mb-4 group-hover:bg-[#1e88e5] group-hover:text-white transition duration-300">
                            <i class="fa-solid fa-code-commit"></i>
                        </div>
                        <h4 class="font-bold text-slate-800 mb-1">Practical Code</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Focus on writing code, debugging, and deploying production apps.</p>
                    </div>

                    <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm hover:shadow-md transition text-center group cursor-pointer" onclick="showToast('Principle: Peer Support - Vibrant student discussion boards.')">
                        <div class="w-14 h-14 bg-blue-50 text-[#1e88e5] rounded-2xl flex items-center justify-center text-2xl mx-auto mb-4 group-hover:bg-[#1e88e5] group-hover:text-white transition duration-300">
                            <i class="fa-solid fa-people-group"></i>
                        </div>
                        <h4 class="font-bold text-slate-800 mb-1">Community Support</h4>
                        <p class="text-xs text-slate-500 leading-relaxed">Active student forum, peer feedback circles, and mentor advice.</p>
                    </div>
                </div>
            </div>

            <!-- Enhanced Instructors Showcase -->
            <div class="bg-white rounded-2xl p-8 border border-slate-200 shadow-sm">
                <div class="flex flex-col sm:flex-row items-center justify-between gap-4 mb-8">
                    <div>
                        <h2 class="text-2xl font-bold text-[#1b365d]">Academic Faculty & Mentors</h2>
                        <p class="text-slate-500 text-xs mt-1">Guiding students with research experience and real software expertise.</p>
                    </div>
                    <button onclick="navigateTo('contact')" class="text-xs font-semibold text-blue-600 hover:text-blue-800 flex items-center">
                        Contact Faculty Office <i class="fa-solid fa-chevron-right text-[10px] ml-1"></i>
                    </button>
                </div>
                
                <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-6">
                    <!-- Member 1 -->
                    <div class="border border-slate-100 bg-slate-50/50 rounded-xl p-5 hover:bg-white hover:shadow-md transition">
                        <div class="flex items-center space-x-4 mb-4">
                            <img src="https://images.unsplash.com/photo-1534528741775-53994a69daeb?auto=format&fit=crop&w=300&q=80" alt="Dr. Sarah Jenkins" class="w-16 h-16 rounded-full object-cover shadow border-2 border-white">
                            <div>
                                <h4 class="font-bold text-slate-800 text-base">Dr. Sarah Jenkins</h4>
                                <p class="text-xs text-blue-600 font-medium">Head of Computer Science</p>
                                <span class="text-[10px] text-slate-400">15+ Yrs Academic Exp.</span>
                            </div>
                        </div>
                        <p class="text-xs text-slate-500 leading-relaxed mb-4">Specialized in algorithmic efficiency, computer architecture, and introductory CS curriculum design.</p>
                        <div class="flex items-center justify-between pt-3 border-t border-slate-200/60 text-xs">
                            <span class="bg-blue-100 text-blue-700 text-[10px] font-semibold px-2 py-0.5 rounded">CS Fundamentals</span>
                            <div class="flex space-x-2 text-slate-400">
                                <a href="javascript:void(0)" onclick="showToast('Contacting Dr. Sarah Jenkins...')" class="hover:text-blue-600"><i class="fa-solid fa-envelope"></i></a>
                                <a href="https://linkedin.com" target="_blank" class="hover:text-blue-600"><i class="fa-brands fa-linkedin"></i></a>
                            </div>
                        </div>
                    </div>

                    <!-- Member 2 -->
                    <div class="border border-slate-100 bg-slate-50/50 rounded-xl p-5 hover:bg-white hover:shadow-md transition">
                        <div class="flex items-center space-x-4 mb-4">
                            <img src="https://images.unsplash.com/photo-1507003211169-0a1dd7228f2d?auto=format&fit=crop&w=300&q=80" alt="Prof. David Chen" class="w-16 h-16 rounded-full object-cover shadow border-2 border-white">
                            <div>
                                <h4 class="font-bold text-slate-800 text-base">Prof. David Chen</h4>
                                <p class="text-xs text-blue-600 font-medium">Web Systems Director</p>
                                <span class="text-[10px] text-slate-400">Ex-TechCorp Principal Eng.</span>
                            </div>
                        </div>
                        <p class="text-xs text-slate-500 leading-relaxed mb-4">Focuses on frontend engineering, full-stack web application architectures, and JavaScript ecosystems.</p>
                        <div class="flex items-center justify-between pt-3 border-t border-slate-200/60 text-xs">
                            <span class="bg-blue-100 text-blue-700 text-[10px] font-semibold px-2 py-0.5 rounded">Full-Stack Dev</span>
                            <div class="flex space-x-2 text-slate-400">
                                <a href="https://github.com" target="_blank" class="hover:text-blue-600"><i class="fa-brands fa-github"></i></a>
                                <a href="https://linkedin.com" target="_blank" class="hover:text-blue-600"><i class="fa-brands fa-linkedin"></i></a>
                            </div>
                        </div>
                    </div>

                    <!-- Member 3 -->
                    <div class="border border-slate-100 bg-slate-50/50 rounded-xl p-5 hover:bg-white hover:shadow-md transition">
                        <div class="flex items-center space-x-4 mb-4">
                            <img src="https://images.unsplash.com/photo-1573496359142-b8d87734a5a2?auto=format&fit=crop&w=300&q=80" alt="Elena Rostova" class="w-16 h-16 rounded-full object-cover shadow border-2 border-white">
                            <div>
                                <h4 class="font-bold text-slate-800 text-base">Elena Rostova</h4>
                                <p class="text-xs text-blue-600 font-medium">AI & Analytics Lead</p>
                                <span class="text-[10px] text-slate-400">Neural Network Researcher</span>
                            </div>
                        </div>
                        <p class="text-xs text-slate-500 leading-relaxed mb-4">Teaches data science pipelines, Python machine learning models, and ethical AI integration.</p>
                        <div class="flex items-center justify-between pt-3 border-t border-slate-200/60 text-xs">
                            <span class="bg-blue-100 text-blue-700 text-[10px] font-semibold px-2 py-0.5 rounded">AI & Data Science</span>
                            <div class="flex space-x-2 text-slate-400">
                                <a href="javascript:void(0)" onclick="showToast('Contacting Prof. Elena Rostova...')" class="hover:text-blue-600"><i class="fa-solid fa-envelope"></i></a>
                                <a href="https://linkedin.com" target="_blank" class="hover:text-blue-600"><i class="fa-brands fa-linkedin"></i></a>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <!-- Stats Bar -->
            <div class="bg-[#1b365d] text-white rounded-xl p-8 grid grid-cols-2 sm:grid-cols-4 gap-6 text-center">
                <div>
                    <span class="text-3xl sm:text-4xl font-extrabold text-blue-300">25,000+</span>
                    <p class="text-xs text-blue-100 mt-1 uppercase tracking-wider">Active Students</p>
                </div>
                <div>
                    <span class="text-3xl sm:text-4xl font-extrabold text-blue-300">48+</span>
                    <p class="text-xs text-blue-100 mt-1 uppercase tracking-wider">Accredited Courses</p>
                </div>
                <div>
                    <span class="text-3xl sm:text-4xl font-extrabold text-blue-300">94%</span>
                    <p class="text-xs text-blue-100 mt-1 uppercase tracking-wider">Completion Rate</p>
                </div>
                <div>
                    <span class="text-3xl sm:text-4xl font-extrabold text-blue-300">120+</span>
                    <p class="text-xs text-blue-100 mt-1 uppercase tracking-wider">Partner Universities</p>
                </div>
            </div>
        </div>


        <!-- ================= PAGE 3: COURSES PAGE ================= -->
        <div id="page-courses" class="page-section hidden space-y-6">
            
            <!-- Page Header & Filters -->
            <div class="bg-white rounded-2xl p-6 border border-slate-200 shadow-sm">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                    <div>
                        <h1 class="text-2xl font-bold text-[#1b365d]">Academic Course Catalog</h1>
                        <p class="text-xs text-slate-500 mt-1">Explore structured courses from foundations to advanced specializations.</p>
                    </div>

                    <!-- Search & Level Dropdown -->
                    <div class="flex flex-col sm:flex-row gap-3">
                        <div class="relative">
                            <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-slate-400 text-xs"></i>
                            <input id="course-search-input" type="text" placeholder="Search courses..." onkeyup="filterCourses()" class="pl-9 pr-3 py-1.5 border border-slate-200 rounded-lg text-xs w-full sm:w-48 focus:outline-none focus:ring-1 focus:ring-blue-500">
                        </div>
                        <select id="course-level-filter" onchange="filterCourses()" class="px-3 py-1.5 border border-slate-200 rounded-lg text-xs text-slate-700 focus:outline-none focus:ring-1 focus:ring-blue-500">
                            <option value="all">All Difficulty Levels</option>
                            <option value="Beginner">Beginner</option>
                            <option value="Intermediate">Intermediate</option>
                            <option value="Advanced">Advanced</option>
                        </select>
                    </div>
                </div>

                <!-- Category Tabs -->
                <div class="flex flex-wrap gap-2 border-t border-slate-100 pt-4 text-xs font-medium">
                    <button onclick="setCourseCategory('all', this)" class="course-cat-btn bg-[#1b365d] text-white px-3.5 py-1.5 rounded-full transition">All Courses</button>
                    <button onclick="setCourseCategory('CS', this)" class="course-cat-btn bg-slate-100 text-slate-600 hover:bg-slate-200 px-3.5 py-1.5 rounded-full transition">Computer Science</button>
                    <button onclick="setCourseCategory('Web', this)" class="course-cat-btn bg-slate-100 text-slate-600 hover:bg-slate-200 px-3.5 py-1.5 rounded-full transition">Web Dev</button>
                    <button onclick="setCourseCategory('Data', this)" class="course-cat-btn bg-slate-100 text-slate-600 hover:bg-slate-200 px-3.5 py-1.5 rounded-full transition">Data Science</button>
                    <button onclick="setCourseCategory('AI', this)" class="course-cat-btn bg-slate-100 text-slate-600 hover:bg-slate-200 px-3.5 py-1.5 rounded-full transition">AI & Machine Learning</button>
                </div>
            </div>

            <!-- Course Grid -->
            <div id="course-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Dynamically Rendered -->
            </div>
        </div>


        <!-- ================= PAGE 4: RESOURCES PAGE ================= -->
        <div id="page-resources" class="page-section hidden space-y-6">
            
            <!-- Resource Header -->
            <div class="bg-white rounded-2xl p-6 border border-slate-200 shadow-sm">
                <div class="flex flex-col md:flex-row md:items-center justify-between gap-4 mb-6">
                    <div>
                        <h1 class="text-2xl font-bold text-[#1b365d]">Learning Resources & Downloads</h1>
                        <p class="text-xs text-slate-500 mt-1">Access lecture notes, cheat sheets, developer toolkits, and practice datasets.</p>
                    </div>

                    <!-- Search Input -->
                    <div class="relative w-full md:w-64">
                        <i class="fa-solid fa-magnifying-glass absolute left-3 top-2.5 text-slate-400 text-xs"></i>
                        <input id="resource-search-input" type="text" placeholder="Search resources..." onkeyup="filterResources()" class="pl-9 pr-3 py-1.5 border border-slate-200 rounded-lg text-xs w-full focus:outline-none focus:ring-1 focus:ring-blue-500">
                    </div>
                </div>

                <!-- Resource Category Pills -->
                <div class="flex flex-wrap gap-2 border-t border-slate-100 pt-4 text-xs font-medium">
                    <button onclick="setResourceCategory('all', this)" class="res-cat-btn bg-[#1b365d] text-white px-3 py-1.5 rounded-full transition">All Resources</button>
                    <button onclick="setResourceCategory('notes', this)" class="res-cat-btn bg-slate-100 text-slate-600 hover:bg-slate-200 px-3 py-1.5 rounded-full transition">Lecture Notes (PDF)</button>
                    <button onclick="setResourceCategory('cheatsheet', this)" class="res-cat-btn bg-slate-100 text-slate-600 hover:bg-slate-200 px-3 py-1.5 rounded-full transition">Cheat Sheets</button>
                    <button onclick="setResourceCategory('tools', this)" class="res-cat-btn bg-slate-100 text-slate-600 hover:bg-slate-200 px-3 py-1.5 rounded-full transition">Dev Toolkits</button>
                    <button onclick="setResourceCategory('datasets', this)" class="res-cat-btn bg-slate-100 text-slate-600 hover:bg-slate-200 px-3 py-1.5 rounded-full transition">Datasets</button>
                </div>
            </div>

            <!-- Resource Grid -->
            <div id="resource-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-6">
                <!-- Dynamically Rendered -->
            </div>

            <!-- Request Resource Banner -->
            <div class="bg-gradient-to-r from-blue-900 to-[#1b365d] text-white rounded-2xl p-6 shadow-sm flex flex-col sm:flex-row items-center justify-between gap-4">
                <div class="flex items-center space-x-4">
                    <div class="p-3 bg-white/10 rounded-xl text-blue-300 text-2xl">
                        <i class="fa-solid fa-box-open"></i>
                    </div>
                    <div>
                        <h4 class="font-bold text-base">Need specific study materials or guides?</h4>
                        <p class="text-xs text-blue-200 mt-0.5">Submit a resource request to our academic team for immediate review.</p>
                    </div>
                </div>
                <button onclick="navigateTo('contact')" class="bg-[#1e88e5] hover:bg-blue-600 text-white font-medium text-xs px-5 py-2.5 rounded-lg transition whitespace-nowrap shadow">
                    Request Resource <i class="fa-solid fa-paper-plane ml-1"></i>
                </button>
            </div>
        </div>


        <!-- ================= PAGE 5: CONTACT PAGE ================= -->
        <div id="page-contact" class="page-section hidden space-y-8">
            
            <!-- Header -->
            <div class="bg-white rounded-2xl p-8 border border-slate-200 shadow-sm text-center">
                <span class="bg-blue-50 text-blue-600 text-xs font-bold uppercase tracking-wider px-3 py-1 rounded-full inline-block mb-2">Get In Touch</span>
                <h1 class="text-3xl font-extrabold text-[#1b365d]">We're Here to Help You Succeed</h1>
                <p class="text-slate-500 text-sm max-w-lg mx-auto mt-2">Have questions about enrollment, course content, or technical support? Our academic advisers are ready to assist.</p>
            </div>

            <!-- Info Cards -->
            <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-6">
                <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm text-center">
                    <div class="w-12 h-12 bg-blue-50 text-[#1e88e5] rounded-full flex items-center justify-center text-xl mx-auto mb-3">
                        <i class="fa-solid fa-location-dot"></i>
                    </div>
                    <h4 class="font-bold text-slate-800 text-sm mb-1">Campus Location</h4>
                    <p class="text-xs text-slate-500 leading-relaxed">100 University Plaza, Science Block C, Cambridge, MA 02138</p>
                </div>

                <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm text-center">
                    <div class="w-12 h-12 bg-blue-50 text-[#1e88e5] rounded-full flex items-center justify-center text-xl mx-auto mb-3">
                        <i class="fa-solid fa-envelope"></i>
                    </div>
                    <h4 class="font-bold text-slate-800 text-sm mb-1">Email Support</h4>
                    <p class="text-xs text-blue-600 leading-relaxed font-medium">support@campuslearn.edu</p>
                    <p class="text-[11px] text-slate-400">Response time: &lt; 24 hrs</p>
                </div>

                <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm text-center">
                    <div class="w-12 h-12 bg-blue-50 text-[#1e88e5] rounded-full flex items-center justify-center text-xl mx-auto mb-3">
                        <i class="fa-solid fa-phone"></i>
                    </div>
                    <h4 class="font-bold text-slate-800 text-sm mb-1">Admissions Phone</h4>
                    <p class="text-xs text-slate-700 font-semibold">+1 (800) 555-CAMPUS</p>
                    <p class="text-[11px] text-slate-400">Mon-Fri, 9am - 5pm EST</p>
                </div>

                <div class="bg-white p-6 rounded-xl border border-slate-200 shadow-sm text-center">
                    <div class="w-12 h-12 bg-blue-50 text-[#1e88e5] rounded-full flex items-center justify-center text-xl mx-auto mb-3">
                        <i class="fa-solid fa-clock"></i>
                    </div>
                    <h4 class="font-bold text-slate-800 text-sm mb-1">Office Hours</h4>
                    <p class="text-xs text-slate-500 leading-relaxed">Monday – Friday</p>
                    <p class="text-xs text-slate-500">08:00 AM – 06:00 PM</p>
                </div>
            </div>

            <!-- Form & FAQ -->
            <div class="grid grid-cols-1 lg:grid-cols-12 gap-8">
                
                <!-- Main Form -->
                <div class="lg:col-span-7 bg-white rounded-2xl p-6 sm:p-8 border border-slate-200 shadow-sm">
                    <h3 class="text-xl font-bold text-[#1b365d] mb-4">Send Us a Direct Message</h3>
                    
                    <form onsubmit="handleFullContactSubmit(event)" class="space-y-4 text-xs">
                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-slate-700 font-semibold mb-1">Full Name <span class="text-red-500">*</span></label>
                                <input required type="text" placeholder="John Doe" class="w-full px-3.5 py-2.5 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs">
                            </div>
                            <div>
                                <label class="block text-slate-700 font-semibold mb-1">Email Address <span class="text-red-500">*</span></label>
                                <input required type="email" placeholder="john@example.com" class="w-full px-3.5 py-2.5 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs">
                            </div>
                        </div>

                        <div class="grid grid-cols-1 sm:grid-cols-2 gap-4">
                            <div>
                                <label class="block text-slate-700 font-semibold mb-1">Inquiry Department</label>
                                <select class="w-full px-3.5 py-2.5 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs text-slate-700">
                                    <option>General Admissions</option>
                                    <option>Technical Support</option>
                                    <option>Course Content & Syllabus</option>
                                    <option>Financial Aid & Grants</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-slate-700 font-semibold mb-1">Subject</label>
                                <input type="text" placeholder="e.g. Question about CS Fundamentals" class="w-full px-3.5 py-2.5 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs">
                            </div>
                        </div>

                        <div>
                            <label class="block text-slate-700 font-semibold mb-1">Message Detail <span class="text-red-500">*</span></label>
                            <textarea required rows="4" placeholder="Please write your questions or comments here..." class="w-full px-3.5 py-2.5 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs resize-none"></textarea>
                        </div>

                        <div>
                            <label class="block text-slate-700 font-semibold mb-1">Attachment (Optional PDF/Doc)</label>
                            <input type="file" class="w-full px-3 py-2 border border-slate-200 rounded-lg text-slate-500 text-xs bg-slate-50">
                        </div>

                        <button type="submit" class="w-full bg-[#1b365d] hover:bg-blue-900 text-white font-semibold py-3 rounded-lg transition shadow-md text-sm">
                            Submit Inquiry <i class="fa-solid fa-paper-plane ml-2"></i>
                        </button>
                    </form>
                </div>

                <!-- Accordion & Map -->
                <div class="lg:col-span-5 space-y-6">
                    
                    <div class="bg-white rounded-2xl p-6 border border-slate-200 shadow-sm">
                        <h3 class="text-lg font-bold text-[#1b365d] mb-4 flex items-center">
                            <i class="fa-solid fa-circle-question text-blue-500 mr-2"></i> Frequently Asked Questions
                        </h3>

                        <div class="space-y-3 text-xs">
                            <div class="border border-slate-200 rounded-lg overflow-hidden">
                                <button onclick="toggleFaq('faq-1')" class="w-full text-left p-3 font-semibold text-slate-800 bg-slate-50 hover:bg-slate-100 flex justify-between items-center">
                                    <span>How do I enroll in a course?</span>
                                    <i id="faq-icon-faq-1" class="fa-solid fa-chevron-down text-slate-400 transition-transform"></i>
                                </button>
                                <div id="faq-1" class="hidden p-3 text-slate-600 bg-white border-t border-slate-100 leading-relaxed">
                                    Simply browse our Courses catalog, select your desired course, and click "Enroll Now". Access to beginner modules is granted immediately upon account confirmation.
                                </div>
                            </div>

                            <div class="border border-slate-200 rounded-lg overflow-hidden">
                                <button onclick="toggleFaq('faq-2')" class="w-full text-left p-3 font-semibold text-slate-800 bg-slate-50 hover:bg-slate-100 flex justify-between items-center">
                                    <span>Are certificates provided upon completion?</span>
                                    <i id="faq-icon-faq-2" class="fa-solid fa-chevron-down text-slate-400 transition-transform"></i>
                                </button>
                                <div id="faq-2" class="hidden p-3 text-slate-600 bg-white border-t border-slate-100 leading-relaxed">
                                    Yes! Verified certificates of achievement are issued digitally after completing all course assignments, quizzes, and passing the final assessment.
                                </div>
                            </div>

                            <div class="border border-slate-200 rounded-lg overflow-hidden">
                                <button onclick="toggleFaq('faq-3')" class="w-full text-left p-3 font-semibold text-slate-800 bg-slate-50 hover:bg-slate-100 flex justify-between items-center">
                                    <span>Can I download lecture notes offline?</span>
                                    <i id="faq-icon-faq-3" class="fa-solid fa-chevron-down text-slate-400 transition-transform"></i>
                                </button>
                                <div id="faq-3" class="hidden p-3 text-slate-600 bg-white border-t border-slate-100 leading-relaxed">
                                    Absolutely. Visit our Resources section to download PDF lecture summaries, cheat sheets, and source code packages anytime.
                                </div>
                            </div>
                        </div>
                        <div class="mt-5 border-t border-slate-100 pt-4">
                            <h4 class="text-sm font-bold text-[#1b365d] mb-1">Still have a question?</h4>
                            <p class="text-xs text-slate-500 mb-3">Ask us something that is not covered above.</p>
                            <form onsubmit="handleFaqQuestion(event)" class="space-y-3">
                                <label for="faq-question-input" class="sr-only">Ask a question</label>
                                <textarea id="faq-question-input" name="question" required rows="3" placeholder="Type your question here..." class="w-full px-3 py-2 border border-slate-200 rounded-lg text-xs focus:outline-none focus:ring-1 focus:ring-blue-500 resize-y"></textarea>
                                <button type="submit" class="w-full bg-[#1e88e5] hover:bg-blue-600 text-white font-medium py-2 rounded-lg transition shadow-sm">
                                    Ask Question <i class="fa-solid fa-paper-plane ml-1.5 text-[10px]"></i>
                                </button>
                            </form>
                        </div>
                    </div>

                    <div class="bg-white rounded-2xl overflow-hidden border border-slate-200 shadow-sm">
                        <div class="p-3 bg-slate-50 border-b border-slate-100 flex items-center justify-between text-xs font-semibold text-slate-700">
                            <span>Campus Map Location</span>
                            <span class="text-blue-600"><i class="fa-solid fa-compass mr-1"></i> Science Quad</span>
                        </div>
                        <div class="relative h-48 bg-slate-200 flex items-center justify-center group overflow-hidden">
                            <img src="https://images.unsplash.com/photo-1526778548025-fa2f459cd5c1?auto=format&fit=crop&w=600&q=80" alt="Map Preview" class="w-full h-full object-cover">
                            <div class="absolute inset-0 bg-slate-900/40 flex items-center justify-center">
                                <div class="bg-white text-slate-800 px-4 py-2 rounded-lg shadow-lg text-xs font-bold flex items-center space-x-2">
                                    <i class="fa-solid fa-location-dot text-red-500"></i>
                                    <span>CampusLearn Main Campus</span>
                                </div>
                            </div>
                        </div>
                    </div>

                </div>
            </div>
        </div>

    </div>

    <!-- Footer -->
    <footer class="bg-[#1b365d] text-white mt-12 py-8 border-t border-blue-900/80">
        <div class="max-w-7xl mx-auto px-4 sm:px-6 lg:px-8">
            <div class="flex flex-col md:flex-row items-center justify-between gap-6">
                
                <div class="flex items-center space-x-3">
                    <i class="fa-solid fa-graduation-cap text-2xl text-blue-300"></i>
                    <div>
                        <span class="text-lg font-bold tracking-tight block">CampusLearn</span>
                        <span class="text-[10px] text-blue-200 uppercase tracking-widest block font-medium">Learn · Build · Grow</span>
                    </div>
                </div>

                <div class="flex flex-wrap justify-center items-center space-x-4 text-xs font-medium text-blue-100">
                    <button onclick="navigateTo('home')" class="hover:text-white transition">Home</button>
                    <span>|</span>
                    <button onclick="navigateTo('about')" class="hover:text-white transition">About</button>
                    <span>|</span>
                    <button onclick="navigateTo('courses')" class="hover:text-white transition">Courses</button>
                    <span>|</span>
                    <button onclick="navigateTo('resources')" class="hover:text-white transition">Resources</button>
                    <span>|</span>
                    <button onclick="navigateTo('contact')" class="hover:text-white transition">Contact</button>
                </div>

                <div class="flex flex-col items-center md:items-end space-y-2">
                    <div class="flex space-x-4 text-base text-blue-200">
                        <a href="#" class="hover:text-white transition"><i class="fa-brands fa-facebook"></i></a>
                        <a href="#" class="hover:text-white transition"><i class="fa-brands fa-x-twitter"></i></a>
                        <a href="#" class="hover:text-white transition"><i class="fa-brands fa-linkedin"></i></a>
                        <a href="#" class="hover:text-white transition"><i class="fa-brands fa-youtube"></i></a>
                    </div>
                    <p class="text-[11px] text-blue-300">© 2026 CampusLearn. All rights reserved.</p>
                </div>

            </div>
        </div>
    </footer>

    <!-- Course Modal -->
    <div id="course-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-2xl w-full overflow-hidden shadow-2xl border border-slate-100 modal-fade-in max-h-[90vh] flex flex-col">
            <div id="modal-header" class="bg-[#1b365d] text-white p-6 relative">
                <button onclick="closeCourseModal()" class="absolute top-4 right-4 text-white/80 hover:text-white text-xl">
                    <i class="fa-solid fa-xmark"></i>
                </button>
                <span id="modal-category" class="bg-blue-500/30 text-blue-200 text-[10px] font-bold uppercase tracking-wider px-2.5 py-1 rounded-full inline-block mb-2">Category</span>
                <h3 id="modal-title" class="text-xl font-bold">Course Title</h3>
                <p id="modal-instructor" class="text-xs text-blue-200 mt-1">Instructor: Dr. Sarah Jenkins</p>
            </div>

            <div class="p-6 overflow-y-auto flex-grow space-y-4 text-xs">
                <div>
                    <h4 class="font-bold text-slate-800 text-sm mb-1">Course Description</h4>
                    <p id="modal-description" class="text-slate-600 leading-relaxed">Detailed overview of course modules and syllabus expectations.</p>
                </div>

                <div class="grid grid-cols-3 gap-3 bg-slate-50 p-3 rounded-lg text-center">
                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase">Duration</span>
                        <strong id="modal-duration" class="text-slate-800 text-xs">6 Weeks</strong>
                    </div>
                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase">Level</span>
                        <strong id="modal-level" class="text-slate-800 text-xs">Beginner</strong>
                    </div>
                    <div>
                        <span class="text-slate-400 block text-[10px] uppercase">Rating</span>
                        <strong id="modal-rating" class="text-amber-600 text-xs"><i class="fa-solid fa-star mr-0.5"></i> 4.9</strong>
                    </div>
                </div>

                <div>
                    <h4 class="font-bold text-slate-800 text-sm mb-2">Syllabus Breakdown</h4>
                    <ul id="modal-syllabus" class="space-y-2 list-disc list-inside text-slate-600">
                        <!-- Dynamic content -->
                    </ul>
                </div>
            </div>

            <div class="p-4 bg-slate-50 border-t border-slate-100 flex items-center justify-between">
                <button onclick="closeCourseModal()" class="px-4 py-2 border border-slate-200 rounded-lg font-medium text-slate-600 hover:bg-slate-100 text-xs">Close</button>
                <button id="modal-enroll-btn" onclick="confirmEnrollment()" class="px-5 py-2 bg-[#1e88e5] hover:bg-blue-600 text-white rounded-lg font-medium text-xs shadow">Enroll Now (Free)</button>
            </div>
        </div>
    </div>

    <!-- Student Forum Modal -->
    <div id="forum-modal" class="fixed inset-0 bg-slate-900/60 backdrop-blur-sm z-50 hidden flex items-center justify-center p-4">
        <div class="bg-white rounded-2xl max-w-2xl w-full overflow-hidden shadow-2xl border border-slate-100 modal-fade-in max-h-[90vh] flex flex-col">
            <div class="bg-[#1b365d] text-white p-6 relative">
                <button onclick="closeForumModal()" class="absolute top-4 right-4 text-white/80 hover:text-white text-xl">
                    <i class="fa-solid fa-xmark"></i>
                </button>
                <span class="bg-blue-500/30 text-blue-200 text-[10px] font-bold uppercase tracking-wider px-2.5 py-1 rounded-full inline-block mb-2">Campus Community</span>
                <h3 class="text-xl font-bold">Student Discussion Forum</h3>
                <p class="text-xs text-blue-200 mt-1">Ask questions, share code solutions, and connect with peers.</p>
            </div>

            <div class="p-6 overflow-y-auto flex-grow space-y-4 text-xs">
                <div class="bg-slate-50 p-4 rounded-xl border border-slate-200">
                    <h4 class="font-bold text-slate-800 mb-2">Start a New Discussion Thread</h4>
                    <form onsubmit="handleForumPost(event)" class="space-y-3">
                        <input id="forum-topic-input" required type="text" placeholder="Topic Title (e.g., Question on Module 2 Logic)" class="w-full px-3 py-2 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs">
                        <textarea id="forum-body-input" required rows="2" placeholder="Write your question or code snippet detail here..." class="w-full px-3 py-2 border border-slate-200 rounded-lg focus:outline-none focus:ring-1 focus:ring-blue-500 text-xs resize-none"></textarea>
                        <button type="submit" class="bg-[#1e88e5] hover:bg-blue-600 text-white font-medium px-4 py-2 rounded-lg transition shadow-sm">
                            Post Discussion <i class="fa-solid fa-paper-plane ml-1"></i>
                        </button>
                    </form>
                </div>

                <div>
                    <h4 class="font-bold text-slate-800 text-sm mb-3">Recent Discussion Threads</h4>
                    <div id="forum-threads-list" class="space-y-3">
                        <div class="p-3 bg-white border border-slate-200 rounded-lg">
                            <div class="flex items-center justify-between mb-1">
                                <span class="font-bold text-blue-600">How to handle recursion stack overflow in Python?</span>
                                <span class="text-[10px] text-slate-400">2 hrs ago</span>
                            </div>
                            <p class="text-slate-600 mb-2">I am trying to solve the Fibonacci module exercise, but running into recursion depth limits...</p>
                            <div class="flex items-center justify-between text-[11px] text-slate-400 border-t border-slate-100 pt-2">
                                <span><i class="fa-solid fa-user text-slate-400 mr-1"></i> Posted by Alex M.</span>
                                <span class="text-blue-600 font-medium"><i class="fa-regular fa-comment mr-1"></i> 4 Replies</span>
                            </div>
                        </div>

                        <div class="p-3 bg-white border border-slate-200 rounded-lg">
                            <div class="flex items-center justify-between mb-1">
                                <span class="font-bold text-blue-600">Tailwind CSS Flexbox alignment query</span>
                                <span class="text-[10px] text-slate-400">1 day ago</span>
                            </div>
                            <p class="text-slate-600 mb-2">What is the cleanest way to center a grid item vertically on mobile screens using Tailwind classes?</p>
                            <div class="flex items-center justify-between text-[11px] text-slate-400 border-t border-slate-100 pt-2">
                                <span><i class="fa-solid fa-user text-slate-400 mr-1"></i> Posted by Sarah K.</span>
                                <span class="text-blue-600 font-medium"><i class="fa-regular fa-comment mr-1"></i> 7 Replies</span>
                            </div>
                        </div>
                    </div>
                </div>
            </div>

            <div class="p-4 bg-slate-50 border-t border-slate-100 flex items-center justify-end">
                <button onclick="closeForumModal()" class="px-4 py-2 border border-slate-200 rounded-lg font-medium text-slate-600 hover:bg-slate-100 text-xs">Close</button>
            </div>
        </div>
    </div>

    <!-- Toast Container -->
    <div id="toast-container" class="fixed bottom-5 right-5 z-50 flex flex-col space-y-2 pointer-events-none"></div>

    <script>
        // Datasets
        const coursesData = [
            {
                id: 1,
                title: "Computer Science Fundamentals",
                category: "CS",
                level: "Beginner",
                rating: 4.9,
                duration: "6 Weeks",
                instructor: "Dr. Sarah Jenkins",
                image: "https://images.unsplash.com/photo-1517694712202-14dd9538aa97?auto=format&fit=crop&w=600&q=80",
                description: "Master algorithms, computer systems, binary logic, data structures, and web technologies in this foundational course.",
                syllabus: [
                    "Week 1: Computer Systems, Memory & CPU Architecture",
                    "Week 2-3: Programming Logic & Python Syntax Foundations",
                    "Week 4-5: Web Development Essentials (HTML & CSS)",
                    "Week 6: Problem Solving & Computational Thinking"
                ]
            },
            {
                id: 2,
                title: "Modern Full-Stack Web Development",
                category: "Web",
                level: "Intermediate",
                rating: 4.8,
                duration: "8 Weeks",
                instructor: "Prof. David Chen",
                image: "https://images.unsplash.com/photo-1555066931-4365d14bab8c?auto=format&fit=crop&w=600&q=80",
                description: "Build modern web applications using HTML5, CSS3, Tailwind, JavaScript ES6+, and API integration.",
                syllabus: [
                    "Week 1-2: Advanced DOM Manipulation & Async JS",
                    "Week 3-4: Responsive Layouts with Tailwind CSS",
                    "Week 5-6: RESTful API Integration & Fetch",
                    "Week 7-8: Capstone Single Page Web App Project"
                ]
            },
            {
                id: 3,
                title: "Python for Data Science & Analytics",
                category: "Data",
                level: "Intermediate",
                rating: 4.9,
                duration: "7 Weeks",
                instructor: "Elena Rostova",
                image: "https://images.unsplash.com/photo-1551288049-bebda4e38f71?auto=format&fit=crop&w=600&q=80",
                description: "Analyze real datasets using Pandas, NumPy, Data Cleaning techniques, and Matplotlib visualization.",
                syllabus: [
                    "Week 1: NumPy Arrays & Vectorized Calculations",
                    "Week 2-3: Data Wrangling with Pandas",
                    "Week 4-5: Data Visualization with Matplotlib & Seaborn",
                    "Week 6-7: Statistical Modeling & Case Studies"
                ]
            },
            {
                id: 4,
                title: "Introduction to Artificial Intelligence",
                category: "AI",
                level: "Advanced",
                rating: 4.7,
                duration: "10 Weeks",
                instructor: "Elena Rostova",
                image: "https://images.unsplash.com/photo-1677442136019-21780ecad995?auto=format&fit=crop&w=600&q=80",
                description: "Explore neural networks, machine learning algorithms, computer vision, and AI ethics.",
                syllabus: [
                    "Week 1-2: Supervised vs Unsupervised Learning",
                    "Week 3-5: Neural Network Architectures",
                    "Week 6-8: Computer Vision & Natural Language Processing",
                    "Week 9-10: AI Ethics & Deployment"
                ]
            },
            {
                id: 5,
                title: "Data Structures & Algorithms",
                category: "CS",
                level: "Intermediate",
                rating: 4.9,
                duration: "8 Weeks",
                instructor: "Dr. Sarah Jenkins",
                image: "https://images.unsplash.com/photo-1516116211223-4c71414a0f60?auto=format&fit=crop&w=600&q=80",
                description: "Deep dive into Arrays, Linked Lists, Trees, Graphs, Sorting algorithms, and Big O notation time complexity.",
                syllabus: [
                    "Week 1-2: Time Complexity & Arrays vs Lists",
                    "Week 3-4: Stacks, Queues, and Hash Tables",
                    "Week 5-6: Binary Trees, BSTs, and Heap Graphs",
                    "Week 7-8: Dynamic Programming & Sorting Algorithms"
                ]
            },
            {
                id: 6,
                title: "UI/UX Design Systems & Figma",
                category: "Web",
                level: "Beginner",
                rating: 4.6,
                duration: "5 Weeks",
                instructor: "Prof. David Chen",
                image: "https://images.unsplash.com/photo-1507238691740-187a5b1d37b8?auto=format&fit=crop&w=600&q=80",
                description: "Learn wireframing, color theory, typography, interactive prototyping, and design tokens in Figma.",
                syllabus: [
                    "Week 1: Fundamentals of User Centered Design",
                    "Week 2: Color Systems & Typography Scales",
                    "Week 3-4: Interactive Component Prototyping in Figma",
                    "Week 5: Usability Testing & Developer Hand-off"
                ]
            }
        ];

        const resourcesData = [
            {
                id: 101,
                title: "CS101 Complete Lecture Notes Summary",
                category: "notes",
                type: "PDF",
                size: "4.2 MB",
                downloads: 1420,
                description: "Handwritten and typed lecture highlights covering modules 1 through 4."
            },
            {
                id: 102,
                title: "HTML5 & Modern CSS3 Flexbox Cheat Sheet",
                category: "cheatsheet",
                type: "PDF",
                size: "1.8 MB",
                downloads: 3890,
                description: "Quick reference guide for CSS grid, flexbox syntax, and essential semantic HTML elements."
            },
            {
                id: 103,
                title: "Python 3 Syntax Quick Reference Guide",
                category: "cheatsheet",
                type: "PDF",
                size: "2.1 MB",
                downloads: 2750,
                description: "Key functions, list comprehensions, dictionary methods, and exception handling snippets."
            },
            {
                id: 104,
                title: "VS Code & Git Developer Starter Pack",
                category: "tools",
                type: "ZIP",
                size: "15.4 MB",
                downloads: 940,
                description: "Pre-configured VS Code settings, extension recommendations, and Git command aliases."
            },
            {
                id: 105,
                title: "Global Student Demographics Dataset",
                category: "datasets",
                type: "CSV",
                size: "8.6 MB",
                downloads: 620,
                description: "Cleaned tabular dataset formatted for Python Pandas data analytics projects."
            },
            {
                id: 106,
                title: "Algorithms & Big O Complexity Chart",
                category: "cheatsheet",
                type: "PDF",
                size: "1.2 MB",
                downloads: 4100,
                description: "Visual time and space complexity chart for searching, sorting, and graph algorithms."
            }
        ];

        let activeCourseCategory = 'all';
        let activeResourceCategory = 'all';
        let currentModalCourseId = null;
        let assignmentCalendarMonth = new Date();
        assignmentCalendarMonth.setDate(1);
        let selectedAssignmentCalendarDate = assignmentCalendarDateKey(new Date());
        let assignmentCalendarEntries = [];
        const assignmentCalendarStorageKey = "campuslearn-assignment-calendar-v1";

        function assignmentCalendarDateKey(date) {
            const year = date.getFullYear();
            const month = String(date.getMonth() + 1).padStart(2, "0");
            const day = String(date.getDate()).padStart(2, "0");
            return year + "-" + month + "-" + day;
        }

        function parseAssignmentCalendarDate(dateKey) {
            const parts = dateKey.split("-").map(Number);
            return new Date(parts[0], parts[1] - 1, parts[2]);
        }

        function persistAssignmentCalendar() {
            try {
                window.localStorage.setItem(assignmentCalendarStorageKey, JSON.stringify(assignmentCalendarEntries));
                return true;
            } catch (error) {
                return false;
            }
        }

        function renderAssignmentCalendar() {
            const monthLabel = document.getElementById("assignment-calendar-month");
            const grid = document.getElementById("assignment-calendar-grid");
            const selectedLabel = document.getElementById("assignment-calendar-selected-date");
            const dayList = document.getElementById("assignment-calendar-day-list");
            const dateInput = document.getElementById("assignment-calendar-date");
            if (!monthLabel || !grid || !selectedLabel || !dayList) return;

            const year = assignmentCalendarMonth.getFullYear();
            const month = assignmentCalendarMonth.getMonth();
            monthLabel.textContent = assignmentCalendarMonth.toLocaleDateString(undefined, { month: "long", year: "numeric" });
            grid.setAttribute("aria-label", "Assignment due dates for " + monthLabel.textContent);
            grid.replaceChildren();

            const firstWeekday = new Date(year, month, 1).getDay();
            const daysInMonth = new Date(year, month + 1, 0).getDate();
            for (let blank = 0; blank < firstWeekday; blank += 1) {
                const emptyCell = document.createElement("div");
                emptyCell.setAttribute("aria-hidden", "true");
                grid.appendChild(emptyCell);
            }

            const todayKey = assignmentCalendarDateKey(new Date());
            for (let day = 1; day <= daysInMonth; day += 1) {
                const date = new Date(year, month, day);
                const dateKey = assignmentCalendarDateKey(date);
                const dueEntries = assignmentCalendarEntries.filter(entry => entry.dueDate === dateKey);
                const isSelected = dateKey === selectedAssignmentCalendarDate;
                const isToday = dateKey === todayKey;
                const dayButton = document.createElement("button");
                dayButton.type = "button";
                dayButton.setAttribute("role", "gridcell");
                dayButton.setAttribute("aria-pressed", String(isSelected));
                dayButton.setAttribute("aria-label", date.toLocaleDateString(undefined, { weekday: "long", month: "long", day: "numeric", year: "numeric" }) + (dueEntries.length ? ", " + dueEntries.length + " assignment" + (dueEntries.length === 1 ? "" : "s") + " due" : ""));
                dayButton.className = "min-h-[3.5rem] rounded-lg border p-1.5 text-left text-xs transition hover:border-blue-300 focus:outline-none focus:ring-2 focus:ring-blue-200 sm:min-h-[4.2rem] " + (isSelected ? "border-blue-500 bg-blue-50" : isToday ? "border-blue-200 bg-white" : "border-slate-100 bg-white");

                const dayNumber = document.createElement("span");
                dayNumber.className = "font-semibold " + (isToday ? "text-blue-700" : "text-slate-700");
                dayNumber.textContent = String(day);
                dayButton.appendChild(dayNumber);
                if (dueEntries.length > 0) {
                    const marker = document.createElement("span");
                    marker.className = "mt-1 block truncate rounded bg-blue-100 px-1 py-0.5 text-[9px] font-semibold text-blue-700";
                    marker.textContent = dueEntries.length + " due";
                    dayButton.appendChild(marker);
                }
                dayButton.onclick = () => {
                    selectedAssignmentCalendarDate = dateKey;
                    renderAssignmentCalendar();
                };
                grid.appendChild(dayButton);
            }

            const selectedDate = parseAssignmentCalendarDate(selectedAssignmentCalendarDate);
            selectedLabel.textContent = "Due on " + selectedDate.toLocaleDateString(undefined, { weekday: "long", month: "long", day: "numeric" });
            if (dateInput) dateInput.value = selectedAssignmentCalendarDate;
            dayList.replaceChildren();
            const selectedEntries = assignmentCalendarEntries.filter(entry => entry.dueDate === selectedAssignmentCalendarDate);
            if (selectedEntries.length === 0) {
                const emptyMessage = document.createElement("p");
                emptyMessage.className = "text-[11px] leading-5 text-slate-500";
                emptyMessage.textContent = "Nothing due this day. Add an assignment below.";
                dayList.appendChild(emptyMessage);
            } else {
                selectedEntries.forEach(entry => {
                    const row = document.createElement("div");
                    row.className = "flex items-start justify-between gap-2 rounded-lg border border-blue-100 bg-white p-2.5";
                    const name = document.createElement("p");
                    name.className = "text-[11px] font-semibold leading-4 text-slate-700";
                    name.textContent = entry.title;
                    const removeButton = document.createElement("button");
                    removeButton.type = "button";
                    removeButton.className = "shrink-0 text-[10px] font-semibold text-rose-600 hover:text-rose-800";
                    removeButton.textContent = "Remove";
                    removeButton.setAttribute("aria-label", "Remove " + entry.title + " from the calendar");
                    removeButton.onclick = () => removeAssignmentCalendarEntry(entry.id);
                    row.append(name, removeButton);
                    dayList.appendChild(row);
                });
            }
            renderUpcomingAssignmentDeadlines();
        }

        function renderUpcomingAssignmentDeadlines() {
            const list = document.getElementById("upcoming-assignment-due-list");
            if (!list) return;
            list.replaceChildren();
            const todayKey = assignmentCalendarDateKey(new Date());
            const upcoming = assignmentCalendarEntries
                .filter(entry => entry.dueDate >= todayKey)
                .sort((a, b) => a.dueDate.localeCompare(b.dueDate))
                .slice(0, 4);

            if (upcoming.length === 0) {
                const emptyMessage = document.createElement("p");
                emptyMessage.className = "text-slate-500 text-[11px]";
                emptyMessage.textContent = "No assignment deadlines scheduled.";
                list.appendChild(emptyMessage);
                return;
            }
            upcoming.forEach(entry => {
                const row = document.createElement("div");
                row.className = "flex items-start gap-2";
                const icon = document.createElement("i");
                icon.className = "fa-solid fa-caret-right mt-0.5 text-blue-500";
                icon.setAttribute("aria-hidden", "true");
                const details = document.createElement("div");
                const title = document.createElement("p");
                title.className = "font-bold text-slate-800";
                title.textContent = entry.title;
                const date = document.createElement("p");
                date.className = "text-slate-400 text-[11px]";
                date.textContent = parseAssignmentCalendarDate(entry.dueDate).toLocaleDateString(undefined, { month: "short", day: "numeric", year: "numeric" });
                details.append(title, date);
                row.append(icon, details);
                list.appendChild(row);
            });
        }

        function changeAssignmentCalendarMonth(offset) {
            assignmentCalendarMonth = new Date(assignmentCalendarMonth.getFullYear(), assignmentCalendarMonth.getMonth() + offset, 1);
            selectedAssignmentCalendarDate = assignmentCalendarDateKey(assignmentCalendarMonth);
            renderAssignmentCalendar();
        }

        function goToAssignmentCalendarToday() {
            const today = new Date();
            assignmentCalendarMonth = new Date(today.getFullYear(), today.getMonth(), 1);
            selectedAssignmentCalendarDate = assignmentCalendarDateKey(today);
            renderAssignmentCalendar();
        }

        function addAssignmentCalendarEntry(event) {
            event.preventDefault();
            const titleInput = document.getElementById("assignment-calendar-title");
            const dateInput = document.getElementById("assignment-calendar-date");
            const status = document.getElementById("assignment-calendar-status");
            const title = titleInput ? titleInput.value.trim() : "";
            const dueDate = dateInput ? dateInput.value : "";
            if (!title || !/^\d{4}-\d{2}-\d{2}$/.test(dueDate)) return;

            assignmentCalendarEntries.push({
                id: "deadline-" + Date.now() + "-" + Math.random().toString(36).slice(2, 8),
                title,
                dueDate
            });
            const wasSaved = persistAssignmentCalendar();
            const selectedDate = parseAssignmentCalendarDate(dueDate);
            assignmentCalendarMonth = new Date(selectedDate.getFullYear(), selectedDate.getMonth(), 1);
            selectedAssignmentCalendarDate = dueDate;
            if (titleInput) titleInput.value = "";
            if (status) status.textContent = wasSaved ? "Deadline added and saved in this browser." : "Deadline added for this visit, but this browser could not save it.";
            renderAssignmentCalendar();
            if (wasSaved) showToast("Assignment deadline added to the calendar.");
        }

        function removeAssignmentCalendarEntry(entryId) {
            assignmentCalendarEntries = assignmentCalendarEntries.filter(entry => entry.id !== entryId);
            persistAssignmentCalendar();
            renderAssignmentCalendar();
        }

        function restoreAssignmentCalendar() {
            try {
                const savedEntries = window.localStorage.getItem(assignmentCalendarStorageKey);
                if (savedEntries) {
                    const parsedEntries = JSON.parse(savedEntries);
                    assignmentCalendarEntries = Array.isArray(parsedEntries) ? parsedEntries.filter(entry => entry && typeof entry.id === "string" && typeof entry.title === "string" && typeof entry.dueDate === "string" && /^\d{4}-\d{2}-\d{2}$/.test(entry.dueDate)) : [];
                }
            } catch (error) {
                assignmentCalendarEntries = [];
            }
            selectedAssignmentCalendarDate = assignmentCalendarDateKey(new Date());
            renderAssignmentCalendar();
        }
        const featuredCourseIds = [1, 2, 3];
        let featuredCourseIndex = 0;

        function renderFeaturedCourse(index) {
            const featuredCourses = featuredCourseIds.map(id => coursesData.find(course => course.id === id)).filter(Boolean);
            if (featuredCourses.length === 0) return;

            featuredCourseIndex = (index + featuredCourses.length) % featuredCourses.length;
            const course = featuredCourses[featuredCourseIndex];
            const image = document.getElementById("featured-course-image");
            const title = document.getElementById("featured-course-title");
            const description = document.getElementById("featured-course-description");
            const meta = document.getElementById("featured-course-meta");
            const cta = document.getElementById("featured-course-cta");
            const dots = document.getElementById("featured-course-dots");
            const count = document.getElementById("featured-course-count");

            image.onerror = function () {
                this.onerror = null;
                this.src = "https://placehold.co/1200x600/1b365d/ffffff?text=Featured+Course";
            };
            image.src = course.image.replace("w=600", "w=1200");
            image.alt = course.title;
            title.textContent = course.title;
            description.textContent = course.description;
            meta.textContent = course.level + " · " + course.duration + " · " + course.rating + " ★";
            cta.onclick = () => openCourseModal(course.id);
            count.textContent = "Slide " + (featuredCourseIndex + 1) + " of " + featuredCourses.length;

            dots.replaceChildren();
            featuredCourses.forEach((featuredCourse, dotIndex) => {
                const dot = document.createElement("button");
                const isCurrent = dotIndex === featuredCourseIndex;
                dot.type = "button";
                dot.className = "h-2.5 w-2.5 rounded-full transition focus:outline-none focus:ring-2 focus:ring-white " + (isCurrent ? "bg-white" : "bg-white/50 hover:bg-white/80");
                dot.setAttribute("aria-label", "Show featured course " + (dotIndex + 1) + ": " + featuredCourse.title);
                dot.setAttribute("aria-pressed", String(isCurrent));
                dot.onclick = () => renderFeaturedCourse(dotIndex);
                dots.appendChild(dot);
            });
        }

        function changeFeaturedCourse(direction) {
            renderFeaturedCourse(featuredCourseIndex + direction);
        }

        // Activity-based Module Progress
        function toggleModuleActivities(moduleId) {
            const panel = document.getElementById("module" + moduleId + "-activities-row");
            const button = document.getElementById("module" + moduleId + "-activity-toggle");
            const label = document.getElementById("module" + moduleId + "-toggle-label");
            if (!panel || !button || !label) return;

            const isOpening = panel.classList.contains("hidden");
            panel.classList.toggle("hidden", !isOpening);
            button.setAttribute("aria-expanded", String(isOpening));
            label.textContent = isOpening ? "Hide activities" : "View activities";
        }

        function toggleAssignmentDetails(assignmentId) {
            const selectedPanel = document.getElementById("assignment-details-" + assignmentId);
            if (!selectedPanel) return;

            const isOpening = selectedPanel.classList.contains("hidden");
            document.querySelectorAll('[id^="assignment-details-"]').forEach(panel => {
                panel.classList.add("hidden");
                const button = document.getElementById(panel.id.replace("assignment-details-", "assignment-button-"));
                if (button) {
                    button.setAttribute("aria-expanded", "false");
                    const label = button.querySelector("[data-assignment-label]");
                    if (label) label.textContent = "Open assignment";
                }
            });

            if (isOpening) {
                selectedPanel.classList.remove("hidden");
                const button = document.getElementById("assignment-button-" + assignmentId);
                if (button) {
                    button.setAttribute("aria-expanded", "true");
                    const label = button.querySelector("[data-assignment-label]");
                    if (label) label.textContent = "Close assignment";
                }
            }
        }
        function showAssignmentFileName(assignmentId) {
            const input = document.getElementById("assignment-file-" + assignmentId);
            const label = document.getElementById("assignment-file-name-" + assignmentId);
            if (!input || !label) return;
            label.textContent = input.files && input.files[0] ? "Selected: " + input.files[0].name : "No file selected";
        }

        function saveAssignmentDraft(event, assignmentId) {
            event.preventDefault();
            const responseInput = document.getElementById("assignment-response-" + assignmentId);
            const fileInput = document.getElementById("assignment-file-" + assignmentId);
            const status = document.getElementById("assignment-submit-status-" + assignmentId);
            if (!responseInput || !fileInput || !status) return;

            const response = responseInput.value.trim();
            const fileName = fileInput.files && fileInput.files[0] ? fileInput.files[0].name : "";
            if (!response && !fileName) {
                status.textContent = "Write a response or attach a file before saving.";
                status.classList.remove("text-emerald-700");
                status.classList.add("text-rose-700");
                return;
            }

            try {
                window.localStorage.setItem("campuslearn-assignment-draft-" + assignmentId, JSON.stringify({ response, fileName }));
                status.textContent = fileName ? "Draft saved in this browser. Reattach the selected file if you reopen this page." : "Draft saved in this browser.";
                status.classList.remove("text-rose-700");
                status.classList.add("text-emerald-700");
                showToast("Assignment draft saved in this browser.");
            } catch (error) {
                status.textContent = "This browser could not save the draft. Copy your response somewhere safe.";
                status.classList.remove("text-emerald-700");
                status.classList.add("text-rose-700");
            }
        }

        function restoreAssignmentDrafts() {
            [1, 2].forEach(assignmentId => {
                try {
                    const savedDraft = window.localStorage.getItem("campuslearn-assignment-draft-" + assignmentId);
                    if (!savedDraft) return;
                    const draft = JSON.parse(savedDraft);
                    const responseInput = document.getElementById("assignment-response-" + assignmentId);
                    const status = document.getElementById("assignment-submit-status-" + assignmentId);
                    if (responseInput && typeof draft.response === "string") responseInput.value = draft.response;
                    if (status) {
                        status.textContent = draft.fileName ? "Saved response restored. Reattach this file to include it: " + draft.fileName : "Saved draft restored from this browser.";
                    }
                } catch (error) {
                    // Leave the submission fields available if saved browser data is unavailable.
                }
            });
        }
        function openModuleActivities(moduleId) {
            const panel = document.getElementById("module" + moduleId + "-activities-row");
            if (panel && panel.classList.contains("hidden")) {
                toggleModuleActivities(moduleId);
            }
            scrollToSection("modules-section");
        }
        function updateLearningSnapshot() {
            const steps = Array.from(document.querySelectorAll(".module-step-checkbox"));
            const total = steps.length;
            const completed = steps.filter(step => step.checked).length;
            const progress = total ? Math.round((completed / total) * 100) : 0;
            const bar = document.getElementById("learning-progress-bar");
            const track = document.getElementById("learning-progress-track");
            const copy = document.getElementById("learning-progress-copy");
            const nextStepLabel = document.getElementById("learning-next-step");
            const nextStep = steps.find(step => !step.checked);

            if (bar) bar.style.width = progress + "%";
            if (track) track.setAttribute("aria-valuenow", String(progress));
            if (copy) copy.textContent = completed + " of " + total + " steps complete";
            if (nextStepLabel) {
                const activity = nextStep ? nextStep.parentElement.textContent.replace(/\s+/g, " ").trim() : "Every activity is complete. Great work!";
                nextStepLabel.textContent = nextStep ? "Next up: " + activity : activity;
            }
        }

        function resumeLearning() {
            const nextStep = document.querySelector(".module-step-checkbox:not(:checked)");
            const moduleId = nextStep ? Number(nextStep.dataset.module) : 1;
            openModuleActivities(moduleId);
            if (nextStep) {
                window.setTimeout(() => nextStep.scrollIntoView({ behavior: "smooth", block: "center" }), 180);
            }
        }

        function dailyQuizStorageKey() {
            const quizDate = new Date();
            const year = quizDate.getFullYear();
            const month = String(quizDate.getMonth() + 1).padStart(2, "0");
            const day = String(quizDate.getDate()).padStart(2, "0");
            return "campuslearn-daily-quiz-v1-" + year + "-" + month + "-" + day;
        }
        const dailyQuizQuestions = [
            { prompt: "What does HTML stand for?", options: ["HyperText Markup Language", "High Transfer Machine Language", "Home Tool Markup Language"], answerIndex: 0, explanation: "HTML gives web pages their structure." },
            { prompt: "Which HTML tag is used for a paragraph?", options: ["<p>", "<img>", "<table>"], answerIndex: 0, explanation: "The <p> element marks a paragraph." },
            { prompt: "What is CSS mainly used for?", options: ["Styling and laying out web pages", "Storing data in a database", "Sending email"], answerIndex: 0, explanation: "CSS controls how page content looks and is arranged." },
            { prompt: "Which JavaScript keyword declares a variable you can reassign?", options: ["const", "let", "return"], answerIndex: 1, explanation: "A variable declared with let can be reassigned." },
            { prompt: "What does 2 + 3 * 2 evaluate to in JavaScript?", options: ["10", "8", "12"], answerIndex: 1, explanation: "Multiplication happens before addition, so the result is 8." }
        ];
        let dailyQuizIndex = 0;
        let dailyQuizScore = 0;
        let dailyQuizAnswered = false;

        function renderDailyQuizQuestion() {
            const question = dailyQuizQuestions[dailyQuizIndex];
            const title = document.getElementById("daily-quiz-title");
            const count = document.getElementById("daily-quiz-count");
            const prompt = document.getElementById("daily-quiz-prompt");
            const options = document.getElementById("daily-quiz-options");
            const feedback = document.getElementById("daily-quiz-feedback");
            const button = document.getElementById("daily-quiz-submit");
            const retryButton = document.getElementById("daily-quiz-retry");
            const points = document.getElementById("daily-quiz-points");

            title.textContent = "Quick check";
            count.textContent = "Question " + (dailyQuizIndex + 1) + " of " + dailyQuizQuestions.length;
            prompt.textContent = question.prompt;
            options.replaceChildren();
            question.options.forEach((answer, index) => {
                const label = document.createElement("label");
                label.className = "flex cursor-pointer items-start gap-2 rounded-lg border border-slate-200 bg-white px-3 py-2 text-xs text-slate-700 transition hover:border-blue-300";
                const input = document.createElement("input");
                input.type = "radio";
                input.name = "daily-quiz-answer";
                input.value = String(index);
                input.className = "daily-quiz-answer mt-0.5 accent-blue-600";
                const answerText = document.createElement("span");
                answerText.textContent = answer;
                label.append(input, answerText);
                options.appendChild(label);
            });
            feedback.textContent = "Correct: +10 XP · Incorrect: -5 XP.";
            feedback.className = "text-[11px] font-medium text-slate-500";
            button.disabled = false;
            button.textContent = "Check answer";
            button.onclick = checkDailyQuiz;
            button.classList.remove("cursor-default", "opacity-70");
            retryButton.classList.add("hidden");
            points.textContent = dailyQuizScore + " XP";
            dailyQuizAnswered = false;
        }

        function checkDailyQuiz() {
            if (dailyQuizAnswered) return;
            const selected = document.querySelector(".daily-quiz-answer:checked");
            const feedback = document.getElementById("daily-quiz-feedback");
            if (!selected) {
                feedback.textContent = "Choose an answer first.";
                feedback.className = "text-[11px] font-semibold text-amber-700";
                return;
            }
            const question = dailyQuizQuestions[dailyQuizIndex];
            if (Number(selected.value) !== question.answerIndex) {
                dailyQuizScore -= 5;
                document.getElementById("daily-quiz-points").textContent = dailyQuizScore + " XP";
                feedback.textContent = "Incorrect. -5 XP. Correct answer: " + question.options[question.answerIndex] + ". " + question.explanation;
                feedback.className = "text-[11px] font-semibold text-rose-700";
                return;
            }
            dailyQuizAnswered = true;
            dailyQuizScore += 10;
            document.querySelectorAll(".daily-quiz-answer").forEach(input => { input.disabled = true; });
            document.getElementById("daily-quiz-points").textContent = dailyQuizScore + " XP";
            feedback.textContent = "Correct! " + question.explanation + " +10 XP.";
            feedback.className = "text-[11px] font-semibold text-emerald-700";
            const button = document.getElementById("daily-quiz-submit");
            if (dailyQuizIndex === dailyQuizQuestions.length - 1) {
                button.textContent = "Finish quiz";
                button.onclick = completeDailyQuiz;
            } else {
                button.textContent = "Next question";
                button.onclick = nextDailyQuizQuestion;
            }
        }

        function nextDailyQuizQuestion() {
            if (!dailyQuizAnswered || dailyQuizIndex >= dailyQuizQuestions.length - 1) return;
            dailyQuizIndex += 1;
            renderDailyQuizQuestion();
        }

        function completeDailyQuiz() {
            try { window.localStorage.setItem(dailyQuizStorageKey(), String(dailyQuizScore)); } catch (error) { /* Keep the result for this visit. */ }
            const title = document.getElementById("daily-quiz-title");
            const count = document.getElementById("daily-quiz-count");
            const prompt = document.getElementById("daily-quiz-prompt");
            const options = document.getElementById("daily-quiz-options");
            const feedback = document.getElementById("daily-quiz-feedback");
            const button = document.getElementById("daily-quiz-submit");
            const retryButton = document.getElementById("daily-quiz-retry");
            const points = document.getElementById("daily-quiz-points");
            title.textContent = "Quiz complete!";
            count.textContent = dailyQuizQuestions.length + " of " + dailyQuizQuestions.length + " complete";
            prompt.textContent = "You completed today's web fundamentals quiz.";
            options.replaceChildren();
            feedback.textContent = "Quiz complete! Final score: " + dailyQuizScore + " XP. Come back tomorrow for a new quiz.";
            feedback.className = "text-[11px] font-semibold text-emerald-700";
            button.disabled = true;
            button.textContent = "Completed today";
            button.onclick = null;
            button.classList.add("cursor-default", "opacity-70");
            retryButton.classList.remove("hidden");
            points.textContent = dailyQuizScore + " XP";
            dailyQuizAnswered = true;
        }

        function retryDailyQuiz() {
            try { window.localStorage.removeItem(dailyQuizStorageKey()); } catch (error) { /* Restart for this visit. */ }
            dailyQuizIndex = 0;
            dailyQuizScore = 0;
            dailyQuizAnswered = false;
            renderDailyQuizQuestion();
        }
        function restoreDailyQuiz() {
            try {
                const savedScore = window.localStorage.getItem(dailyQuizStorageKey());
                if (savedScore !== null) {
                    dailyQuizScore = savedScore === "complete" ? 50 : Number(savedScore);
                    if (!Number.isFinite(dailyQuizScore)) dailyQuizScore = 50;
                    completeDailyQuiz();
                    return;
                }
            } catch (error) {
                // The quiz still works for this visit if local storage is unavailable.
            }
            renderDailyQuizQuestion();
        }
        function updateModuleProgressFromSteps(moduleId, saveProgress = true) {
            const steps = Array.from(document.querySelectorAll('.module-step-checkbox[data-module="' + moduleId + '"]'));
            const total = steps.length;
            if (total === 0) return;

            const completed = steps.filter(step => step.checked).length;
            const progress = Math.round((completed / total) * 100);
            const isComplete = completed === total;
            const isNotStarted = completed === 0;
            const progressBar = document.getElementById("module" + moduleId + "-bar");
            const progressTrack = document.getElementById("module" + moduleId + "-progress-track");
            const percentText = document.getElementById("module" + moduleId + "-percent");
            const badge = document.getElementById("module" + moduleId + "-badge");
            const stepSummary = document.getElementById("module" + moduleId + "-step-summary");
            const activitySummary = document.getElementById("module" + moduleId + "-activity-summary");

            if (!progressBar || !progressTrack || !percentText || !badge) return;

            const progressColor = isComplete ? "bg-emerald-500" : isNotStarted ? "bg-slate-300" : "bg-amber-500";
            progressBar.style.width = progress + "%";
            progressBar.classList.remove("bg-emerald-500", "bg-amber-500", "bg-slate-300");
            progressBar.classList.add(progressColor);
            progressTrack.setAttribute("aria-valuenow", String(progress));

            percentText.textContent = progress + "%";
            percentText.classList.remove("text-emerald-600", "text-amber-600", "text-slate-400");
            percentText.classList.add(isComplete ? "text-emerald-600" : isNotStarted ? "text-slate-400" : "text-amber-600");

            badge.textContent = isComplete ? "Completed" : isNotStarted ? "Not Started" : "In Progress";
            badge.className = "status-badge " + (isComplete ? "status-completed" : isNotStarted ? "status-not-started" : "status-in-progress");

            if (stepSummary) stepSummary.textContent = completed + "/" + total + " steps";
            if (activitySummary) activitySummary.textContent = completed + " of " + total + " complete";

            if (saveProgress) {
                try {
                    window.localStorage.setItem("campuslearn-module-" + moduleId, JSON.stringify(steps.map(step => step.checked)));
                } catch (error) {
                    // Progress still updates for this visit if local storage is unavailable.
                }
            }
            updateLearningSnapshot();
        }

        function restoreModuleProgress() {
            [1, 2, 3, 4].forEach(moduleId => {
                const steps = Array.from(document.querySelectorAll('.module-step-checkbox[data-module="' + moduleId + '"]'));
                try {
                    const savedSteps = window.localStorage.getItem("campuslearn-module-" + moduleId);
                    const checkedStates = savedSteps ? JSON.parse(savedSteps) : null;
                    if (Array.isArray(checkedStates) && checkedStates.length === steps.length) {
                        steps.forEach((step, index) => {
                            step.checked = checkedStates[index] === true;
                        });
                    }
                } catch (error) {
                    // Use the page defaults if saved progress is unavailable.
                }
                updateModuleProgressFromSteps(moduleId, false);
            });
        }
        // Navigation Controller
        function navigateTo(pageId) {
            document.querySelectorAll('.page-section').forEach(page => page.classList.add('hidden'));
            
            const target = document.getElementById('page-' + pageId);
            if (target) {
                target.classList.remove('hidden');
                window.scrollTo({ top: 0, behavior: 'smooth' });
            }

            const navIds = ['home', 'about', 'courses', 'resources', 'contact'];
            navIds.forEach(id => {
                const navBtn = document.getElementById('nav-' + id);
                if (navBtn) {
                    if (id === pageId) {
                        navBtn.classList.add('nav-link-active');
                    } else {
                        navBtn.classList.remove('nav-link-active');
                    }
                }
            });

            document.getElementById('mobile-menu').classList.add('hidden');
        }

        function scrollToSection(sectionId) {
            navigateTo('home');
            setTimeout(() => {
                const el = document.getElementById(sectionId);
                if (el) el.scrollIntoView({ behavior: 'smooth' });
            }, 100);
        }

        // Quick Search
        function toggleQuickSearch() {
            const bar = document.getElementById('quick-search-bar');
            bar.classList.toggle('hidden');
            if (!bar.classList.contains('hidden')) {
                document.getElementById('global-search-input').focus();
            }
        }

        function handleGlobalSearch(e) {
            if (e.key === 'Enter') {
                const query = e.target.value.toLowerCase().trim();
                if (!query) return;
                toggleQuickSearch();
                navigateTo('courses');
                document.getElementById('course-search-input').value = query;
                filterCourses();
                showToast(`Search applied for: "${query}"`);
            }
        }

        // Courses Catalog
        function renderCourses(list) {
            const container = document.getElementById('course-grid');
            if (!container) return;

            if (list.length === 0) {
                container.innerHTML = `
                    <div class="col-span-full bg-white p-8 rounded-xl text-center border border-slate-200">
                        <i class="fa-solid fa-circle-exclamation text-slate-300 text-4xl mb-3"></i>
                        <p class="text-slate-600 font-semibold text-sm">No courses matching your criteria were found.</p>
                        <button onclick="resetCourseFilters()" class="mt-3 text-xs text-blue-600 hover:underline">Reset Filters</button>
                    </div>
                `;
                return;
            }

            container.innerHTML = list.map(c => `
                <div class="bg-white rounded-xl overflow-hidden border border-slate-200 shadow-sm hover:shadow-md transition flex flex-col justify-between">
                    <div>
                        <div class="relative h-40 overflow-hidden">
                            <img src="${c.image}" alt="${c.title}" class="w-full h-full object-cover">
                            <span class="absolute top-3 left-3 bg-[#1b365d] text-white text-[10px] font-bold uppercase tracking-wider px-2.5 py-1 rounded-md shadow">
                                ${c.level}
                            </span>
                        </div>
                        <div class="p-5">
                            <div class="flex items-center justify-between text-xs text-slate-400 mb-2">
                                <span><i class="fa-regular fa-clock mr-1"></i> ${c.duration}</span>
                                <span class="text-amber-500 font-semibold"><i class="fa-solid fa-star text-xs"></i> ${c.rating}</span>
                            </div>
                            <h3 class="font-bold text-slate-800 text-base mb-1 leading-snug">${c.title}</h3>
                            <p class="text-xs text-blue-600 font-medium mb-3">${c.instructor}</p>
                            <p class="text-xs text-slate-500 line-clamp-2 leading-relaxed mb-4">${c.description}</p>
                        </div>
                    </div>
                    <div class="px-5 pb-5 pt-0 flex items-center justify-between gap-2">
                        <button onclick="openCourseModal(${c.id})" class="w-full bg-slate-100 hover:bg-slate-200 text-slate-700 text-xs font-semibold py-2 rounded-lg transition">
                            View Syllabus
                        </button>
                        <button onclick="quickEnroll(${c.id})" class="w-full bg-[#1e88e5] hover:bg-blue-600 text-white text-xs font-semibold py-2 rounded-lg transition shadow-sm">
                            Enroll Now
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function filterCourses() {
            const query = (document.getElementById('course-search-input')?.value || '').toLowerCase();
            const level = document.getElementById('course-level-filter')?.value || 'all';

            const filtered = coursesData.filter(c => {
                const matchesCat = activeCourseCategory === 'all' || c.category === activeCourseCategory;
                const matchesSearch = c.title.toLowerCase().includes(query) || c.description.toLowerCase().includes(query);
                const matchesLevel = level === 'all' || c.level === level;
                return matchesCat && matchesSearch && matchesLevel;
            });

            renderCourses(filtered);
        }

        function setCourseCategory(cat, btn) {
            activeCourseCategory = cat;
            document.querySelectorAll('.course-cat-btn').forEach(b => {
                b.classList.remove('bg-[#1b365d]', 'text-white');
                b.classList.add('bg-slate-100', 'text-slate-600');
            });
            btn.classList.remove('bg-slate-100', 'text-slate-600');
            btn.classList.add('bg-[#1b365d]', 'text-white');
            filterCourses();
        }

        function resetCourseFilters() {
            activeCourseCategory = 'all';
            if (document.getElementById('course-search-input')) document.getElementById('course-search-input').value = '';
            if (document.getElementById('course-level-filter')) document.getElementById('course-level-filter').value = 'all';
            filterCourses();
        }

        // Resources Catalog
        function renderResources(list) {
            const container = document.getElementById('resource-grid');
            if (!container) return;

            if (list.length === 0) {
                container.innerHTML = `
                    <div class="col-span-full bg-white p-8 rounded-xl text-center border border-slate-200">
                        <i class="fa-solid fa-folder-open text-slate-300 text-4xl mb-3"></i>
                        <p class="text-slate-600 font-semibold text-sm">No resources found.</p>
                    </div>
                `;
                return;
            }

            container.innerHTML = list.map(r => `
                <div class="bg-white rounded-xl p-5 border border-slate-200 shadow-sm hover:shadow-md transition flex flex-col justify-between">
                    <div>
                        <div class="flex items-center justify-between mb-3">
                            <span class="px-2.5 py-1 bg-blue-50 text-blue-600 rounded-md font-bold text-[10px] uppercase">
                                ${r.type}
                            </span>
                            <span class="text-[11px] text-slate-400">
                                <i class="fa-solid fa-download text-[10px] mr-1"></i> ${r.downloads} downloads
                            </span>
                        </div>
                        <h3 class="font-bold text-slate-800 text-sm mb-2 leading-snug">${r.title}</h3>
                        <p class="text-xs text-slate-500 leading-relaxed mb-4">${r.description}</p>
                    </div>
                    <div class="pt-3 border-t border-slate-100 flex items-center justify-between text-xs">
                        <span class="text-slate-400 font-medium text-[11px]">${r.size}</span>
                        <button onclick="downloadResource('${r.title}')" class="bg-[#1b365d] hover:bg-blue-900 text-white px-3.5 py-1.5 rounded-lg font-medium transition shadow-sm flex items-center">
                            Download <i class="fa-solid fa-arrow-down ml-1.5 text-[10px]"></i>
                        </button>
                    </div>
                </div>
            `).join('');
        }

        function filterResources() {
            const query = (document.getElementById('resource-search-input')?.value || '').toLowerCase();
            const filtered = resourcesData.filter(r => {
                const matchesCat = activeResourceCategory === 'all' || r.category === activeResourceCategory;
                const matchesSearch = r.title.toLowerCase().includes(query) || r.description.toLowerCase().includes(query);
                return matchesCat && matchesSearch;
            });
            renderResources(filtered);
        }

        function setResourceCategory(cat, btn) {
            activeResourceCategory = cat;
            document.querySelectorAll('.res-cat-btn').forEach(b => {
                b.classList.remove('bg-[#1b365d]', 'text-white');
                b.classList.add('bg-slate-100', 'text-slate-600');
            });
            btn.classList.remove('bg-slate-100', 'text-slate-600');
            btn.classList.add('bg-[#1b365d]', 'text-white');
            filterResources();
        }

        function downloadResource(name) {
            showToast(`Started downloading: "${name}"`);
        }

        // Course Modal
        function openCourseModal(id) {
            const course = coursesData.find(c => c.id === id);
            if (!course) return;

            currentModalCourseId = id;
            document.getElementById('modal-category').innerText = course.category;
            document.getElementById('modal-title').innerText = course.title;
            document.getElementById('modal-instructor').innerText = "Instructor: " + course.instructor;
            document.getElementById('modal-description').innerText = course.description;
            document.getElementById('modal-duration').innerText = course.duration;
            document.getElementById('modal-level').innerText = course.level;
            document.getElementById('modal-rating').innerHTML = `<i class="fa-solid fa-star mr-0.5"></i> ${course.rating}`;

            const syllabusList = document.getElementById('modal-syllabus');
            syllabusList.innerHTML = course.syllabus.map(s => `<li class="py-0.5">${s}</li>`).join('');

            document.getElementById('course-modal').classList.remove('hidden');
        }

        function closeCourseModal() {
            document.getElementById('course-modal').classList.add('hidden');
        }

        function confirmEnrollment() {
            closeCourseModal();
            const course = coursesData.find(c => c.id === currentModalCourseId);
            showToast(`Success! You have enrolled in "${course ? course.title : 'the course'}".`);
        }

        function quickEnroll(id) {
            const course = coursesData.find(c => c.id === id);
            showToast(`Success! Enrolled in "${course ? course.title : 'Course'}".`);
        }

        // Forum Modal Handlers
        function openForumModal() {
            document.getElementById('forum-modal').classList.remove('hidden');
        }

        function closeForumModal() {
            document.getElementById('forum-modal').classList.add('hidden');
        }

        function handleForumPost(e) {
            e.preventDefault();
            const titleInput = document.getElementById('forum-topic-input');
            const bodyInput = document.getElementById('forum-body-input');
            const title = titleInput.value;
            const body = bodyInput.value;

            const list = document.getElementById('forum-threads-list');
            const newThread = document.createElement('div');
            newThread.className = 'p-3 bg-white border border-slate-200 rounded-lg';
            newThread.innerHTML = `
                <div class="flex items-center justify-between mb-1">
                    <span class="font-bold text-blue-600">${title}</span>
                    <span class="text-[10px] text-slate-400">Just now</span>
                </div>
                <p class="text-slate-600 mb-2">${body}</p>
                <div class="flex items-center justify-between text-[11px] text-slate-400 border-t border-slate-100 pt-2">
                    <span><i class="fa-solid fa-user text-slate-400 mr-1"></i> Posted by You</span>
                    <span class="text-blue-600 font-medium"><i class="fa-regular fa-comment mr-1"></i> 0 Replies</span>
                </div>
            `;

            list.prepend(newThread);
            e.target.reset();
            showToast("Your forum thread has been posted!");
        }

        // Forms

        function handleQuickContact(e) {
            e.preventDefault();
            e.currentTarget.reset();
            showToast("Your inquiry has been submitted.");
        }

        function handleFullContactSubmit(e) {
            e.preventDefault();
            e.target.reset();
            showToast("Thank you! Your ticket has been logged with Admissions.");
        }

        function handleFaqQuestion(e) {
            e.preventDefault();
            e.currentTarget.reset();
            showToast("Your question has been submitted.");
        }
        function toggleFaq(faqId) {
            const faqContent = document.getElementById(faqId);
            const faqIcon = document.getElementById('faq-icon-' + faqId);
            if (faqContent) {
                faqContent.classList.toggle('hidden');
                if (faqIcon) {
                    faqIcon.classList.toggle('rotate-180');
                }
            }
        }

        // Toast Feedback System
        function showToast(message) {
            const container = document.getElementById('toast-container');
            if (!container) return;

            const toast = document.createElement('div');
            toast.className = 'bg-[#1b365d] text-white text-xs px-4 py-3 rounded-xl shadow-xl border border-blue-400/30 flex items-center space-x-2 pointer-events-auto toast-slide-up max-w-sm';
            toast.innerHTML = `
                <i class="fa-solid fa-circle-check text-green-400 text-sm"></i>
                <span class="flex-grow">${message}</span>
            `;

            container.appendChild(toast);

            setTimeout(() => {
                toast.style.opacity = '0';
                toast.style.transition = 'opacity 0.3s ease';
                setTimeout(() => toast.remove(), 300);
            }, 3500);
        }

        // Initialize App
        function dismissSiteIntro() {
            const intro = document.getElementById("site-intro");
            if (!intro || intro.classList.contains("is-dismissed")) return;

            intro.classList.add("is-dismissed");
            intro.setAttribute("aria-hidden", "true");
            document.body.classList.remove("site-intro-open");
            window.setTimeout(() => intro.remove(), 600);
        }

        window.addEventListener('DOMContentLoaded', () => {
            renderCourses(coursesData);
            renderFeaturedCourse(0);
            renderResources(resourcesData);
            restoreModuleProgress();
            restoreAssignmentDrafts();
            restoreDailyQuiz();
            restoreAssignmentCalendar();

            const intro = document.getElementById("site-intro");
            if (intro) {
                const prefersReducedMotion = window.matchMedia("(prefers-reduced-motion: reduce)").matches;
                window.setTimeout(dismissSiteIntro, prefersReducedMotion ? 1200 : 3200);
            }
        });
    </script>
</body>
</html>﻿
