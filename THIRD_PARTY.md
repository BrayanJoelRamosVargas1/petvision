# Licencias y procedencia

PetVision AI: AGPL-3.0-only, texto en LICENSE. Es un proyecto independiente. Se reutiliza el binario YOLOv10 Small cuantizado que sirve StockVision, sin alterar ese repositorio. El nuevo adaptador conserva el contrato de entrada y salida, pero filtra exclusivamente COCO 15 (cat) y 16 (dog). No reutiliza heurísticas de botellas.

- Pesos: https://huggingface.co/onnx-community/yolov10s, onnx/model_quantized.onnx, 7.636.121 bytes. SHA-256: 0be5b02c522e295e9820105f813e41993ae16d29b79eea4cb7de7e97058d85c7. Modelo original: https://github.com/THU-MIG/yolov10. AGPL-3.0.
- Preprocesamiento verificado contra preprocessor_config.json de la ficha: RGB 0–1, NCHW 640, escala conservada, relleno cero inferior/derecho. Salida [1,N,6], xyxy, score, COCO class ID. No hay entrenamiento nuevo.
- ONNX Runtime Web 1.20.1: MIT, licencia en vendor/LICENSE. Archivos JS/MJS/WASM del runtime, mismo origen, WASM monohilo en Worker dedicado. https://github.com/microsoft/onnxruntime/tree/v1.20.1
- Fotografía de perro para pruebas: https://raw.githubusercontent.com/pytorch/hub/master/images/dog.jpg (repositorio PyTorch Hub). Foto gatos: https://huggingface.co/datasets/huggingface/documentation-images/resolve/main/coco_sample.png (muestra COCO). Negativo: https://raw.githubusercontent.com/ultralytics/ultralytics/main/ultralytics/assets/bus.jpg (Ultralytics, AGPL-3.0). Son fixtures de evaluación, no imágenes del usuario ni resultados ilustrativos de la interfaz. Deben conservarse atribuciones del conjunto original.

Documentación de ejecución: https://onnxruntime.ai/docs/tutorials/web/deploy.html
