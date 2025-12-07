<script>
  // URL de destino
  const TARGET_URL = "https://devuploads.com/nvgoz9e9zjag";

  // Mostrar body después de que todo cargue
  window.addEventListener('load', function() {
    document.body.style.display = 'block';
    document.getElementById('loading').style.display = 'none';

    // Verificar solo una vez
    setTimeout(checkAdBlock, 800);
  });

  // Función principal de verificación
  function checkAdBlock() {
    let adBlockDetected = false;

    // MÉTODO 1: detectar si los elementos "ads" fueron alterados
    const adElements = ['ad1', 'ad2', 'ad3'];

    adElements.forEach(id => {
      const el = document.getElementById(id);
      if (!el) return;

      const style = window.getComputedStyle(el);

      // Solo marcar como bloqueo si el bloque fue **eliminado u ocultado completamente**
      if (
        el.offsetHeight === 0 &&
        el.offsetWidth === 0 &&
        style.display === 'none'
      ) {
        adBlockDetected = true;
      }
    });

    // MÉTODO 2: script de ads bloqueado (este no da falsos positivos)
    if (window.adScriptBlocked === true) {
      adBlockDetected = true;
    }

    // Mostrar resultado
    showResult(adBlockDetected);
  }

  // Mostrar éxito o error sin volver a ejecutar jamás
  function showResult(adBlockDetected) {
    if (adBlockDetected) {
      document.getElementById('errorMessage').style.display = 'block';
      document.getElementById('successMessage').style.display = 'none';
      console.log("Bloqueador detectado");
    } else {
      document.getElementById('successMessage').style.display = 'block';
      document.getElementById('errorMessage').style.display = 'none';
      setupRedirect();
    }
  }

  // Redirección automática
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

    let seconds = 3;
    const interval = setInterval(function() {
      seconds--;

      if (countdownEl) countdownEl.textContent = seconds;
      if (progressBar) {
        progressBar.style.width = ((3 - seconds) / 3 * 100) + "%";
      }

      if (seconds <= 0) {
        clearInterval(interval);
        window.location.href = TARGET_URL;
      }
    }, 1000);
  }
</script>
