<html lang="fr">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>SchoolManager Pro - Gestion Scolaire</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link rel="stylesheet" href="https://cdnjs.cloudflare.com/ajax/libs/font-awesome/6.4.0/css/all.min.css">
    <script src="https://cdnjs.cloudflare.com/ajax/libs/jspdf/2.5.1/jspdf.umd.min.js"></script>
    <script src="https://cdnjs.cloudflare.com/ajax/libs/html2canvas/1.4.1/html2canvas.min.js"></script>
    <script src="https://cdn.jsdelivr.net/npm/chart.js"></script>
    
    <style>
        .sidebar {
            transition: all 0.3s ease;
        }
        @media (max-width: 768px) {
            .sidebar {
                transform: translateX(-100%);
            }
            .sidebar.active {
                transform: translateX(0);
            }
        }
        .loading {
            display: inline-block;
            width: 20px;
            height: 20px;
            border: 3px solid #f3f3f3;
            border-radius: 50%;
            border-top-color: #3498db;
            animation: spin 1s ease-in-out infinite;
        }
        @keyframes spin {
            to { transform: rotate(360deg); }
        }
        .fade-in {
            animation: fadeIn 0.5s ease-in;
        }
        @keyframes fadeIn {
            from { opacity: 0; }
            to { opacity: 1; }
        }
    </style>
