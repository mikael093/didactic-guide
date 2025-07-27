<!DOCTYPE html>
<html lang="pt-BR">
<head>
    <meta charset="UTF-8">
    <meta name="viewport" content="width=device-width, initial-scale=1.0">
    <title>Voz da Comunidade v2</title>
    <script src="https://cdn.tailwindcss.com"></script>
    <link href="https://fonts.googleapis.com/css2?family=Montserrat:wght@400;500;600;700&display=swap" rel="stylesheet">
    <style>
        body {
            font-family: 'Montserrat', sans-serif;
            background-color: #f8fafc;
        }
        .recording-pulse {
            animation: pulse 1.5s infinite;
        }
        @keyframes pulse {
            0% {
                transform: scale(1);
                opacity: 1;
            }
            50% {
                transform: scale(1.1);
                opacity: 0.8;
            }
            100% {
                transform: scale(1);
                opacity: 1;
            }
        }
        .map-marker {
            width: 20px;
            height: 20px;
            background-color: #ef4444;
            border-radius: 50%;
            position: absolute;
            transform: translate(-50%, -50%);
        }
        .map-marker::after {
            content: '';
            position: absolute;
            top: 50%;
            left: 50%;
            transform: translate(-50%, -50%);
            width: 10px;
            height: 10px;
            background-color: white;
            border-radius: 50%;
        }
        .map-marker.green {
            background-color: #10b981;
        }
        .map-marker.yellow {
            background-color: #f59e0b;
        }
        .category-pill {
            transition: all 0.3s ease;
        }
        .category-pill.active {
            background-color: #2563eb;
            color: white;
        }
        .tab-content {
            display: none;
        }
        .tab-content.active {
            display: block;
        }
        .slide-in {
            animation: slideIn 0.3s forwards;
        }
        @keyframes slideIn {
            from {
                transform: translateY(100%);
                opacity: 0;
            }
            to {
                transform: translateY(0);
                opacity: 1;
            }
        }
    </style>
