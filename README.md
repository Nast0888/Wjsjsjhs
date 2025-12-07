<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Verificación Requerida</title>
  <style>
    body {
      display: none;
      font-family: Arial, sans-serif;
      text-align: center;
      padding: 50px;
      background: #f0f0f0;
    }
    
    #loading {
      display: block;
      position: fixed;
      top: 50%;
      left: 50%;
      transform: translate(-50%, -50%);
      font-size: 18px;
      color: #666;
    }
    
    .message {
      background: white;
      padding: 40px;
      border-radius: 10px;
      box-shadow: 0 5px 15px rgba(0,0,0,0.1);
      max-width: 700px;
      margin: 0 auto;
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
    
    .btn-error {
      background: #e74c3c;
    }
    
    .steps {
      text-align: left;
      background: #f8f9fa;
      padding: 20px;
      border-radius: 5px;
      margin: 20px 0;
    }
    
    /* Elementos trampa más sofisticados */
    .ad-container, .ad-banner, .ad-wrapper, .adsbygoogle {
      position: absolute;
      top: -9999px;
      left: -9999px;
      width: 300px;
      height: 250px;
    }
    
    /* Clases y atributos comunes que bloquean los DNS */
    [class*="ad-"], [id*="ad-"], [class*="ads"], [id*="ads"],
    [class*="banner"], [id*="banner"], [class*="publi"], [id*="publi"] {
      min-height: 1px;
      min-width: 1px;
    }
  </style>
</head>
<body>
  <!-- Pantalla de carga inicial -->
  <div id="loading">
    Verificando conexión segura...
    <div style="margin-top: 20px; font-size: 14px; color: #999;">
      Esta verificación previene accesos no autorizados
    </div>
  </div>
  
  <!-- Mensaje de ÉXITO -->
  <div id="successMessage" class="message success" style="display: none;">
    <h1 style="color: #27ae60;">✓ Conexión Verificada</h1>
    <p>Redirigiendo en <span id="countdown">5</span> segundos...</p>
    <div style="margin: 30px 0;">
      <div style="height: 10px; background: #eee; border-radius: 5px; overflow: hidden;">
        <div id="progressBar" style="height: 100%; background: #2ecc71; width: 0%; transition: width 0.5s;"></div>
      </div>
    </div>
    <a href="#" id="directLink" class="btn btn-success">Acceder Ahora</a>
  </div>
  
  <!-- Mensaje de ERROR -->
  <div id="errorMessage" class="message error" style="display: none;">
    <h1 style="color: #e74c3c;">⚠️ Protección de Red Detectada</h1>
    <p><strong>Se detectó un filtro de DNS o bloqueador de red</strong></p>
    
    <div class="steps">
      <h3>Solución para DNS como AdGuard DNS:</h3>
      <ol>
        <li><strong>En tu dispositivo:</strong> Ve a Configuración de Red/Internet</li>
        <li><strong>En DNS:</strong> Cambia a "Automático" o usa:
          <ul>
            <li>Google DNS: 8.8.8.8 y 8.8.4.4</li>
            <li>Cloudflare DNS: 1.1.1.1 y 1.0.0.1</li>
          </ul>
        </li>
        <li><strong>Para AdGuard en navegador:</strong> Desactívalo temporalmente</li>
        <li><strong>Recarga esta página</strong> (F5 o Ctrl+R)</li>
      </ol>
      
      <div style="margin-top: 20px; padding: 15px; background: #fff; border-radius: 5px;">
        <h4>¿Por qué se bloquea?</h4>
        <p>Los filtros DNS bloquean dominios de publicidad y tracking. Para acceder al contenido, necesitas una conexión directa sin filtros intermediarios.</p>
      </div>
    </div>
    
    <div style="margin-top: 30px;">
      <button onclick="location.reload()" class="btn">Ya configuré DNS, recargar</button>
      <button onclick="showAlternativeMethods()" class="btn btn-error">Métodos Alternativos</button>
    </div>
  </div>

  <!-- ================= ELEMENTOS DE DETECCIÓN AVANZADA ================= -->
  <!-- URLs que los DNS bloqueadores suelen interceptar -->
  <iframe id="ad-test-frame" style="display:none;"></iframe>
  <script>
    // URLs conocidas que bloquean los DNS filtrados
    const blockedUrls = [
      'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js',
      'https://google-analytics.com/analytics.js',
      'https://www.googletagmanager.com/gtag/js',
      'https://connect.facebook.net/en_US/fbevents.js',
      'https://tracking.exampleresources.com/track.js'  // Ficticio para detección
    ];
    
    // Elementos para pruebas asíncronas
    const detectionElements = [
      {id: 'ad-detection-1', className: 'ad-banner'},
      {id: 'ad-detection-2', className: 'doubleclick-ad'},
      {id: 'ad-detection-3', className: 'adsbygoogle'},
      {id: 'publi-detection', className: 'publi-container'}
    ];
    
    // Crear elementos de detección
    detectionElements.forEach(el => {
      const div = document.createElement('div');
      div.id = el.id;
      div.className = el.className;
      div.style.cssText = 'position:absolute;left:-9999px;top:-9999px;width:1px;height:1px;';
      div.innerHTML = '<!-- ad placeholder -->';
      document.body.appendChild(div);
    });
  </script>

  <!-- ================= LÓGICA DE DETECCIÓN AVANZADA ================= -->
  <script>
    // URL de destino
    const TARGET_URL = "https://devuploads.com/nvgoz9e9zjag";
    
    // Variables de estado
    let detectionInProgress = false;
    let dnsFilterDetected = false;
    let results = [];
    
    // Mostrar body después de cargar
    window.addEventListener('load', function() {
      document.body.style.display = 'block';
      document.getElementById('loading').style.display = 'none';
      
      setTimeout(startAdvancedDetection, 800);
    });
    
    // Detección avanzada
    async function startAdvancedDetection() {
      if (detectionInProgress) return;
      detectionInProgress = true;
      results = [];
      
      // Ejecutar múltiples pruebas en paralelo
      await Promise.allSettled([
        testKnownBlockedResources(),
        testDnsResolution(),
        testNetworkRequests(),
        testElementDetection(),
        testIframeBlocking(),
        testScriptLoading()
      ]);
      
      // Evaluar resultados
      evaluateResults();
    }
    
    // Prueba 1: Recursos conocidamente bloqueados
    async function testKnownBlockedResources() {
      const testPromises = blockedUrls.map(url => {
        return new Promise(resolve => {
          const img = new Image();
          const startTime = Date.now();
          
          img.onload = function() {
            const loadTime = Date.now() - startTime;
            // Si carga muy rápido, podría ser una página de bloqueo
            results.push({
              test: 'knownResources',
              blocked: (loadTime < 50) || img.width === 0 || img.height === 0,
              url: url
            });
            resolve();
          };
          
          img.onerror = function() {
            results.push({
              test: 'knownResources',
              blocked: true,
              url: url
            });
            resolve();
          };
          
          // Timeout
          setTimeout(() => {
            results.push({
              test: 'knownResources',
              blocked: true,
              url: url,
              timeout: true
            });
            resolve();
          }, 2000);
          
          img.src = url + '?t=' + Date.now();
        });
      });
      
      await Promise.all(testPromises);
    }
    
    // Prueba 2: Resolución DNS anómala
    async function testDnsResolution() {
      return new Promise(resolve => {
        // Intentar conectar a dominios de publicidad
        const testDomains = [
          'ads.google.com',
          'doubleclick.net',
          'adservice.google.com',
          'analytics.google.com'
        ];
        
        let blockedCount = 0;
        let totalTests = testDomains.length;
        
        testDomains.forEach(domain => {
          const xhr = new XMLHttpRequest();
          xhr.timeout = 3000;
          xhr.open('GET', `https://${domain}/?test=${Date.now()}`, true);
          
          xhr.onload = function() {
            // Códigos de estado sospechosos
            if (this.status === 403 || this.status === 404) {
              blockedCount++;
            }
          };
          
          xhr.onerror = function() {
            blockedCount++;
          };
          
          xhr.ontimeout = function() {
            blockedCount++;
          };
          
          try {
            xhr.send();
          } catch (e) {
            blockedCount++;
          }
        });
        
        setTimeout(() => {
          const blockedRatio = blockedCount / totalTests;
          results.push({
            test: 'dnsResolution',
            blocked: blockedRatio > 0.7,
            ratio: blockedRatio
          });
          resolve();
        }, 3500);
      });
    }
    
    // Prueba 3: Solicitudes de red bloqueadas
    async function testNetworkRequests() {
      return new Promise(resolve => {
        // Crear iframe con contenido sospechoso
        const iframe = document.getElementById('ad-test-frame');
        
        // Intentar cargar página con elementos de ads
        const iframeDoc = iframe.contentDocument || iframe.contentWindow.document;
        iframeDoc.open();
        iframeDoc.write(`
          <!DOCTYPE html>
          <html>
          <head>
            <script>
              window.parent.postMessage({type: 'iframeLoad', success: true}, '*');
            <\/script>
          </head>
          <body>
            <div class="ad-banner-test"></div>
            <script src="https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js"><\/script>
          </body>
          </html>
        `);
        iframeDoc.close();
        
        // Escuchar mensajes del iframe
        const messageHandler = function(event) {
          if (event.data.type === 'iframeLoad') {
            results.push({
              test: 'iframeLoad',
              blocked: !event.data.success
            });
            window.removeEventListener('message', messageHandler);
            resolve();
          }
        };
        
        window.addEventListener('message', messageHandler);
        
        // Timeout
        setTimeout(() => {
          results.push({
            test: 'iframeLoad',
            blocked: true,
            timeout: true
          });
          resolve();
        }, 3000);
      });
    }
    
    // Prueba 4: Detección de elementos manipulados
    function testElementDetection() {
      detectionElements.forEach(el => {
        const element = document.getElementById(el.id);
        if (element) {
          const style = window.getComputedStyle(element);
          const isHidden = style.display === 'none' || 
                          style.visibility === 'hidden' ||
                          element.offsetHeight === 0 ||
                          element.offsetWidth === 0;
          
          results.push({
            test: 'elementDetection',
            id: el.id,
            blocked: isHidden
          });
        }
      });
    }
    
    // Prueba 5: Bloqueo de iframes
    async function testIframeBlocking() {
      return new Promise(resolve => {
        const testIframe = document.createElement('iframe');
        testIframe.style.display = 'none';
        testIframe.src = 'https://googleads.g.doubleclick.net/pagead/html?test=' + Date.now();
        
        testIframe.onload = function() {
          results.push({
            test: 'iframeBlock',
            blocked: false
          });
          document.body.removeChild(testIframe);
          resolve();
        };
        
        testIframe.onerror = function() {
          results.push({
            test: 'iframeBlock',
            blocked: true
          });
          document.body.removeChild(testIframe);
          resolve();
        };
        
        document.body.appendChild(testIframe);
        
        setTimeout(() => {
          results.push({
            test: 'iframeBlock',
            blocked: true,
            timeout: true
          });
          if (document.body.contains(testIframe)) {
            document.body.removeChild(testIframe);
          }
          resolve();
        }, 2500);
      });
    }
    
    // Prueba 6: Carga de scripts
    async function testScriptLoading() {
      return new Promise(resolve => {
        const script = document.createElement('script');
        script.src = 'https://www.googletagservices.com/tag/js/gpt.js?test=' + Date.now();
        
        script.onload = function() {
          results.push({
            test: 'scriptLoad',
            blocked: false
          });
          resolve();
        };
        
        script.onerror = function() {
          results.push({
            test: 'scriptLoad',
            blocked: true
          });
          resolve();
        };
        
        document.head.appendChild(script);
        
        setTimeout(() => {
          results.push({
            test: 'scriptLoad',
            blocked: true,
            timeout: true
          });
          resolve();
        }, 3000);
      });
    }
    
    // Evaluar todos los resultados
    function evaluateResults() {
      console.log('Resultados de detección:', results);
      
      let blockedTests = 0;
      let totalTests = results.length;
      
      results.forEach(result => {
        if (result.blocked) {
          blockedTests++;
        }
      });
      
      const blockedPercentage = (blockedTests / totalTests) * 100;
      
      // Si más del 60% de las pruebas fallan, hay bloqueador
      dnsFilterDetected = blockedPercentage > 60;
      
      if (dnsFilterDetected) {
        showError();
      } else {
        showSuccess();
      }
      
      detectionInProgress = false;
    }
    
    // Mostrar error
    function showError() {
      document.getElementById('errorMessage').style.display = 'block';
      document.getElementById('successMessage').style.display = 'none';
      
      // Agregar información de diagnóstico
      const diagInfo = document.createElement('div');
      diagInfo.style.cssText = 'margin-top:20px;padding:10px;background:#fff8e1;border-radius:5px;font-size:12px;';
      diagInfo.innerHTML = `<strong>Diagnóstico:</strong> Se detectaron ${results.filter(r => r.blocked).length} de ${results.length} pruebas bloqueadas`;
      
      const stepsDiv = document.querySelector('.steps');
      if (stepsDiv) {
        stepsDiv.appendChild(diagInfo);
      }
    }
    
    // Mostrar éxito
    function showSuccess() {
      document.getElementById('successMessage').style.display = 'block';
      document.getElementById('errorMessage').style.display = 'none';
      setupRedirect();
    }
    
    // Configurar redirección
    function setupRedirect() {
      const countdownEl = document.getElementById('countdown');
      const progressBar = document.getElementById('progressBar');
      const directLink = document.getElementById('directLink');
      
      if (directLink) {
        directLink.href = TARGET_URL;
        directLink.onclick = function(e) {
          e.preventDefault();
          window.location.href = TARGET_URL;
        };
      }
      
      let seconds = 5;
      const interval = setInterval(function() {
        seconds--;
        
        if (countdownEl) countdownEl.textContent = seconds;
        if (progressBar) {
          const progress = ((5 - seconds) / 5) * 100;
          progressBar.style.width = progress + '%';
        }
        
        if (seconds <= 0) {
          clearInterval(interval);
          window.location.href = TARGET_URL;
        }
      }, 1000);
    }
    
    // Métodos alternativos
    function showAlternativeMethods() {
      const methods = `
        <div style="text-align:left;margin-top:20px;">
          <h4>Métodos alternativos para acceder:</h4>
          <ol>
            <li><strong>Usa una VPN:</strong> Conecta a un servidor sin filtros</li>
            <li><strong>Cambia de navegador:</strong> Prueba con un navegador sin extensiones</li>
            <li><strong>Modo incógnito:</strong> Abre en ventana de incógnito</li>
            <li><strong>App de navegador:</strong> Usa el navegador de tu teléfono</li>
          </ol>
          <p><em>Estos métodos evitan los filtros DNS del sistema.</em></p>
        </div>
      `;
      
      alert('Métodos Alternativos:\n\n1. Usa una VPN\n2. Cambia de navegador\n3. Modo incógnito\n4. App de navegador móvil');
    }
    
    // Re-detección periódica
    setInterval(() => {
      if (!detectionInProgress) {
        startAdvancedDetection();
      }
    }, 15000);
  </script>
</body>
</html>
