<svelte:head>
    <title>CRIOTEC - Laboratorio ISO/IEC 17025</title>
</svelte:head>

<div class="py-8 sm:py-12 px-4 sm:px-6 lg:px-8 max-w-7xl mx-auto w-full">
    <div class="text-center max-w-3xl mx-auto mb-8 sm:mb-10">
        <p class="text-[11px] sm:text-xs font-bold text-cyan-400 tracking-widest uppercase mb-2">PLATAFORMA METROLÓGICA CRIOTEC</p>
        <h1 class="text-2xl sm:text-4xl font-extrabold text-white">Módulos Especializados por Cliente y Normativa</h1>
        <p class="text-slate-400 text-xs sm:text-sm mt-2">Seleccione el área de pruebas o normativa correspondiente para realizar evaluaciones y generar informes de laboratorio.</p>
    </div>

    <div class="grid grid-cols-1 sm:grid-cols-2 lg:grid-cols-4 gap-4 sm:gap-6">
        
        <!-- Módulo 1: NOM-022-ENER -->
        <a href="/etiqueta" class="bg-[#0f172a]/75 backdrop-blur-md p-5 sm:p-6 rounded-2xl border border-slate-800 hover:border-cyan-500/50 transition-all duration-300 text-center group flex flex-col justify-between hover:-translate-y-1">
            <div>
                <div class="w-14 h-14 sm:w-16 sm:h-16 mx-auto flex items-center justify-center mb-3 sm:mb-4 group-hover:scale-110 transition-transform">
                    <img src="/logo-nom.png" alt="Logo NOM" class="h-10 sm:h-12 w-auto object-contain" />
                </div>
                <h2 class="text-base sm:text-lg font-bold text-white mb-1">NOM-022-ENER</h2>
                <p class="text-xs text-slate-400 leading-relaxed mb-4">Eficiencia energética para enfriadores comerciales y cálculo de límites FIDE.</p>
            </div>
            <span class="inline-flex items-center justify-center text-xs font-semibold text-cyan-400 group-hover:text-cyan-300">
                Acceder al módulo <i class="fa-solid fa-arrow-right ml-1.5 text-[10px]"></i>
            </span>
        </a>

        <!-- Módulo 2: Coca-Cola System -->
        <div class="bg-[#0f172a]/75 backdrop-blur-md p-5 sm:p-6 rounded-2xl border border-slate-800 hover:border-red-500/50 transition-all duration-300 text-center group flex flex-col justify-between hover:-translate-y-1 cursor-pointer">
            <div>
                <div class="w-14 h-14 sm:w-16 sm:h-16 mx-auto flex items-center justify-center mb-3 sm:mb-4 group-hover:scale-110 transition-transform">
                    <img src="/logo-coca-cola.png" alt="Logo Coca-Cola" class="h-10 sm:h-12 w-auto object-contain" />
                </div>
                <h2 class="text-base sm:text-lg font-bold text-white mb-1">Coca-Cola System</h2>
                <p class="text-xs text-slate-400 leading-relaxed mb-4">Protocolos de ensayo KORE y homologación corporativa para equipos de refrigeración.</p>
            </div>
            <span class="inline-flex items-center justify-center text-xs font-semibold text-red-400 group-hover:text-red-300">
                Ver especificaciones <i class="fa-solid fa-arrow-right ml-1.5 text-[10px]"></i>
            </span>
        </div>

        <!-- Módulo 3: PepsiCo Global -->
        <div class="bg-[#0f172a]/75 backdrop-blur-md p-5 sm:p-6 rounded-2xl border border-slate-800 hover:border-blue-500/50 transition-all duration-300 text-center group flex flex-col justify-between hover:-translate-y-1 cursor-pointer">
            <div>
                <div class="w-14 h-14 sm:w-16 sm:h-16 mx-auto flex items-center justify-center mb-3 sm:mb-4 group-hover:scale-110 transition-transform">
                    <img src="/logo-pepsico.png" alt="Logo PepsiCo" class="h-10 sm:h-12 w-auto object-contain" />
                </div>
                <h2 class="text-base sm:text-lg font-bold text-white mb-1">PepsiCo Global</h2>
                <p class="text-xs text-slate-400 leading-relaxed mb-4">Estándares globales de enfriadores, pruebas de empaque y desempeño térmico.</p>
            </div>
            <span class="inline-flex items-center justify-center text-xs font-semibold text-blue-400 group-hover:text-blue-300">
                Ver especificaciones <i class="fa-solid fa-arrow-right ml-1.5 text-[10px]"></i>
            </span>
        </div>

        <!-- Módulo 4: Energy Star -->
        <div class="bg-[#0f172a]/75 backdrop-blur-md p-5 sm:p-6 rounded-2xl border border-slate-800 hover:border-sky-400/50 transition-all duration-300 text-center group flex flex-col justify-between hover:-translate-y-1 cursor-pointer">
            <div>
                <div class="w-14 h-14 sm:w-16 sm:h-16 mx-auto flex items-center justify-center mb-3 sm:mb-4 group-hover:scale-110 transition-transform">
                    <img src="/logo-energy-star.png" alt="Logo Energy Star" class="h-10 sm:h-12 w-auto object-contain" />
                </div>
                <h2 class="text-base sm:text-lg font-bold text-white mb-1">Energy Star</h2>
                <p class="text-xs text-slate-400 leading-relaxed mb-4">Certificación de eficiencia energética bajo metodologías reconocidas por la EPA.</p>
            </div>
            <span class="inline-flex items-center justify-center text-xs font-semibold text-sky-400 group-hover:text-sky-300">
                Ver especificaciones <i class="fa-solid fa-arrow-right ml-1.5 text-[10px]"></i>
            </span>
        </div>

    </div>
</div>