</head>
<body>
    <div id="app" class="max-w-md mx-auto bg-white min-h-screen shadow-lg pb-20">
        <!-- Header -->
        <header class="bg-blue-600 text-white p-4 flex justify-between items-center sticky top-0 z-10">
            <div>
                <h1 class="text-xl font-bold">Voz da Comunidade</h1>
                <p class="text-sm">Você fala, a cidade muda.</p>
            </div>
            <div class="flex items-center space-x-2">
                <button id="notifications-btn" class="p-2 rounded-full bg-blue-700 text-white">
                    <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                        <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M15 17h5l-1.405-1.405A2.032 2.032 0 0118 14.158V11a6.002 6.002 0 00-4-5.659V5a2 2 0 10-4 0v.341C7.67 6.165 6 8.388 6 11v3.159c0 .538-.214 1.055-.595 1.436L4 17h5m6 0v1a3 3 0 11-6 0v-1m6 0H9" />
                    </svg>
                </button>
                <div class="rounded-full bg-white h-10 w-10 flex items-center justify-center">
                    <span class="text-blue-600 font-bold">m</span>
                </div>
            </div>
        </header>

        <!-- Main Content -->
        <main>
            <!-- Home Screen -->
            <div id="home-screen" class="p-4">
                <div class="bg-blue-50 rounded-lg p-4 mb-6">
                    <h2 class="font-semibold text-blak-800">Olá,Bem-vindo, MIKAEL!</h2>
                    <p class="text-sm text-blak-600">Ajude a melhorar sua cidade reportando problemas.</p>
                    <div class="mt-3 flex items-center">
                        <div class="bg-blue-100 rounded-full p-1 mr-2">
                            <svg xmlns="http://www.w3.org/2000/svg" class="h-4 w-4 text-blue-600" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M13 16h-1v-4h-1m1-4h.01M21 12a9 9 0 11-18 0 9 9 0 0118 0z" />
                            </svg>
                        </div>
                        <p class="text-xs text-blak-700">Você já contribuiu com 5 denúncias este mês!</p>
                    </div>
                </div>

                <!-- Category Filter -->
                <div class="mb-6 overflow-x-auto">
                    <div class="flex space-x-2 pb-2">
                        <button class="category-pill active whitespace-nowrap px-4 py-2 bg-gray-100 text-gray-800 rounded-full text-sm font-medium">
                            Todos
                        </button>
                        <button class="category-pill whitespace-nowrap px-4 py-2 bg-gray-100 text-gray-800 rounded-full text-sm font-medium">
                            Infraestrutura
                        </button>
                        <button class="category-pill whitespace-nowrap px-4 py-2 bg-gray-100 text-gray-800 rounded-full text-sm font-medium">
                            Segurança
                        </button>
                        <button class="category-pill whitespace-nowrap px-4 py-2 bg-gray-100 text-gray-800 rounded-full text-sm font-medium">
                            Meio Ambiente
                        </button>
                        <button class="category-pill whitespace-nowrap px-4 py-2 bg-gray-100 text-gray-800 rounded-full text-sm font-medium">
                            Saúde
                        </button>
                    </div>
                </div>

                <!-- Map Section -->
                <div class="mb-6">
                    <div class="flex justify-between items-center mb-2">
                        <h3 class="font-semibold text-gray-700">Problemas próximos a você</h3>
                        <button id="expand-map-btn" class="text-blak-600 text-sm font-medium">Ver mapa completo</button>
                    </div>
                    <div class="relative h-48 bg-gray-200 rounded-lg overflow-hidden mb-2">
                        <div id="map" class="w-full h-full bg-blue-100 relative">
                            <!-- Simplified map representation -->
                            <div class="map-marker" style="top: 30%; left: 40%;" title="Buraco na rua"></div>
                            <div class="map-marker yellow" style="top: 50%; left: 60%;" title="Iluminação quebrada"></div>
                            <div class="map-marker" style="top: 70%; left: 30%;" title="Lixo acumulado"></div>
                            <div class="map-marker green" style="top: 40%; left: 70%;" title="Problema resolvido"></div>
                            <div class="map-marker yellow" style="top: 60%; left: 20%;" title="Em análise"></div>
                        </div>
                        <div class="absolute bottom-2 right-2 bg-white rounded-lg shadow p-2 flex space-x-2">
                            <div class="flex items-center">
                                <div class="w-3 h-3 rounded-full bg-red-500 mr-1"></div>
                                <span class="text-xs">Pendente</span>
                            </div>
                            <div class="flex items-center">
                                <div class="w-3 h-3 rounded-full bg-yellow-500 mr-1"></div>
                                <span class="text-xs">Em análise</span>
                            </div>
                            <div class="flex items-center">
                                <div class="w-3 h-3 rounded-full bg-green-500 mr-1"></div>
                                <span class="text-xs">Resolvido</span>
                            </div>
                        </div>
                    </div>
                    <p class="text-xs text-gray-500">12 problemas reportados em sua área</p>
                </div>

                <!-- Tabs -->
                <div class="mb-4">
                    <div class="flex border-b border-gray-200">
                        <button class="tab-btn flex-1 py-2 font-medium text-blue-600 border-b-2 border-blue-600" data-tab="recent">
                            Recentes
                        </button>
                        <button class="tab-btn flex-1 py-2 font-medium text-gray-500" data-tab="popular">
                            Populares
                        </button>
                        <button class="tab-btn flex-1 py-2 font-medium text-gray-500" data-tab="mine">
                            Minhas
                        </button>
                    </div>
                </div>

                <!-- Recent Tab Content -->
                <div id="recent-tab" class="tab-content active">
                    <div class="space-y-4">
                        <div class="bg-white border border-gray-200 rounded-lg p-3 shadow-sm">
                            <div class="flex justify-between">
                                <span class="bg-yellow-100 text-yellow-800 text-xs px-2 py-1 rounded-full">Infraestrutura</span>
                                <span class="text-xs text-gray-500">Há 2h</span>
                            </div>
                            <p class="text-sm my-2">Buraco na Rua das Flores próximo ao número 123</p>
                            <div class="flex justify-between items-center">
                                <span class="bg-orange-100 text-orange-800 text-xs px-2 py-1 rounded-full">Em análise</span>
                                <div class="flex items-center">
                                    <button class="text-gray-400 hover:text-blue-500">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10h4.764a2 2 0 011.789 2.894l-3.5 7A2 2 0 0115.263 21h-4.017c-.163 0-.326-.02-.485-.06L7 20m7-10V5a2 2 0 00-2-2h-.095c-.5 0-.905.405-.905.905 0 .714-.211 1.412-.608 2.006L7 11v9m7-10h-2M7 20H5a2 2 0 01-2-2v-6a2 2 0 012-2h2.5" />
                                        </svg>
                                    </button>
                                    <span class="text-sm ml-1">23</span>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white border border-gray-200 rounded-lg p-3 shadow-sm">
                            <div class="flex justify-between">
                                <span class="bg-red-100 text-red-800 text-xs px-2 py-1 rounded-full">Segurança</span>
                                <span class="text-xs text-gray-500">Há 5h</span>
                            </div>
                            <p class="text-sm my-2">Iluminação quebrada na Praça Central</p>
                            <div class="flex justify-between items-center">
                                <span class="bg-orange-100 text-orange-800 text-xs px-2 py-1 rounded-full">Em análise</span>
                                <div class="flex items-center">
                                    <button class="text-blue-500">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10h4.764a2 2 0 011.789 2.894l-3.5 7A2 2 0 0115.263 21h-4.017c-.163 0-.326-.02-.485-.06L7 20m7-10V5a2 2 0 00-2-2h-.095c-.5 0-.905.405-.905.905 0 .714-.211 1.412-.608 2.006L7 11v9m7-10h-2M7 20H5a2 2 0 01-2-2v-6a2 2 0 012-2h2.5" />
                                        </svg>
                                    </button>
                                    <span class="text-sm ml-1">47</span>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white border border-gray-200 rounded-lg p-3 shadow-sm">
                            <div class="flex justify-between">
                                <span class="bg-green-100 text-green-800 text-xs px-2 py-1 rounded-full">Meio Ambiente</span>
                                <span class="text-xs text-gray-500">Há 8h</span>
                            </div>
                            <p class="text-sm my-2">Lixo acumulado na esquina da Rua dos Pinheiros com Av. Brasil</p>
                            <div class="flex justify-between items-center">
                                <span class="bg-red-100 text-red-800 text-xs px-2 py-1 rounded-full">Pendente</span>
                                <div class="flex items-center">
                                    <button class="text-gray-400 hover:text-blue-500">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10h4.764a2 2 0 011.789 2.894l-3.5 7A2 2 0 0115.263 21h-4.017c-.163 0-.326-.02-.485-.06L7 20m7-10V5a2 2 0 00-2-2h-.095c-.5 0-.905.405-.905.905 0 .714-.211 1.412-.608 2.006L7 11v9m7-10h-2M7 20H5a2 2 0 01-2-2v-6a2 2 0 012-2h2.5" />
                                        </svg>
                                    </button>
                                    <span class="text-sm ml-1">12</span>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white border border-gray-200 rounded-lg p-3 shadow-sm">
                            <div class="flex justify-between">
                                <span class="bg-blue-100 text-blue-800 text-xs px-2 py-1 rounded-full">Saúde</span>
                                <span class="text-xs text-gray-500">Há 12h</span>
                            </div>
                            <p class="text-sm my-2">Água parada em terreno baldio na Rua das Palmeiras, 456</p>
                            <div class="flex justify-between items-center">
                                <span class="bg-green-100 text-green-800 text-xs px-2 py-1 rounded-full">Resolvido</span>
                                <div class="flex items-center">
                                    <button class="text-blue-500">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10h4.764a2 2 0 011.789 2.894l-3.5 7A2 2 0 0115.263 21h-4.017c-.163 0-.326-.02-.485-.06L7 20m7-10V5a2 2 0 00-2-2h-.095c-.5 0-.905.405-.905.905 0 .714-.211 1.412-.608 2.006L7 11v9m7-10h-2M7 20H5a2 2 0 01-2-2v-6a2 2 0 012-2h2.5" />
                                        </svg>
                                    </button>
                                    <span class="text-sm ml-1">35</span>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white border border-gray-200 rounded-lg p-3 shadow-sm">
                            <div class="flex justify-between">
                                <span class="bg-purple-100 text-purple-800 text-xs px-2 py-1 rounded-full">Transporte</span>
                                <span class="text-xs text-gray-500">Há 1d</span>
                            </div>
                            <p class="text-sm my-2">Ponto de ônibus danificado na Av. Principal, próximo ao Shopping</p>
                            <div class="flex justify-between items-center">
                                <span class="bg-orange-100 text-orange-800 text-xs px-2 py-1 rounded-full">Em análise</span>
                                <div class="flex items-center">
                                    <button class="text-gray-400 hover:text-blue-500">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10h4.764a2 2 0 011.789 2.894l-3.5 7A2 2 0 0115.263 21h-4.017c-.163 0-.326-.02-.485-.06L7 20m7-10V5a2 2 0 00-2-2h-.095c-.5 0-.905.405-.905.905 0 .714-.211 1.412-.608 2.006L7 11v9m7-10h-2M7 20H5a2 2 0 01-2-2v-6a2 2 0 012-2h2.5" />
                                        </svg>
                                    </button>
                                    <span class="text-sm ml-1">19</span>
                                </div>
                            </div>
                        </div>
                    </div>
                </div>

                <!-- Popular Tab Content -->
                <div id="popular-tab" class="tab-content">
                    <div class="space-y-4">
                        <div class="bg-white border border-gray-200 rounded-lg p-3 shadow-sm">
                            <div class="flex justify-between">
                                <span class="bg-red-100 text-red-800 text-xs px-2 py-1 rounded-full">Segurança</span>
                                <span class="text-xs text-gray-500">Há 5h</span>
                            </div>
                            <p class="text-sm my-2">Iluminação quebrada na Praça Central</p>
                            <div class="flex justify-between items-center">
                                <span class="bg-orange-100 text-orange-800 text-xs px-2 py-1 rounded-full">Em análise</span>
                                <div class="flex items-center">
                                    <button class="text-blue-500">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10h4.764a2 2 0 011.789 2.894l-3.5 7A2 2 0 0115.263 21h-4.017c-.163 0-.326-.02-.485-.06L7 20m7-10V5a2 2 0 00-2-2h-.095c-.5 0-.905.405-.905.905 0 .714-.211 1.412-.608 2.006L7 11v9m7-10h-2M7 20H5a2 2 0 01-2-2v-6a2 2 0 012-2h2.5" />
                                        </svg>
                                    </button>
                                    <span class="text-sm ml-1">47</span>
                                </div>
                            </div>
                        </div>

                        <div class="bg-white border border-gray-200 rounded-lg p-3 shadow-sm">
                            <div class="flex justify-between">
                                <span class="bg-blue-100 text-blue-800 text-xs px-2 py-1 rounded-full">Saúde</span>
                                <span class="text-xs text-gray-500">Há 12h</span>
                            </div>
                            <p class="text-sm my-2">Água parada em terreno baldio na Rua das Palmeiras, 456</p>
                            <div class="flex justify-between items-center">
                                <span class="bg-green-100 text-green-800 text-xs px-2 py-1 rounded-full">Resolvido</span>
                                <div class="flex items-center">
                                    <button class="text-blue-500">
                                        <svg xmlns="http://www.w3.org/2000/svg" class="h-5 w-5" fill="none" viewBox="0 0 24 24" stroke="currentColor">
                                            <path stroke-linecap="round" stroke-linejoin="round" stroke-width="2" d="M14 10h4.764a2     
