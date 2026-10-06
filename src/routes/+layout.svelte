<script lang="ts">
    import '../app.css';

    let openDropdown: string | null = null;

    function toggleDropdown(id: string, e: Event) {
        e.stopPropagation();
        openDropdown = openDropdown === id ? null : id;
    }

    function closeDropdowns() {
        openDropdown = null;
    }
</script>

<svelte:window on:click={closeDropdowns} />

<!-- CONTENEDOR CON TEXTURA Y DESTELLOS DE FONDO -->
<div class="min-h-screen bg-[#030e21] bg-tech-pattern text-slate-100 flex flex-col font-sans relative overflow-x-hidden">
    
    <!-- EFECTO DE RESPLANDOR AZUL EN EL FONDO -->
    <div class="pointer-events-none fixed top-0 left-1/2 -translate-x-1/2 w-[320px] sm:w-[800px] h-[200px] sm:h-[400px] bg-cyan-500/10 blur-[80px] sm:blur-[130px] rounded-full z-0"></div>
    <div class="pointer-events-none fixed top-1/3 right-0 w-[200px] sm:w-[400px] h-[200px] sm:h-[400px] bg-blue-600/10 blur-[100px] sm:blur-[150px] rounded-full z-0"></div>

    <!-- FRANJA SUPERIOR (ISO/IEC 17025 VIGENTE) -->
    <div class="bg-[#020712]/90 backdrop-blur-sm border-b border-slate-800 text-xs py-2 px-4 print:hidden z-10">
        <div class="max-w-7xl mx-auto flex items-center justify-between">
            <div class="flex items-center space-x-4">
                <span class="inline-flex items-center gap-1.5 px-2.5 py-0.5 rounded-full text-[11px] sm:text-xs font-semibold bg-emerald-500/10 text-emerald-400 border border-emerald-500/30">
                    <span class="w-1.5 h-1.5 rounded-full bg-emerald-400 animate-pulse"></span>
                    ISO/IEC 17025:2017 VIGENTE
                </span>
            </div>
        </div>
    </div>

    <!-- ENCABEZADO CRIOTEC Y MENÚ PRINCIPAL -->
    <header class="sticky top-0 z-50 bg-[#0f172a]/95 backdrop-blur-md border-b border-cyan-500/10 shadow-lg print:hidden">
        <div class="max-w-7xl mx-auto px-3 sm:px-2 lg:px-8">
            <div class="flex flex-col md:flex-row items-center justify-between py 0 gap- md:gap-6">
                
                <!-- Logo Criotec & Tag Metrológico (SIEMPRE VISIBLE) -->
                <a href="/" class="flex items-center space-x-3 py-1">
    <!-- Se incrementó la altura del logo (h-20 en móvil, h-28 en pantalla grande) -->
    <img 
        src="/logo-criotec-productos.png" 
        alt="Logo Criotec" 
        class="h-20 sm:h-28 w-auto max-w-none object-contain transition-all" 
    />
    <div class="border-l border-slate-700 pl-3">
        <div class="flex items-center space-x-2">
            <span class="font-extrabold text-base sm:text-xl tracking-tight text-white">LABORATORIO</span>
            <span class="bg-cyan-500/20 text-cyan-300 text-[10px] font-bold px-1.5 py-0.5 rounded border border-cyan-400/30">17025</span>
        </div>
        <p class="text-[10px] sm:text-xs text-slate-400 tracking-wider">PLATAFORMA METROLÓGICA</p>
    </div>
