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
      max-width: 600px;
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
    
    /* Elementos trampa específicos para AdGuard */
    .adguard-trap, .dns-filter-test, .adblock-detector {
      position: absolute;
      top: -9999px;
      left: -9999px;
      width: 1px;
      height: 1px;
    }
    
    .adguard-trap:before {
      content: "AdGuard-Block";
      color: #999;
      font-size: 10px;
    }
  </style>
</head>
<body>
  <!-- Pantalla de carga -->
  <div id="loading">
    Analizando configuración de red...
    <div style="margin-top: 20px; font-size: 14px; color: #999;">
      Detectando filtros DNS
    </div>
  </div>
  
  <!-- Mensaje de ÉXITO -->
  <div id="successMessage" class="message success" style="display: none;">
    <h1 style="color: #27ae60;">✓ DNS Verificado</h1>
    <p>No se detectó AdGuard DNS</p>
    <p>Redirigiendo en <span id="countdown">5</span> segundos...</p>
    <div style="margin: 30px 0;">
      <div style="height: 10px; background: #eee; border-radius: 5px; overflow: hidden;">
        <div id="progressBar" style="height: 100%; background: #2ecc71; width: 0%; transition: width 0.5s;"></div>
      </div>
    </div>
    <a href="#" id="directLink" class="btn btn-success">Acceder Ahora</a>
  </div>
  
  <!-- Mensaje de ERROR - AdGuard DNS detectado -->
  <div id="errorMessage" class="message error" style="display: none;">
    <h1 style="color: #e74c3c;">⛔ AdGuard DNS Detectado</h1>
    <p><strong>Se ha detectado dns.adguard.com en tu configuración de red</strong></p>
    
    <div class="steps">
      <h3>¿Qué está bloqueando AdGuard DNS?</h3>
      <ul>
        <li><strong>Publicidad:</strong> Dominios de anuncios</li>
        <li><strong>Trackers:</strong> Rastreadores y analytics</li>
        <li><strong>Malware:</strong> Sitios maliciosos</li>
        <li><strong>Phishing:</strong> Sitios fraudulentos</li>
      </ul>
      
      <h3 style="margin-top: 20px;">Para desactivar AdGuard DNS:</h3>
      <ol>
        <li><strong>Android/iOS:</strong> Configuración → WiFi → DNS → Cambiar a Automático</li>
        <li><strong>Windows:</strong> Panel de Control → Red → Propiedades TCP/IPv4 → DNS Automático</li>
        <li><strong>Mac:</strong> Preferencias → Red → Avanzado → DNS → Quitar dns.adguard.com</li>
        <li><strong>Router:</strong> Accede a 192.168.1.1 → DNS → Restaurar predeterminado</li>
      </ol>
      
      <div style="margin-top: 20px; padding: 10px; background: #e8f4f8; border-radius: 5px;">
        <p><strong>DNS alternativos recomendados:</strong></p>
        <ul style="font-size: 14px;">
          <li><code>8.8.8.8</code> y <code>8.8.4.4</code> (Google)</li>
          <li><code>1.1.1.1</code> y <code>1.0.0.1</code> (Cloudflare)</li>
        </ul>
      </div>
    </div>
    
    <div style="margin-top: 30px;">
      <button onclick="location.reload()" class="btn">Ya cambié DNS, recargar</button>
      <button onclick="testWithoutDNS()" class="btn btn-error">Probar sin DNS</button>
    </div>
    
    <!-- Información específica de lo que detectamos -->
    <div id="detectionInfo" style="margin-top: 20px; font-size: 12px; color: #666; display: none;">
      <p>Lo que detectamos: <span id="blockedItems"></span></p>
    </div>
  </div>

  <!-- ================= DETECCIÓN ESPECÍFICA PARA ADGUARD DNS ================= -->
  <!-- Elementos que AdGuard DNS específicamente bloquea -->
  <div class="adguard-trap" id="ag-trap1"></div>
  <div class="dns-filter-test" id="ag-trap2"></div>
  <div class="adblock-detector" id="ag-trap3"></div>
  
  <!-- Scripts que AdGuard DNS bloquea específicamente -->
  <script>
    // URLs específicamente bloqueadas por AdGuard DNS
    const adguardSpecificUrls = [
      'https://ad.doubleclick.net/ddm/adj/',
      'https://googleads.g.doubleclick.net/pagead/',
      'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js',
      'https://www.googletagmanager.com/gtag/js',
      'https://www.google-analytics.com/analytics.js',
      'https://connect.facebook.net/en_US/fbevents.js',
      'https://sb.scorecardresearch.com/beacon.js',
      'https://cdn.ampproject.org/v0/amp-ad-',
      'https://cdn.taboola.com/libtrc/',
      'https://s.amazon-adsystem.com/ecm3.js'
    ];
    
    // Elementos con clases que AdGuard DNS filtra
    const adguardFilteredClasses = [
      'ad', 'ads', 'advert', 'banner', 'publi', 'sponsor',
      'doubleclick', 'googlead', 'adservice', 'adcontainer'
    ];
  </script>

  <!-- ================= LÓGICA PRINCIPAL MEJORADA ================= -->
  <script>
    // URL de destino
    const TARGET_URL = "https://devuploads.com/nvgoz9e9zjag";
    
    // Tu detección original (bloqueadores de navegador)
    let browserAdBlockDetected = false;
    
    // Nueva detección para AdGuard DNS
    let adguardDNSDetected = false;
    let blockedItems = [];
    
    // Mostrar body después de cargar
    window.addEventListener('load', function() {
      document.body.style.display = 'block';
      document.getElementById('loading').style.display = 'none';
      
      // Primero tu detección original
      setTimeout(checkOriginalAdBlock, 1000);
      // Luego detección específica de AdGuard DNS
      setTimeout(checkAdGuardDNS, 1500);
    });
    
    // ============ TU DETECCIÓN ORIGINAL (sin cambios) ============
    function checkOriginalAdBlock() {
      let adBlockDetected = false;
      
      // MÉTODO 1: Verificar si los elementos "ad" están ocultos
      const adElements = ['ad1', 'ad2', 'ad3'];
      adElements.forEach(id => {
        const el = document.getElementById(id);
        if (el) {
          const style = window.getComputedStyle(el);
          if (style.display === 'none' || 
              style.visibility === 'hidden' || 
              el.offsetHeight === 0 ||
              el.offsetWidth === 0) {
            adBlockDetected = true;
          }
        }
      });
      
      // MÉTODO 2: Verificar si el script de ads fue bloqueado
      if (window.adScriptBlocked) {
        adBlockDetected = true;
      }
      
      // MÉTODO 3: Intentar cargar un recurso de ads
      const img = new Image();
      img.src = 'https://www.google-analytics.com/collect?v=1&t=pageview&tid=UA-TEST-1';
      img.onerror = function() {
        adBlockDetected = true;
      };
      
      // Asignar resultado
      browserAdBlockDetected = adBlockDetected;
      
      // Timeout para mostrar resultado
      setTimeout(function() {
        if (!adguardDNSDetected) { // Solo si no hay AdGuard DNS
          showFinalResult();
        }
      }, 2000);
    }
    
    // ============ DETECCIÓN ESPECÍFICA PARA ADGUARD DNS ============
    async function checkAdGuardDNS() {
      console.log("Iniciando detección específica de AdGuard DNS...");
      
      let adguardBlockCount = 0;
      let totalTests = 0;
      blockedItems = [];
      
      // PRUEBA 1: Elementos DOM específicos de AdGuard
      adguardFilteredClasses.forEach(className => {
        totalTests++;
        const testDiv = document.createElement('div');
        testDiv.className = className + '-test-' + Date.now();
        testDiv.style.cssText = 'position:absolute;left:-9999px;width:1px;height:1px;';
        testDiv.innerHTML = '<!-- ' + className + ' content -->';
        document.body.appendChild(testDiv);
        
        const style = window.getComputedStyle(testDiv);
        if (style.display === 'none' || testDiv.offsetHeight === 0) {
          adguardBlockCount++;
          blockedItems.push('Clase: ' + className);
        }
        
        document.body.removeChild(testDiv);
      });
      
      // PRUEBA 2: Recursos específicamente bloqueados por AdGuard DNS
      const resourceTests = await testAdGuardSpecificResources();
      adguardBlockCount += resourceTests.blockedCount;
      totalTests += resourceTests.totalTests;
      blockedItems = blockedItems.concat(resourceTests.blockedUrls);
      
      // PRUEBA 3: DNS resolution - Intento de conexión directa
      const dnsTest = await testDNSResolution();
      if (dnsTest.blocked) {
        adguardBlockCount++;
        totalTests++;
        blockedItems.push('DNS: ' + dnsTest.domain);
      }
      
      // PRUEBA 4: Patrones específicos de AdGuard
      const patternTest = await testAdGuardPatterns();
      adguardBlockCount += patternTest.blockedCount;
      totalTests += patternTest.totalTests;
      blockedItems = blockedItems.concat(patternTest.blockedPatterns);
      
      // Evaluar resultados
      const blockedPercentage = (adguardBlockCount / totalTests) * 100;
      
      // Si más del 70% de las pruebas específicas de AdGuard fallan
      adguardDNSDetected = blockedPercentage > 70;
      
      console.log("Resultado AdGuard DNS:", {
        detected: adguardDNSDetected,
        blockedPercentage: blockedPercentage,
        blockedItems: blockedItems
      });
      
      // Mostrar resultado final
      setTimeout(showFinalResult, 500);
    }
    
    // Probar recursos específicos de AdGuard
    async function testAdGuardSpecificResources() {
      let blockedCount = 0;
      let blockedUrls = [];
      
      const promises = adguardSpecificUrls.slice(0, 5).map(url => {
        return new Promise(resolve => {
          const xhr = new XMLHttpRequest();
          xhr.timeout = 3000;
          xhr.open('HEAD', url + '?t=' + Date.now(), true);
          
          xhr.onload = function() {
            // AdGuard DNS a menudo devuelve 0 o códigos específicos
            if (this.status === 0 || this.status === 403 || this.status === 404) {
              blockedCount++;
              blockedUrls.push(url.split('/')[2]); // Solo el dominio
            }
            resolve();
          };
          
          xhr.onerror = function() {
            blockedCount++;
            blockedUrls.push(url.split('/')[2]);
            resolve();
          };
          
          xhr.ontimeout = function() {
            blockedCount++;
            blockedUrls.push(url.split('/')[2]);
            resolve();
          };
          
          try {
            xhr.send();
          } catch(e) {
            blockedCount++;
            blockedUrls.push(url.split('/')[2]);
            resolve();
          }
        });
      });
      
      await Promise.allSettled(promises);
      
      return {
        blockedCount: blockedCount,
        totalTests: adguardSpecificUrls.slice(0, 5).length,
        blockedUrls: blockedUrls
      };
    }
    
    // Probar resolución DNS
    async function testDNSResolution() {
      return new Promise(resolve => {
        // Dominios específicamente bloqueados por AdGuard DNS
        const testDomain = 'doubleclick.net';
        const testUrl = `https://${testDomain}/test-${Date.now()}.gif`;
        
        const img = new Image();
        const startTime = Date.now();
        
        img.onload = function() {
          const loadTime = Date.now() - startTime;
          // Si carga extremadamente rápido (<10ms), podría ser respuesta falsa de AdGuard
          resolve({
            blocked: loadTime < 10 || img.width === 0,
            domain: testDomain,
            time: loadTime
          });
        };
        
        img.onerror = function() {
          resolve({
            blocked: true,
            domain: testDomain,
            time: null
          });
        };
        
        // Timeout corto
        setTimeout(() => {
          resolve({
            blocked: true,
            domain: testDomain,
            timeout: true
          });
        }, 2000);
        
        img.src = testUrl;
      });
    }
    
    // Probar patrones específicos de AdGuard
    async function testAdGuardPatterns() {
      let blockedCount = 0;
      let blockedPatterns = [];
      
      // Patrones de URL que AdGuard DNS bloquea
      const patterns = [
        '/adsystem/',
        '/adserver/',
        '/tracking/',
        '/analytics/',
        '/beacon/'
      ];
      
      patterns.forEach(pattern => {
        // Crear elemento con patrón sospechoso
        const testId = 'test-' + pattern.replace(/\//g, '-') + Date.now();
        const testDiv = document.createElement('div');
        testDiv.id = testId;
        testDiv.innerHTML = `<!-- ${pattern} -->`;
        testDiv.style.cssText = 'position:absolute;left:-9999px;width:1px;height:1px;';
        
        document.body.appendChild(testDiv);
        
        // Verificar si fue modificado/bloqueado
        const computed = window.getComputedStyle(testDiv);
        const isBlocked = computed.display === 'none' || 
                         computed.opacity === '0' || 
                         testDiv.offsetParent === null;
        
        if (isBlocked) {
          blockedCount++;
          blockedPatterns.push('Patrón: ' + pattern);
        }
        
        document.body.removeChild(testDiv);
      });
      
      return {
        blockedCount: blockedCount,
        totalTests: patterns.length,
        blockedPatterns: blockedPatterns
      };
    }
    
    // ============ MOSTRAR RESULTADO FINAL ============
    function showFinalResult() {
      console.log("Resultados finales:", {
        browserAdBlock: browserAdBlockDetected,
        adguardDNS: adguardDNSDetected,
        blockedItems: blockedItems
      });
      
      if (adguardDNSDetected) {
        // Mostrar mensaje específico de AdGuard DNS
        document.getElementById('errorMessage').style.display = 'block';
        document.getElementById('successMessage').style.display = 'none';
        
        // Mostrar qué detectamos
        if (blockedItems.length > 0) {
          document.getElementById('detectionInfo').style.display = 'block';
          const uniqueItems = [...new Set(blockedItems)];
          document.getElementById('blockedItems').textContent = 
            uniqueItems.slice(0, 5).join(', ') + (uniqueItems.length > 5 ? '...' : '');
        }
        
        console.log('⚠️ AdGuard DNS detectado - Acceso denegado');
      } else if (browserAdBlockDetected) {
        // Mostrar tu mensaje original de bloqueador
        document.getElementById('errorMessage').style.display = 'block';
        document.getElementById('successMessage').style.display = 'none';
        
        // Cambiar texto para bloqueador de navegador
        document.querySelector('#errorMessage h1').textContent = '⛔ Bloqueador Detectado';
        document.querySelector('#errorMessage p strong').textContent = 'Se ha detectado un bloqueador de anuncios';
        
        console.log('Bloqueador de navegador detectado - Acceso denegado');
      } else {
        // Sin bloqueadores - Acceso permitido
        document.getElementById('successMessage').style.display = 'block';
        document.getElementById('errorMessage').style.display = 'none';
        setupRedirect();
      }
    }
    
    // ============ REDIRECCIÓN ============
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
    
    // Probar sin DNS (método alternativo)
    function testWithoutDNS() {
      // Crear iframe con URL diferente para bypass
      const testFrame = document.createElement('iframe');
      testFrame.style.display = 'none';
      testFrame.src = TARGET_URL + '?bypass=1';
      
      testFrame.onload = function() {
        alert('Conexión exitosa. Puedes acceder usando:\n\n1. VPN\n2. Proxy web\n3. Navegador Tor\n\nEl acceso directo está bloqueado por AdGuard DNS.');
      };
      
      testFrame.onerror = function() {
        alert('Aún bloqueado. AdGuard DNS está filtrando esta conexión.');
      };
      
      document.body.appendChild(testFrame);
      
      setTimeout(() => {
        if (document.body.contains(testFrame)) {
          document.body.removeChild(testFrame);
        }
      }, 5000);
    }
    
    // Script de ads que será bloqueado (tu original)
    var fakeAdScript = document.createElement('script');
    fakeAdScript.src = 'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js';
    fakeAdScript.onerror = function() {
      window.adScriptBlocked = true;
    };
    document.head.appendChild(fakeAdScript);
    
    // Verificar periódicamente
    setInterval(function() {
      if (!adguardDNSDetected) {
        checkOriginalAdBlock();
      }
    }, 10000);
  </script>
</body>
</html>
