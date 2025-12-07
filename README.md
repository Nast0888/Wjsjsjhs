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
    
    .steps {
      text-align: left;
      background: #f8f9fa;
      padding: 20px;
      border-radius: 5px;
      margin: 20px 0;
    }
  </style>
</head>
<body>
  <div id="loading">
    Verificando seguridad...
    <div style="margin-top: 20px; font-size: 14px; color: #999;">
      Por favor, espera unos segundos
    </div>
  </div>
  
  <div id="successMessage" class="message success" style="display: none;">
    <h1 style="color: #27ae60;">✓ Verificación Exitosa</h1>
    <p>Redirigiendo al contenido en <span id="countdown">3</span> segundos...</p>
    <div style="margin: 30px 0;">
      <div style="height: 10px; background: #eee; border-radius: 5px; overflow: hidden;">
        <div id="progressBar" style="height: 100%; background: #2ecc71; width: 0%; transition: width 0.5s;"></div>
      </div>
    </div>
    <p>Si no redirige automáticamente:</p>
    <a href="#" id="directLink" class="btn btn-success">Acceder al contenido ahora</a>
  </div>
  
  <div id="errorMessage" class="message error" style="display: none;">
    <h1 style="color: #e74c3c;">⛔ Bloqueador Detectado</h1>
    <p><strong>Se ha detectado un bloqueador de anuncios o contenido.</strong></p>
    
    <div class="steps">
      <h3>Para continuar necesitas:</h3>
      <ol>
        <li><strong>Haz clic en el icono de tu bloqueador</strong> (AdBlock, uBlock, etc.) en la barra de extensiones</li>
        <li><strong>Selecciona "Desactivar en este sitio"</strong> o "Pausar"</li>
        <li><strong>Recarga esta página</strong> presionando F5 o Ctrl+R</li>
      </ol>
      <p><em>Si usas Brave: Desactiva "Brave Shields" en la barra de direcciones</em></p>
      <p><em>Si usas Opera: Desactiva "Bloqueador de anuncios" en Configuración del sitio</em></p>
    </div>
    
    <div style="margin-top: 30px;">
      <button onclick="location.reload()" class="btn">Ya desactivé el bloqueador, recargar</button>
    </div>
  </div>

  <!-- ================= ELEMENTOS TRAMPA ================= -->
  <!-- Elementos que serán modificados por bloqueadores -->
  <div id="adBanner" class="ad-banner" style="height: 1px; width: 1px; position: absolute; top: 0; left: 0; background: transparent;"></div>
  <div id="googleAd" class="adsbygoogle" style="display: inline-block !important; width: 1px; height: 1px;"></div>
  <ins class="adsbygoogle" style="display:inline-block;width:1px;height:1px" data-ad-client="ca-pub-123456789" data-ad-slot="123456789"></ins>
  
  <!-- Elemento que los bloqueadores intentan ocultar -->
  <div id="adContainer" style="position: absolute; width: 300px; height: 250px; top: -1000px; left: -1000px;">
    <div class="advertisement" style="width: 100%; height: 100%;"></div>
  </div>

  <!-- ================= LÓGICA DE DETECCIÓN MEJORADA ================= -->
  <script>
    // URL de destino
    const TARGET_URL = "https://devuploads.com/nvgoz9e9zjag";
    let adBlockDetected = false;
    
    // Función para detectar bloqueadores de forma agresiva
    function detectAdBlock() {
      console.log('🔍 Iniciando detección de bloqueador...');
      
      // MÉTODO 1: Verificar si elementos ad están ocultos
      const checkElements = () => {
        const elements = [
          { id: 'adBanner', name: 'Banner Ad' },
          { id: 'googleAd', name: 'Google Ad' },
          { id: 'adContainer', name: 'Ad Container' }
        ];
        
        elements.forEach(el => {
          const element = document.getElementById(el.id);
          if (element) {
            const style = window.getComputedStyle(element);
            console.log(`Elemento ${el.name}: display=${style.display}, visibility=${style.visibility}, height=${element.offsetHeight}`);
            
            // Si el elemento está completamente oculto
            if (style.display === 'none' || 
                style.visibility === 'hidden' ||
                element.offsetHeight === 0 ||
                element.offsetWidth === 0) {
              console.log(`🚨 ${el.name} está oculto - Posible bloqueador`);
              adBlockDetected = true;
            }
          }
        });
      };
      
      // MÉTODO 2: Verificar scripts bloqueados
      const checkScripts = () => {
        return new Promise((resolve) => {
          const testScript = document.createElement('script');
          testScript.src = 'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js';
          testScript.async = true;
          
          let scriptBlocked = false;
          
          testScript.onerror = function() {
            console.log('🚨 Script de Google Ads bloqueado');
            scriptBlocked = true;
            resolve(true);
          };
          
          testScript.onload = function() {
            console.log('✓ Script de Google Ads cargado');
            resolve(false);
          };
          
          // Timeout para scripts que no cargan
          setTimeout(() => {
            if (!scriptBlocked) {
              console.log('⚠️ Timeout en script de ads');
              resolve(false);
            }
          }, 1500);
          
          document.head.appendChild(testScript);
        });
      };
      
      // MÉTODO 3: Fetch a dominio de ads
      const checkFetch = () => {
        return new Promise((resolve) => {
          fetch('https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js', {
            method: 'HEAD',
            mode: 'no-cors',
            cache: 'no-cache'
          })
          .then(() => {
            console.log('✓ Fetch a adsbygoogle exitoso');
            resolve(false);
          })
          .catch(error => {
            console.log('🚨 Fetch bloqueado:', error);
            resolve(true);
          });
        });
      };
      
      // MÉTODO 4: Crear iframe de ad y verificar
      const checkIframe = () => {
        return new Promise((resolve) => {
          const iframe = document.createElement('iframe');
          iframe.src = 'https://googleads.g.doubleclick.net/pagead/html/r20241204/r20190131/zrt_lookup.html';
          iframe.style.width = '1px';
          iframe.style.height = '1px';
          iframe.style.position = 'absolute';
          iframe.style.top = '-100px';
          iframe.style.left = '-100px';
          iframe.style.visibility = 'hidden';
          
          let iframeLoaded = false;
          
          iframe.onload = function() {
            console.log('✓ Iframe de ads cargado');
            iframeLoaded = true;
            document.body.removeChild(iframe);
            resolve(false);
          };
          
          iframe.onerror = function() {
            console.log('🚨 Iframe de ads bloqueado');
            document.body.removeChild(iframe);
            resolve(true);
          };
          
          document.body.appendChild(iframe);
          
          // Timeout
          setTimeout(() => {
            if (!iframeLoaded) {
              console.log('⚠️ Iframe timeout - posible bloqueo');
              if (document.body.contains(iframe)) {
                document.body.removeChild(iframe);
              }
              resolve(true);
            }
          }, 2000);
        });
      };
      
      // MÉTODO 5: Verificar si hay reglas CSS de bloqueadores
      const checkCSSRules = () => {
        try {
          // Verificar reglas CSS comunes de bloqueadores
          const testDiv = document.createElement('div');
          testDiv.className = 'adsbygoogle ad-container ad-banner';
          testDiv.style.cssText = 'width: 300px; height: 250px; position: absolute; top: 0; left: 0;';
          document.body.appendChild(testDiv);
          
          setTimeout(() => {
            const style = window.getComputedStyle(testDiv);
            if (style.display === 'none' || style.visibility === 'hidden') {
              console.log('🚨 Reglas CSS de bloqueador detectadas');
              adBlockDetected = true;
            }
            document.body.removeChild(testDiv);
          }, 100);
        } catch (e) {
          console.log('Error en checkCSSRules:', e);
        }
      };
      
      // Ejecutar todas las verificaciones
      const runAllChecks = async () => {
        try {
          console.log('=== EJECUTANDO VERIFICACIONES ===');
          
          // Verificación 1: Elementos
          checkElements();
          checkCSSRules();
          
          // Verificación 2: Scripts
          const scriptBlocked = await checkScripts();
          if (scriptBlocked) adBlockDetected = true;
          
          // Verificación 3: Fetch
          const fetchBlocked = await checkFetch();
          if (fetchBlocked) adBlockDetected = true;
          
          // Verificación 4: Iframe
          const iframeBlocked = await checkIframe();
          if (iframeBlocked) adBlockDetected = true;
          
          console.log('=== RESULTADO FINAL ===');
          console.log('Bloqueador detectado:', adBlockDetected);
          
          // Mostrar resultado después de 2 segundos
          setTimeout(showResult, 2000);
          
        } catch (error) {
          console.error('Error en verificación:', error);
          setTimeout(showResult, 2000);
        }
      };
      
      runAllChecks();
    }
    
    // Mostrar resultado
    function showResult() {
      document.body.style.display = 'block';
      document.getElementById('loading').style.display = 'none';
      
      if (adBlockDetected) {
        console.log('Mostrando mensaje de bloqueador');
        document.getElementById('errorMessage').style.display = 'block';
        document.getElementById('successMessage').style.display = 'none';
      } else {
        console.log('Mostrando éxito - redirigiendo');
        document.getElementById('successMessage').style.display = 'block';
        document.getElementById('errorMessage').style.display = 'none';
        setupRedirect();
      }
    }
    
    // Configurar redirección
    function setupRedirect() {
      const countdownEl = document.getElementById('countdown');
      const progressBar = document.getElementById('progressBar');
      const directLink = document.getElementById('directLink');
      
      // Configurar enlace
      if (directLink) {
        directLink.href = TARGET_URL;
        directLink.onclick = function(e) {
          e.preventDefault();
          window.location.href = TARGET_URL;
        };
      }
      
      // Contador de 3 segundos
      let seconds = 3;
      const interval = setInterval(function() {
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
    
    // Iniciar detección cuando la página cargue
    window.addEventListener('load', function() {
      // Pequeño delay para que los bloqueadores actúen
      setTimeout(detectAdBlock, 500);
    });
    
    // Verificación periódica por si cambia el estado
    setInterval(() => {
      if (!adBlockDetected && document.getElementById('errorMessage').style.display === 'block') {
        console.log('Verificando estado actual...');
        location.reload();
      }
    }, 5000);
  </script>
</body>
</html>
