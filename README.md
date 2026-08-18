# 🐷 Chanchito

**App de finanzas personales local-first — sin backend, sin base de datos externa, sin que tus datos financieros salgan nunca de tu navegador.**

![PWA](https://img.shields.io/badge/PWA-Installable-5A0FC8) ![Vanilla JS](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E) ![No Build Step](https://img.shields.io/badge/Build%20Step-None-success) ![LocalStorage](https://img.shields.io/badge/Storage-100%25%20Local-blue) ![OCR+AI](https://img.shields.io/badge/Import-OCR%20%2B%20Claude%20Haiku-CC785C)

> Este repositorio es una vitrina del proyecto. El código fuente es privado — es una app de uso personal que maneja mis finanzas reales, así que el repo de desarrollo no es público. Aquí documento las decisiones técnicas y lo que hace.

## 📸 Capturas

_🚧 La UI todavía está en desarrollo activo — capturas próximamente. (Cuando estén listas: `docs/screenshots/01-dashboard.png`, `02-plan-deudas.png`, `03-importar-revision.png`, con montos/saldos difuminados antes de publicar.)_

## 💡 El problema

Los trackers de gastos genéricos (Mint, YNAB, hojas de cálculo) no cubren bien el caso de tener cuentas en distintas monedas y países, deudas con tasas de interés que hay que simular para decidir cómo pagarlas, y colecciones (cartas coleccionables) que también son parte del patrimonio real. Construí una app a medida para mi propio caso de uso, con control total sobre dónde viven mis datos.

## ✅ La solución

Una PWA instalable de un solo usuario que va bastante más allá de un tracker de gastos: también es un tracker de deudas con calculadora de pago y simulador de estrategia "avalancha", un planificador con calendario financiero y simulador predictivo "¿qué pasa si...?", y un dashboard de patrimonio neto consolidado en USD que incluye colecciones como activo no financiero.

## 🏗️ Decisiones de arquitectura

- **Local-first de verdad, no solo de palabra.** Sin framework, sin bundler, sin paso de build: HTML/CSS/JS planos cargados directamente. Todo el estado vive en `localStorage`. Esto no es una limitación técnica — es una decisión deliberada: mis datos financieros no tocan ningún servidor.
- **Una sola excepción a la regla, justificada:** el importador de fotos/PDFs escaneados necesita OCR e IA para leer capturas de apps bancarias, lo cual requiere credenciales que no pueden vivir en el cliente. Esa única funcionalidad pasa por un pequeño proxy serverless (Cloudflare Worker) — la única pieza de "backend" en todo el proyecto, y solo toca las imágenes de forma efímera, nunca las persiste.
- **IA como último recurso, no como default.** El pipeline de importación de documentos prueba primero un parser local (PDFs con texto nativo), luego OCR dedicado (Google Vision) para escaneados, y solo si eso falla recurre a un modelo de lenguaje (Claude Haiku) — y nunca se llama automáticamente, siempre con confirmación explícita. Prioriza costo, velocidad y privacidad sobre usar IA por defecto.
- **Nunca confiar ciegamente en una extracción automática.** Todos los métodos de importación (spreadsheet, IA, Gmail) pasan por una pantalla de revisión editable con detección de duplicados antes de guardar nada. Ninguna automatización escribe directo a la base de datos.
- **PDFs procesados 100% en el navegador**, incluyendo PDFs con contraseña — la contraseña nunca sale del cliente.
- **Reconstrucción histórica desde snapshots**: el sistema reconstruye el historial completo caminando hacia atrás desde el último snapshot manual a través de las transacciones ya importadas, y proyecta el saldo hacia adelante cuando hay transacciones más recientes que el último snapshot confirmado.
- **Bloqueo local con PIN**, con hash PBKDF2-SHA256 y salt aleatorio (nunca se guarda el PIN en texto plano) — pensado como barrera de privacidad ante acceso casual al dispositivo, no como cifrado del almacenamiento.
- **Motor de gráficos propio**, sin librería externa — línea, barra y donut hechos a mano, con eje de tiempo real para que el hover/tooltip sea preciso.
- **Cobertura de tests** sobre las piezas de mayor riesgo: calendario financiero y proyecciones, conciliación bancaria, motor de metas de ahorro, y pipeline de importación completo.

## ✨ Funcionalidades destacadas

- **Dashboard**: KPIs de flujo neto y presupuesto disponible, próximos compromisos a 14 días, gráfico de gasto de los últimos 6 meses, donut interactivo por categoría (clickeable, filtra transacciones), y proyección de saldo a 45 días con simulador predictivo en vivo.
- **Transacciones**: búsqueda instantánea, filtros por cuenta/categoría/moneda, inbox de revisión rápida con badge en tiempo real para movimientos importados pendientes de confirmar, conciliación y vinculación de conversiones de moneda.
- **Acceso rápido global**: botón universal para capturar gastos, ingresos o transferencias desde cualquier pantalla.
- **Patrimonio**: cuentas con saldos proyectados y conciliación bancaria, deudas con TEA/APR y líneas compartidas, y patrimonio neto consolidado (cuentas + colecciones − deudas) con histórico.
- **Plan**: calendario financiero con compromisos recurrentes y metas, simulador "¿qué pasa si...?" con diagnóstico de sobregiro, calculadora de metas de ahorro con ritmo de aporte sugerido, y simulador de estrategia "avalancha" para priorizar el pago de deudas por tasa de interés — verificado a mano contra la fórmula cerrada de amortización.
- **Importación**: archivos del banco (CSV/Excel), foto o PDF con OCR/IA, y sincronización directa desde el correo (Gmail, autenticado 100% desde el navegador sin backend), con reglas de categorización editables desde la UI. Cobertura de bancos en expansión.
- **Seguridad**: bloqueo local con PIN, bloqueo automático por inactividad, y backup cifrado a Google Drive.

## 🛠️ Stack técnico

**Cliente:** JavaScript vanilla (sin framework), HTML/CSS planos, `localStorage` como única base de datos, `pdf.js` para render de PDF en cliente.
**IA/OCR:** Google Vision OCR para documentos escaneados, Claude Haiku (API de Anthropic) con tool-use forzado como último recurso para extracción estructurada.
**Backend (mínimo, solo para el importador por IA):** Cloudflare Worker como proxy que protege las credenciales.
**Auth de importación por correo:** Google Identity Services (OAuth) directo desde el navegador, sin backend.

## 👨‍💻 Mi rol

Diseño y desarrollo end-to-end en solitario: arquitectura local-first, los pipelines de importación (spreadsheet, OCR/IA multimodal, Gmail), motores de cálculo financiero (amortización, proyección de saldos, conciliación, metas), motor de gráficos propio, y la suite de tests sobre la lógica financiera.

## 🛣️ Roadmap

Ampliar cobertura de bancos y tarjetas soportadas en la sincronización automática por correo, y explorar extracción de texto real de PDF (en vez de renderizado a imagen) cuando el documento ya trae una capa de texto nativa.

---

**¿Quieres ver el código o el detalle técnico completo?** Puedo dar acceso puntual al repositorio privado para procesos de entrevista.
