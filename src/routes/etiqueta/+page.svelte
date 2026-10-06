<script lang="ts">
    import { onMount } from 'svelte';

    function redondearProgresivo(valor: number, decimales: number): number {
        const factor = Math.pow(10, decimales);
        return Math.round((valor + Number.EPSILON) * factor) / factor;
    }

    function validarUnDecimal(input: HTMLInputElement | null) {
        if (input && input.value && input.value.includes('.')) {
            const partes = input.value.split('.');
            if (partes[1].length > 1) {
                input.value = partes[0] + '.' + partes[1].slice(0, 1);
            }
        }
    }

    function handleCapacidadInput(e: Event) {
        const target = e.currentTarget as HTMLInputElement;
        validarUnDecimal(target);
        updateLabel();
    }

    function calcularFormulaNOM(tipo: string, V: number): number {
        let A = 0, B = 0;
        switch(tipo) {
            case "ev_cristal": A = 199.5; B = -0.4537; break;
            case "ev_solida":  A = 188.4; B = -0.4537; break;
            case "ev_placas":  A = 996.5; B = -0.8763; break;
            case "eh_forzada": A = 3926.3; B = -1.0162; break;
            case "eh_placas":  A = 996.5; B = -0.8763; break;
            case "cv_cristal": A = 68.2;  B = -0.1136; break;
            case "cv_solida":  A = 56.2;  B = -0.1136; break;
            case "cv_placas":  A = 219.2; B = -0.4189; break;
            case "ch_solida":  A = 35.3;  B = -0.2142; break;
            case "ch_cristal": A = 74.8;  B = -0.2839; break;
            case "vc_media":   A = 140.3; B = -0.2915; break;
            case "vc_baja":    A = 97.8;  B = -0.1228; break;
            case "cons_hielo": A = 224.5; B = -0.5674; break;
            default: return 0;
        }
        const rawVal = A * Math.pow(V, B);
        const r4 = redondearProgresivo(rawVal, 4);
        const r3 = redondearProgresivo(r4, 3);
        const r2 = redondearProgresivo(r3, 2);
        return redondearProgresivo(r2, 1);
    }

    function calcularFormulaFIDE(tipo: string, V: number): number {
        switch(tipo) {
            case "ev_cristal": return 210.6 * Math.pow(V, -0.4537);
            case "ev_solida":  return 210.6 * Math.pow(V, -0.4537);
            case "ev_placas":  return 946.6 * Math.pow(V, -0.8763);
            case "eh_forzada": return 4144.4 * Math.pow(V, -1.0162);
            case "eh_placas":  return 966.5 * Math.pow(V, -0.8763);
            case "cv_cristal": return 68.8 * Math.pow(V, -0.1136);
            case "cv_solida":  return 68.8 * Math.pow(V, -0.1136);
            case "cv_placas":  return 219.0 * Math.pow(V, -0.4189);
            case "ch_solida":  return 28.2 * Math.pow(V, -0.2141);
            case "ch_cristal": return 74.4 * Math.pow(V, -0.2839);
            case "vc_media":   return 140.3 * Math.pow(V, -0.2915);
            case "vc_baja":    return 92.9 * Math.pow(V, -0.1228);
            case "cons_hielo": return 213.3 * Math.pow(V, -0.5674);
            default: return 0;
        }
    }

    function aplicarEstiloCumple(elem: HTMLElement) {
        elem.style.backgroundColor = '#d4edda';
        elem.style.color = '#155724';
        elem.style.border = '1px solid #c3e6cb';
    }

    function aplicarEstiloExcede(elem: HTMLElement) {
        elem.style.backgroundColor = '#f8d7da';
        elem.style.color = '#721c24';
        elem.style.border = '1px solid #f5c6cb';
    }

    function updateLabel() {
        const marcaInput = document.getElementById('in-marca') as HTMLInputElement | null;
        const modeloInput = document.getElementById('in-modelo') as HTMLInputElement | null;
        
        if (!marcaInput || !modeloInput) return;

        const marcaVal = marcaInput.value.trim();
        const modeloVal = modeloInput.value.trim();
        
        const lblMarca = document.getElementById('lbl-marca');
        const lblModelo = document.getElementById('lbl-modelo');
        if (lblMarca) lblMarca.innerText = marcaVal !== '' ? marcaVal : '---';
        if (lblModelo) lblModelo.innerText = modeloVal !== '' ? modeloVal : '---';
        
        const tipoSelect = document.getElementById('in-tipo') as HTMLSelectElement | null;
        if (!tipoSelect) return;
        const tipoOption = tipoSelect.options[tipoSelect.selectedIndex];

        const refSelect = document.getElementById('in-refrigerante') as HTMLSelectElement | null;
        const refOption = refSelect ? refSelect.options[refSelect.selectedIndex] : null;

        const lblRef = document.getElementById('lbl-refrigerante');
        const inEco = document.getElementById('in-eco') as HTMLInputElement | null;
        const lblEco = document.getElementById('lbl-eco');

        if (refOption && refOption.value) {
            if (lblRef) lblRef.innerText = refOption.value;
            const isEco = refOption.getAttribute('data-eco') === 'true';
            if (inEco) inEco.checked = isEco;
            if (lblEco) lblEco.style.display = isEco ? 'flex' : 'none';
        } else {
            if (lblRef) lblRef.innerText = '---';
            if (inEco) inEco.checked = false;
            if (lblEco) lblEco.style.display = 'none';
        }

        const inNom = document.getElementById('in-nom') as HTMLInputElement | null;
        const lblNom = document.getElementById('lbl-nom');
        const inAparato = document.getElementById('in-aparato') as HTMLInputElement | null;
        const lblAparato = document.getElementById('lbl-aparato');
        const resNomWhl = document.getElementById('res-nom-whl');
        const resNomKwh = document.getElementById('res-nom-kwh');
        const resApWhl = document.getElementById('res-ap-whl');
        const resApKwh = document.getElementById('res-ap-kwh');
        const lblAhorro = document.getElementById('lbl-ahorro');
        const resAhorroPct = document.getElementById('res-ahorro-pct');
        const dictamenNomTxt = document.getElementById('dictamen-nom-txt');
        const inFideWhl = document.getElementById('in-fide-whl') as HTMLInputElement | null;
        const inFideKwh = document.getElementById('in-fide-kwh') as HTMLInputElement | null;
        const inAparatoKwh = document.getElementById('in-aparato-kwh') as HTMLInputElement | null;
        const boxFide = document.getElementById('box-dictamen-fide');
        const boxDictamenGlobal = document.getElementById('box-dictamen-global');
        const sliderArrow = document.getElementById('slider-arrow');
        const ahorroDisplay = document.getElementById('ahorro-display');

        if (!tipoOption || !tipoOption.value) {
            const lblTipo = document.getElementById('lbl-tipo');
            if (lblTipo) lblTipo.innerText = '---';
            if (inNom) inNom.value = '0.0';
            if (lblNom) lblNom.innerText = '0.0';
            if (lblAparato) lblAparato.innerText = '0.0';
            if (resNomWhl) resNomWhl.innerText = '0.0';
            if (resNomKwh) resNomKwh.innerText = '0.000';
            if (resApWhl) resApWhl.innerText = '0.0';
            if (resApKwh) resApKwh.innerText = '0.000';
            if (lblAhorro) lblAhorro.innerText = '0.0 %';
            if (resAhorroPct) resAhorroPct.innerText = '0.0 %';
            if (dictamenNomTxt) {
                dictamenNomTxt.innerText = '---';
                dictamenNomTxt.style.color = '#333333';
            }
            if (inFideWhl) inFideWhl.value = '0.0';
            if (inFideKwh) inFideKwh.value = '0.000';
            if (inAparatoKwh) inAparatoKwh.value = '0.000';
            if (boxFide) {
                aplicarEstiloCumple(boxFide);
                boxFide.innerText = "EVALUACIÓN FIDE: ---";
            }
            if (boxDictamenGlobal) {
                aplicarEstiloCumple(boxDictamenGlobal);
                boxDictamenGlobal.innerText = "NOM: --- | FIDE: ---";
            }
            if (sliderArrow) sliderArrow.style.left = '0%';
            if (ahorroDisplay) ahorroDisplay.style.left = '0%';
            return;
        }

        const lblTipo = document.getElementById('lbl-tipo');
        if (lblTipo) lblTipo.innerText = tipoOption.text;
        
        const maxCap = parseFloat(tipoOption.getAttribute('data-max') || '0');
        const fijoNom = parseFloat(tipoOption.getAttribute('data-fijo') || '0');
        const fijoFide = parseFloat(tipoOption.getAttribute('data-fide-fijo') || '0');
        
        const inCapacidad = document.getElementById('in-capacidad') as HTMLInputElement | null;
        const capacidad = inCapacidad ? parseFloat(inCapacidad.value) || 0 : 0;
        const lblCapacidad = document.getElementById('lbl-capacidad');
        if (lblCapacidad) lblCapacidad.innerText = capacidad.toFixed(1);

        let nomCalculadoWhL = 0;
        if (capacidad > maxCap) {
            nomCalculadoWhL = fijoNom;
        } else if (capacidad > 0) {
            nomCalculadoWhL = calcularFormulaNOM(tipoOption.value, capacidad);
        }
        if (inNom) inNom.value = nomCalculadoWhL.toFixed(1);

        const nomWhL = inNom ? parseFloat(inNom.value) || 0 : 0;
        const aparatoWhL = inAparato ? parseFloat(inAparato.value) || 0 : 0;

        if (lblNom) lblNom.innerText = nomWhL.toFixed(1);
        if (lblAparato) lblAparato.innerText = aparatoWhL.toFixed(1);

        const nomKwhDiaStep1 = redondearProgresivo(nomWhL * capacidad, 4);
        const nomKwhDia = redondearProgresivo(nomKwhDiaStep1 / 1000, 4);

        const aparatoKwhDia = (aparatoWhL * capacidad) / 1000;

        if (resNomWhl) resNomWhl.innerText = nomWhL.toFixed(1);
        if (resNomKwh) resNomKwh.innerText = nomKwhDia.toFixed(3);
        if (resApWhl) resApWhl.innerText = aparatoWhL.toFixed(1);
        if (resApKwh) resApKwh.innerText = aparatoKwhDia.toFixed(3);

        let ahorro = 0;
        if (nomWhL > 0) {
            ahorro = (1 - (aparatoWhL / nomWhL)) * 100;
        }
        ahorro = Math.max(0, ahorro);
        const ahorroTruncado = (Math.floor(ahorro * 10) / 10).toFixed(1);
        
        if (lblAhorro) lblAhorro.innerText = ahorroTruncado + ' %';
        if (resAhorroPct) resAhorroPct.innerText = ahorroTruncado + ' %';

        const cumpleNOM = aparatoWhL <= nomWhL;
        if (dictamenNomTxt) {
            dictamenNomTxt.innerText = cumpleNOM ? "CUMPLE" : "EXCEDE";
            dictamenNomTxt.style.color = cumpleNOM ? "#155724" : "#721c24";
        }

        let fideCalculadoWhL = 0;
        if (capacidad > maxCap) {
            fideCalculadoWhL = fijoFide;
        } else if (capacidad > 0) {
            fideCalculadoWhL = calcularFormulaFIDE(tipoOption.value, capacidad);
        }

        const fideKwhDia = (fideCalculadoWhL * capacidad) / 1000;

        if (inFideWhl) inFideWhl.value = fideCalculadoWhL.toFixed(1);
        if (inFideKwh) inFideKwh.value = fideKwhDia.toFixed(3);
        if (inAparatoKwh) inAparatoKwh.value = aparatoKwhDia.toFixed(3);

        const cumpleFIDE = aparatoKwhDia <= fideKwhDia;
        
        if (boxFide) {
            if (cumpleFIDE) {
                aplicarEstiloCumple(boxFide);
                boxFide.innerText = "EVALUACIÓN FIDE: CUMPLE CON CRITERIO FIDE-2026";
            } else {
                aplicarEstiloExcede(boxFide);
                boxFide.innerText = "EVALUACIÓN FIDE: EXCEDE LÍMITE FIDE-2026";
            }
        }

        if (boxDictamenGlobal) {
            if (cumpleNOM && cumpleFIDE) {
                aplicarEstiloCumple(boxDictamenGlobal);
                boxDictamenGlobal.innerText = "DICTAMEN TOTAL: CUMPLE CON NOM Y FIDE";
            } else if (cumpleNOM && !cumpleFIDE) {
                aplicarEstiloExcede(boxDictamenGlobal);
                boxDictamenGlobal.innerText = "DICTAMEN TOTAL: CUMPLE NOM | EXCEDE FIDE";
            } else {
                aplicarEstiloExcede(boxDictamenGlobal);
                boxDictamenGlobal.innerText = "DICTAMEN TOTAL: NO CUMPLE NORMATIVAS";
            }
        }

        let percentOnScale = 0;
        if (ahorro > 50) {
            percentOnScale = 100; 
        } else {
            percentOnScale = (ahorro / 50) * 90.90; 
        }
        
        if (sliderArrow) sliderArrow.style.left = percentOnScale + '%';
        if (ahorroDisplay) ahorroDisplay.style.left = percentOnScale + '%';
    }

    function imprimir() {
        window.print();
    }

    onMount(() => {
        updateLabel();
    });
