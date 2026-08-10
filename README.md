# 💰 Personal Finance PWA

**App de finanzas personales local-first — sin backend, sin base de datos externa, sin que tus datos financieros salgan nunca de tu navegador.**

![PWA](https://img.shields.io/badge/PWA-Installable-5A0FC8) ![Vanilla JS](https://img.shields.io/badge/JavaScript-Vanilla-F7DF1E) ![No Build Step](https://img.shields.io/badge/Build%20Step-None-success) ![LocalStorage](https://img.shields.io/badge/Storage-100%25%20Local-blue) ![Claude](https://img.shields.io/badge/AI-Claude%20Vision-CC785C)

> Este repositorio es una vitrina del proyecto. El código fuente es privado — es una app de uso personal que maneja mis finanzas reales, así que el repo de desarrollo no es público. Aquí documento las decisiones técnicas y lo que hace.

## 📸 Capturas

_🚧 La UI todavía está en desarrollo activo — capturas próximamente. (Cuando estén listas: `docs/screenshots/01-dashboard.png`, `02-plan-deudas.png`, `03-importar-revision.png`, con montos/saldos difuminados antes de publicar.)_

## 💡 El problema

Los trackers de gastos genéricos (Mint, YNAB, hojas de cálculo) no cubren bien el caso de tener cuentas en dos monedas y países distintos, deudas con tasas de interés que hay que simular para decidir cómo pagarlas, y colecciones (cartas coleccionables) que también son parte del patrimonio real. Construí una app a medida para mi propio caso de uso, con control total sobre dónde viven mis datos.

## ✅ La solución

Una PWA instalable que cubre tracking de gastos, gestión de deudas con calculadora de pago y simulador de estrategia de pago (avalancha), y un dashboard de patrimonio neto que incluye tanto cuentas bancarias como colecciones como activos — todo en soles y dólares, con conversión automática solo donde tiene sentido (totales agregados, nunca en el registro individual).

## 🏗️ Decisiones de arquitectura

- **Local-first de verdad, no solo de palabra.** Sin framework, sin bundler, sin paso de build: HTML/CSS/JS planos cargados directamente. Todo el estado vive en una sola clave de `localStorage`. Esto no es una limitación técnica — es una decisión deliberada: mis datos financieros no tocan ningún servidor.
- **Una sola excepción a la regla, justificada:** el importador de movimientos por foto/PDF usa IA (Claude) para leer capturas de apps bancarias, lo cual requiere una API key. Como esa key no puede vivir en el cliente (sería visible en devtools), esa única funcionalidad pasa por un pequeño proxy serverless (Cloudflare Worker) que la mantiene del lado del servidor. Es la única pieza de "backend" en todo el proyecto, y solo toca las imágenes de forma efímera — nunca persiste nada.
- **Nunca confiar ciegamente en una extracción automática.** Tanto el importador de hojas de cálculo como el de IA muestran cada movimiento en una pantalla de revisión editable antes de guardar nada — incluyendo aviso de posibles duplicados. Ninguna automatización escribe directo a la base de datos.
- **PDFs procesados 100% en el navegador**, incluyendo PDFs con contraseña — la contraseña nunca sale del cliente. Cada página se renderiza a imagen localmente antes de mandarse (ya sin la contraseña) al proxy de IA.
- **Reconstrucción histórica desde snapshots**: en vez de depender de que actualices manualmente cada saldo todos los meses, el sistema reconstruye el historial completo caminando hacia atrás desde el último snapshot manual a través de las transacciones ya importadas, y proyecta el saldo actual hacia adelante cuando hay transacciones más recientes que el último snapshot confirmado.
- **Motor de gráficos propio**, sin librería externa — línea, barra y donut hechos a mano, con eje de tiempo real (no por índice) para que el hover/tooltip sea preciso.

## ✨ Funcionalidades destacadas

- **Dashboard de resumen** con KPIs del mes, navegación mes a mes, y un donut de gastos por categoría completamente clickeable (perfora directo a la lista de transacciones ya filtrada).
- **Gestión de deudas**: saldo, TEA, pago mínimo y próxima fecha por cada deuda, con soporte para líneas de crédito compartidas entre monedas.
- **Calculadora de plan de pago**: dado un pago mensual, calcula meses hasta liquidar, interés total, y advierte si el pago propuesto ni siquiera cubre el interés (nunca se liquidaría).
- **Simulador de estrategia "avalancha"**: dado un presupuesto mensual total, ordena el pago óptimo entre todas las deudas priorizando la de mayor tasa. Los cálculos se verificaron a mano contra la fórmula cerrada de amortización de préstamos.
- **Patrimonio neto** que combina cuentas, deudas y colecciones (activos no financieros) en un solo número, en el tiempo.
- **Tres métodos de importación**: archivos exportados por el banco (CSV/Excel), foto o PDF vía IA con detección automática de a qué cuenta corresponde, y sincronización directa desde el correo (Gmail API, autenticado 100% desde el navegador sin backend — un Client ID de OAuth no es secreto, así que ahí sí tiene sentido no usar proxy).
- **Motor de reglas de categorización editable** desde la UI (antes vivía hardcodeado en el código).

## 🛠️ Stack técnico

**Cliente:** JavaScript vanilla (sin framework), HTML/CSS planos, `localStorage` como única base de datos, `pdf.js` para render de PDF en cliente.
**IA:** Claude (API de Anthropic) con tool-use forzado para extracción estructurada desde imágenes.
**Backend (mínimo, solo para la IA):** Cloudflare Worker como proxy que protege la API key.
**Auth de importación por correo:** Google Identity Services (OAuth) directo desde el navegador, sin backend.

## 👨‍💻 Mi rol

Diseño y desarrollo end-to-end en solitario: arquitectura local-first, los tres pipelines de importación (spreadsheet, IA multimodal, Gmail), motor de cálculo financiero (amortización, proyección de saldos), y motor de gráficos propio.

## 🛣️ Roadmap

Extender la proyección de saldo hacia adelante al dashboard de patrimonio neto, ampliar cobertura de bancos soportados en los importadores automáticos, y explorar extracción de texto real de PDF (en vez de renderizado a imagen) cuando el documento ya trae una capa de texto nativa.

---

**¿Quieres ver el código o el detalle técnico completo?** Puedo dar acceso puntual al repositorio privado para procesos de entrevista.
