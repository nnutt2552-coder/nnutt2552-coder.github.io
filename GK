<!DOCTYPE html>
<html lang="th" class="light">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0, maximum-scale=1.0, user-scalable=no">
    <title>งานคงค้าง - ระบบจัดการงานและแบบฝึกหัด</title>
    
    <!-- Tailwind CSS -->
    <script src="https://cdn.tailwindcss.com"></script>
    <script>
        tailwind.config = {
            darkMode: 'class',
            theme: {
                extend: {
                    colors: {
                        slate: { 850: '#151e2e' },
                        priority: { red: '#ef4444', yellow: '#eab308', green: '#10b981', gray: '#94a3b8' }
                    },
                    fontFamily: { sans: ['Kanit', 'Prompt', 'sans-serif'] }
                }
            }
        }
    </script>
    
    <!-- FontAwesome & Google Fonts -->
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <link href="https://fonts.googleapis.com/css2?family=Kanit:wght@300;400;500;600;700&display=swap" rel="stylesheet">
    
    <style>
        body { font-family: 'Kanit', sans-serif; -webkit-tap-highlight-color: transparent; }
        .custom-scrollbar::-webkit-scrollbar { width: 4px; height: 4px; }
        .custom-scrollbar::-webkit-scrollbar-track { background: transparent; }
        .custom-scrollbar::-webkit-scrollbar-thumb { background: #cbd5e1; border-radius: 9999px; }
        .dark .custom-scrollbar::-webkit-scrollbar-thumb { background: #334155; }
        
        .modal-overlay { opacity: 0; visibility: hidden; transition: all 0.2s ease-out; }
        .modal-overlay.active { opacity: 1; visibility: visible; }
        .modal-content { opacity: 0; transform: scale(0.95) translateY(10px); transition: all 0.2s ease-out; }
        .modal-overlay.active .modal-content { opacity: 1; transform: scale(1) translateY(0); }
        
        .drawer-overlay { opacity: 0; visibility: hidden; transition: all 0.3s ease-out; }
        .drawer-overlay.active { opacity: 1; visibility: visible; }
        .drawer-content { transform: translateY(100%); transition: all 0.3s cubic-bezier(0.4, 0, 0.2, 1); }
        .drawer-overlay.active .drawer-content { transform: translateY(0); }

        .line-clamp-3 { display: -webkit-box; -webkit-line-clamp: 3; -webkit-box-orient: vertical; overflow: hidden; }
        input[type="time"]::-webkit-calendar-picker-indicator, input[type="date"]::-webkit-calendar-picker-indicator { cursor: pointer; }
    </style>
</head>
<body class="bg-slate-50 text-slate-800 dark:bg-slate-900 dark:text-slate-100 transition-colors duration-200 min-h-screen flex flex-col md:flex-row">

    <!-- Mobile Header -->
    <div class="md:hidden flex items-center justify-between p-4 bg-white dark:bg-slate-800 border-b border-slate-200 dark:border-slate-700 sticky top-0 z-30 shadow-sm">
        <div class="flex items-center gap-2">
            <div class="w-8 h-8 rounded-lg bg-indigo-600 text-white flex items-center justify-center font-bold text-lg"><i class="fa-solid fa-list-check"></i></div>
            <span class="font-bold text-lg">งานคงค้าง</span>
        </div>
        <div class="flex items-center gap-2">
            <button onclick="toggleTheme()" class="p-2 rounded-lg text-slate-600 dark:text-slate-300 hover:bg-slate-100 dark:hover:bg-slate-700"><i class="fa-solid fa-moon dark:hidden"></i><i class="fa-solid fa-sun hidden dark:inline"></i></button>
            <button onclick="openTaskForm()" class="bg-indigo-600 text-white px-3 py-1.5 rounded-lg text-sm font-medium"><i class="fa-solid fa-plus"></i></button>
        </div>
    </div>

    <!-- Desktop Sidebar -->
    <aside class="hidden md:flex flex-col w-64 bg-white dark:bg-slate-800 border-r border-slate-200 dark:border-slate-700 p-5 min-h-screen justify-between shrink-0 sticky top-0">
        <div>
            <div class="flex items-center gap-3 mb-8">
                <div class="w-10 h-10 rounded-xl bg-indigo-600 text-white flex items-center justify-center font-bold text-xl shadow-md"><i class="fa-solid fa-list-check"></i></div>
                <div><h1 class="font-bold text-xl leading-tight">งานคงค้าง</h1><p class="text-[10px] text-slate-500 uppercase tracking-widest">Cloud Synced</p></div>
            </div>

            <button onclick="openTaskForm()" class="w-full bg-indigo-600 hover:bg-indigo-700 text-white font-medium py-2.5 px-4 rounded-xl shadow-sm transition flex items-center justify-center gap-2 mb-6">
                <i class="fa-solid fa-plus"></i> <span>เพิ่มงานใหม่</span>
            </button>

            <nav class="space-y-1">
                <button onclick="switchTab('pending')" id="nav-pending" class="nav-btn w-full flex items-center justify-between px-3.5 py-2.5 rounded-xl font-medium transition active-nav">
                    <div class="flex items-center gap-3"><i class="fa-solid fa-clock-rotate-left w-5"></i><span>งานที่ยังไม่เสร็จ</span></div>
                    <span id="badge-pending" class="bg-indigo-100 text-indigo-700 dark:bg-indigo-900/50 dark:text-indigo-300 text-xs px-2 py-0.5 rounded-full font-bold">0</span>
                </button>
                <button onclick="switchTab('completed')" id="nav-completed" class="nav-btn w-full flex items-center justify-between px-3.5 py-2.5 rounded-xl font-medium transition text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-850">
                    <div class="flex items-center gap-3"><i class="fa-solid fa-circle-check w-5"></i><span>งานที่เสร็จแล้ว</span></div>
                    <span id="badge-completed" class="bg-slate-100 text-slate-600 dark:bg-slate-800 dark:text-slate-400 text-xs px-2 py-0.5 rounded-full font-bold">0</span>
                </button>
                <button onclick="switchTab('subjects')" id="nav-subjects" class="nav-btn w-full flex items-center gap-3 px-3.5 py-2.5 rounded-xl font-medium transition text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-850">
                    <i class="fa-solid fa-graduation-cap w-5"></i><span>รายวิชา</span>
                </button>
                <button onclick="switchTab('calendar')" id="nav-calendar" class="nav-btn w-full flex items-center gap-3 px-3.5 py-2.5 rounded-xl font-medium transition text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-850">
                    <i class="fa-regular fa-calendar-days w-5"></i><span>ปฏิทิน</span>
                </button>
                <button onclick="switchTab('trash')" id="nav-trash" class="nav-btn w-full flex items-center justify-between px-3.5 py-2.5 rounded-xl font-medium transition text-slate-600 dark:text-slate-400 hover:bg-slate-100 dark:hover:bg-slate-850">
                    <div class="flex items-center gap-3"><i class="fa-solid fa-trash-can w-5"></i><span>ถังขยะ</span></div>
                    <span id="badge-trash" class="bg-rose-100 text-rose-700 dark:bg-rose-900/50 dark:text-rose-300 text-xs px-2 py-0.5 rounded-full font-bold hidden">0</span>
                </button>
            </nav>
        </div>
        <div class="border-t border-slate-200 dark:border-slate-700 pt-4 flex justify-between items-center">
            <span class="text-xs text-slate-500">เปลี่ยนธีม</span>
            <button onclick="toggleTheme()" class="p-2 rounded-lg bg-slate-100 dark:bg-slate-800 hover:bg-slate-200 dark:hover:bg-slate-700"><i class="fa-solid fa-moon dark:hidden"></i><i class="fa-solid fa-sun hidden dark:inline"></i></button>
        </div>
    </aside>

    <!-- Mobile Bottom Nav -->
    <div class="md:hidden fixed bottom-0 left-0 right-0 bg-white dark:bg-slate-800 border-t border-slate-200 dark:border-slate-700 z-30 pb-safe shadow-[0_-4px_6px_-1px_rgba(0,0,0,0.05)]">
        <div class="flex justify-around p-1.5">
            <button onclick="switchTab('pending')" class="mobile-nav-btn text-indigo-600 dark:text-indigo-400 flex flex-col items-center gap-1 p-2 w-1/5" data-tab="pending"><i class="fa-solid fa-clock-rotate-left text-lg"></i><span class="text-[10px] font-medium">รอทำ</span></button>
            <button onclick="switchTab('completed')" class="mobile-nav-btn text-slate-400 hover:text-slate-800 dark:hover:text-slate-200 flex flex-col items-center gap-1 p-2 w-1/5" data-tab="completed"><i class="fa-solid fa-circle-check text-lg"></i><span class="text-[10px] font-medium">เสร็จแล้ว</span></button>
            <button onclick="switchTab('subjects')" class="mobile-nav-btn text-slate-400 hover:text-slate-800 dark:hover:text-slate-200 flex flex-col items-center gap-1 p-2 w-1/5" data-tab="subjects"><i class="fa-solid fa-graduation-cap text-lg"></i><span class="text-[10px] font-medium">วิชา</span></button>
            <button onclick="switchTab('calendar')" class="mobile-nav-btn text-slate-400 hover:text-slate-800 dark:hover:text-slate-200 flex flex-col items-center gap-1 p-2 w-1/5" data-tab="calendar"><i class="fa-regular fa-calendar-days text-lg"></i><span class="text-[10px] font-medium">ปฏิทิน</span></button>
            <button onclick="switchTab('trash')" class="mobile-nav-btn text-slate-400 hover:text-slate-800 dark:hover:text-slate-200 flex flex-col items-center gap-1 p-2 w-1/5" data-tab="trash"><i class="fa-solid fa-trash-can text-lg"></i><span class="text-[10px] font-medium">ถังขยะ</span></button>
        </div>
    </div>

    <!-- Main Content Area -->
    <main class="flex-1 flex flex-col min-w-0 h-screen overflow-hidden bg-slate-50 dark:bg-slate-900">
        <!-- Header Info -->
        <header class="hidden md:flex items-center justify-between px-8 py-5 border-b border-slate-200 dark:border-slate-800 bg-white/80 dark:bg-slate-900/80 backdrop-blur z-10">
            <div>
                <h2 id="header-title" class="text-2xl font-bold">งานที่ยังไม่เสร็จ</h2>
                <p id="header-desc" class="text-xs text-slate-500 mt-1">จัดการและติดตามงานที่รอส่ง</p>
            </div>
            <!-- Sync indicator -->
            <div id="sync-indicator" class="hidden items-center gap-2 text-xs font-medium text-emerald-600 bg-emerald-50 dark:bg-emerald-900/30 dark:text-emerald-400 px-3 py-1.5 rounded-full border border-emerald-200 dark:border-emerald-800/50">
                <i class="fa-solid fa-cloud-arrow-up animate-pulse"></i> ซิงค์ข้อมูลคลาวด์แล้ว
            </div>
        </header>

        <!-- View Container -->
        <div class="flex-1 overflow-y-auto custom-scrollbar p-4 md:p-8 pb-24 md:pb-8">
            <div class="max-w-6xl mx-auto w-full relative">
                
                <!-- 1. Pending View -->
                <section id="view-pending" class="view-section space-y-6 block">
                    <div class="flex flex-col gap-4">
                        <div class="flex flex-col sm:flex-row gap-3">
                            <div class="relative flex-1">
                                <i class="fa-solid fa-search absolute left-3.5 top-1/2 -translate-y-1/2 text-slate-400"></i>
                                <input type="text" id="search-pending" oninput="renderPending()" placeholder="ค้นหางาน, วิชา, ครูผู้สอน..." class="w-full pl-10 pr-4 py-2.5 bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl text-sm focus:ring-2 focus:ring-indigo-500 outline-none">
                            </div>
                            <select id="sort-pending" onchange="renderPending()" class="px-4 py-2.5 bg-white dark:bg-slate-800 border border-slate-200 dark:border-slate-700 rounded-xl text-sm focus:ring-2 focus:ring-indigo-500 outline-none">
                                <option value="dueDate">เรียงตามวันที่กำหนดส่ง</option>
                                <option value="priority">เรียงตามความสำคัญ</option>
                            </select>
                        </div>
                        
                        <div class="flex flex-wrap gap-4 text-xs bg-white dark:bg-slate-800 p-3 rounded-xl border border-slate-200 dark:border-slate-700 shadow-sm">
                            <span class="font-semibold text-slate-500"><i class="fa-solid fa-circle-info mr-1"></i> ระดับความสำคัญ (สีแถบการ์ด):</span>
                            <div class="flex gap-4">
                                <span class="flex items-center gap-1.5"><span class="w-2.5 h-2.5 rounded-full bg-priority-red"></span> ≤ 7 วัน (ด่วนมาก)</span>
                                <span class="flex items-center gap-1.5"><span class="w-2.5 h-2.5 rounded-full bg-priority-yellow"></span> ≤ 14 วัน (ปานกลาง)</span>
                                <span class="flex items-center gap-1.5"><span class="w-2.5 h-2.5 rounded-full bg-priority-green"></span> ≤ 21 วัน (ปกติ)</span>
                                <span class="flex items-center gap-1.5"><span class="w-2.5 h-2.5 rounded-full bg-priority-gray"></span> > 21 วัน (ชิลๆ)</span>
                            </div>
                        </div>
                    </div>

                    <div id="pending-grid" class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-3 gap-4"></div>
                </section>

                <!-- 2. Completed View -->
                <section id="view-completed" class="view-section space-y-6 hidden">
                    <div class="flex flex-col sm:flex-row gap-4 items-center justify-between bg-white dark:bg-slate-800 p-4 rounded-xl border border-slate-200 dark:border-slate-700 shadow-sm">
                        <div class="flex items-center gap-3">
                            <span class="text-sm font-medium">จัดกลุ่มตาม:</span>
                            <select id="group-completed" onchange="renderCompleted()" class="px-3 py-1.5 bg-slate-50 dark:bg-slate-900 border border-slate-200 dark:border-slate-700 rounded-lg text-sm outline-none">
                                <option value="date">วันที่ทำเสร็จ</option>
                                <option value="month">เดือนที่ทำเสร็จ</option>
                                <option value="subject">รายวิชา</option>
                            </select>
                        </div>
                        <span class="text-xs bg-emerald-100 text-emerald-700 dark:bg-emerald-900/30 dark:text-emerald-400 px-3 py-1 rounded-full font-bold border border-emerald-200 dark:border-emerald-800" id="completed-count-text">รวม 0 งาน</span>
                    </div>
                    <div id="completed-container" class="space-y-6"></div>
                </section>

                <!-- 3. Subjects View -->
                <section id="view-subjects" class="view-section space-y-6 hidden">
                    <div class="grid grid-cols-3 gap-3 sm:gap-4 mb-2">
                        <div class="bg-white dark:bg-slate-800 p-4 rounded-xl border border-slate-200 dark:border-slate-700 text-center shadow-sm">
                            <p class="text-xs text-slate-500 mb-1">งานทั้งหมด</p>
                            <h4 id="stat-total" class="text-2xl font-bold">0</h4>
                        </div>
                        <div class="bg-white dark:bg-slate-800 p-4 rounded-xl border border-amber-200 dark:border-amber-900/50 text-center shadow-sm">
                            <p class="text-xs text-slate-500 mb-1">ค้างส่ง</p>
                            <h4 id="stat-pending" class="text-2xl font-bold text-amber-600 dark:text-amber-500">0</h4>
                        </div>
                        <div class="bg-white dark:bg-slate-800 p-4 rounded-xl border border-emerald-200 dark:border-emerald-900/50 text-center shadow-sm">
                            <p class="text-xs text-slate-500 mb-1">เสร็จแล้ว</p>
                            <h4 id="stat-done" class="text-2xl font-bold text-emerald-600 dark:text-emerald-500">0</h4>
                        </div>
                    </div>
                    <div id="subjects-grid" class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-3 gap-4"></div>
                </section>

                <!-- 4. Calendar View -->
                <section id="view-calendar" class="view-section space-y-4 hidden">
                    <div class="flex items-center justify-between bg-white dark:bg-slate-800 p-4 rounded-xl border border-slate-200 dark:border-slate-700 shadow-sm">
                        <h3 id="cal-month-title" class="font-bold text-lg"></h3>
                        <div class="flex items-center gap-2">
                            <button onclick="calNavigate(-1)" class="w-8 h-8 rounded-lg bg-slate-100 hover:bg-slate-200 dark:bg-slate-700 dark:hover:bg-slate-600 flex justify-center items-center"><i class="fa-solid fa-chevron-left"></i></button>
                            <button onclick="calGoToday()" class="px-3 py-1 text-sm font-medium bg-slate-100 hover:bg-slate-200 dark:bg-slate-700 dark:hover:bg-slate-600 rounded-lg">วันนี้</button>
                            <button onclick="calNavigate(1)" class="w-8 h-8 rounded-lg bg-slate-100 hover:bg-slate-200 dark:bg-slate-700 dark:hover:bg-slate-600 flex justify-center items-center"><i class="fa-solid fa-chevron-right"></i></button>
                        </div>
                    </div>
                    <div class="bg-white dark:bg-slate-800 rounded-xl border border-slate-200 dark:border-slate-700 shadow-sm p-4">
                        <div class="grid grid-cols-7 gap-1 text-center text-xs font-bold text-slate-400 mb-2">
                            <div class="text-rose-500">อา</div><div>จ</div><div>อ</div><div>พ</div><div>พฤ</div><div>ศ</div><div class="text-indigo-500">ส</div>
                        </div>
                        <div id="calendar-grid" class="grid grid-cols-7 gap-1 sm:gap-2"></div>
                    </div>
                </section>

                <!-- 5. Trash View -->
                <section id="view-trash" class="view-section space-y-4 hidden">
                    <div class="bg-rose-50 dark:bg-rose-900/20 p-4 rounded-xl border border-rose-200 dark:border-rose-900/50 flex flex-col sm:flex-row justify-between items-start sm:items-center gap-4">
                        <div class="flex gap-3">
                            <i class="fa-solid fa-circle-info text-rose-500 text-xl"></i>
                            <div>
                                <p class="text-sm font-bold text-rose-700 dark:text-rose-400">ถังขยะชั่วคราว</p>
                                <p class="text-xs text-rose-600/80 dark:text-rose-400/80">ระบบจะลบข้อมูลที่อยู่ในถังขยะถาวรอัตโนมัติเมื่อครบ 30 วัน</p>
                            </div>
                        </div>
                        <button onclick="emptyTrash()" id="btn-empty-trash" class="text-xs bg-rose-600 text-white px-3 py-1.5 rounded-lg font-medium hover:bg-rose-700 whitespace-nowrap"><i class="fa-solid fa-dumpster"></i> ล้างถังขยะ</button>
                    </div>
                    <div id="trash-list" class="space-y-3"></div>
                </section>

            </div>
        </div>
    </main>

    <!-- ================= MODALS & DRAWERS ================= -->

    <!-- Task Form Modal -->
    <div id="modal-task-form" class="fixed inset-0 z-50 modal-overlay flex items-center justify-center bg-slate-900/50 backdrop-blur-sm p-4">
        <div class="modal-content bg-white dark:bg-slate-800 w-full max-w-2xl rounded-2xl shadow-2xl flex flex-col max-h-full">
            <div class="flex justify-between items-center p-5 border-b border-slate-200 dark:border-slate-700">
                <h3 id="form-title" class="font-bold text-lg text-slate-900 dark:text-white">เพิ่มงานใหม่</h3>
                <button onclick="closeModal('modal-task-form')" class="w-8 h-8 rounded-full bg-slate-100 dark:bg-slate-700 flex items-center justify-center hover:bg-slate-200 text-slate-500"><i class="fa-solid fa-xmark"></i></button>
            </div>
            <div class="p-5 overflow-y-auto custom-scrollbar flex-1">
                <form id="taskForm" class="space-y-4">
                    <input type="hidden" id="f-id">
                    
                    <div>
                        <label clas
