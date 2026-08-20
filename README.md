<p align="center">
  <img src="logo.png" alt="Chanchito Logo" width="160">
</p>

<h1 align="center">Chanchito</h1>

<p align="center">
  <strong>App de finanzas personales local-first — sin backend, sin base de datos externa, con privacidad total y respaldos cifrados.</strong>
</p>

<p align="center">
  <img src="https://img.shields.io/badge/PWA-Installable-5A0FC8?style=flat-square" alt="PWA">
  <img src="https://img.shields.io/badge/JavaScript-Vanilla%20(No%20Framework)-F7DF1E?style=flat-square&logo=javascript&logoColor=black" alt="Vanilla JS">
  <img src="https://img.shields.io/badge/Build%20Step-None%20(100%25%20Static)-success?style=flat-square" alt="No Build Step">
  <img src="https://img.shields.io/badge/Storage-100%25%20Local%20(localStorage)-blue?style=flat-square" alt="LocalStorage">
  <img src="https://img.shields.io/badge/Import-OCR%20%2B%20Claude%20Haiku%20%2B%20Gmail-CC785C?style=flat-square" alt="Import Methods">
  <img src="https://img.shields.io/badge/Security-PBKDF2%20PIN%20%2B%20AES--GCM-darkgreen?style=flat-square" alt="Security">
</p>

---

> **Nota sobre este repositorio**: Este repositorio es una vitrina técnica (*showcase*) del proyecto. El código fuente de desarrollo es privado al tratarse de una aplicación de uso personal que gestiona mis finanzas reales. Aquí se documentan las decisiones de arquitectura, diseño de sistemas y características técnicas implementadas.

---

## 📸 Capturas de Pantalla

_Todas las cifras de estas capturas son datos sintéticos generados para demostración — no representan información financiera real._

<p align="center">
  <img src="docs/screenshots/01-dashboard.png" alt="Dashboard Principal" width="800">
  <br>
  <em><strong>1. Dashboard Principal</strong>: KPIs de flujo neto y disponible por moneda, widget de próximos compromisos a 14 días, gráfico de gastos históricos, donut interactivo por categoría y proyección de saldo a 45 días.</em>
</p>

<p align="center">
  <img src="docs/screenshots/02-plan-deudas.png" alt="Plan de Deudas y Estrategia Avalancha" width="800">
  <br>
  <em><strong>2. Plan de Deudas & Avalancha</strong>: Calculadora de amortización por tarjeta y simulador interactivo de cascada por TEA (Avalancha) con análisis de impacto de ahorro en presupuestos.</em>
</p>

<p align="center">
  <img src="docs/screenshots/03-importar-revision.png" alt="Inbox de Revisión Rápida de Transacciones" width="800">
  <br>
  <em><strong>3. Revisión Editable de Importación (OCR / IA / Gmail / CSV)</strong>: Triage de movimientos importados con detección visual de cuenta, advertencias de duplicados y validación aritmética de balance antes de confirmar.</em>
</p>

<p align="center">
  <img src="docs/screenshots/04-categoria-presupuesto.png" alt="Detalle de Categoría y Presupuesto" width="800">
  <br>
  <em><strong>4. Presupuestos con Conversión de Moneda en Vivo</strong>: Configuración de límites por categoría con rollover automático y conversión de tipo de cambio en tiempo real (PEN ↔ USD).</em>
</p>

<p align="center">
  <img src="docs/screenshots/05-calendario-agenda.png" alt="Calendario Financiero Unificado y Agenda" width="800">
  <br>
  <em><strong>5. Calendario Financiero Unificado & Agenda</strong>: Vista mensual y agenda a 45 días con conciliación automática de pagos recurrentes, cuotas y compromisos.</em>
</p>

<p align="center">
  <img src="docs/screenshots/06-simulador-predictivo.png" alt="Simulador Predictivo ¿Qué pasa si...?" width="800">
  <br>
  <em><strong>6. Simulador Predictivo "¿Qué pasa si...?"</strong>: Evaluación de impacto en flujo de caja ante gastos extraordinarios hipotéticos con diagnóstico de sobregiro y sugerencia de fecha segura.</em>
</p>

<p align="center">
  <img src="docs/screenshots/07-transacciones-seleccion-masiva.png" alt="Transacciones con Selección Múltiple y Clasificación ML" width="800">
  <br>
  <em><strong>7. Transacciones & Selección Múltiple</strong>: Búsqueda instantánea, filtros combinados, sugerencias por Machine Learning local (✨) y modo de edición/borrado masivo en lote.</em>
</p>

