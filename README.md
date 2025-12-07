<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Verificación de Seguridad</title>
  <style>
    body {
      display: none;
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 50px;
      background: #f0f0f0;
      margin: 0;
    }
    
    #loading {
      display: block;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 18px;
      color: #666;
      text-align: center;
    }
    
    .message {
      background: white;
      padding: 40px;
      border-radius: 10px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.1);
      max-width: 600px;
      margin: 20px auto;
    }
    
    .success {
      border-left: 5px solid #27ae60;
    }
    
    .error {
      border-left: 5px solid #e74c3c;
    }
    
    .btn {
      display: inline-block;
      padding: 12px 30px;
      background: #3498db;
      color: white;
      text-decoration: none;
      border-radius: 5px;
      margin: 10px;
      border: none;
      cursor: pointer;
      font-size: 16px;
    }
    
    .btn-success {
      background: #2ecc71;
    }
    
    .steps {
      text-align: left;
      background: #f8f9fa;
      padding: 20px;
      border-radius: 5px;
      margin: 20px 0;
    }
    
    /* ESTILOS QUE ATRAEN A BLOQUEADORES - NO MODIFICAR */
    .pub_300x250, .pub_300x250m, .pub_728x90, .textad {
      width: 1px !important;
      height: 1px !important;
      position: absolute !important;
      left: -10000px !important;
      top: -10000px !important;
      overflow: hidden !important;
    }
    
    #div-gpt-ad, #google_image_div, #aw0, #ad_unit {
      display: block !important;
      visibility: visible !important;
      opacity: 0.001 !important;
      position: absolute !important;
      z-index: -9999 !important;
    }
    
    .advert, .advertisement, .ads-area, .ads-wrapper {
      background: transparent !important;
      border: none !important;
      margin: 0 !important;
      padding: 0 !important;
    }
  </style>
