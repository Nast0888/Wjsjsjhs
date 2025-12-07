<!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <title>Verificar AdBlock — continuar</title>
  <style>
    body { font-family: sans-serif; text-align: center; padding: 50px; }
    #content { display: none; }
    #blocked { display: none; color: red; }
    /* Elementos trampa más evidentes */
    .adsbox, .advertisement, [class*="ad_"], [id*="ad-"] {
      width: 300px;
      height: 250px;
      position: absolute;
      top: -9999px;
      left: -9999px;
    }
    /* Usar nombres que los bloqueadores SI buscan */
    #google_ads_iframe_1, #carbonads, .publi-inner {
      display: block !important;
    }
  </style>
</head>
<body>

  <div id="content">
    <p>Redirigiendo… Si no pasa nada, <a id="go" href="https://devuploads.com/nvgoz9e9zjag">haz clic aquí</a>.</p>
  </div>

  <div id="blocked">
    <p>⚠️ Parece que usas AdBlock o un bloqueador de anuncios. Por favor, desactívalo para continuar.</p>
    <p><button onclick="retryDetection()">Ya lo desactivué, continuar</button></p>
  </div>

  <!-- Múltiples elementos trampa con nombres que los bloqueadores buscan -->
  <div class="adsbox" id="adbox"></div>
  <div class="advertisement" id="ad-728x90"></div>
  <ins class="adsbygoogle" style="display:inline-block;width:728px;height:90px"></ins>
  <div id="carbonads"></div>

  <script>
    // Función más robusta de detección
    function detectAdBlock() {
      var detected = false;
      
      // Método 1: Verificar elementos ocultados
      var testAd = document.createElement('div');
      testAd.innerHTML = '&nbsp;';
      testAd.className = 'adsbox';
      testAd.style.position = 'absolute';
      testAd.style.top = '-1000px';
      testAd.style.left = '-1000px';
      testAd.style.height = '1px';
      document.body.appendChild(testAd);
      
      setTimeout(function() {
        if (testAd.offsetHeight === 0 || 
            testAd.style.display === 'none' || 
            getComputedStyle(testAd).display === 'none') {
          detected = true;
        }
        document.body.removeChild(testAd);
      }, 100);
      
      // Método 2: Intentar cargar un script de ads conocido
      var fakeAd = document.createElement('script');
      fakeAd.src = 'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js';
      fakeAd.onerror = function() {
        detected = true;
      };
      document.head.appendChild(fakeAd);
      
      // Método 3: Verificar si requests de ads son bloqueados
      fetch('https://www.google-analytics.com/analytics.js', { method: 'HEAD', mode: 'no-cors' })
        .catch(function() {
          detected = true;
        });
      
      return detected;
    }

    window.addEventListener('load', function() {
      // Pequeño retraso para dar tiempo a los bloqueadores
      setTimeout(function() {
        if (detectAdBlock()) {
          document.getElementById('blocked').style.display = 'block';
        } else {
          document.getElementById('content').style.display = 'block';
          setTimeout(function() {
            window.location.href = "https://devuploads.com/nvgoz9e9zjag";
          }, 500);
        }
      }, 1000);
    });

    function retryDetection() {
      if (!detectAdBlock()) {
        window.location.href = "https://devuploads.com/nvgoz9e9zjag";
      } else {
        alert('Aún se detecta un bloqueador. Por favor, desactívalo completamente.');
      }
    }
    
    // Detección en tiempo real mientras el usuario navega
    document.addEventListener('DOMContentLoaded', function() {
      setInterval(function() {
        if (detectAdBlock() && document.getElementById('content').style.display !== 'none') {
          document.getElementById('content').style.display = 'none';
          document.getElementById('blocked').style.display = 'block';
        }
      }, 2000);
    });
  </script>

</body>
</html>