<p align="center">
  <img src="docs/screenshots/08-patrimonio-colecciones.png" alt="Patrimonio Neto y Colecciones TCG" width="800">
  <br>
  <em><strong>8. Patrimonio Neto & Portafolio de Coleccionables</strong>: Consolidado en USD de cuentas, pasivos y activos coleccionables (cartas TCG) con carrusel visual de ítems de alto valor.</em>
</p>

<p align="center">
  <img src="docs/screenshots/09-metas-ahorro.png" alt="Metas de Ahorro y Págate Primero" width="800">
  <br>
  <em><strong>9. Metas de Ahorro & "Págate Primero"</strong>: Reservas de ahorro y fondos de emergencia con avance porcentual y cálculo automático de ritmo sugerido mensual.</em>
</p>

<p align="center">
  <img src="docs/screenshots/10-captura-rapida.png" alt="Acceso Rápido Global" width="800">
  <br>
  <em><strong>10. Acceso Rápido Global (+)</strong>: Modal optimizado para captura instantánea de gastos, ingresos y transferencias en un solo toque.</em>
</p>

---

## 💡 El Problema

Los gestores de finanzas comerciales convencionales (Mint, YNAB, hojas de cálculo) presentan limitaciones estructurales para usuarios con necesidades particulares:
1. **Multi-moneda real y multi-país**: Manejo simultáneo de cuentas en Soles (PEN) y Dólares (USD) con tipos de cambio dinámicos y transacciones cruzadas.
2. **Deudas complejas y cálculo de intereses**: Simulación de amortización con TEA/APR real, pagos mínimos y optimización de pago en cascada.
3. **Patrimonio integral (activos no bancarios)**: Inclusión de colecciones de alto valor (cartas coleccionables TCG, inventarios) dentro del cálculo de patrimonio neto.
4. **Privacidad y soberanía de datos**: Garantía absoluta de que la información bancaria y financiera personal jamás resida en servidores de terceros.

---

## ✅ La Solución

**Chanchito** es una Progressive Web App (PWA) construida a medida con arquitectura *local-first*, que integra:
- **Control de Flujo de Caja y Presupuestos**: KPIs en tiempo real, desglose de gastos y asignación mensual con rollover.
- **Planificador Financiero Predictivo**: Calendario unificado de compromisos, conciliación automática contra movimientos reales y simulador interactivo *"¿Qué pasa si...?"* con diagnóstico de sobregiro.
- **Gestor de Deudas con Estrategia Avalancha**: Calculadora de amortización matemática verificada y simulador de cascada por costo financiero.
- **Patrimonio Neto Consolidado**: Cuentas bancarias + Activos/Coleccionables − Deudas en USD con historial evolutivo.
- **Pipeline de Importación Multimodal**: Extractos CSV/Excel bancarios, OCR/IA multimodal para capturas/PDFs y sincronización automática vía Gmail API.

---

## 🏗️ Decisiones de Arquitectura

- **Local-first sin concesiones**: Cero frameworks, cero bundlers, cero build step. HTML/CSS/JS plano cargado directamente por el navegador. Todo el estado reside en `localStorage` (`finapp:v1`). Los datos financieros nunca abandonan el dispositivo.
- **Sistema de Parsers Declarativos (*Declarative-first*)**: Los extractos de bancos se procesan mediante esquemas JSON declarativos (`declarative-parser-schema.js`) ejecutados por un motor agnóstico (`declarative-parser-engine.js`), permitiendo añadir soporte a nuevos formatos o generar parsers asistidos por IA sin tocar código fuente.
- **Machine Learning Local en el Navegador**: Clasificador Naive Bayes local con tokenización bancaria, n-gramas y guardrails de seguridad (`category-classifier.js`) para sugerir categorías con alta precisión respetando siempre la precedencia de reglas declarativas del usuario.
- **IA como último recurso, no como default**: El pipeline de documentos prioriza parsers locales (PDFs nativos), luego OCR dedicado (Google Vision), y únicamente ante capturas complejas recurre a LLM (Claude Haiku) con confirmación explícita del usuario.
- **Revisión obligatoria antes de persistir**: Ningún método de importación escribe directamente en la base de datos. Todos los movimientos pasan por un *Inbox de Revisión Rápida* con detección inteligente de duplicados.
- **Seguridad y Cifrado**:
  - Bloqueo de app con PIN local de 6 dígitos mediante hash **PBKDF2-SHA256** con salt aleatorio de 100,000 iteraciones (`app-lock.js`).
  - Exportación de backups cifrados con **AES-GCM (256 bits)** protegidos por contraseña, con integración directa a **Google Drive**.