</script>

<svelte:head>
    <title>REPORTE DE DATOS Y CÁLCULOS TÉCNICOS NOM-022/FIDE</title>
</svelte:head>

<div class="app-container">
    <!-- PANEL DE CONTROL Y REPORTES (HOJA 1 EN IMPRESIÓN) -->
    <div class="sidebar">
        
        <!-- ENCABEZADO CON LOGO Y TÍTULO CENTRADO -->
        <div class="header-reporte">
            <img src="/logo-criotec-productos.png" alt="Logo Criotec" class="header-logo" />
            <div class="header-titulos">
                <h2 style="color: #333333 !important;">REPORTE DE DATOS Y CÁLCULOS TÉCNICOS NOM-022/FIDE</h2>
                <div class="header-subcodigo" style="color: #444444 !important;">FLA-107 Ver.01</div>
            </div>
        </div>
        
        <div class="section-header">1. Datos Básicos del Equipo</div>
        
        <div class="form-row">
            <div class="form-group">
                <label for="in-marca" style="color: #333333 !important;">Marca(s):</label>
                <input type="text" id="in-marca" value="" placeholder="Ingrese Marca(s)..." on:input={updateLabel} style="color: #000000 !important; background-color: #ffffff !important;">
            </div>
            <div class="form-group">
                <label for="in-modelo" style="color: #333333 !important;">Modelo(s):</label>
                <input type="text" id="in-modelo" value="" placeholder="Ingrese Modelo(s)..." on:input={updateLabel} style="color: #000000 !important; background-color: #ffffff !important;">
            </div>
        </div>

        <div class="form-group">
            <label for="in-tipo" style="color: #333333 !important;">Tipo (Aparato/Familia):</label>
            <select id="in-tipo" on:change={updateLabel} style="color: #000000 !important; background-color: #ffffff !important;">
                <option value="" disabled selected hidden>Seleccione Tipo</option>
                <option value="ev_cristal" data-max="1200" data-fijo="8.0" data-fide-fijo="8.4">Enfriador vertical con circulación forzada de aire y puerta de cristal</option>
                <option value="ev_solida" data-max="1200" data-fijo="7.6" data-fide-fijo="8.4">Enfriador vertical con circulación forzada de aire y puerta sólida</option>
                <option value="ev_placas" data-max="1200" data-fijo="2.0" data-fide-fijo="1.8">Enfriador vertical con sistema de refrigeración de placas frías</option>
                <option value="eh_forzada" data-max="500" data-fijo="7.2" data-fide-fijo="7.1">Enfriador horizontal con circulación forzada de aire</option>
                <option value="eh_placas" data-max="500" data-fijo="4.2" data-fide-fijo="3.9">Enfriador horizontal con sistema de refrigeración de placas frías</option>
                <option value="cv_cristal" data-max="1200" data-fijo="30.5" data-fide-fijo="30.6">Congelador vertical con puerta de cristal y circulación forzada de aire</option>
                <option value="cv_solida" data-max="1200" data-fijo="25.1" data-fide-fijo="30.6">Congelador vertical con puerta sólida y circulación forzada de aire</option>
                <option value="cv_placas" data-max="1500" data-fijo="10.3" data-fide-fijo="9.5">Congelador vertical con puerta de cristal o solida y sistema de refrigeración de placas frías</option>
                <option value="ch_solida" data-max="700" data-fijo="8.7" data-fide-fijo="6.9">Congelador horizontal con puerta sólida</option>
                <option value="ch_cristal" data-max="700" data-fijo="11.6" data-fide-fijo="12.7">Congelador horizontal con puerta de cristal</option>
                <option value="vc_media" data-max="1200" data-fijo="17.8" data-fide-fijo="15.9">Vitrina cerrada de temperatura media</option>
                <option value="vc_baja" data-max="1200" data-fijo="40.9" data-fide-fijo="36.9">Vitrina cerrada de temperatura baja</option>
                <option value="cons_hielo" data-max="2500" data-fijo="2.6" data-fide-fijo="2.6">Conservadores de bolsas con hielo</option>
            </select>
        </div>

        <div class="form-row">
            <div class="form-group">
                <label for="in-capacidad" style="color: #333333 !important;">Volumen (L):</label>
                <input type="number" step="0.1" min="0" id="in-capacidad" value="" placeholder="Ingrese Volumen (L)..." on:input={handleCapacidadInput} style="color: #000000 !important; background-color: #ffffff !important;">
            </div>
            <div class="form-group">
                <label for="in-refrigerante" style="color: #333333 !important;">Refrigerante:</label>
                <select id="in-refrigerante" on:change={updateLabel} style="color: #000000 !important; background-color: #ffffff !important;">
                    <option value="" disabled selected hidden>Seleccione Refrigerante</option>
                    <optgroup label="Ecológicos">
                        <option value="R-152a" data-eco="true">R-152a</option>
                        <option value="R-170" data-eco="true">R-170</option>
                        <option value="R-290" data-eco="true">R-290</option>
                        <option value="R-449a" data-eco="true">R-449a</option>                    
                        <option value="R-454C" data-eco="true">R-454C</option>
                        <option value="R-600a" data-eco="true">R-600a</option>
                        <option value="R-600" data-eco="true">R-600</option>
                        <option value="R-1150" data-eco="true">R-1150</option>
                        <option value="R-E170" data-eco="true">R-E170</option>
                        <option value="R-1270" data-eco="true">R-1270</option>
                    </optgroup>
                    <optgroup label="No Ecológicos">
                        <option value="R-134a" data-eco="false">R-134a</option>
                        <option value="R-142b" data-eco="false">R-142b</option>
                        <option value="R-143a" data-eco="false">R-143a</option>
                        <option value="R-404A" data-eco="false">R-404A</option>
                    </optgroup>
                </select>
            </div>
        </div>

        <div class="form-group" style="display: flex; align-items: center; margin-bottom: 5px;">
            <input type="checkbox" id="in-eco" disabled style="transform: scale(1.2);">
            <label style="margin: 0 0 0 8px; color: #333333 !important;" for="in-eco">Es ecológico (Automático)</label>
        </div>

        <div class="section-header">2. Resumen NOM-022-ENER/SE-2025</div>
        
        <div class="form-row">
            <div class="form-group">
                <label for="in-nom" style="color: #333333 !important;">Consumo NOM (Wh/L):</label>
                <input type="text" id="in-nom" value="0.0" readonly style="color: #000000 !important; background-color: #e9ecef !important;">
            </div>
            <div class="form-group">
                <label for="in-aparato" style="color: #333333 !important;">Consumo del aparato (Wh/L):</label>
                <input type="number" step="0.1" min="0" id="in-aparato" value="" placeholder="0.0" on:input={handleCapacidadInput} style="color: #000000 !important; background-color: #ffffff !important;">
            </div>
        </div>

        <table class="resumen-table" style="color: #000000 !important;">
            <thead>
                <tr>
                    <th style="color: #000000 !important; background-color: #f0f0f0 !important;">Criterio NOM-022</th>
                    <th style="color: #000000 !important; background-color: #f0f0f0 !important;">Valor Wh/L</th>
                    <th style="color: #000000 !important; background-color: #f0f0f0 !important;">Resultante kWh/día</th>
                </tr>
            </thead>
            <tbody>
                <tr>
                    <td style="color: #000000 !important;">Límite Consumo NOM</td>
                    <td id="res-nom-whl" class="val-highlight" style="color: #000000 !important;">0.0</td>
                    <td id="res-nom-kwh" class="val-highlight" style="color: #000000 !important;">0.000</td>
                </tr>
                <tr>
                    <td style="color: #000000 !important;">Consumo Real del Aparato</td>
                    <td id="res-ap-whl" class="val-highlight" style="color: #000000 !important;">0.0</td>
                    <td id="res-ap-kwh" class="val-highlight" style="color: #000000 !important;">0.000</td>
                </tr>
                <tr>
                    <td style="color: #000000 !important;">Ahorro Resultante</td>
                    <td id="res-ahorro-pct" class="val-highlight" style="color: #000000 !important;">0.0 %</td>
                    <td style="color: #000000 !important;">Dictamen: <span id="dictamen-nom-txt" style="font-weight: bold; color: #333333;">---</span></td>
                </tr>
            </tbody>
        </table>

        <div class="section-header">3. Evaluación FIDE-2026 (Rev.6)</div>
        
        <div class="form-row">
            <div class="form-group">
                <label for="in-fide-whl" style="color: #333333 !important;">Límite FIDE (Wh/L):</label>
                <input type="text" id="in-fide-whl" value="0.0" readonly style="color: #000000 !important; background-color: #e9ecef !important;">
            </div>
            <div class="form-group">
                <label for="in-fide-kwh" style="color: #333333 !important;">Límite FIDE (kWh/día):</label>
                <input type="text" id="in-fide-kwh" value="0.000" readonly style="color: #000000 !important; background-color: #e9ecef !important;">
            </div>
        </div>

        <div class="form-group">
            <label for="in-aparato-kwh" style="color: #333333 !important;">Consumo Calculado del Aparato (kWh/día):</label>
            <input type="text" id="in-aparato-kwh" value="0.000" readonly style="color: #000000 !important; background-color: #e9ecef !important;">
        </div>

        <div id="box-dictamen-fide" class="dictamen-box" style="background-color: #d4edda; color: #155724; border: 1px solid #c3e6cb;">
            EVALUACIÓN FIDE: ---
        </div>

        <div class="section-header">4. Dictamen Total</div>
        <div id="box-dictamen-global" class="dictamen-box" style="background-color: #d4edda; color: #155724; border: 1px solid #c3e6cb; font-size: 14px;">
            NOM: --- | FIDE: ---
        </div>

        <button class="btn-print" on:click={imprimir}>🖨️️ Imprimir / Guardar PDF</button>
    </div>

    <!-- PANEL DERECHO: VISUALIZACIÓN ETIQUETA AMARILLA (HOJA 2 EN IMPRESIÓN) -->
    <div class="main">
        
        <img src="/logo-criotec-productos.png" alt="Logo Criotec" class="logo-hoja2" />
        <div class="etiqueta-container">
            <div class="header">
                <h1>EFICIENCIA ENERGÉTICA</h1>
                <p>Consumo de energía determinado como se establece en la</p>
                <h2>NOM-022-ENER/SE-2025</h2>
            </div>
            
            <div class="divider"></div>
            
            <div class="info-grid">
                <div><span class="label-text">Marca (s):</span> <span class="bold-val" id="lbl-marca">---</span></div>
                <div><span class="label-text">Tipo:</span> <span class="bold-val" id="lbl-tipo">---</span></div>
                
                <div><span class="label-text">Modelo (s):</span> <span class="bold-val" id="lbl-modelo">---</span></div>
                <div><span class="label-text">Capacidad:</span> <span class="bold-val"><span id="lbl-capacidad">0.0</span> (L)</span></div>
                
                <div><span class="label-text">Tipo de refrigerante:</span> <span class="bold-val" id="lbl-refrigerante">---</span></div>
                <div>
                    <div class="eco-box" id="lbl-eco">ECOLÓGICO</div>
                </div>
            </div>

            <div class="divider"></div>

            <div class="consumo-section">
                <div class="consumo-row">
                    <span style="width: 75%;">Consumo establecido en la Norma Oficial<br>Mexicana en 24 h en (Wh/L):</span>
                    <div class="consumo-box" id="lbl-nom">0.0</div>
                </div>
                <div class="consumo-row">
                    <span>Consumo del aparato en 24 h en (Wh/L):</span>
                    <div class="consumo-box" id="lbl-aparato">0.0</div>
                </div>
            </div>

            <div class="divider"></div>

            <div class="ahorro-section">
                <div class="ahorro-title">Ahorro de energía de este aparato</div>
                
                <div class="ahorro-track">
                    <div class="ahorro-display" id="ahorro-display">
                        <div style="position: relative;">
                            <div class="ahorro-box"><span id="lbl-ahorro">0.0 %</span></div>
                            <div class="ahorro-asterisk">*</div>
                        </div>
                    </div>
                </div>
                
                <div class="slider-container">
                    <svg class="plug-icon" width="32" height="24" viewBox="0 0 40 30">
                        <path d="M 5 5 C 15 10, 5 20, 14 18" fill="none" stroke="black" stroke-width="2.5" stroke-linecap="round"/>
                        <rect x="13" y="15" width="4" height="6" rx="1" fill="black"/>
                        <rect x="16" y="11" width="11" height="14" rx="3" fill="black"/>
                        <rect x="25" y="9" width="5" height="18" rx="1.5" fill="black"/>
                        <line x1="28" y1="13.5" x2="38" y2="13.5" stroke="black" stroke-width="2.5" stroke-linecap="round"/>
                        <line x1="28" y1="22.5" x2="38" y2="22.5" stroke="black" stroke-width="2.5" stroke-linecap="round"/>
                    </svg>
                    
                    <div class="slider-line">
                        <svg class="slider-arrow" id="slider-arrow" width="32" height="22" viewBox="0 0 32 22">
                            <polygon points="0,0 30,0 16,18" fill="black"/>
                        </svg>
                    </div>
                    
                    <div class="slider-dots">
                        <span style="left: 0%;">0</span>
                        <span style="left: 9.09%;">5</span>
                        <span style="left: 18.18%;">10</span>
                        <span style="left: 27.27%;">15</span>
                        <span style="left: 36.36%;">20</span>
                        <span style="left: 45.45%;">25</span>
                        <span style="left: 54.54%;">30</span>
                        <span style="left: 63.63%;">35</span>
                        <span style="left: 72.72%;">40</span>
                        <span style="left: 81.81%;">45</span>
                        <span style="left: 90.90%;">50</span>
                        <span style="left: 100%;">%</span>
                    </div>
                    <div class="mayor-ahorro">Mayor<br>Ahorro</div>
                </div>

                <div class="ahorro-texts">
                    Esta etiqueta garantiza que este modelo cumple con la eficiencia mínima<br>establecida en esta NOM-ENER.<br>
                    *Este porcentaje representa un ahorro adicional
                </div>
            </div>

            <div class="divider"></div>

            <div class="footer">
                <h3>I M P O R T A N T E</h3>
                <ul>
                    <li>El ahorro de energía adicional del aparato depende de los hábitos de uso y ubicación del mismo.</li>
                    <li>Este aparato cumple con los requisitos de seguridad al usuario.</li>
                    <li>La etiqueta no debe retirarse del aparato hasta que haya sido adquirido por el consumidor final.</li>
                </ul>
                <div class="footer-conuee">La NOM-ENER fue desarrollada en la CONUEE.</div>
            </div>
        </div>
    </div>
