# ConectaNFC
!DOCTYPE html>
<html lang="es">
<head>
  <meta charset="UTF-8">
  <meta name="viewport" content="width=device-width, initial-scale=1.0">
  <meta name="description" content="ConectaNFC: herramienta de comunicación escrita accesible.">
  <title>ConectaNFC | Comunicación inclusiva</title>
  <style>
    :root {
      --azul: #164e8a;
      --azul-oscuro: #10365d;
      --fondo: #f3f7fb;
      --texto: #172536;
      --borde: #cbd7e4;
      --blanco: #ffffff;
      --verde: #176b45;
    }

    * { box-sizing: border-box; }

    body {
      margin: 0;
      font-family: Arial, Helvetica, sans-serif;
      background: var(--fondo);
      color: var(--texto);
      line-height: 1.5;
    }

    header {
      background: var(--azul-oscuro);
      color: white;
      padding: 22px 16px;
      text-align: center;
    }

    header h1 {
      margin: 0;
      font-size: clamp(1.7rem, 5vw, 2.3rem);
    }

    header p {
      margin: 6px 0 0;
      font-size: 1rem;
    }

    main {
      width: min(760px, 100%);
      margin: 0 auto;
      padding: 20px 14px 40px;
    }

    .panel {
      background: var(--blanco);
      border: 1px solid var(--borde);
      border-radius: 16px;
      padding: 20px;
      margin-bottom: 18px;
      box-shadow: 0 3px 12px rgba(16, 54, 93, 0.06);
    }

    h2 {
      margin-top: 0;
      font-size: 1.35rem;
    }

    .ayuda {
      color: #3d4d5e;
      margin-top: -6px;
    }

    .categorias, .frases {
      display: grid;
      grid-template-columns: repeat(2, minmax(0, 1fr));
      gap: 10px;
    }

    button, .boton {
      min-height: 54px;
      border: 2px solid var(--azul);
      border-radius: 12px;
      padding: 12px 14px;
      font: inherit;
      font-weight: 700;
      cursor: pointer;
      background: white;
      color: var(--azul-oscuro);
      text-align: center;
    }

    button:hover { background: #eaf3fc; }

    button:focus-visible, textarea:focus-visible, input:focus-visible {
      outline: 3px solid #f0a51b;
      outline-offset: 3px;
    }

    button.primario {
      background: var(--azul);
      color: white;
    }

    button.primario:hover { background: var(--azul-oscuro); }

    button.secundario {
      border-color: var(--borde);
      color: var(--texto);
    }

    textarea {
      display: block;
      width: 100%;
      min-height: 120px;
      resize: vertical;
      border: 2px solid var(--borde);
      border-radius: 12px;
      padding: 13px;
      font: inherit;
      font-size: 1.05rem;
      margin: 10px 0 12px;
    }

    .acciones {
      display: flex;
      flex-wrap: wrap;
      gap: 10px;
    }

    .acciones button { flex: 1 1 180px; }

    .oculto { display: none !important; }

    .mensaje-grande {
      min-height: 180px;
      display: flex;
      align-items: center;
      justify-content: center;
      text-align: center;
      padding: 22px;
      border: 2px dashed var(--borde);
      border-radius: 14px;
      background: #f8fbff;
      font-size: clamp(1.8rem, 6vw, 3rem);
      font-weight: 700;
      overflow-wrap: anywhere;
      white-space: pre-wrap;
    }

    .nota {
      font-size: 0.9rem;
      color: #46576a;
    }

    .estado {
      min-height: 1.5em;
      margin: 8px 0 0;
      color: var(--verde);
      font-weight: 700;
    }

    footer {
      text-align: center;
      padding: 0 16px 24px;
      color: #46576a;
      font-size: 0.9rem;
    }

    @media (max-width: 420px) {
      .categorias, .frases { grid-template-columns: 1fr; }
      .panel { padding: 16px; }
    }
  </style>
</head>
<body>
  <header>
    <h1>ConectaNFC</h1>
    <p>Una herramienta para comunicarnos por escrito</p>
  </header>

  <main>
    <section id="inicio" class="panel">
      <h2>¿Qué necesitas comunicar?</h2>
      <p class="ayuda">Elige una categoría para ver frases rápidas.</p>
      <div class="categorias">
        <button type="button" data-categoria="escuela">🏫 Escuela</button>
        <button type="button" data-categoria="ayuda">🆘 Ayuda</button>
        <button type="button" data-categoria="preguntas">❓ Preguntas</button>
        <button type="button" data-categoria="cotidiano">💬 Comunicación cotidiana</button>
      </div>
    </section>

    <section id="seccion-frases" class="panel oculto">
      <h2 id="titulo-categoria">Frases</h2>
      <p class="ayuda">Toca una frase para mostrarla en grande.</p>
      <div id="lista-frases" class="frases"></div>
      <div class="acciones" style="margin-top: 14px;">
        <button type="button" class="secundario" id="volver-inicio">Volver a categorías</button>
      </div>
    </section>

    <section id="personalizado" class="panel">
      <h2>Escribir un mensaje propio</h2>
      <label for="texto-personalizado">Escribe el mensaje que quieres mostrar:</label>
      <textarea id="texto-personalizado" maxlength="300"
        placeholder="Por ejemplo: ¿Podría ayudarme, por favor?"></textarea>
      <div class="acciones">
        <button type="button" class="primario" id="mostrar-personalizado">Mostrar mensaje</button>
      </div>
      <p class="nota">Máximo 300 caracteres. El mensaje solo se muestra en este dispositivo; esta versión no lo envía a otra persona.</p>
    </section>

    <section id="pantalla-mensaje" class="panel oculto" aria-live="polite">
      <h2>Mensaje para mostrar</h2>
      <div id="mensaje" class="mensaje-grande"></div>
      <div class="acciones" style="margin-top: 14px;">
        <button type="button" class="primario" id="copiar-mensaje">Copiar texto</button>
        <button type="button" class="secundario" id="volver-mensaje">Elegir otro mensaje</button>
      </div>
      <p id="estado-copia" class="estado" role="status"></p>
    </section>
  </main>

  <footer>
    Prototipo escolar · ConectaNFC · Comunicación escrita accesible
  </footer>

  <script>
    // 1. Frases organizadas por categoría.
    const frases = {
      escuela: [
        "¿Puede escribir las instrucciones, por favor?",
        "Tengo una pregunta.",
        "¿Puede repetirlo por escrito?",
        "Necesito hablar con mi profesor."
      ],
      ayuda: [
        "Necesito ayuda, por favor.",
        "¿Puede acompañarme a recepción?",
        "No me siento bien.",
        "Necesito atención médica."
      ],
      preguntas: [
        "¿Dónde está el baño?",
        "¿Dónde está la dirección?",
        "¿Puede indicarme cómo llegar?",
        "¿Puede escribir la respuesta?"
      ],
      cotidiano: [
        "Hola, mucho gusto.",
        "Gracias por ayudarme.",
        "Por favor, comuníquese conmigo por escrito.",
        "Un momento, voy a escribir mi respuesta."
      ]
    };

    const nombresCategorias = {
      escuela: "Escuela",
      ayuda: "Ayuda",
      preguntas: "Preguntas",
      cotidiano: "Comunicación cotidiana"
    };

    const inicio = document.getElementById("inicio");
    const seccionFrases = document.getElementById("seccion-frases");
    const tituloCategoria = document.getElementById("titulo-categoria");
    const listaFrases = document.getElementById("lista-frases");
    const personalizado = document.getElementById("personalizado");
    const pantallaMensaje = document.getElementById("pantalla-mensaje");
    const mensaje = document.getElementById("mensaje");
    const textoPersonalizado = document.getElementById("texto-personalizado");
    const estadoCopia = document.getElementById("estado-copia");

    // 2. Mostrar una frase en la pantalla grande.
    function mostrarMensaje(texto) {
      mensaje.textContent = texto;
      estadoCopia.textContent = "";
      pantallaMensaje.classList.remove("oculto");
      seccionFrases.classList.add("oculto");
      personalizado.classList.add("oculto");
      inicio.classList.add("oculto");
      pantallaMensaje.scrollIntoView({ behavior: "smooth", block: "start" });
    }

    // 3. Al tocar una categoría, construir sus botones de frases.
    document.querySelectorAll("[data-categoria]").forEach((boton) => {
      boton.addEventListener("click", () => {
        const categoria = boton.dataset.categoria;
        tituloCategoria.textContent = nombresCategorias[categoria];
        listaFrases.replaceChildren();

        frases[categoria].forEach((frase) => {
          const botonFrase = document.createElement("button");
          botonFrase.type = "button";
          botonFrase.textContent = frase;
          botonFrase.addEventListener("click", () => mostrarMensaje(frase));
          listaFrases.appendChild(botonFrase);
        });

        inicio.classList.add("oculto");
        personalizado.classList.add("oculto");
        pantallaMensaje.classList.add("oculto");
        seccionFrases.classList.remove("oculto");
        seccionFrases.scrollIntoView({ behavior: "smooth", block: "start" });
      });
    });

    // 4. Mostrar el mensaje que la persona escribió.
    document.getElementById("mostrar-personalizado").addEventListener("click", () => {
      const texto = textoPersonalizado.value.trim();
      if (!texto) {
        alert("Primero escribe un mensaje.");
        textoPersonalizado.focus();
        return;
      }
      mostrarMensaje(texto);
    });

    // 5. Volver a la pantalla principal.
    document.getElementById("volver-inicio").addEventListener("click", () => {
      seccionFrases.classList.add("oculto");
      inicio.classList.remove("oculto");
      personalizado.classList.remove("oculto");
      inicio.scrollIntoView({ behavior: "smooth", block: "start" });
    });

    document.getElementById("volver-mensaje").addEventListener("click", () => {
      pantallaMensaje.classList.add("oculto");
      inicio.classList.remove("oculto");
      personalizado.classList.remove("oculto");
      inicio.scrollIntoView({ behavior: "smooth", block: "start" });
    });

    // 6. Copiar el mensaje, si el navegador lo permite.
    document.getElementById("copiar-mensaje").addEventListener("click", async () => {
      try {
        await navigator.clipboard.writeText(mensaje.textContent);
        estadoCopia.textContent = "Mensaje copiado.";
      } catch (error) {
        estadoCopia.textContent = "No se pudo copiar automáticamente. Puedes mostrar el texto en pantalla.";
      }
    });

    // 7. Si la URL contiene ?lugar=biblioteca, abrir una categoría concreta.
    // Se puede ampliar cuando se configuren los enlaces de las etiquetas NFC.
    const parametros = new URLSearchParams(window.location.search);
    const lugar = parametros.get("lugar");
    const categoriasPorLugar = {
      biblioteca: "preguntas",
      enfermeria: "ayuda",
      salon: "escuela",
      recepcion: "cotidiano"
    };

    if (lugar && categoriasPorLugar[lugar]) {
      const botonCategoria = document.querySelector(
        `[data-categoria="${categoriasPorLugar[lugar]}"]`
      );
      if (botonCategoria) botonCategoria.click();
    }
  </script>
</body>
</html>