- **Motor de Gráficos Nativo**: Visualizaciones de línea, barra, donut y stacked trends renderizadas directamente en SVG/Canvas sin librerías externas pesadas.
- **Deep Linking y Atajos de iOS**: Soporte para parámetros URL en la PWA que habilitan captura instantánea de gastos en 1 segundo vía el Botón de Acción del iPhone o Siri Shortcuts.

---

## ✨ Módulos y Funcionalidades

### 1. Resumen (`Dashboard`)
- KPIs de flujo neto y presupuesto disponible por moneda (PEN / USD / Combinado).
- Widget de próximos compromisos a 14 días con resolución inteligente contra transacciones bancarias.
- Gráfico de gasto histórico de 6/12 meses y donut interactivo con filtrado contextual.
- Proyección de saldo a 45 días con alertas de umbral mínimo de seguridad.

### 2. Transacciones (`Transactions`)
- Búsqueda instantánea multi-criterio y filtros por cuenta, categoría y moneda.
- Modo de **Selección Múltiple y Edición Masiva** para recategorización y gestión por lotes.
- Inbox de triage con badge en tiempo real (`🔔 N por revisar`).
- Conciliación contable por cuenta/moneda y vinculación de movimientos de cambio de divisas (FX).

### 3. Patrimonio (`Accounts`)
- **Cuentas**: Saldos calculados, reconciliación con extractos bancarios y gráficos de balance histórico.
- **Deudas**: Monitoreo de líneas de crédito, cálculo de intereses reales (TEA/APR) y fechas de corte/pago.
- **Patrimonio Neto**: Consolidación total de activos líquidos, inventarios coleccionables y pasivos.

### 4. Plan (`Plan`)
- **Calendario Financiero**: Vista Grid mensual y Agenda de compromisos recurrentes, cuotas de préstamos y metas de ahorro.
- **Simulador "¿Qué pasa si...?"**: Evaluación de impacto de gastos extraordinarios con detección preventiva de sobregiro y sugerencia de fecha segura.
- **Metas de Ahorro ("Págate Primero")**: Asignación prioritaria sobre el disponible mensual y cálculo de ritmo sugerido de ahorro.
- **Simulador Avalancha**: Comparativa matemática de ahorro en intereses priorizando deudas por costo financiero.

### 5. Configuración e Importación (`Import & Settings`)
- **Importadores**: CSV/Excel declarativo, capturas/PDFs vía OCR/IA y sincronización directa con Gmail vía OAuth2 cliente.
- **Generador de Parsers con IA**: Creación guiada de esquemas de extracción a partir de archivos de muestra.
- **Gestión de Datos**: Calidad de datos, optimización de cuota de almacenamiento local y respaldos cifrados en Google Drive.

---

## 🛠️ Stack Tecnológico

| Capa | Tecnologías |
| :--- | :--- |
| **Frontend Core** | HTML5, CSS3 moderno (Custom Properties, Backdrop Filter, Safe Areas), JavaScript ES2022 Vanilla (sin frameworks ni bundlers) |
| **Persistencia Local** | `localStorage` API con verificación de cuota e integridad referencial |
| **Seguridad** | Web Crypto API (`PBKDF2`, `AES-GCM`, `SHA-256`), App Lock por inactividad y anti-destello |
| **Integraciones & Auth** | Google Identity Services (OAuth2 en cliente para Gmail y Drive), Cloudflare Workers (Proxy serverless efímero para IA) |
| **Procesamiento de Docs** | `pdf.js` (renderizado local), Google Vision OCR, Claude Haiku (Anthropic API con tool calling) |
| **Testing** | Node.js Test Suite nativa (11 suites automatizadas cubriendo cálculo financiero, parsers, ML y seguridad) |

---

## 👨‍💻 Rol y Desarrollo

Diseño y desarrollo end-to-end por **Álvaro Oliveros**:
- Definición de arquitectura *local-first* y diseño de experiencia de usuario (UI/UX) móvil y desktop.
- Implementación de los motores de cálculo financiero (amortización de deuda, proyección de flujo de caja, reconciliación contable y metas de ahorro).
- Construcción del motor de parsers declarativos y clasificador de machine learning local.
- Desarrollo de la suite completa de pruebas unitarias y de integración.

---

<p align="center">
  <strong>¿Te interesa conocer más sobre la arquitectura o detalles técnicos?</strong><br>
  Puedo facilitar acceso puntual al repositorio de desarrollo para procesos de entrevista técnica.
</p>