</div>

<style>
    * {
        box-sizing: border-box;
    }

    /* CONTENEDOR FLEX PRINCIPAL */
    .app-container {
        display: flex;
        width: 100%;
        height: calc(100vh - 65px);
        overflow: hidden;
    }

    .header-reporte {
        display: flex;
        align-items: center;
        justify-content: flex-start;
        gap: 15px;
        margin-bottom: 10px;
        width: 100%;
    }
    .header-logo {
        height: 90px;
        width: auto;
        object-fit: contain;
    }
    .header-titulos {
        flex: 1;
        text-align: center;
        margin-right: 45px;
    }
    .header-titulos h2 {
        font-size: 17px;
        margin: 0;
        text-transform: uppercase;
    }
    .header-subcodigo {
        font-size: 13px;
        font-weight: bold;
        margin-top: 3px;
    }

    /* PANEL IZQUIERDO */
    .sidebar {
        width: 520px;
        min-width: 500px;
        background: white;
        padding: 20px;
        box-shadow: 2px 0 5px rgba(0,0,0,0.1);
        overflow-y: auto;
        height: 100%;
    }
    
    .section-header {
        background-color: #333;
        color: white;
        padding: 6px 10px;
        font-weight: bold;
        font-size: 14px;
        margin-top: 15px;
        margin-bottom: 12px;
        border-radius: 3px;
        text-transform: uppercase;
    }

    .form-row {
        display: flex;
        gap: 10px;
        margin-bottom: 10px;
    }
    .form-group { flex: 1; margin-bottom: 10px; }
    .form-group label { display: block; font-weight: bold; margin-bottom: 4px; font-size: 12px; }
    .form-group input[type="text"], 
    .form-group input[type="number"], 
    .form-group select {
        width: 100%; padding: 6px 8px; border: 1px solid #ccc; border-radius: 4px; font-size: 13px; font-family: inherit;
    }

    .resumen-table {
        width: 100%;
        border-collapse: collapse;
        margin-top: 8px;
        margin-bottom: 8px;
        font-size: 12px;
    }
    .resumen-table th, .resumen-table td {
        border: 1px solid #ccc;
        padding: 6px 8px;
        text-align: left;
    }
    .val-highlight {
        font-weight: bold;
    }

    /* CAJAS DE DICTAMEN */
    .dictamen-box {
        padding: 10px 12px;
        border-radius: 4px;
        text-align: center;
        font-weight: bold;
        font-size: 13px;
        margin-top: 8px;
        width: 100%;
        display: flex;
        align-items: center;
        justify-content: center;
        box-sizing: border-box;
    }

    .btn-print {
        background: #000; color: white; border: none; padding: 12px; border-radius: 4px;
        cursor: pointer; width: 100%; font-size: 15px; font-weight: bold; margin-top: 15px; margin-bottom: 20px;
    }
    .btn-print:hover { background: #333; }

    /* PANEL DERECHO */
    .main {
        flex: 1;
        display: flex;
        flex-direction: column;
        justify-content: center;
        align-items: center;
        padding: 20px;
        background: #666;
        height: 100%;
        overflow-y: auto;
    }

    .logo-hoja2 {
        display: none;
        height: 45px;
        width: auto;
    }

    .etiqueta-container {
        width: 440px;
        background-color: #fff200; 
        border: 3px solid black;
        border-radius: 8px;
        padding: 15px 15px;
        box-sizing: border-box;
        color: black;
        box-shadow: 0 5px 15px rgba(0,0,0,0.3);
        position: relative;
    }
    
    .header { text-align: center; margin-bottom: 1px; }
    .header h1 { margin: 0; font-size: 24px; font-weight: 1000; letter-spacing: 0.5px; display: inline-block; transform: scaleY(1.3); transform-origin: bottom;  }
    .header p { margin: 4px 0; font-size: 12.5px; font-weight: bold; }
    .header h2 { margin: 0; font-size: 21px; font-weight: 900; }
    
    .divider { border-top: 1.5px solid black; margin: 8px 0; }
    
    .info-grid {
        display: grid;
        grid-template-columns: 55% 45%;
        row-gap: 6px;
        font-size: 13px;
    }
    .info-grid div { display: flex; align-items: center; }
    .label-text { margin-right: 5px; }
    .bold-val { font-weight: bold; }
    
    .eco-box {
        border: 2.4px solid black;
        border-radius: 4px;
        padding: 1px 6px;
        font-weight: bold;
        font-size: 15px;
        display: none; 
    }

    .consumo-section { font-size: 13px; font-weight: bold; line-height: 1.2; margin-top: 8px; }
    .consumo-row { display: flex; justify-content: space-between; align-items: center; margin-bottom: 8px; }
    .consumo-box {
        border: 2px solid black;
        border-radius: 5px;
        padding: 2px 8px;
        font-size: 16px;
        font-weight: bold;
        min-width: 40px;
        text-align: center;
    }

    .ahorro-section { text-align: center; margin-top: 8px; }
    .ahorro-title { font-size: 14px; font-weight: 900; margin-bottom: 4px; }
    
    .ahorro-track {
        position: relative;
        width: 60%;
        margin: 0 auto;
        height: 65px;
    }

    .ahorro-display {
        position: absolute;
        bottom: 18px; 
        transform: translateX(-50%);
        transition: left 0.3s ease;
        display: inline-flex;
        white-space: nowrap; 
        z-index: 10;
    }
    
    .ahorro-box {
        border: 2px solid black;
        border-radius: 8px;
        padding: 0px 6px;
        font-size: 28px;
        font-weight: 900;
        background-color: #fff200; 
        white-space: nowrap; 
    }
    
    .ahorro-asterisk { 
        position: absolute;
        top: -5px;
        right: -14px;
        font-size: 24px; 
        font-weight: 900; 
    }

    .slider-container {
        position: relative;
        width: 72%; 
        margin: 0 auto;
    }
    
    .slider-line {
        border-top: 3px solid black;
        position: relative;
        width: 90%;
    }
    
    .slider-line::before {
        content: ''; position: absolute; top: -5.5px; left: 0; width: 8px; height: 8px; background: black; border-radius: 50%; transform: translateX(-50%);
    }
    .slider-line::after {
        content: ''; position: absolute; top: -5.5px; right: 0; width: 8px; height: 8px; background: black; border-radius: 50%; transform: translateX(50%);
    }
    
    .slider-arrow {
        position: absolute;
        top: -19px; 
        transform: translateX(-50%);
        transition: left 0.3s ease;
        z-index: 5;
        height: 18px;
    }

    .slider-dots {
        position: relative;
        height: 16px;
        margin-top: 4px;
        font-size: 12px;
        font-weight: 530;
        width: 90%;
    }
    
    .slider-dots span { 
        position: absolute; 
        transform: translateX(-50%); 
        top: 2px;
    }

    .mayor-ahorro {
        position: absolute;
        right: -50px;
        top: -10px;
        font-size: 14px;
        font-weight: 800;
        text-align: center;
        line-height: 1.2;
    }

    .plug-icon {
        position: absolute;
        left: -80px;
        top: -13px;
        width: 100px;
    }

    .ahorro-texts {
        margin-top: 5px;
        font-size: 10px;
        font-weight: bold;
        text-align: center;
        line-height: 1.1;
    }

    .footer { font-size: 9px; margin-top: 8px; line-height: 1.15; }
    .footer h3 { text-align: center; font-size: 15px; letter-spacing: 2px; margin: 4px 0; font-weight: 900;}
    .footer ul { padding-left: 20px; margin: 0 0 6px 0; }
    .footer li { margin-bottom: 2px; font-size: 12px; font-weight: normal; }
    .footer-conuee { text-align: center; font-weight: 900; font-size: 13px; margin-top: 6px; }

    /* RESPONSIVIDAD PARA PANTALLAS PEQUEÑAS (MÓVILES / TABLETAS) */
    @media (max-width: 1024px) {
        .app-container {
            flex-direction: column !important;
            height: auto !important;
            overflow: visible !important;
        }

        .sidebar {
            width: 100% !important;
            min-width: 100% !important;
            height: auto !important;
        }

        .main {
            width: 100% !important;
            height: auto !important;
            padding: 40px 15px !important;
        }

        .etiqueta-container {
            width: 100% !important;
            max-width: 440px !important;
        }
    }

    /* ESTILOS DE IMPRESIÓN FORZADOS */
    @media print {
        @page {
            size: letter portrait;
            margin: 0.8cm;
        }

        :global(body),
        :global(.min-h-screen),
        .app-container {
            background: white !important;
            background-color: white !important;
            color: #000000 !important;
            display: block !important;
            height: auto !important;
            min-height: 0 !important;
            overflow: visible !important;
        }

        * {
            -webkit-print-color-adjust: exact !important;
            print-color-adjust: exact !important;
            -webkit-text-fill-color: initial !important;
        }
        
        .btn-print { display: none !important; }

        .sidebar { 
            display: block !important; 
            width: 100% !important;
            min-width: 100% !important;
            box-shadow: none !important;
            padding: 0 !important;
            height: auto !important;
            background: white !important;
            overflow: visible !important;
            page-break-after: always !important;
            break-after: page !important;
        }

        .section-header {
            margin-top: 8px !important;
            margin-bottom: 6px !important;
            padding: 4px 8px !important;
            font-size: 12px !important;
            background-color: #333333 !important;
            color: #ffffff !important;
        }

        .form-group {
            margin-bottom: 4px !important;
        }

        .form-group label {
            font-size: 10.5px !important;
            margin-bottom: 2px !important;
        }

        .form-group input, .form-group select {
            padding: 3px 6px !important;
            font-size: 11px !important;
            height: 26px !important;
        }

        .resumen-table {
            margin-top: 4px !important;
            margin-bottom: 4px !important;
            font-size: 10.5px !important;
        }

        .resumen-table th, .resumen-table td {
            padding: 3px 6px !important;
        }

        .dictamen-box {
            padding: 4px !important;
            font-size: 11px !important;
            margin-top: 4px !important;
        }

        .header-reporte {
            margin-bottom: 6px !important;
        }

        .main { 
            background: white !important; 
            padding: 0 !important; 
            display: flex !important; 
            flex-direction: column !important;
            justify-content: flex-start !important;
            align-items: center !important;
            height: auto !important;
            width: 100% !important;
            page-break-before: always !important;
            break-before: page !important;
            position: relative !important;
        }
        
        .logo-hoja2 {
            display: block !important;
            align-self: flex-start !important;
            height: 90px !important;
            margin-bottom: 15px !important;
        }

        .etiqueta-container { 
            box-shadow: none !important; 
            border: 3px solid black !important; 
            margin: 0 auto !important;
            width: 420px !important;
            transform: scale(0.95) !important;
            transform-origin: top center !important;
        }
    }
</style>