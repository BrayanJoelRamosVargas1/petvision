# PetVision AI

Proyecto académico independiente de StockVision. Web móvil con detección real de perros y gatos, cámara, captura, carga de fotos, seguimiento temporal y análisis histórico local.

Universidad Autónoma Gabriel René Moreno (UAGRM). Facultad de Ingeniería en Ciencias de la Computación y Telecomunicaciones. Ingeniería de Sistemas. Sistemas para el Soporte a la Toma de Decisiones (INF432). Docente: Ing. Jesús Bautista. Autor: Brayan Joel Ramos Vargas.

## Ejecutar

Node 22+ para tests: `npm test`. Servir con `python -m http.server 8080` y abrir localhost:8080. En teléfono usar HTTPS. GitHub Actions publica el sitio estático sin servidor de inferencia.

## Arquitectura

- `index.html`, `styles.css`: producto mobile-first sin framework.
- `js/app.js`: cámara, captura, cargas, canvas, sesiones y exportación.
- `js/detector.js`: preprocesado RGB y cliente del Worker con timeout y recuperación.
- `js/worker.js`: ONNX Runtime Web / WASM monohilo, caché del modelo propia de PetVision.
- `js/core.js`: decodificación, NMS, tracking IoU, persistencia y estadísticas comprobables.
- `models/pets.onnx`: YOLOv10 Small cuantizado, mismo binario del proyecto original.
- `vendor/`: runtime local 1.20.1. No depende de un CDN para inferir.

WASM se eligió como ruta estable compatible con Pages sin cabeceras COOP/COEP. No se habilitó WebGPU sin validación. Una inferencia a la vez y pausa de 180 ms. Un score mínimo 0.35 filtra resultados. Umbral de revisión explicable: confianza <60% sugiere repetir la captura. El score no es precisión medida del sistema.

## Privacidad y decisiones

Las fotografías no salen del navegador ni se guardan en el historial. Solo metadatos de las últimas 200 sesiones: hora ISO, clase, score e ID temporal. Las fotos permanecen en memoria para la vista previa y se liberan al reiniciar/cambiar. Cache API almacena únicamente el modelo. Clave local propia `petvision_sessions_v1`. Cámara se detiene al ocultar la página. No hay identificación individual, razas ni diagnósticos.

Una sesión de foto registra su resultado una vez, incluso sin detecciones. En vivo se guarda explícitamente una instantánea de los animales presentes. El seguimiento evita duplicación en el mismo fotograma; los IDs no demuestran identidad entre sesiones. La analítica resume detecciones registradas, no animales únicos. Los datos no se sincronizan entre dispositivos. Exportación CSV y JSON y borrado del historial disponibles.

## Probar desde Android

1. Abrir el enlace HTTPS publicado en Chrome Android.
2. Pulsar Activar cámara y permitir el acceso, usar cámara trasera.
3. Encuadrar el perro completo, con buena luz. Ver recuadro, clase y score.
4. Tomar fotografía y pulsar Analizar imagen. Abrir Historial y Analítica.
5. Si la cámara no funciona, Subir imagen desde galería. También sirve para fotografías tomadas con la cámara nativa.

## Pruebas y límites

`npm test`: clases, filtros, IoU, múltiples tracks, caducidad, geometría contain, almacenamiento, estadísticas y CSV. `tests/browser.mjs` ejecuta inferencia real en Edge con fotos de perro, gatos y una escena negativa, además de carga móvil emulada, historial/exportación y permisos denegados. Requiere Playwright disponible en el runtime indicado en el archivo. Evidencias locales en .test-output, excluidas de Git.

La prueba en un teléfono Android físico sigue pendiente del propietario. Escritorio/emulación no verifican su hardware, enfoque o permisos reales. El modelo puede fallar por luz, oclusión o tamaño. Los cuadros pueden retrasarse durante movimiento. No se promete precisión universal ni detección en 60 FPS. No se entrenó con las tres imágenes de prueba. Licencias y fuentes en THIRD_PARTY.md.