</head>
<body class="bg-gray-100">
    <!-- Navigation -->
    <nav class="bg-blue-800 text-white p-4 shadow-lg">
        <div class="container mx-auto flex justify-between items-center">
            <div class="flex items-center space-x-4">
                <button id="menuToggle" class="md:hidden text-xl">
                    <i class="fas fa-bars"></i>
                </button>
                <h1 class="text-xl font-bold">SchoolManager Pro</h1>
            </div>
            <div class="flex items-center space-x-4">
                <span id="userRole" class="hidden md:inline bg-blue-600 px-2 py-1 rounded text-sm"></span>
                <span id="userName" class="text-sm"></span>
                <button id="logoutBtn" class="bg-red-600 hover:bg-red-700 px-3 py-1 rounded text-sm">
                    <i class="fas fa-sign-out-alt mr-1"></i>Déconnexion
                </button>
            </div>
        </div>
    </nav>

    <div class="flex">
        <!-- Sidebar -->
        <div class="sidebar bg-white w-64 min-h-screen shadow-lg fixed md:relative z-10">
            <div class="p-4 border-b">
                <h2 class="text-lg font-semibold text-center">Navigation</h2>
            </div>
            <nav class="mt-4">
                <a href="#dashboard" class="nav-item block py-3 px-6 hover:bg-blue-50 text-blue-700 border-l-4 border-blue-700">
                    <i class="fas fa-tachometer-alt mr-3"></i>Tableau de bord
                </a>
                <a href="#students" class="nav-item block py-3 px-6 hover:bg-gray-100 text-gray-700">
                    <i class="fas fa-users mr-3"></i>Élèves
                </a>
                <a href="#grades" class="nav-item block py-3 px-6 hover:bg-gray-100 text-gray-700">
                    <i class="fas fa-graduation-cap mr-3"></i>Notes & Bulletins
                </a>
                <a href="#attendance" class="nav-item block py-3 px-6 hover:bg-gray-100 text-gray-700">
                    <i class="fas fa-calendar-check mr-3"></i>Assiduité
                </a>
                <a href="#reports" class="nav-item block py-3 px-6 hover:bg-gray-100 text-gray-700">
                    <i class="fas fa-chart-bar mr-3"></i>Rapports
                </a>
                <a href="#settings" class="nav-item block py-3 px-6 hover:bg-gray-100 text-gray-700 admin-only">
                    <i class="fas fa-cogs mr-3"></i>Paramètres
                </a>
                <a href="#staff" class="nav-item block py-3 px-6 hover:bg-gray-100 text-gray-700 admin-only">
                    <i class="fas fa-user-tie mr-3"></i>Personnel
                </a>
            </nav>
        </div>

        <!-- Main Content -->
        <div class="flex-1 md:ml-0 p-4">
            <!-- Login Screen -->
            <div id="loginScreen" class="min-h-screen flex items-center justify-center bg-gray-50 py-12 px-4 sm:px-6 lg:px-8">
                <div class="max-w-md w-full space-y-8">
                    <div>
                        <h2 class="mt-6 text-center text-3xl font-extrabold text-gray-900">
                            SchoolManager Pro
                        </h2>
                        <p class="mt-2 text-center text-sm text-gray-600">
                            Application de gestion scolaire
                        </p>
                    </div>
                    <form id="loginForm" class="mt-8 space-y-6">
                        <div class="rounded-md shadow-sm -space-y-px">
                            <div>
                                <label for="email" class="sr-only">Email</label>
                                <input id="email" name="email" type="email" autocomplete="email" required class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-t-md focus:outline-none focus:ring-blue-500 focus:border-blue-500 focus:z-10 sm:text-sm" placeholder="Adresse email">
                            </div>
                            <div>
                                <label for="password" class="sr-only">Mot de passe</label>
                                <input id="password" name="password" type="password" autocomplete="current-password" required class="appearance-none rounded-none relative block w-full px-3 py-2 border border-gray-300 placeholder-gray-500 text-gray-900 rounded-b-md focus:outline-none focus:ring-blue-500 focus:border-blue-500 focus:z-10 sm:text-sm" placeholder="Mot de passe">
                            </div>
                        </div>

                        <div>
                            <button type="submit" class="group relative w-full flex justify-center py-2 px-4 border border-transparent text-sm font-medium rounded-md text-white bg-blue-600 hover:bg-blue-700 focus:outline-none focus:ring-2 focus:ring-offset-2 focus:ring-blue-500">
                                <span class="absolute left-0 inset-y-0 flex items-center pl-3">
                                    <i class="fas fa-sign-in-alt text-blue-300"></i>
                                </span>
                                Se connecter
                            </button>
                        </div>
                        
                        <div id="loginError" class="hidden bg-red-100 border border-red-400 text-red-700 px-4 py-3 rounded"></div>
                    </form>
                </div>
            </div>

            <!-- Application Content (hidden by default) -->
            <div id="appContent" class="hidden">
                <!-- Dashboard -->
                <div id="dashboard" class="content-section">
                    <div class="bg-white rounded-lg shadow p-6 mb-6">
                        <h2 class="text-2xl font-bold mb-6">Tableau de Bord</h2>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 lg:grid-cols-4 gap-6 mb-8">
                            <div class="bg-blue-50 p-4 rounded-lg border border-blue-200">
                                <div class="flex justify-between items-center">
                                    <div>
                                        <p class="text-blue-800 font-semibold">Élèves inscrits</p>
                                        <p id="totalStudents" class="text-3xl font-bold text-blue-600">0</p>
                                    </div>
                                    <div class="bg-blue-100 p-3 rounded-full">
                                        <i class="fas fa-users text-blue-600 text-xl"></i>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="bg-green-50 p-4 rounded-lg border border-green-200">
                                <div class="flex justify-between items-center">
                                    <div>
                                        <p class="text-green-800 font-semibold">Taux de présence</p>
                                        <p id="attendanceRate" class="text-3xl font-bold text-green-600">0%</p>
                                    </div>
                                    <div class="bg-green-100 p-3 rounded-full">
                                        <i class="fas fa-calendar-check text-green-600 text-xl"></i>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="bg-purple-50 p-4 rounded-lg border border-purple-200">
                                <div class="flex justify-between items-center">
                                    <div>
                                        <p class="text-purple-800 font-semibold">Moyenne générale</p>
                                        <p id="averageGrade" class="text-3xl font-bold text-purple-600">0.0</p>
                                    </div>
                                    <div class="bg-purple-100 p-3 rounded-full">
                                        <i class="fas fa-graduation-cap text-purple-600 text-xl"></i>
                                    </div>
                                </div>
                            </div>
                            
                            <div class="bg-yellow-50 p-4 rounded-lg border border-yellow-200">
                                <div class="flex justify-between items-center">
                                    <div>
                                        <p class="text-yellow-800 font-semibold">Personnel</p>
                                        <p id="totalStaff" class="text-3xl font-bold text-yellow-600">0</p>
                                    </div>
                                    <div class="bg-yellow-100 p-3 rounded-full">
                                        <i class="fas fa-user-tie text-yellow-600 text-xl"></i>
                                    </div>
                                </div>
                            </div>
                        </div>
                        
                        <div class="grid grid-cols-1 lg:grid-cols-2 gap-6">
                            <div class="bg-white p-4 rounded-lg border border-gray-200">
                                <h3 class="text-lg font-semibold mb-4">Répartition par classe</h3>
                                <canvas id="classDistributionChart" height="250"></canvas>
                            </div>
                            
                            <div class="bg-white p-4 rounded-lg border border-gray-200">
                                <h3 class="text-lg font-semibold mb-4">Évolution des moyennes</h3>
                                <canvas id="gradesEvolutionChart" height="250"></canvas>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Students Management -->
                <div id="students" class="content-section hidden">
                    <div class="bg-white rounded-lg shadow p-6">
                        <div class="flex justify-between items-center mb-6">
                            <h2 class="text-2xl font-bold">Gestion des Élèves</h2>
                            <button id="addStudentBtn" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded flex items-center secretary-only">
                                <i class="fas fa-plus mr-2"></i>Ajouter un élève
                            </button>
                        </div>
                        
                        <div class="mb-6 flex flex-col md:flex-row gap-4">
                            <div class="flex-1">
                                <input type="text" id="searchStudents" placeholder="Rechercher un élève..." class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                            </div>
                            <div class="flex gap-2">
                                <select id="filterClass" class="px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    <option value="">Toutes les classes</option>
                                </select>
                                <select id="filterGender" class="px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    <option value="">Tous les genres</option>
                                    <option value="M">Masculin</option>
                                    <option value="F">Féminin</option>
                                </select>
                            </div>
                        </div>
                        
                        <div class="overflow-x-auto">
                            <table class="min-w-full bg-white">
                                <thead class="bg-gray-100">
                                    <tr>
                                        <th class="py-3 px-4 text-left">Matricule</th>
                                        <th class="py-3 px-4 text-left">Nom & Prénoms</th>
                                        <th class="py-3 px-4 text-left">Classe</th>
                                        <th class="py-3 px-4 text-left">Genre</th>
                                        <th class="py-3 px-4 text-left">Contact Parent</th>
                                        <th class="py-3 px-4 text-left">Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="studentsTableBody">
                                    <!-- Students will be loaded here -->
                                </tbody>
                            </table>
                        </div>
                        
                        <div class="mt-4 flex justify-between items-center">
                            <div id="studentsPagination" class="flex gap-2">
                                <!-- Pagination will be generated here -->
                            </div>
                            <div class="flex gap-2">
                                <button id="exportStudentsBtn" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded flex items-center">
                                    <i class="fas fa-file-export mr-2"></i>Exporter CSV
                                </button>
                                <button id="importStudentsBtn" class="bg-indigo-600 hover:bg-indigo-700 text-white px-4 py-2 rounded flex items-center secretary-only">
                                    <i class="fas fa-file-import mr-2"></i>Importer CSV
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Grades & Report Cards -->
                <div id="grades" class="content-section hidden">
                    <div class="bg-white rounded-lg shadow p-6">
                        <h2 class="text-2xl font-bold mb-6">Notes & Bulletins</h2>
                        
                        <div class="mb-6 grid grid-cols-1 md:grid-cols-3 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Sélectionner une classe</label>
                                <select id="selectClassGrades" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    <option value="">Choisir une classe</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Trimestre</label>
                                <select id="selectTrimester" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    <option value="1">Premier trimestre</option>
                                    <option value="2">Deuxième trimestre</option>
                                    <option value="3">Troisième trimestre</option>
                                </select>
                            </div>
                            <div class="flex items-end">
                                <button id="loadGradesBtn" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded w-full">
                                    Charger les notes
                                </button>
                            </div>
                        </div>
                        
                        <div id="gradesContent" class="hidden">
                            <div class="mb-6">
                                <h3 class="text-lg font-semibold mb-4">Saisie des notes</h3>
                                <div class="overflow-x-auto">
                                    <table class="min-w-full bg-white border border-gray-200">
                                        <thead class="bg-gray-100">
                                            <tr>
                                                <th class="py-2 px-4 border">Élève</th>
                                                <th class="py-2 px-4 border">Mathématiques</th>
                                                <th class="py-2 px-4 border">Français</th>
                                                <th class="py-2 px-4 border">Anglais</th>
                                                <th class="py-2 px-4 border">SVT</th>
                                                <th class="py-2 px-4 border">Histoire-Géo</th>
                                                <th class="py-2 px-4 border">Moyenne</th>
                                            </tr>
                                        </thead>
                                        <tbody id="gradesTableBody">
                                            <!-- Grades will be loaded here -->
                                        </tbody>
                                    </table>
                                </div>
                            </div>
                            
                            <div class="flex justify-end">
                                <button id="generateReportCardsBtn" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded flex items-center">
                                    <i class="fas fa-file-pdf mr-2"></i>Générer les bulletins
                                </button>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Attendance -->
                <div id="attendance" class="content-section hidden">
                    <div class="bg-white rounded-lg shadow p-6">
                        <h2 class="text-2xl font-bold mb-6">Gestion de l'Assiduité</h2>
                        
                        <div class="mb-6 grid grid-cols-1 md:grid-cols-3 gap-4">
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Sélectionner une classe</label>
                                <select id="selectClassAttendance" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    <option value="">Choisir une classe</option>
                                </select>
                            </div>
                            <div>
                                <label class="block text-sm font-medium text-gray-700 mb-1">Date</label>
                                <input type="date" id="attendanceDate" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                            </div>
                            <div class="flex items-end">
                                <button id="loadAttendanceBtn" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded w-full">
                                    Charger l'assiduité
                                </button>
                            </div>
                        </div>
                        
                        <div id="attendanceContent" class="hidden">
                            <div class="mb-4 flex justify-between items-center">
                                <h3 class="text-lg font-semibold">Marquage de présence</h3>
                                <div class="flex gap-2">
                                    <button id="markAllPresentBtn" class="bg-green-600 hover:bg-green-700 text-white px-3 py-1 rounded text-sm">
                                        Tous présents
                                    </button>
                                    <button id="saveAttendanceBtn" class="bg-blue-600 hover:bg-blue-700 text-white px-3 py-1 rounded text-sm">
                                        Sauvegarder
                                    </button>
                                </div>
                            </div>
                            
                            <div class="overflow-x-auto">
                                <table class="min-w-full bg-white border border-gray-200">
                                    <thead class="bg-gray-100">
                                        <tr>
                                            <th class="py-2 px-4 border">Élève</th>
                                            <th class="py-2 px-4 border">Statut</th>
                                            <th class="py-2 px-4 border">Heures manquées</th>
                                            <th class="py-2 px-4 border">Remarques</th>
                                        </tr>
                                    </thead>
                                    <tbody id="attendanceTableBody">
                                        <!-- Attendance will be loaded here -->
                                    </tbody>
                                </table>
                            </div>
                            
                            <div class="mt-6">
                                <h3 class="text-lg font-semibold mb-4">Statistiques d'assiduité</h3>
                                <div class="grid grid-cols-1 md:grid-cols-3 gap-4">
                                    <div class="bg-blue-50 p-4 rounded-lg border border-blue-200">
                                        <p class="text-blue-800 font-semibold">Présents aujourd'hui</p>
                                        <p id="presentToday" class="text-2xl font-bold text-blue-600">0</p>
                                    </div>
                                    <div class="bg-red-50 p-4 rounded-lg border border-red-200">
                                        <p class="text-red-800 font-semibold">Absents aujourd'hui</p>
                                        <p id="absentToday" class="text-2xl font-bold text-red-600">0</p>
                                    </div>
                                    <div class="bg-yellow-50 p-4 rounded-lg border border-yellow-200">
                                        <p class="text-yellow-800 font-semibold">Retards aujourd'hui</p>
                                        <p id="lateToday" class="text-2xl font-bold text-yellow-600">0</p>
                                    </div>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Reports -->
                <div id="reports" class="content-section hidden">
                    <div class="bg-white rounded-lg shadow p-6">
                        <h2 class="text-2xl font-bold mb-6">Rapports et Statistiques</h2>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6 mb-8">
                            <div class="bg-white p-4 rounded-lg border border-gray-200">
                                <h3 class="text-lg font-semibold mb-4">Performance par classe</h3>
                                <canvas id="classPerformanceChart" height="250"></canvas>
                            </div>
                            
                            <div class="bg-white p-4 rounded-lg border border-gray-200">
                                <h3 class="text-lg font-semibold mb-4">Taux d'assiduité</h3>
                                <canvas id="attendanceChart" height="250"></canvas>
                            </div>
                        </div>
                        
                        <div class="flex gap-4">
                            <button id="exportReportsBtn" class="bg-green-600 hover:bg-green-700 text-white px-4 py-2 rounded flex items-center">
                                <i class="fas fa-file-excel mr-2"></i>Exporter Rapports Excel
                            </button>
                            <button id="generateSummaryBtn" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded flex items-center">
                                <i class="fas fa-file-pdf mr-2"></i>Rapport Général PDF
                            </button>
                        </div>
                    </div>
                </div>

                <!-- Settings -->
                <div id="settings" class="content-section hidden">
                    <div class="bg-white rounded-lg shadow p-6">
                        <h2 class="text-2xl font-bold mb-6">Paramètres de l'École</h2>
                        
                        <div class="grid grid-cols-1 md:grid-cols-2 gap-6">
                            <div class="bg-white p-4 rounded-lg border border-gray-200">
                                <h3 class="text-lg font-semibold mb-4">Informations de l'école</h3>
                                <form id="schoolInfoForm">
                                    <div class="mb-4">
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Nom de l'école</label>
                                        <input type="text" id="schoolName" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    </div>
                                    <div class="mb-4">
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Adresse</label>
                                        <input type="text" id="schoolAddress" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    </div>
                                    <div class="mb-4">
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Téléphone</label>
                                        <input type="text" id="schoolPhone" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    </div>
                                    <div class="mb-4">
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                                        <input type="email" id="schoolEmail" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    </div>
                                    <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
                                        Enregistrer les informations
                                    </button>
                                </form>
                            </div>
                            
                            <div class="bg-white p-4 rounded-lg border border-gray-200">
                                <h3 class="text-lg font-semibold mb-4">Configuration académique</h3>
                                <form id="academicConfigForm">
                                    <div class="mb-4">
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Format du matricule</label>
                                        <input type="text" id="studentIdFormat" value="ANNEE-XXXX" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    </div>
                                    <div class="mb-4">
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Heures de cours par jour</label>
                                        <input type="number" id="hoursPerDay" value="8" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    </div>
                                    <div class="mb-4">
                                        <label class="block text-sm font-medium text-gray-700 mb-1">Seuil d'absentéisme (%)</label>
                                        <input type="number" id="absenceThreshold" value="15" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                                    </div>
                                    <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
                                        Enregistrer la configuration
                                    </button>
                                </form>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Staff Management -->
                <div id="staff" class="content-section hidden">
                    <div class="bg-white rounded-lg shadow p-6">
                        <div class="flex justify-between items-center mb-6">
                            <h2 class="text-2xl font-bold">Gestion du Personnel</h2>
                            <button id="addStaffBtn" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded flex items-center">
                                <i class="fas fa-plus mr-2"></i>Ajouter du personnel
                            </button>
                        </div>
                        
                        <div class="overflow-x-auto">
                            <table class="min-w-full bg-white">
                                <thead class="bg-gray-100">
                                    <tr>
                                        <th class="py-3 px-4 text-left">Nom</th>
                                        <th class="py-3 px-4 text-left">Email</th>
                                        <th class="py-3 px-4 text-left">Rôle</th>
                                        <th class="py-3 px-4 text-left">Date d'ajout</th>
                                        <th class="py-3 px-4 text-left">Actions</th>
                                    </tr>
                                </thead>
                                <tbody id="staffTableBody">
                                    <!-- Staff will be loaded here -->
                                </tbody>
                            </table>
                        </div>
                    </div>
                </div>
            </div>
        </div>
    </div>

    <!-- Modals -->
    <!-- Add Student Modal -->
    <div id="addStudentModal" class="modal fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full hidden">
        <div class="relative top-20 mx-auto p-5 border w-11/12 md:w-3/4 lg:w-1/2 shadow-lg rounded-md bg-white">
            <div class="mt-3">
                <div class="flex justify-between items-center pb-3 border-b">
                    <h3 class="text-xl font-bold">Ajouter un nouvel élève</h3>
                    <button id="closeStudentModal" class="text-gray-400 hover:text-gray-600">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <form id="addStudentForm" class="mt-4 grid grid-cols-1 md:grid-cols-2 gap-4">
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Nom</label>
                        <input type="text" id="studentLastName" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Prénoms</label>
                        <input type="text" id="studentFirstName" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Date de naissance</label>
                        <input type="date" id="studentBirthDate" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Lieu de naissance</label>
                        <input type="text" id="studentBirthPlace" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Genre</label>
                        <select id="studentGender" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <option value="M">Masculin</option>
                            <option value="F">Féminin</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Classe</label>
                        <select id="studentClass" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <option value="">Choisir une classe</option>
                        </select>
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Nom du parent</label>
                        <input type="text" id="parentName" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div>
                        <label class="block text-sm font-medium text-gray-700 mb-1">Téléphone du parent</label>
                        <input type="text" id="parentPhone" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div class="md:col-span-2">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Adresse</label>
                        <input type="text" id="studentAddress" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div class="md:col-span-2">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Photo de l'élève</label>
                        <input type="file" id="studentPhoto" accept="image/*" class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    
                    <div class="md:col-span-2 flex justify-end space-x-3 mt-4">
                        <button type="button" id="cancelStudentBtn" class="bg-gray-300 hover:bg-gray-400 text-gray-800 px-4 py-2 rounded">
                            Annuler
                        </button>
                        <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
                            Enregistrer l'élève
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Add Staff Modal -->
    <div id="addStaffModal" class="modal fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full hidden">
        <div class="relative top-20 mx-auto p-5 border w-11/12 md:w-3/4 lg:w-1/2 shadow-lg rounded-md bg-white">
            <div class="mt-3">
                <div class="flex justify-between items-center pb-3 border-b">
                    <h3 class="text-xl font-bold">Ajouter un membre du personnel</h3>
                    <button id="closeStaffModal" class="text-gray-400 hover:text-gray-600">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <form id="addStaffForm" class="mt-4">
                    <div class="mb-4">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Nom complet</label>
                        <input type="text" id="staffName" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div class="mb-4">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Email</label>
                        <input type="email" id="staffEmail" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    <div class="mb-4">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Rôle</label>
                        <select id="staffRole" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                            <option value="enseignant">Enseignant</option>
                            <option value="secretaire">Secrétaire</option>
                            <option value="administrateur">Administrateur</option>
                        </select>
                    </div>
                    <div class="mb-4">
                        <label class="block text-sm font-medium text-gray-700 mb-1">Mot de passe temporaire</label>
                        <input type="password" id="staffPassword" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                    </div>
                    
                    <div class="flex justify-end space-x-3 mt-4">
                        <button type="button" id="cancelStaffBtn" class="bg-gray-300 hover:bg-gray-400 text-gray-800 px-4 py-2 rounded">
                            Annuler
                        </button>
                        <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
                            Ajouter
                        </button>
                    </div>
                </form>
            </div>
        </div>
    </div>

    <!-- Import Students Modal -->
    <div id="importStudentsModal" class="modal fixed inset-0 bg-gray-600 bg-opacity-50 overflow-y-auto h-full w-full hidden">
        <div class="relative top-20 mx-auto p-5 border w-11/12 md:w-3/4 lg:w-1/2 shadow-lg rounded-md bg-white">
            <div class="mt-3">
                <div class="flex justify-between items-center pb-3 border-b">
                    <h3 class="text-xl font-bold">Importer des élèves</h3>
                    <button id="closeImportModal" class="text-gray-400 hover:text-gray-600">
                        <i class="fas fa-times"></i>
                    </button>
                </div>
                
                <div class="mt-4">
                    <p class="mb-4">Téléchargez le modèle CSV, remplissez-le avec les données des élèves, puis importez-le ici.</p>
                    
                    <div class="mb-4">
                        <a href="#" id="downloadTemplateBtn" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded inline-flex items-center">
                            <i class="fas fa-download mr-2"></i>Télécharger le modèle CSV
                        </a>
                    </div>
                    
                    <form id="importStudentsForm">
                        <div class="mb-4">
                            <label class="block text-sm font-medium text-gray-700 mb-1">Fichier CSV</label>
                            <input type="file" id="csvFile" accept=".csv" required class="w-full px-4 py-2 border border-gray-300 rounded-lg focus:outline-none focus:ring-2 focus:ring-blue-500">
                        </div>
                        
                        <div class="flex justify-end space-x-3 mt-4">
                            <button type="button" id="cancelImportBtn" class="bg-gray-300 hover:bg-gray-400 text-gray-800 px-4 py-2 rounded">
                                Annuler
                            </button>
                            <button type="submit" class="bg-blue-600 hover:bg-blue-700 text-white px-4 py-2 rounded">
                                Importer
                            </button>
                        </div>
                    </form>
                </div>
            </div>
        </div>
    </div>

    <!-- Loading Spinner -->
    <div id="loadingSpinner" class="fixed inset-0 bg-gray-600 bg-opacity-50 flex items-center justify-center hidden z-50">
        <div class="bg-white p-6 rounded-lg shadow-lg flex items-center">
            <div class="loading mr-3"></div>
            <span>Chargement...</span>
        </div>
    </div>

    <!-- Firebase SDK -->
    <script type="module">
        // Import the functions you need from the SDKs you need
        import { initializeApp } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-app.js";
        import { getFirestore, collection, addDoc, getDocs, query, orderBy, where, updateDoc, deleteDoc, doc, setDoc, getDoc } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-firestore.js";
        import { getAuth, signInWithEmailAndPassword, createUserWithEmailAndPassword, onAuthStateChanged, signOut } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-auth.js";
        import { getStorage, ref, uploadBytes, getDownloadURL } from "https://www.gstatic.com/firebasejs/10.12.2/firebase-storage.js";

        // Your web app's Firebase configuration
        const firebaseConfig = {
            apiKey: "AIzaSyAigx8KtDCEulSWjpu17fnYsrqK7C9o3R8",
            authDomain: "petit-jobs-express.firebaseapp.com",
            projectId: "petit-jobs-express",
            storageBucket: "petit-jobs-express.appspot.com",
            messagingSenderId: "446118780236",
            appId: "1:446118780236:web:08c3a87d56bcd67399c3e9"
        };

        // Initialize Firebase
        const app = initializeApp(firebaseConfig);
        const db = getFirestore(app);
        const auth = getAuth(app);
        const storage = getStorage(app);

        // Rôles définis
        const ROLES = {
            ADMIN: 'administrateur',
            TEACHER: 'enseignant',
            SECRETARY: 'secretaire',
            PARENT: 'parent'
        };

        // Variables globales
        let currentUser = null;
        let students = [];
        let staff = [];
        let classes = ['6ème', '5ème', '4ème', '3ème', '2nde', '1ère', 'Tle'];
        let subjects = ['Mathématiques', 'Français', 'Anglais', 'SVT', 'Histoire-Géo', 'Physique-Chimie', 'Philosophie'];

        // Fonction pour vérifier les permissions
        function checkPermission(requiredRole) {
            if (!currentUser) return false;
            
            if (currentUser.role === ROLES.ADMIN) return true;
            return currentUser.role === requiredRole;
        }

        // Fonctions de permission
        function canAddStudent() {
            return checkPermission(ROLES.SECRETARY) || checkPermission(ROLES.ADMIN);
        }

        function canAddGrades() {
            return checkPermission(ROLES.TEACHER) || checkPermission(ROLES.ADMIN);
        }

        function canManageStaff() {
            return checkPermission(ROLES.ADMIN);
        }

        // Initialisation de l'application
        document.addEventListener('DOMContentLoaded', function() {
            // Initialiser l'interface selon le rôle
            initUI();
            
            // Vérifier l'état d'authentification
            onAuthStateChanged(auth, (user) => {
                if (user) {
                    // Utilisateur connecté
                    getUserData(user.uid).then(userData => {
                        currentUser = userData;
                        showAppContent();
                        updateUIForRole();
                        loadDashboardData();
                        setupDailyScheduler();
                    });
                } else {
                    // Utilisateur non connecté
                    showLoginScreen();
                }
            });

            // Gestionnaires d'événements
            setupEventListeners();
        });

        // Initialiser l'interface
        function initUI() {
            // Remplir les sélecteurs de classe
            const classSelectors = document.querySelectorAll('select[id*="Class"]');
            classSelectors.forEach(select => {
                classes.forEach(cls => {
                    const option = document.createElement('option');
                    option.value = cls;
                    option.textContent = cls;
                    select.appendChild(option);
                });
            });

            // Définir la date d'aujourd'hui pour l'assiduité
            document.getElementById('attendanceDate').valueAsDate = new Date();
        }

        // Configurer les gestionnaires d'événements
        function setupEventListeners() {
            // Navigation
            document.getElementById('menuToggle').addEventListener('click', toggleSidebar);
            document.querySelectorAll('.nav-item').forEach(item => {
                item.addEventListener('click', function(e) {
                    e.preventDefault();
                    const target = this.getAttribute('href').substring(1);
                    showSection(target);
                    setActiveNavItem(this);
                });
            });

            // Authentification
            document.getElementById('loginForm').addEventListener('submit', handleLogin);
            document.getElementById('logoutBtn').addEventListener('click', handleLogout);

            // Gestion des élèves
            document.getElementById('addStudentBtn').addEventListener('click', showAddStudentModal);
            document.getElementById('closeStudentModal').addEventListener('click', hideAddStudentModal);
            document.getElementById('cancelStudentBtn').addEventListener('click', hideAddStudentModal);
            document.getElementById('addStudentForm').addEventListener('submit', handleAddStudent);
            document.getElementById('searchStudents').addEventListener('input', filterStudents);
            document.getElementById('filterClass').addEventListener('change', filterStudents);
            document.getElementById('filterGender').addEventListener('change', filterStudents);
            document.getElementById('exportStudentsBtn').addEventListener('click', exportStudents);
            document.getElementById('importStudentsBtn').addEventListener('click', showImportStudentsModal);
            document.getElementById('closeImportModal').addEventListener('click', hideImportStudentsModal);
            document.getElementById('cancelImportBtn').addEventListener('click', hideImportStudentsModal);
            document.getElementById('importStudentsForm').addEventListener('submit', handleImportStudents);
            document.getElementById('downloadTemplateBtn').addEventListener('click', downloadCSVTemplate);

            // Gestion des notes
            document.getElementById('loadGradesBtn').addEventListener('click', loadGrades);
            document.getElementById('generateReportCardsBtn').addEventListener('click', generateReportCards);

            // Gestion de l'assiduité
            document.getElementById('loadAttendanceBtn').addEventListener('click', loadAttendance);
            document.getElementById('markAllPresentBtn').addEventListener('click', markAllPresent);
            document.getElementById('saveAttendanceBtn').addEventListener('click', saveAttendance);

            // Gestion du personnel
            document.getElementById('addStaffBtn').addEventListener('click', showAddStaffModal);
            document.getElementById('closeStaffModal').addEventListener('click', hideAddStaffModal);
            document.getElementById('cancelStaffBtn').addEventListener('click', hideAddStaffModal);
            document.getElementById('addStaffForm').addEventListener('submit', handleAddStaff);

            // Paramètres
            document.getElementById('schoolInfoForm').addEventListener('submit', saveSchoolInfo);
            document.getElementById('academicConfigForm').addEventListener('submit', saveAcademicConfig);

            // Rapports
            document.getElementById('exportReportsBtn').addEventListener('click', exportReports);
            document.getElementById('generateSummaryBtn').addEventListener('click', generateSummaryReport);
        }

        // Afficher/masquer la sidebar sur mobile
        function toggleSidebar() {
            document.querySelector('.sidebar').classList.toggle('active');
        }

        // Afficher une section spécifique
        function showSection(sectionId) {
            document.querySelectorAll('.content-section').forEach(section => {
                section.classList.add('hidden');
            });
            document.getElementById(sectionId).classList.remove('hidden');
        }

        // Définir l'élément de navigation actif
        function setActiveNavItem(activeItem) {
            document.querySelectorAll('.nav-item').forEach(item => {
                item.classList.remove('text-blue-700', 'border-blue-700', 'bg-blue-50');
                item.classList.add('text-gray-700', 'hover:bg-gray-100');
            });
            activeItem.classList.remove('text-gray-700', 'hover:bg-gray-100');
            activeItem.classList.add('text-blue-700', 'border-blue-700', 'bg-blue-50');
        }

        // Afficher l'écran de connexion
        function showLoginScreen() {
            document.getElementById('loginScreen').classList.remove('hidden');
            document.getElementById('appContent').classList.add('hidden');
        }

        // Afficher le contenu de l'application
        function showAppContent() {
            document.getElementById('loginScreen').classList.add('hidden');
            document.getElementById('appContent').classList.remove('hidden');
        }

        // Mettre à jour l'interface selon le rôle
        function updateUIForRole() {
            // Afficher/masquer les éléments selon le rôle
            document.querySelectorAll('.admin-only').forEach(el => {
                el.style.display = canManageStaff() ? 'block' : 'none';
            });
            
            document.querySelectorAll('.secretary-only').forEach(el => {
                el.style.display = canAddStudent() ? 'block' : 'none';
            });
            
            document.querySelectorAll('.teacher-only').forEach(el => {
                el.style.display = canAddGrades() ? 'block' : 'none';
            });

            // Mettre à jour l'affichage du rôle
            document.getElementById('userRole').textContent = currentUser.role;
            document.getElementById('userName').textContent = currentUser.name;
        }

        // Gérer la connexion
        async function handleLogin(e) {
            e.preventDefault();
            showLoading();
            
            const email = document.getElementById('email').value;
            const password = document.getElementById('password').value;
            
            try {
                const userCredential = await signInWithEmailAndPassword(auth, email, password);
                console.log('Utilisateur connecté:', userCredential.user);
            } catch (error) {
                console.error('Erreur de connexion:', error);
                document.getElementById('loginError').textContent = 'Email ou mot de passe incorrect';
                document.getElementById('loginError').classList.remove('hidden');
            } finally {
                hideLoading();
            }
        }

        // Gérer la déconnexion
        async function handleLogout() {
            try {
                await signOut(auth);
                console.log('Utilisateur déconnecté');
            } catch (error) {
                console.error('Erreur de déconnexion:', error);
            }
        }

        // Obtenir les données utilisateur
        async function getUserData(uid) {
            try {
                const userDoc = await getDoc(doc(db, 'users', uid));
                if (userDoc.exists()) {
                    return userDoc.data();
                } else {
                    console.error('Aucun document utilisateur trouvé');
                    return null;
                }
            } catch (error) {
                console.error('Erreur lors de la récupération des données utilisateur:', error);
                return null;
            }
        }

        // Afficher le modal d'ajout d'élève
        function showAddStudentModal() {
            document.getElementById('addStudentModal').classList.remove('hidden');
        }

        // Masquer le modal d'ajout d'élève
        function hideAddStudentModal() {
            document.getElementById('addStudentModal').classList.add('hidden');
            document.getElementById('addStudentForm').reset();
        }

        // Gérer l'ajout d'un élève
        async function handleAddStudent(e) {
            e.preventDefault();
            showLoading();
            
            // Générer un matricule
            const year = new Date().getFullYear();
            const randomNum = Math.floor(1000 + Math.random() * 9000);
            const studentId = `${year}-${randomNum}`;
            
            // Récupérer les données du formulaire
            const studentData = {
                id: studentId,
                lastName: document.getElementById('studentLastName').value,
                firstName: document.getElementById('studentFirstName').value,
                birthDate: document.getElementById('studentBirthDate').value,
                birthPlace: document.getElementById('studentBirthPlace').value,
                gender: document.getElementById('studentGender').value,
                class: document.getElementById('studentClass').value,
                parentName: document.getElementById('parentName').value,
                parentPhone: document.getElementById('parentPhone').value,
                address: document.getElementById('studentAddress').value,
                photoUrl: '',
                createdAt: new Date().toISOString()
            };
            
            try {
                // Upload de la photo si elle existe
                const photoFile = document.getElementById('studentPhoto').files[0];
                if (photoFile) {
                    const storageRef = ref(storage, `students/${studentId}`);
                    const snapshot = await uploadBytes(storageRef, photoFile);
                    studentData.photoUrl = await getDownloadURL(snapshot.ref);
                }
                
                // Ajouter l'élève à Firestore
                await addDoc(collection(db, 'students'), studentData);
                
                // Mettre à jour l'interface
                hideAddStudentModal();
                loadStudents();
                showNotification('Élève ajouté avec succès', 'success');
            } catch (error) {
                console.error('Erreur lors de l\'ajout de l\'élève:', error);
                showNotification('Erreur lors de l\'ajout de l\'élève', 'error');
            } finally {
                hideLoading();
            }
        }

        // Charger les élèves
        async function loadStudents() {
            showLoading();
            
            try {
                const querySnapshot = await getDocs(collection(db, 'students'));
                students = [];
                querySnapshot.forEach((doc) => {
                    students.push({ id: doc.id, ...doc.data() });
                });
                
                displayStudents(students);
            } catch (error) {
                console.error('Erreur lors du chargement des élèves:', error);
                showNotification('Erreur lors du chargement des élèves', 'error');
            } finally {
                hideLoading();
            }
        }

        // Afficher les élèves dans le tableau
        function displayStudents(studentsList) {
            const tbody = document.getElementById('studentsTableBody');
            tbody.innerHTML = '';
            
            if (studentsList.length === 0) {
                tbody.innerHTML = '<tr><td colspan="6" class="py-4 text-center">Aucun élève trouvé</td></tr>';
                return;
            }
            
            studentsList.forEach(student => {
                const tr = document.createElement('tr');
                tr.className = 'border-b hover:bg-gray-50';
                tr.innerHTML = `
                    <td class="py-3 px-4">${student.id}</td>
                    <td class="py-3 px-4">${student.lastName} ${student.firstName}</td>
                    <td class="py-3 px-4">${student.class}</td>
                    <td class="py-3 px-4">${student.gender === 'M' ? 'Masculin' : 'Féminin'}</td>
                    <td class="py-3 px-4">${student.parentPhone}</td>
                    <td class="py-3 px-4">
                        <button class="view-student text-blue-600 hover:text-blue-800 mr-2" data-id="${student.id}">
                            <i class="fas fa-eye"></i>
                        </button>
                        ${canAddStudent() ? `
                        <button class="edit-student text-green-600 hover:text-green-800 mr-2" data-id="${student.id}">
                            <i class="fas fa-edit"></i>
                        </button>
                        <button class="delete-student text-red-600 hover:text-red-800" data-id="${student.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                        ` : ''}
                    </td>
                `;
                tbody.appendChild(tr);
            });
            
            // Ajouter les événements pour les boutons d'action
            document.querySelectorAll('.view-student').forEach(btn => {
                btn.addEventListener('click', function() {
                    const studentId = this.getAttribute('data-id');
                    viewStudent(studentId);
                });
            });
            
            if (canAddStudent()) {
                document.querySelectorAll('.edit-student').forEach(btn => {
                    btn.addEventListener('click', function() {
                        const studentId = this.getAttribute('data-id');
                        editStudent(studentId);
                    });
                });
                
                document.querySelectorAll('.delete-student').forEach(btn => {
                    btn.addEventListener('click', function() {
                        const studentId = this.getAttribute('data-id');
                        deleteStudent(studentId);
                    });
                });
            }
        }

        // Filtrer les élèves
        function filterStudents() {
            const searchTerm = document.getElementById('searchStudents').value.toLowerCase();
            const classFilter = document.getElementById('filterClass').value;
            const genderFilter = document.getElementById('filterGender').value;
            
            const filteredStudents = students.filter(student => {
                const matchesSearch = student.lastName.toLowerCase().includes(searchTerm) || 
                                     student.firstName.toLowerCase().includes(searchTerm) ||
                                     student.id.toLowerCase().includes(searchTerm);
                const matchesClass = !classFilter || student.class === classFilter;
                const matchesGender = !genderFilter || student.gender === genderFilter;
                
                return matchesSearch && matchesClass && matchesGender;
            });
            
            displayStudents(filteredStudents);
        }

        // Afficher le modal d'importation d'élèves
        function showImportStudentsModal() {
            document.getElementById('importStudentsModal').classList.remove('hidden');
        }

        // Masquer le modal d'importation d'élèves
        function hideImportStudentsModal() {
            document.getElementById('importStudentsModal').classList.add('hidden');
            document.getElementById('importStudentsForm').reset();
        }

        // Télécharger le modèle CSV
        function downloadCSVTemplate() {
            const headers = ['Matricule', 'Nom', 'Prénoms', 'DateNaissance', 'LieuNaissance', 'Genre', 'Classe', 'NomParent', 'TelephoneParent', 'Adresse'];
            const csvContent = "data:text/csv;charset=utf-8," + headers.join(',');
            
            const encodedUri = encodeURI(csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", "modele_import_eleves.csv");
            document.body.appendChild(link);
            
            link.click();
            document.body.removeChild(link);
        }

        // Gérer l'importation d'élèves
        async function handleImportStudents(e) {
            e.preventDefault();
            showLoading();
            
            const file = document.getElementById('csvFile').files[0];
            if (!file) {
                showNotification('Veuillez sélectionner un fichier', 'error');
                hideLoading();
                return;
            }
            
            const reader = new FileReader();
            reader.onload = async function(e) {
                try {
                    const csvText = e.target.result;
                    const lines = csvText.split('\n');
                    const headers = lines[0].split(',');
                    
                    for (let i = 1; i < lines.length; i++) {
                        if (lines[i].trim() === '') continue;
                        
                        const values = lines[i].split(',');
                        if (values.length !== headers.length) continue;
                        
                        const studentData = {
                            id: values[0] || generateStudentId(),
                            lastName: values[1],
                            firstName: values[2],
                            birthDate: values[3],
                            birthPlace: values[4],
                            gender: values[5],
                            class: values[6],
                            parentName: values[7],
                            parentPhone: values[8],
                            address: values[9],
                            createdAt: new Date().toISOString()
                        };
                        
                        await addDoc(collection(db, 'students'), studentData);
                    }
                    
                    hideImportStudentsModal();
                    loadStudents();
                    showNotification('Élèves importés avec succès', 'success');
                } catch (error) {
                    console.error('Erreur lors de l\'importation:', error);
                    showNotification('Erreur lors de l\'importation des élèves', 'error');
                } finally {
                    hideLoading();
                }
            };
            
            reader.readAsText(file);
        }

        // Générer un ID d'élève
        function generateStudentId() {
            const year = new Date().getFullYear();
            const randomNum = Math.floor(1000 + Math.random() * 9000);
            return `${year}-${randomNum}`;
        }

        // Exporter les élèves en CSV
        function exportStudents() {
            if (students.length === 0) {
                showNotification('Aucun élève à exporter', 'warning');
                return;
            }
            
            const headers = ['Matricule', 'Nom', 'Prénoms', 'DateNaissance', 'Genre', 'Classe', 'NomParent', 'TelephoneParent', 'Adresse'];
            let csvContent = headers.join(',') + '\n';
            
            students.forEach(student => {
                const row = [
                    student.id,
                    student.lastName,
                    student.firstName,
                    student.birthDate,
                    student.gender,
                    student.class,
                    student.parentName,
                    student.parentPhone,
                    student.address
                ];
                csvContent += row.join(',') + '\n';
            });
            
            const encodedUri = encodeURI("data:text/csv;charset=utf-8," + csvContent);
            const link = document.createElement("a");
            link.setAttribute("href", encodedUri);
            link.setAttribute("download", `eleves_${new Date().toISOString().split('T')[0]}.csv`);
            document.body.appendChild(link);
            
            link.click();
            document.body.removeChild(link);
        }

        // Afficher le modal d'ajout de personnel
        function showAddStaffModal() {
            document.getElementById('addStaffModal').classList.remove('hidden');
        }

        // Masquer le modal d'ajout de personnel
        function hideAddStaffModal() {
            document.getElementById('addStaffModal').classList.add('hidden');
            document.getElementById('addStaffForm').reset();
        }

        // Gérer l'ajout de personnel
        async function handleAddStaff(e) {
            e.preventDefault();
            showLoading();
            
            const staffData = {
                name: document.getElementById('staffName').value,
                email: document.getElementById('staffEmail').value,
                role: document.getElementById('staffRole').value,
                createdAt: new Date().toISOString()
            };
            
            const password = document.getElementById('staffPassword').value;
            
            try {
                // Créer l'utilisateur dans Firebase Auth
                const userCredential = await createUserWithEmailAndPassword(auth, staffData.email, password);
                const userId = userCredential.user.uid;
                
                // Ajouter les données utilisateur dans Firestore
                await setDoc(doc(db, 'users', userId), staffData);
                
                // Mettre à jour l'interface
                hideAddStaffModal();
                loadStaff();
                showNotification('Membre du personnel ajouté avec succès', 'success');
            } catch (error) {
                console.error('Erreur lors de l\'ajout du personnel:', error);
                showNotification('Erreur lors de l\'ajout du personnel', 'error');
            } finally {
                hideLoading();
            }
        }

        // Charger le personnel
        async function loadStaff() {
            try {
                const querySnapshot = await getDocs(collection(db, 'users'));
                staff = [];
                querySnapshot.forEach((doc) => {
                    if (doc.id !== currentUser.uid) { // Exclure l'utilisateur actuel
                        staff.push({ id: doc.id, ...doc.data() });
                    }
                });
                
                displayStaff(staff);
            } catch (error) {
                console.error('Erreur lors du chargement du personnel:', error);
            }
        }

        // Afficher le personnel dans le tableau
        function displayStaff(staffList) {
            const tbody = document.getElementById('staffTableBody');
            tbody.innerHTML = '';
            
            if (staffList.length === 0) {
                tbody.innerHTML = '<tr><td colspan="5" class="py-4 text-center">Aucun membre du personnel trouvé</td></tr>';
                return;
            }
            
            staffList.forEach(person => {
                const tr = document.createElement('tr');
                tr.className = 'border-b hover:bg-gray-50';
                tr.innerHTML = `
                    <td class="py-3 px-4">${person.name}</td>
                    <td class="py-3 px-4">${person.email}</td>
                    <td class="py-3 px-4">
                        <span class="px-2 py-1 rounded text-xs ${getRoleBadgeClass(person.role)}">
                            ${person.role}
                        </span>
                    </td>
                    <td class="py-3 px-4">${new Date(person.createdAt).toLocaleDateString()}</td>
                    <td class="py-3 px-4">
                        <button class="delete-staff text-red-600 hover:text-red-800" data-id="${person.id}">
                            <i class="fas fa-trash"></i>
                        </button>
                    </td>
                `;
                tbody.appendChild(tr);
            });
            
            // Ajouter les événements pour les boutons de suppression
            document.querySelectorAll('.delete-staff').forEach(btn => {
                btn.addEventListener('click', function() {
                    const staffId = this.getAttribute('data-id');
                    deleteStaff(staffId);
                });
            });
        }

        // Obtenir la classe CSS pour le badge de rôle
        function getRoleBadgeClass(role) {
            switch (role) {
                case 'administrateur': return 'bg-purple-100 text-purple-800';
                case 'enseignant': return 'bg-blue-100 text-blue-800';
                case 'secretaire': return 'bg-green-100 text-green-800';
                default: return 'bg-gray-100 text-gray-800';
            }
        }

        // Charger les données du tableau de bord
        async function loadDashboardData() {
            // Charger les élèves
            await loadStudents();
            
            // Charger le personnel
            await loadStaff();
            
            // Mettre à jour les KPI
            document.getElementById('totalStudents').textContent = students.length;
            document.getElementById('totalStaff').textContent = staff.length + 1; // +1 pour l'utilisateur actuel
            
            // Calculer les autres métriques (simulées pour l'exemple)
            document.getElementById('attendanceRate').textContent = '92%';
            document.getElementById('averageGrade').textContent = '12.5';
            
            // Générer les graphiques
            generateCharts();
        }

        // Générer les graphiques du tableau de bord
        function generateCharts() {
            // Répartition par classe
            const classDistribution = {};
            students.forEach(student => {
                classDistribution[student.class] = (classDistribution[student.class] || 0) + 1;
            });
            
            const classCtx = document.getElementById('classDistributionChart').getContext('2d');
            new Chart(classCtx, {
                type: 'bar',
                data: {
                    labels: Object.keys(classDistribution),
                    datasets: [{
                        label: 'Nombre d\'élèves',
                        data: Object.values(classDistribution),
                        backgroundColor: 'rgba(54, 162, 235, 0.5)',
                        borderColor: 'rgba(54, 162, 235, 1)',
                        borderWidth: 1
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: true
                        }
                    }
                }
            });
            
            // Évolution des moyennes (données simulées)
            const gradesCtx = document.getElementById('gradesEvolutionChart').getContext('2d');
            new Chart(gradesCtx, {
                type: 'line',
                data: {
                    labels: ['Trim 1', 'Trim 2', 'Trim 3'],
                    datasets: [{
                        label: 'Moyenne générale',
                        data: [11.2, 12.1, 12.5],
                        borderColor: 'rgba(75, 192, 192, 1)',
                        backgroundColor: 'rgba(75, 192, 192, 0.1)',
                        tension: 0.3,
                        fill: true
                    }]
                },
                options: {
                    responsive: true,
                    scales: {
                        y: {
                            beginAtZero: false,
                            min: 10
                        }
                    }
                }
            });
        }

        // Charger les notes
        async function loadGrades() {
            const selectedClass = document.getElementById('selectClassGrades').value;
            const trimester = document.getElementById('selectTrimester').value;
            
            if (!selectedClass) {
                showNotification('Veuillez sélectionner une classe', 'warning');
                return;
            }
            
            showLoading();
            
            try {
                // Filtrer les élèves par classe
                const classStudents = students.filter(student => student.class === selectedClass);
                
                // Charger les notes depuis Firestore (simulé pour l'exemple)
                const gradesData = await simulateLoadGrades(classStudents, trimester);
                
                // Afficher les notes
                displayGrades(classStudents, gradesData);
                document.getElementById('gradesContent').classList.remove('hidden');
            } catch (error) {
                console.error('Erreur lors du chargement des notes:', error);
                showNotification('Erreur lors du chargement des notes', 'error');
            } finally {
                hideLoading();
            }
        }

        // Simuler le chargement des notes
        async function simulateLoadGrades(students, trimester) {
            // Dans une application réelle, cela chargerait les notes depuis Firestore
            return new Promise(resolve => {
                setTimeout(() => {
                    const grades = {};
                    students.forEach(student => {
                        grades[student.id] = {};
                        subjects.forEach(subject => {
                            grades[student.id][subject] = {
                                controle: Math.floor(Math.random() * 10) + 10, // 10-20
                                devoir: Math.floor(Math.random() * 10) + 10,
                                examen: Math.floor(Math.random() * 10) + 10
                            };
                        });
                    });
                    resolve(grades);
                }, 1000);
            });
        }

        // Afficher les notes dans le tableau
        function displayGrades(students, gradesData) {
            const tbody = document.getElementById('gradesTableBody');
            tbody.innerHTML = '';
            
            students.forEach(student => {
                const studentGrades = gradesData[student.id] || {};
                let rowHTML = `<tr class="border-b">
                    <td class="py-2 px-4 border font-medium">${student.lastName} ${student.firstName}</td>`;
                
                let totalAverage = 0;
                let subjectCount = 0;
                
                subjects.forEach(subject => {
                    const subjectGrades = studentGrades[subject] || { controle: 0, devoir: 0, examen: 0 };
                    const average = calculateSubjectAverage(subjectGrades);
                    totalAverage += average;
                    subjectCount++;
                    
                    rowHTML += `<td class="py-2 px-4 border">
                        <input type="number" min="0" max="20" step="0.5" 
                            value="${subjectGrades.controle}" 
                            class="grade-input w-12 text-center border rounded" 
                            data-student="${student.id}" data-subject="${subject}" data-type="controle">
                        <input type="number" min="0" max="20" step="0.5" 
                            value="${subjectGrades.devoir}" 
                            class="grade-input w-12 text-center border rounded ml-1" 
                            data-student="${student.id}" data-subject="${subject}" data-type="devoir">
                        <input type="number" min="0" max="20" step="0.5" 
                            value="${subjectGrades.examen}" 
                            class="grade-input w-12 text-center border rounded ml-1" 
                            data-student="${student.id}" data-subject="${subject}" data-type="examen">
                        <span class="ml-2 font-medium">${average.toFixed(2)}</span>
                    </td>`;
                });
                
                const overallAverage = subjectCount > 0 ? (totalAverage / subjectCount).toFixed(2) : '0.00';
                rowHTML += `<td class="py-2 px-4 border font-bold">${overallAverage}</td></tr>`;
                
                tbody.innerHTML += rowHTML;
            });
            
            // Ajouter les événements pour les champs de note
            document.querySelectorAll('.grade-input').forEach(input => {
                input.addEventListener('change', function() {
                    updateGrade(
                        this.getAttribute('data-student'),
                        this.getAttribute('data-subject'),
                        this.getAttribute('data-type'),
                        parseFloat(this.value)
                    );
                });
            });
        }

        // Calculer la moyenne d'une matière
        function calculateSubjectAverage(grades) {
            // Pondérations: contrôle 30%, devoir 30%, examen 40%
            return (grades.controle * 0.3) + (grades.devoir * 0.3) + (grades.examen * 0.4);
        }

        // Mettre à jour une note
        async function updateGrade(studentId, subject, type, value) {
            try {
                // Dans une application réelle, cela mettrait à jour Firestore
                console.log(`Mise à jour note: ${studentId}, ${subject}, ${type}, ${value}`);
                showNotification('Note mise à jour', 'success');
            } catch (error) {
                console.error('Erreur lors de la mise à jour de la note:', error);
                showNotification('Erreur lors de la mise à jour de la note', 'error');
            }
        }

        // Charger l'assiduité
        async function loadAttendance() {
            const selectedClass = document.getElementById('selectClassAttendance').value;
            const date = document.getElementById('attendanceDate').value;
            
            if (!selectedClass) {
                showNotification('Veuillez sélectionner une classe', 'warning');
                return;
            }
            
            showLoading();
            
            try {
                // Filtrer les élèves par classe
                const classStudents = students.filter(student => student.class === selectedClass);
                
                // Charger l'assiduité depuis Firestore (simulé pour l'exemple)
                const attendanceData = await simulateLoadAttendance(classStudents, date);
                
                // Afficher l'assiduité
                displayAttendance(classStudents, attendanceData);
                document.getElementById('attendanceContent').classList.remove('hidden');
                
                // Mettre à jour les statistiques
                updateAttendanceStats(attendanceData);
            } catch (error) {
                console.error('Erreur lors du chargement de l\'assiduité:', error);
                showNotification('Erreur lors du chargement de l\'assiduité', 'error');
            } finally {
                hideLoading();
            }
        }

        // Simuler le chargement de l'assiduité
        async function simulateLoadAttendance(students, date) {
            // Dans une application réelle, cela chargerait l'assiduité depuis Firestore
            return new Promise(resolve => {
                setTimeout(() => {
                    const attendance = {};
                    students.forEach(student => {
                        // Par défaut, tous les élèves sont présents
                        attendance[student.id] = {
                            status: 'present',
                            hoursMissed: 0,
                            notes: ''
                        };
                    });
                    resolve(attendance);
                }, 1000);
            });
        }

        // Afficher l'assiduité dans le tableau
        function displayAttendance(students, attendanceData) {
            const tbody = document.getElementById('attendanceTableBody');
            tbody.innerHTML = '';
            
            students.forEach(student => {
                const studentAttendance = attendanceData[student.id] || { status: 'present', hoursMissed: 0, notes: '' };
                
                const tr = document.createElement('tr');
                tr.className = 'border-b';
                tr.innerHTML = `
                    <td class="py-2 px-4 border">${student.lastName} ${student.firstName}</td>
                    <td class="py-2 px-4 border">
                        <select class="attendance-status w-full p-1 border rounded" data-student="${student.id}">
                            <option value="present" ${studentAttendance.status === 'present' ? 'selected' : ''}>Présent</option>
                            <option value="absent" ${studentAttendance.status === 'absent' ? 'selected' : ''}>Absent</option>
                            <option value="late" ${studentAttendance.status === 'late' ? 'selected' : ''}>Retard</option>
                        </select>
                    </td>
                    <td class="py-2 px-4 border">
                        <input type="number" min="0" max="8" value="${studentAttendance.hoursMissed}" 
                            class="hours-missed w-full p-1 border rounded" data-student="${student.id}">
                    </td>
                    <td class="py-2 px-4 border">
                        <input type="text" value="${studentAttendance.notes}" 
                            class="attendance-notes w-full p-1 border rounded" data-student="${student.id}" 
                            placeholder="Remarques...">
                    </td>
                `;
                tbody.appendChild(tr);
            });
            
            // Ajouter les événements pour les champs d'assiduité
            document.querySelectorAll('.attendance-status').forEach(select => {
                select.addEventListener('change', function() {
                    updateAttendanceStatus(
                        this.getAttribute('data-student'),
                        this.value
                    );
                });
            });
            
            document.querySelectorAll('.hours-missed').forEach(input => {
                input.addEventListener('change', function() {
                    updateHoursMissed(
                        this.getAttribute('data-student'),
                        parseInt(this.value)
                    );
                });
            });
        }

        // Marquer tous les élèves comme présents
        function markAllPresent() {
            document.querySelectorAll('.attendance-status').forEach(select => {
                select.value = 'present';
                updateAttendanceStatus(select.getAttribute('data-student'), 'present');
            });
            
            document.querySelectorAll('.hours-missed').forEach(input => {
                input.value = 0;
                updateHoursMissed(input.getAttribute('data-student'), 0);
            });
            
            showNotification('Tous les élèves marqués comme présents', 'success');
        }

        // Mettre à jour le statut de présence
        function updateAttendanceStatus(studentId, status) {
            // Dans une application réelle, cela mettrait à jour Firestore
            console.log(`Mise à jour statut: ${studentId}, ${status}`);
        }

        // Mettre à jour les heures manquées
        function updateHoursMissed(studentId, hours) {
            // Dans une application réelle, cela mettrait à jour Firestore
            console.log(`Mise à jour heures manquées: ${studentId}, ${hours}`);
        }

        // Sauvegarder l'assiduité
        async function saveAttendance() {
            showLoading();
            
            try {
                // Dans une application réelle, cela sauvegarderait dans Firestore
                await new Promise(resolve => setTimeout(resolve, 1000));
                showNotification('Assiduité sauvegardée avec succès', 'success');
            } catch (error) {
                console.error('Erreur lors de la sauvegarde de l\'assiduité:', error);
                showNotification('Erreur lors de la sauvegarde de l\'assiduité', 'error');
            } finally {
                hideLoading();
            }
        }

        // Mettre à jour les statistiques d'assiduité
        function updateAttendanceStats(attendanceData) {
            let presentCount = 0;
            let absentCount = 0;
            let lateCount = 0;
            
            Object.values(attendanceData).forEach(attendance => {
                if (attendance.status === 'present') presentCount++;
                else if (attendance.status === 'absent') absentCount++;
                else if (attendance.status === 'late') lateCount++;
            });
            
            document.getElementById('presentToday').textContent = presentCount;
            document.getElementById('absentToday').textContent = absentCount;
            document.getElementById('lateToday').textContent = lateCount;
        }

        // Générer les bulletins
        async function generateReportCards() {
            showLoading();
            
            try {
                // Dans une application réelle, cela générerait les bulletins PDF
                await new Promise(resolve => setTimeout(resolve, 2000));
                showNotification('Bulletins générés avec succès', 'success');
                
                // Télécharger un exemple de bulletin
                const { jsPDF } = window.jspdf;
                const doc = new jsPDF();
                
                doc.setFontSize(20);
                doc.text('BULLETIN SCOLAIRE', 105, 20, { align: 'center' });
                
                doc.setFontSize(12);
                doc.text('École: SchoolManager Pro', 20, 40);
                doc.text('Classe: ' + document.getElementById('selectClassGrades').value, 20, 50);
                doc.text('Trimestre: ' + document.getElementById('selectTrimester').value, 20, 60);
                doc.text('Date: ' + new Date().toLocaleDateString(), 20, 70);
                
                doc.line(20, 80, 190, 80);
                
                doc.text('Ceci est un exemple de bulletin. Dans une application réelle,', 20, 100);
                doc.text('le bulletin contiendrait les notes, moyennes, classement,', 20, 110);
                doc.text('absences et appréciations de chaque élève.', 20, 120);
                
                doc.save('exemple_bulletin.pdf');
            } catch (error) {
                console.error('Erreur lors de la génération des bulletins:', error);
                showNotification('Erreur lors de la génération des bulletins', 'error');
            } finally {
                hideLoading();
            }
        }

        // Planificateur quotidien pour l'assiduité
        function setupDailyScheduler() {
            // Vérifier si nous avons déjà initialisé aujourd'hui
            const lastInitialized = localStorage.getItem('lastInitialized');
            const today = new Date().toISOString().split('T')[0];
            
            if (lastInitialized !== today) {
                initializeNewDay();
                localStorage.setItem('lastInitialized', today);
            }
            
            // Planifier pour le lendemain
            const now = new Date();
            const tomorrow = new Date(now);
            tomorrow.setDate(tomorrow.getDate() + 1);
            tomorrow.setHours(0, 0, 0, 0);
            
            const timeUntilTomorrow = tomorrow.getTime() - now.getTime();
            
            setTimeout(() => {
                initializeNewDay();
                // Répéter chaque jour
                setInterval(initializeNewDay, 24 * 60 * 60 * 1000);
            }, timeUntilTomorrow);
        }

        // Initialiser un nouveau jour
        function initializeNewDay() {
            // Sauvegarder les données de la veille
            saveDailyAttendance();
            
            // Initialiser la présence du jour
            initializeDailyAttendance();
            
            console.log("Nouveau jour initialisé avec succès");
        }

        // Initialiser la présence quotidienne par défaut
        function initializeDailyAttendance() {
            const today = new Date().toISOString().split('T')[0];
            
            // Dans une application réelle, cela initialiserait l'assiduité pour tous les élèves
            console.log(`Initialisation de l'assiduité pour le ${today}`);
        }

        // Sauvegarder l'assiduité quotidienne
        function saveDailyAttendance() {
            const yesterday = new Date();
            yesterday.setDate(yesterday.getDate() - 1);
            const yesterdayStr = yesterday.toISOString().split('T')[0];
            
            // Dans une application réelle, cela sauvegarderait l'assiduité de la veille
            console.log(`Sauvegarde de l'assiduité pour le ${yesterdayStr}`);
        }

        // Sauvegarder les informations de l'école
        async function saveSchoolInfo(e) {
            e.preventDefault();
            showLoading();
            
            try {
                // Dans une application réelle, cela sauvegarderait dans Firestore
                await new Promise(resolve => setTimeout(resolve, 1000));
                showNotification('Informations de l\'école sauvegardées', 'success');
            } catch (error) {
                console.error('Erreur lors de la sauvegarde des informations:', error);
                showNotification('Erreur lors de la sauvegarde des informations', 'error');
            } finally {
                hideLoading();
            }
        }

        // Sauvegarder la configuration académique
        async function saveAcademicConfig(e) {
            e.preventDefault();
            showLoading();
            
            try {
                // Dans une application réelle, cela sauvegarderait dans Firestore
                await new Promise(resolve => setTimeout(resolve, 1000));
                showNotification('Configuration académique sauvegardée', 'success');
            } catch (error) {
                console.error('Erreur lors de la sauvegarde de la configuration:', error);
                showNotification('Erreur lors de la sauvegarde de la configuration', 'error');
            } finally {
                hideLoading();
            }
        }

        // Exporter les rapports
        function exportReports() {
            showNotification('Fonctionnalité d\'export en cours de développement', 'info');
        }

        // Générer le rapport général
        function generateSummaryReport() {
            showNotification('Fonctionnalité de rapport général en cours de développement', 'info');
        }

        // Afficher une notification
        function showNotification(message, type = 'info') {
            // Créer l'élément de notification
            const notification = document.createElement('div');
            notification.className = `fixed top-4 right-4 p-4 rounded-lg shadow-lg z-50 fade-in ${
                type === 'success' ? 'bg-green-100 text-green-800 border border-green-200' :
                type === 'error' ? 'bg-red-100 text-red-800 border border-red-200' :
                type === 'warning' ? 'bg-yellow-100 text-yellow-800 border border-yellow-200' :
                'bg-blue-100 text-blue-800 border border-blue-200'
            }`;
            
            notification.innerHTML = `
                <div class="flex items-center">
                    <i class="fas ${
                        type === 'success' ? 'fa-check-circle' :
                        type === 'error' ? 'fa-exclamation-circle' :
                        type === 'warning' ? 'fa-exclamation-triangle' :
                        'fa-info-circle'
                    } mr-2"></i>
                    <span>${message}</span>
                </div>
            `;
            
            document.body.appendChild(notification);
            
            // Supprimer la notification après 5 secondes
            setTimeout(() => {
                notification.remove();
            }, 5000);
        }

        // Afficher l'indicateur de chargement
        function showLoading() {
            document.getElementById('loadingSpinner').classList.remove('hidden');
        }

        // Masquer l'indicateur de chargement
        function hideLoading() {
            document.getElementById('loadingSpinner').classList.add('hidden');
        }

        // Fonctions à implémenter (placeholders)
        function viewStudent(studentId) {
            showNotification('Visualisation de l\'élève en cours de développement', 'info');
        }

        function editStudent(studentId) {
            showNotification('Modification de l\'élève en cours de développement', 'info');
        }

        function deleteStudent(studentId) {
            if (confirm('Êtes-vous sûr de vouloir supprimer cet élève ?')) {
                showNotification('Suppression de l\'élève en cours de développement', 'info');
            }
        }

        function deleteStaff(staffId) {
            if (confirm('Êtes-vous sûr de vouloir supprimer ce membre du personnel ?')) {
                showNotification('Suppression du personnel en cours de développement', 'info');
            }
        }
    </script>
</body>