</a>

                <!-- Opciones de Navegación Interactivas en Móvil y Escritorio -->
                <nav class="flex flex-wrap items-center justify-center gap-1 sm:gap-2">

                    <a href="/" class="px-2.5 py-1.5 rounded-lg text-xs sm:text-sm font-medium text-slate-200 hover:text-cyan-400 hover:bg-slate-800/60 transition-all">
                        Inicio
                    </a>

                    <!-- Certificación -->
                    <div class="relative">
                        <button 
                            on:click={(e) => toggleDropdown('cert', e)}
                            class="flex items-center space-x-1 px-2.5 py-1.5 rounded-lg text-xs sm:text-sm font-medium text-slate-200 hover:text-cyan-400 hover:bg-slate-800/60 transition-all"
                        >
                            <span>Certificación</span>
                            <i class="fa-solid fa-chevron-down text-[10px] sm:text-xs transition-transform text-cyan-400 {openDropdown === 'cert' ? 'rotate-180' : ''}"></i>
                        </button>
                        {#if openDropdown === 'cert'}
                            <div class="absolute left-1/2 -translate-x-1/2 sm:left-auto sm:right-0 sm:translate-x-0 top-full pt-2 w-64 z-50">
                                <div class="bg-[#0a192f] rounded-xl p-2 border border-cyan-500/30 shadow-2xl space-y-1">
                                    <a href="#cert-17025" on:click={closeDropdowns} class="flex items-start p-2.5 rounded-lg hover:bg-cyan-500/10 text-slate-300 hover:text-cyan-300 transition-colors">
                                        <i class="fa-solid fa-certificate text-cyan-400 mt-1 mr-3"></i>
                                        <div>
                                            <div class="text-sm font-semibold">ISO/IEC 17025</div>
                                            <p class="text-xs text-slate-400">Acreditación y cumplimiento</p>
                                        </div>
                                    </a>
                                    <a href="#alcance" on:click={closeDropdowns} class="flex items-start p-2.5 rounded-lg hover:bg-cyan-500/10 text-slate-300 hover:text-cyan-300 transition-colors">
                                        <i class="fa-solid fa-microscope text-cyan-400 mt-1 mr-3"></i>
                                        <div>
                                            <div class="text-sm font-semibold">Alcance de Acreditación</div>
                                            <p class="text-xs text-slate-400">Capacidades de medición</p>
                                        </div>
                                    </a>
                                </div>
                            </div>
                        {/if}
                    </div>

                    <!-- NOM-022 -->
                    <div class="relative">
                        <button 
                            on:click={(e) => toggleDropdown('nom', e)}
                            class="flex items-center space-x-1 px-2.5 py-1.5 rounded-lg text-xs sm:text-sm font-medium text-slate-200 hover:text-cyan-400 hover:bg-slate-800/60 transition-all"
                        >
                            <span>NOM-022</span>
                            <i class="fa-solid fa-chevron-down text-[10px] sm:text-xs transition-transform text-cyan-400 {openDropdown === 'nom' ? 'rotate-180' : ''}"></i>
                        </button>
                        {#if openDropdown === 'nom'}
                            <div class="absolute left-1/2 -translate-x-1/2 sm:left-auto sm:right-0 sm:translate-x-0 top-full pt-2 w-72 z-50">
                                <div class="bg-[#0a192f] rounded-xl p-2 border border-cyan-500/30 shadow-2xl space-y-1">
                                    <a href="/etiqueta" on:click={closeDropdowns} class="flex items-start p-2.5 rounded-lg hover:bg-cyan-500/10 text-slate-300 hover:text-cyan-300 transition-colors">
                                        <i class="fa-solid fa-bolt text-cyan-400 mt-1 mr-3"></i>
                                        <div>
                                            <div class="text-sm font-semibold">Reporte NOM-022 / FIDE</div>
                                            <p class="text-xs text-slate-400">Generador de etiqueta y dictamen</p>
                                        </div>
                                    </a>
                                </div>
                            </div>
                        {/if}
                    </div>

                    <!-- Coca-Cola -->
                    <div class="relative">
                        <button 
                            on:click={(e) => toggleDropdown('coca', e)}
                            class="flex items-center space-x-1 px-2.5 py-1.5 rounded-lg text-xs sm:text-sm font-medium text-slate-200 hover:text-cyan-400 hover:bg-slate-800/60 transition-all"
                        >
                            <span>Coca-Cola</span>
                            <i class="fa-solid fa-chevron-down text-[10px] sm:text-xs transition-transform text-cyan-400 {openDropdown === 'coca' ? 'rotate-180' : ''}"></i>
                        </button>
                        {#if openDropdown === 'coca'}
                            <div class="absolute left-1/2 -translate-x-1/2 sm:left-auto sm:right-0 sm:translate-x-0 top-full pt-2 w-72 z-50">
                                <div class="bg-[#0a192f] rounded-xl p-2 border border-red-500/40 shadow-2xl space-y-1">
                                    <a href="#coca-requisitos" on:click={closeDropdowns} class="flex items-start p-2.5 rounded-lg hover:bg-red-500/10 text-slate-300 hover:text-red-300 transition-colors">
                                        <i class="fa-solid fa-list-check text-red-400 mt-1 mr-3"></i>
                                        <div>
                                            <div class="text-sm font-semibold">Protocolo Coca-cola</div>
                                            <p class="text-xs text-slate-400">Requisitos de enfriadores</p>
                                        </div>
                                    </a>
                                </div>
                            </div>
                        {/if}
                    </div>

                    <!-- PepsiCo -->
                    <div class="relative">
                        <button 
                            on:click={(e) => toggleDropdown('pepsi', e)}
                            class="flex items-center space-x-1 px-2.5 py-1.5 rounded-lg text-xs sm:text-sm font-medium text-slate-200 hover:text-cyan-400 hover:bg-slate-800/60 transition-all"
                        >
                            <span>PepsiCo</span>
                            <i class="fa-solid fa-chevron-down text-[10px] sm:text-xs transition-transform text-cyan-400 {openDropdown === 'pepsi' ? 'rotate-180' : ''}"></i>
                        </button>
                        {#if openDropdown === 'pepsi'}
                            <div class="absolute left-1/2 -translate-x-1/2 sm:left-auto sm:right-0 sm:translate-x-0 top-full pt-2 w-72 z-50">
                                <div class="bg-[#0a192f] rounded-xl p-2 border border-blue-500/40 shadow-2xl space-y-1">
                                    <a href="#pepsi-estandares" on:click={closeDropdowns} class="flex items-start p-2.5 rounded-lg hover:bg-blue-500/10 text-slate-300 hover:text-blue-300 transition-colors">
                                        <i class="fa-solid fa-globe text-blue-400 mt-1 mr-3"></i>
                                        <div>
                                            <div class="text-sm font-semibold">Estándares Globales</div>
                                            <p class="text-xs text-slate-400">Validación de equipos fríos</p>
                                        </div>
                                    </a>
                                </div>
                            </div>
                        {/if}
                    </div>

                    <!-- Energy Star -->
                    <div class="relative">
                        <button 
                            on:click={(e) => toggleDropdown('energy', e)}
                            class="flex items-center space-x-1 px-2.5 py-1.5 rounded-lg text-xs sm:text-sm font-medium text-slate-200 hover:text-cyan-400 hover:bg-slate-800/60 transition-all"
                        >
                            <span>Energy Star</span>
                            <i class="fa-solid fa-chevron-down text-[10px] sm:text-xs transition-transform text-cyan-400 {openDropdown === 'energy' ? 'rotate-180' : ''}"></i>
                        </button>
                        {#if openDropdown === 'energy'}
                            <div class="absolute left-1/2 -translate-x-1/2 sm:left-auto sm:right-0 sm:translate-x-0 top-full pt-2 w-72 z-50">
                                <div class="bg-[#0a192f] rounded-xl p-2 border border-sky-400/40 shadow-2xl space-y-1">
                                    <a href="#energy-ensayos" on:click={closeDropdowns} class="flex items-start p-2.5 rounded-lg hover:bg-sky-500/10 text-slate-300 hover:text-sky-300 transition-colors">
                                        <i class="fa-solid fa-star text-sky-400 mt-1 mr-3"></i>
                                        <div>
                                            <div class="text-sm font-semibold">Certificación</div>
                                            <p class="text-xs text-slate-400">Calificación de Eficiencia</p>
                                        </div>
                                    </a>
                                </div>
                            </div>
                        {/if}
                    </div>

                </nav>
            </div>
        </div>
    </header>

    <!-- ÁREA DE CONTENIDO PRINCIPAL -->
    <main class="w-full flex-1 z-10 relative">
        <slot />
    </main>
</div>

<style>
    .bg-tech-pattern {
        background-image: radial-gradient(rgba(56, 189, 248, 0.12) 1px, transparent 1px);
        background-size: 24px 24px;
    }

    @media print {
        :global(body),
        :global(.min-h-screen) {
            background: white !important;
            background-color: white !important;
            background-image: none !important;
        }
    }
</style>