</head>
<body>
  <div id="loading">
    <div style="margin-bottom: 20px; font-size: 20px; color: #3498db;">🛡️</div>
    Verificando seguridad del navegador...
    <div style="margin-top: 20px; font-size: 14px; color: #999;">
      Esta verificación previene el acceso automatizado
    </div>
  </div>
  
  <div id="successMessage" class="message success" style="display: none;">
    <h1 style="color: #27ae60;">✅ Verificación Completada</h1>
    <p>Acceso autorizado. Redirigiendo en <span id="countdown">3</span> segundos...</p>
    <div style="margin: 30px 0;">
      <div style="height: 10px; background: #eee; border-radius: 5px; overflow: hidden;">
        <div id="progressBar" style="height: 100%; background: #2ecc71; width: 0%; transition: width 0.5s;"></div>
      </div>
    </div>
    <a href="#" id="directLink" class="btn btn-success">Acceder ahora</a>
    <p style="font-size: 12px; color: #666; margin-top: 20px;">
      Si tienes problemas, desactiva temporalmente las extensiones de bloqueo
    </p>
  </div>
  
  <div id="errorMessage" class="message error" style="display: none;">
    <h1 style="color: #e74c3c;">⚠️ Bloqueador Detectado</h1>
    <p><strong>Se requiere desactivar el bloqueador para continuar</strong></p>
    
    <div class="steps">
      <h3>📋 Instrucciones paso a paso:</h3>
      <ol>
        <li><strong>Localiza el icono de tu bloqueador</strong> (🔒, 🛡️, ⛔) en la barra superior derecha</li>
        <li><strong>Haz clic derecho sobre él</strong> y selecciona "Desactivar en este sitio"</li>
        <li><strong>O haz clic izquierdo</strong> y busca la opción "Pausar" o "Desactivar"</li>
        <li><strong>Recarga esta página</strong> con F5 o el botón de abajo</li>
      </ol>
      
      <div style="background: #fff3cd; border-left: 4px solid #ffc107; padding: 10px; margin: 10px 0;">
        <strong>💡 Tipos de bloqueadores comunes:</strong><br>
        • AdBlock, AdBlock Plus<br>
        • uBlock Origin<br>
        • Ghostery<br>
        • Brave Shields (en navegador Brave)<br>
        • Bloqueador de Opera
      </div>
    </div>
    
    <div style="margin-top: 30px; padding-top: 20px; border-top: 1px solid #eee;">
      <button onclick="location.reload(true)" class="btn" style="background: #2ecc71;">
        🔄 Ya desactivé el bloqueador, recargar
      </button>
      <p style="font-size: 12px; color: #666; margin-top: 10px;">
        La recarga es necesaria para re-evaluar la configuración
      </p>
    </div>
  </div>

  <!-- ================ ELEMENTOS TRAMPA AVANZADOS ================ -->
  <!-- Estos elementos tienen nombres y clases que LOS BLOQUEADORES BUSCAN ACTIVAMENTE -->
  
  <!-- Elementos con IDs que los bloqueadores monitorean -->
  <div id="div-gpt-ad-123456789-0" style="display: block !important; height: 1px !important; width: 1px !important; position: absolute !important; overflow: hidden !important;"></div>
  <div id="google_ads_iframe_1" style="display: block !important; visibility: visible !important; height: 1px !important; width: 1px !important;"></div>
  <div id="ad_unit" class="ad-unit" style="display: block !important; opacity: 0.001 !important;"></div>
  <div id="aw0" class="ad-wrapper" style="display: block !important;"></div>
  
  <!-- Elementos con clases específicas de anuncios -->
  <div class="pub_300x250 pub_300x250m pub_728x90 textad textAd text_ad text_ads text-ads text-ad-links"></div>
  <div class="advert advertisement ads-area ads-wrapper ad-area ad-wrapper"></div>
  <div class="adsbygoogle adsbygoogle-noablate" data-ad-client="ca-pub-123456789" data-ad-slot="987654321" style="display: block !important; height: 1px !important; width: 1px !important;"></div>
  
  <!-- Iframe trampa -->
  <iframe id="iframe_ad" src="about:blank" style="display: block !important; visibility: visible !important; height: 1px !important; width: 1px !important; border: none !important; position: absolute !important; top: -1000px !important;"></iframe>
  
  <!-- Script trampa que parece analytics -->
  <script data-adblockcheck="true">
    // Script trampa que se ejecuta inmediatamente
    (function() {
      window._adblockDetector = {
        checks: 0,
        blocked: false,
        lastCheck: Date.now()
      };
      
      // Crear elemento que parece un anuncio
      const fakeAd = document.createElement('div');
      fakeAd.id = 'adsense-container';
      fakeAd.className = 'adsbygoogle ads-container';
      fakeAd.innerHTML = '<ins class="adsbygoogle" style="display:inline-block;width:1px;height:1px" data-ad-client="ca-pub-fake" data-ad-slot="fake-slot"></ins>';
      fakeAd.style.cssText = 'display:block !important; visibility:visible !important; height:1px !important; width:1px !important; position:absolute !important; top:-999px !important; left:-999px !important;';
      document.body.appendChild(fakeAd);
      
      // Verificar inmediatamente si el elemento fue alterado
      setTimeout(function() {
        const adElement = document.getElementById('adsense-container');
        if (adElement) {
          const style = window.getComputedStyle(adElement);
          if (style.display === 'none' || style.visibility === 'hidden' || adElement.offsetHeight === 0) {
            window._adblockDetector.blocked = true;
            console.warn('[AdBlock Detector] Elemento de anuncio alterado');
          }
        }
      }, 100);
    })();
  </script>

  <!-- ================ LÓGICA DE DETECCIÓN SUPER AGRESIVA ================ -->
  <script>
    // URL de destino
    const TARGET_URL = "https://devuploads.com/nvgoz9e9zjag";
    
    // Contador de detecciones
    let detectionScore = 0;
    const detectionMethods = [];
    
    // MÉTODO 1: Verificación de recursos bloqueados
    function checkBlockedResources() {
      return new Promise((resolve) => {
        const resources = [
          'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js',
          'https://www.googletagmanager.com/gtag/js',
          'https://www.google-analytics.com/analytics.js',
          'https://connect.facebook.net/signals/config/123456789',
          'https://bat.bing.com/bat.js'
        ];
        
        let blockedCount = 0;
        let checked = 0;
        
        resources.forEach(url => {
          const script = document.createElement('script');
          script.src = url;
          script.onerror = () => {
            blockedCount++;
            checked++;
            if (checked === resources.length) {
              if (blockedCount >= 2) {
                detectionScore += 2;
                detectionMethods.push(`Recursos bloqueados: ${blockedCount}`);
              }
              resolve();
            }
          };
          script.onload = () => {
            checked++;
            if (checked === resources.length) {
              resolve();
            }
            document.head.removeChild(script);
          };
          script.style.display = 'none';
          document.head.appendChild(script);
        });
        
        // Timeout de seguridad
        setTimeout(resolve, 3000);
      });
    }
    
    // MÉTODO 2: Verificación de patrones CSS de bloqueadores
    function checkCSSPatterns() {
      // Crear elementos de prueba con clases que bloqueadores modifican
      const testElements = [
        { className: 'adsbygoogle', id: 'test-ad-1' },
        { className: 'ad-container advert', id: 'test-ad-2' },
        { className: 'pub_300x250 advertisement', id: 'test-ad-3' },
        { className: 'textad ad-unit', id: 'test-ad-4' }
      ];
      
      testElements.forEach(test => {
        const el = document.createElement('div');
        el.id = test.id;
        el.className = test.className;
        el.style.cssText = 'display:block !important; width:1px !important; height:1px !important; position:absolute !important; top:0 !important; left:0 !important; visibility:visible !important;';
        document.body.appendChild(el);
        
        // Verificar después de un tick
        setTimeout(() => {
          const element = document.getElementById(test.id);
          if (element) {
            const computed = window.getComputedStyle(element);
            const isHidden = computed.display === 'none' || 
                            computed.visibility === 'hidden' ||
                            element.offsetHeight === 0 ||
                            element.offsetWidth === 0;
            
            if (isHidden) {
              detectionScore++;
              detectionMethods.push(`CSS bloqueado: ${test.className}`);
            }
            document.body.removeChild(element);
          }
        }, 50);
      });
    }
    
    // MÉTODO 3: Detección de funciones sobrescritas
    function checkOverwrittenFunctions() {
      try {
        // Verificar si fetch ha sido modificado
        const originalFetch = window.fetch;
        const fetchString = originalFetch.toString();
        if (fetchString.includes('native code') || fetchString.includes('[native code]')) {
          // fetch es nativo
        } else {
          detectionScore++;
          detectionMethods.push('Fetch modificado');
        }
        
        // Verificar XMLHttpRequest
        const xhrProto = XMLHttpRequest.prototype;
        const originalOpen = xhrProto.open;
        const openString = originalOpen.toString();
        if (!openString.includes('native code') && !openString.includes('[native code]')) {
          detectionScore++;
          detectionMethods.push('XMLHttpRequest.open modificado');
        }
        
      } catch (e) {
        console.log('Error en checkOverwrittenFunctions:', e);
      }
    }
    
    // MÉTODO 4: Prueba de carga de iframe de ads
    function checkAdIframe() {
      return new Promise((resolve) => {
        const iframe = document.createElement('iframe');
        iframe.src = 'https://googleads.g.doubleclick.net/pagead/html/r20241204/r20190131/zrt_lookup.html';
        iframe.style.cssText = 'width:1px !important; height:1px !important; position:absolute !important; top:-100px !important; left:-100px !important; display:block !important;';
        iframe.onload = () => {
          console.log('Iframe cargado - sin bloqueo');
          document.body.removeChild(iframe);
          resolve(false);
        };
        iframe.onerror = () => {
          detectionScore += 2;
          detectionMethods.push('Iframe de ads bloqueado');
          if (document.body.contains(iframe)) {
            document.body.removeChild(iframe);
          }
          resolve(true);
        };
        
        document.body.appendChild(iframe);
        
        // Timeout
        setTimeout(() => {
          if (document.body.contains(iframe)) {
            detectionScore++;
            detectionMethods.push('Iframe timeout (posible bloqueo)');
            document.body.removeChild(iframe);
          }
          resolve(true);
        }, 2000);
      });
    }
    
    // MÉTODO 5: Detectar listas de filtros conocidas
    function checkFilterLists() {
      // Crear un elemento que coincida con reglas de filtro comunes
      const bait = document.createElement('div');
      bait.id = 'Mpopup';
      bait.className = 'Mpopup';
      bait.style.cssText = 'position:absolute; top:0; left:0; width:1px; height:1px;';
      bait.innerHTML = '<div class="ModalPopup"></div>';
      document.body.appendChild(bait);
      
      setTimeout(() => {
        const element = document.getElementById('Mpopup');
        if (element) {
          const style = window.getComputedStyle(element);
          if (style.display === 'none') {
            detectionScore++;
            detectionMethods.push('Regla de filtro "Mpopup" detectada');
          }
          document.body.removeChild(element);
        }
      }, 100);
    }
    
    // MÉTODO 6: Verificación de variables globales de bloqueadores
    function checkAdBlockGlobals() {
      const adBlockVars = [
        'adblock',
        'uBlock',
        'Ghostery',
        'brave',
        'adblockplus',
        'adsbypasser'
      ];
      
      adBlockVars.forEach(varName => {
        if (window[varName] !== undefined) {
          detectionScore += 2;
          detectionMethods.push(`Variable global detectada: ${varName}`);
        }
      });
      
      // Verificar en el objeto window
      for (let prop in window) {
        if (prop.toLowerCase().includes('adblock') || 
            prop.toLowerCase().includes('ublock') ||
            prop.toLowerCase().includes('ghostery')) {
          detectionScore++;
          detectionMethods.push(`Propiedad window.${prop} detectada`);
        }
      }
    }
    
    // FUNCIÓN PRINCIPAL DE DETECCIÓN
    async function runAdBlockDetection() {
      console.log('🚀 Iniciando detección agresiva de bloqueadores...');
      
      // Ejecutar todos los métodos de detección
      checkCSSPatterns();
      checkOverwrittenFunctions();
      checkFilterLists();
      checkAdBlockGlobals();
      
      await checkBlockedResources();
      await checkAdIframe();
      
      // Verificar detector inicial
      if (window._adblockDetector && window._adblockDetector.blocked) {
        detectionScore += 3;
        detectionMethods.push('Detector inicial activado');
      }
      
      console.log('📊 Resultado:', {
        score: detectionScore,
        methods: detectionMethods,
        threshold: 3
      });
      
      // Evaluar resultado después de 2 segundos
      setTimeout(evaluateDetection, 2000);
    }
    
    // EVALUAR RESULTADO FINAL
    function evaluateDetection() {
      document.body.style.display = 'block';
      document.getElementById('loading').style.display = 'none';
      
      // UMBRAL DE DETECCIÓN (ajustable)
      const THRESHOLD = 3;
      
      if (detectionScore >= THRESHOLD) {
        console.log('⛔ BLOQUEADOR DETECTADO - Score:', detectionScore);
        document.getElementById('errorMessage').style.display = 'block';
        document.getElementById('successMessage').style.display = 'none';
        
        // Mostrar métodos de detección en consola
        if (detectionMethods.length > 0) {
          console.table(detectionMethods);
        }
      } else {
        console.log('✅ SIN BLOQUEADOR - Score:', detectionScore);
        document.getElementById('successMessage').style.display = 'block';
        document.getElementById('errorMessage').style.display = 'none';
        setupRedirect();
      }
    }
    
    // CONFIGURAR REDIRECCIÓN
    function setupRedirect() {
      const countdownEl = document.getElementById('countdown');
      const progressBar = document.getElementById('progressBar');
      const directLink = document.getElementById('directLink');
      
      // Configurar enlace directo
      if (directLink) {
        directLink.href = TARGET_URL;
        directLink.onclick = (e) => {
          e.preventDefault();
          window.location.href = TARGET_URL;
        };
      }
      
      // Contador de 3 segundos
      let seconds = 3;
      const interval = setInterval(() => {
        seconds--;
        
        if (countdownEl) countdownEl.textContent = seconds;
        if (progressBar) {
          const progress = ((3 - seconds) / 3) * 100;
          progressBar.style.width = progress + '%';
        }
        
        if (seconds <= 0) {
          clearInterval(interval);
          window.location.href = TARGET_URL;
        }
      }, 1000);
    }
    
    // INICIAR DETECCIÓN CUANDO EL DOM ESTÉ LISTO
    if (document.readyState === 'loading') {
      document.addEventListener('DOMContentLoaded', () => {
        setTimeout(runAdBlockDetection, 1000);
      });
    } else {
      setTimeout(runAdBlockDetection, 1000);
    }
    
    // DETECCIÓN PERIÓDICA
    setInterval(() => {
      if (detectionScore >= 3 && document.getElementById('errorMessage').style.display === 'block') {
        console.log('🔄 Re-verificando estado del bloqueador...');
        location.reload(true);
      }
    }, 10000);
    
    // PREVENIR ACCESO DIRECTO CON F12
    document.addEventListener('keydown', (e) => {
      if (e.key === 'F12' || (e.ctrlKey && e.shiftKey && e.key === 'I')) {
        if (detectionScore >= 3) {
          e.preventDefault();
          console.warn('Acceso a herramientas de desarrollo bloqueado durante verificación');
        }
      }
    });
  </script>
</body>
</html>
