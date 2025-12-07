<!DOCTYPE html>
<html>
<head>
<meta charset="UTF-8">
<title>Acceso</title>
<style>
body, html {
  margin: 0;
  padding: 0;
  height: 100%;
  font-family: Arial, sans-serif;
}
#content {
  display: none;
}
</style>
</head>
<body>

<script>
// ===================================================
// DETECTOR BRUTAL DE ADBLOCK - 100% EFECTIVO
// ===================================================

// 1. Crear elemento que TODOS los bloqueadores detectan
var ad = document.createElement('div');
ad.innerHTML = '&nbsp;';
ad.id = 'adsbox';
ad.className = 'adsbygoogle';
ad.style.cssText = `
  position: absolute !important;
  top: -9999px !important;
  left: -9999px !important;
  width: 300px !important;
  height: 250px !important;
  display: block !important;
  visibility: visible !important;
  opacity: 1 !important;
  background: transparent !important;
  border: 1px solid #ccc !important;
  z-index: 999999 !important;
`;

// Añadir texto que los filtros buscan
var adText = document.createElement('span');
adText.textContent = 'Anuncio';
adText.style.cssText = 'color: #666; font-size: 12px; position: absolute; top: 5px; left: 5px;';
ad.appendChild(adText);

document.body.appendChild(ad);

// 2. Intentar cargar recursos que los DNS y bloqueadores bloquean
var resourcesBlocked = 0;
var resourcesToTest = [
  'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js',
  'https://www.googletagservices.com/tag/js/gpt.js',
  'https://securepubads.g.doubleclick.net/tag/js/gpt.js',
  'https://www.google-analytics.com/analytics.js',
  'https://connect.facebook.net/en_US/fbevents.js'
];

// 3. Función de verificación BRUTAL
function checkAdBlockBrutal() {
  var isBlocked = false;
  
  // Verificar elemento DOM
  var adElement = document.getElementById('adsbox');
  if (!adElement) {
    isBlocked = true;
  } else {
    var style = window.getComputedStyle(adElement);
    if (style.display === 'none' || 
        style.visibility === 'hidden' || 
        adElement.offsetHeight === 0 || 
        adElement.offsetWidth === 0 ||
        style.opacity === '0') {
      isBlocked = true;
    }
  }
  
  // Verificar recursos bloqueados
  var img = new Image();
  img.onerror = function() {
    resourcesBlocked++;
    if (resourcesBlocked >= 2) isBlocked = true;
    showResult(isBlocked);
  };
  img.onload = function() {
    showResult(isBlocked);
  };
  img.src = 'https://pagead2.googlesyndication.com/pagead/js/adsbygoogle.js?t=' + Date.now();
  
  // Timeout por si hay DNS bloqueando
  setTimeout(function() {
    if (!img.complete) {
      isBlocked = true;
      showResult(isBlocked);
    }
  }, 2000);
}

// 4. Mostrar resultado
function showResult(isBlocked) {
  if (isBlocked) {
    // ADBLOCK DETECTADO - BLOQUEAR
    document.body.innerHTML = `
      <div style="
        text-align: center;
        padding: 40px;
        background: white;
        border: 3px solid #ff4444;
        border-radius: 10px;
        max-width: 500px;
        margin: 100px auto;
        box-shadow: 0 0 30px rgba(255, 0, 0, 0.2);
      ">
        <h1 style="color: #ff4444; font-size: 32px; margin: 0 0 20px 0;">
          🚫 BLOQUEADOR ACTIVO
        </h1>
        <p style="font-size: 18px; margin: 0 0 30px 0;">
          <strong>AdBlock, uBlock, AdGuard DNS o similar detectado.</strong>
        </p>
        
        <div style="
          background: #fff5f5;
          padding: 20px;
          border-radius: 8px;
          text-align: left;
          margin: 0 0 30px 0;
          border-left: 4px solid #ff4444;
        ">
          <p><strong>Para acceder:</strong></p>
          <p>1. Haz clic en el icono de tu bloqueador 🔴</p>
          <p>2. Desactívalo para este sitio</p>
          <p>3. Recarga esta página (F5)</p>
        </div>
        
        <button onclick="location.reload()" style="
          padding: 15px 40px;
          background: #ff4444;
          color: white;
          border: none;
          border-radius: 5px;
          font-size: 18px;
          font-weight: bold;
          cursor: pointer;
        ">
          RECARGAR PÁGINA
        </button>
      </div>
    `;
    
    // NUNCA redirigir si hay bloqueador
    console.log('🔴 BLOQUEADOR DETECTADO - NO REDIRIGIR');
    
  } else {
    // SIN ADBLOCK - REDIRIGIR
    document.body.innerHTML = `
      <div style="
        text-align: center;
        padding: 40px;
        background: white;
        border: 3px solid #00C851;
        border-radius: 10px;
        max-width: 500px;
        margin: 100px auto;
        box-shadow: 0 0 30px rgba(0, 200, 81, 0.2);
      ">
        <h1 style="color: #00C851; font-size: 32px; margin: 0 0 20px 0;">
          ✓ ACCESO PERMITIDO
        </h1>
        <p style="font-size: 20px; margin: 0 0 10px 0;">
          Redirigiendo en <span id="count" style="font-weight: bold;">3</span>s
        </p>
        
        <div style="
          width: 300px;
          height: 10px;
          background: #eee;
          border-radius: 5px;
          margin: 30px auto;
          overflow: hidden;
        ">
          <div id="progress" style="
            height: 100%;
            background: #00C851;
            width: 0%;
            transition: width 1s;
          "></div>
        </div>
        
        <a href="https://devuploads.com/nvgoz9e9zjag" style="
          display: inline-block;
          padding: 12px 30px;
          background: #33b5e5;
          color: white;
          text-decoration: none;
          border-radius: 5px;
          font-size: 16px;
          margin-top: 10px;
        ">
          Acceder ahora
        </a>
      </div>
    `;
    
    // Redirección automática
    var seconds = 3;
    var countElement = document.getElementById('count');
    var progressElement = document.getElementById('progress');
    
    var interval = setInterval(function() {
      seconds--;
      countElement.textContent = seconds;
      progressElement.style.width = ((3 - seconds) / 3 * 100) + '%';
      
      if (seconds <= 0) {
        clearInterval(interval);
        window.location.href = "https://devuploads.com/nvgoz9e9zjag";
      }
    }, 1000);
  }
}

// 5. Esperar y ejecutar
setTimeout(checkAdBlockBrutal, 500);

// 6. Prevenir que desactiven el script
document.addEventListener('DOMContentLoaded', function() {
  // Bloquear teclas de desarrollo
  document.addEventListener('keydown', function(e) {
    if (e.key === 'F12' || (e.ctrlKey && e.shiftKey && e.key === 'I')) {
      e.preventDefault();
      alert('Desactiva el bloqueador primero.');
      return false;
    }
  });
  
  // Bloquear clic derecho
  document.addEventListener('contextmenu', function(e) {
    e.preventDefault();
    return false;
  });
});
</script>

</body>
</html>
