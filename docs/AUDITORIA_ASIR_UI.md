# Auditoría técnica, pedagógica y visual — Dominios-WEB

**Fecha:** 2026-10-20  
**Repositorio:** `WolcenOn/Dominios-WEB`  
**Rama de pruebas:** `design/asir-visual-system-v1`  
**Producción:** `main` sin modificar.  
**Alcance:** revisión estática del HTML/CSS/JS y prototipo visual v1; faltan pruebas manuales en navegador.

## 1. Diagnóstico general

La aplicación ya es un recurso didáctico valioso para 2.º de ASIR. Conviene **evolucionarla, no reescribirla**. Incluye contenidos técnicos, laboratorios, selectores de Linux/Windows y Apache/Nginx, pruebas de verificación y repaso.

- Un `index.html` de unos **305 KB**.
- CSS embebido de unos **25,9 KB**.
- JavaScript embebido de unos **276 KB** (incluyendo los datos didácticos).
- **8 módulos**, **28 objetivos**, **44 pasos de procedimiento**, **33 fragmentos de configuración**, **36 pruebas**, **21 preguntas de repaso** y **29 casos de error/detección**.
- En la navegación hay búsqueda, glosario, simulador DNS, consulta de comandos, tema claro/oscuro, tamaño de fuente, modo técnico, impresión, exportación de apuntes y progreso guardado localmente.

### Cobertura por módulo

| Nº | Módulo | Prácticas / fragmentos | Verificaciones | Preguntas |
|---:|---|---:|---:|---:|
| 1 | Publicar sitios y aplicaciones | 6 | 6 | 2 |
| 2 | Crear subdominios | 3 | 3 | 2 |
| 3 | Administrar la zona DNS | 3 | 3 | 2 |
| 4 | Gestionar correo electrónico | 3 | 3 | 2 |
| 5 | Proxy inverso y redirecciones | 4 | 5 | 2 |
| 6 | Seguridad del dominio | 2 | 3 | 2 |
| 7 | Balanceo y alta disponibilidad | 10 | 10 | 7 |
| 8 | Otros servicios de red | 2 | 3 | 2 |

## 2. Problemas y oportunidades

| Prioridad | Área | Diagnóstico | Acción recomendada |
|---|---|---|---|
| Alta | Código mantenible | Contenido, estilos y lógica concentrados en un HTML monolítico | Separar datos, presentación y lógica en fases. **No hacerlo a ciegas**, ya que hay mucho contenido y estado interactivo |
| Alta | Accesibilidad | Algunos botones originales usaban texto blanco sobre el azul muy claro del tema oscuro | Definir el color **sobre acento** independiente para cada tema; añadido en CSS |
| Alta | Consistencia | Paletas y patrones diferentes entre Dominios-WEB, KaliHelper, ATT&CK Lab y ASIR Battle | Adoptar tokens de diseño y especificar cabeceras, navegación, tarjetas, código, formularios y botones |
| Media | CSS incompleto | Se usa `var(--shade)` sin ninguna declaración en el CSS original | Variable definida explícitamente en el nuevo CSS |
| Media | Móvil | Los cinco controles superiores pueden abarrotar pantallas pequeñas | Darles desplazamiento horizontal accesible en móvil y reducir los espacios de contenido |
| Media | Accesibilidad | El botón de tema cambia de estado visual pero no exponía `aria-pressed` | Exponer y actualizar `aria-pressed`, aplicado en la rama |
| Media | Lectura | Contrastes, espaciados y radios variados | Estabilizar escala, sombras, bordes y tamaños sin reducir densidad informativa |
| Media | Continuidad | La apariencia oscura no era la predeterminada, frente a varias aplicaciones del conjunto ASIR | Usar modo oscuro inicial **solo para nuevas visitas**; respetar elección guardada |
| Baja | Metadatos | El README principal apenas describe la herramienta | Ampliar README con guía de uso, arquitectura, capturas, pruebas y enlaces a la documentación |

## 3. Sistema visual propuesto (ASIR UI v1)

**Lenguaje común:** panel técnico educativo sobrio, fondo azul marino, acento turquesa/cian, superficies de profundidad moderada y componentes reconocibles. Los contenidos y las prácticas tienen prioridad visual sobre la decoración.

**Colores de ejemplo:**

| Uso | Claro | Oscuro |
|---|---|---|
| Fondo | `#f2f6fa` | `#08111d` |
| Superficie | `#ffffff` | `#0e1a29` |
| Texto | `#172c3d` | `#eaf4f8` |
| Texto secundario | `#476173` | `#b1c8d4` |
| Acento | `#116b74` | `#53e0ce` |
| Texto de botón sobre acento | `#ffffff` | `#07252a` |

Contrastes calculados de ejemplos representativos: texto principal claro 14,34:1, texto secundario claro 6,51:1; texto principal oscuro 15,68:1, texto secundario oscuro 10,08:1; botón principal claro 6,21:1 y botón principal oscuro 9,89:1. **No equivale a una auditoría WCAG integral**: quedan por comprobar estados interactivos, todos los colores heredados y tamaños finales en navegador.

**Convenciones reutilizables:** escala de 4/8/12/16/24 px; radios de 9/13/18 px; áreas de foco visibles; superficies semánticas; bloques de código en tonos oscuros; opciones de movimiento reducido; impresión legible.

## 4. Qué se ha modificado en esta rama

1. Nuevo archivo `styles/asir-ui.css` cargado **después del CSS existente** para facilitar una reversión sin tocar el contenido.
2. Ajustes visuales de cabecera, navegación, fichas, bloques de código, selector de plataforma y botones.
3. Corrección de la variable CSS `--shade` que se usaba sin estar declarada.
4. Contraste corregido para botones de acción y pasos de prácticas en ambos temas.
5. Mejoras responsive de toolbar, navegación y selector de Linux/Windows.
6. Preferencia **oscura por defecto en primera visita**; los usuarios con preferencia almacenada mantienen su elección previa.
7. `aria-pressed` sincronizado en el botón de tema.
8. Respeto a `prefers-reduced-motion` y estilos específicos de impresión.

**No se ha modificado la base de contenidos:** ni los ocho capítulos, ni los 33 fragmentos, ni las guías específicas, ni las preguntas, ni los ejemplos de configuración. Tampoco se ha modificado `main`.

## 5. Plan de evolución recomendado

### Fase 1 — piloto visual (esta rama)

Aprobar o ajustar paleta y componentes; revisar manualmente desktop y móvil; comprobar ambos temas y flujos.

### Fase 2 — separación del código

Extraer los estilos originales por bloques, dejando `styles/tokens.css`, `styles/layout.css`, `styles/components.css` y `styles/asir-ui.css`. Extraer los datos de los capítulos a JS/JSON estático versionado y las rutinas de la interfaz a archivos JS independientes. Mantener despliegue estático en GitHub Pages y funcionamiento sin conexión una vez descargados todos los archivos.

### Fase 3 — normalización de la colección ASIR

Crear paquete CSS compartido de tokens y componentes genéricos. Migrar **por adaptadores** (cada aplicación conserva sus clases y JS) comenzando por Dominios-WEB, KaliHelper, Guía Terminal y ASIR Battle. No copiar selectores como `.navitem` indiscriminadamente entre proyectos: los componentes no tienen el mismo DOM.

### Fase 4 — calidad didáctica

Revisión técnica de comandos de Apache/Nginx según distribución, ejercicios con escenarios y solución comprobable, progreso exportable, accesibilidad por teclado y pruebas automatizadas de navegación. Valorar distinción visual entre teoría, simulación, comandos que se ejecutan y resultados esperados.

## 6. Matriz mínima de pruebas antes de incorporar a main

- [ ] Abrir la app en Chromium y Firefox, escritorio 1440/1024 px y móvil 390/360 px; comprobar que no hay desbordamiento horizontal inesperado.
- [ ] Ver inicio y los **8 módulos**; buscar un concepto y entrar desde el menú.
- [ ] Cambiar tema claro/oscuro; recargar; comprobar persistencia y texto de botones legible.
- [ ] Navegar mediante Tab/Enter/Espacio; comprobar foco visible, `aria-pressed` y controles accesibles.
- [ ] Seleccionar Linux/Windows, Debian/Rocky/Windows/WSL y Apache/Nginx; verificar actualización de los tutoriales.
- [ ] Expandir pasos y copiar fragmentos de código.
- [ ] Probar simulador DNS, comandos, glosario, ejercicios, notas y progreso.
- [ ] Cambiar tamaño de texto y activar/desactivar modo técnico.
- [ ] Probar exportación de apuntes y modo impresión.
- [ ] Comprobar modo offline con los archivos completos.
- [ ] Revisar contraste de elementos secundarios, errores, estados hover/focus y capturas de ambos temas.

## 7. Validaciones realizadas y límites

- Los cambios de `index.html` se han comprobado como **cinco sustituciones localizadas y reversibles**; retirarlas reproduce exactamente el HTML de `main`.
- La nueva hoja tiene llaves CSS equilibradas y resuelve las referencias a variables CSS detectadas en el documento.
- Se han calculado contrastes de combinaciones clave.
- **No se ha ejecutado una batería de pruebas end-to-end en un navegador contra la versión modificada**. El servidor de GitHub Pages no publica automáticamente ramas de pruebas y el acceso directo a la web no estaba disponible en la herramienta de consulta. Antes de fusionar, hacer las comprobaciones manuales indicadas.

## 8. Cómo probar y revertir

Consultar la rama `design/asir-visual-system-v1` en GitHub. Descargar la rama como ZIP o clonarla y abrir `index.html` conservando la carpeta `styles`. Para levantar un servidor estático local: `python -m http.server 8000` y visitar `http://localhost:8000`.

**Revertir:** no fusionar la rama, o eliminar `<link rel="stylesheet" href="styles/asir-ui.css">` y restaurar los cuatro cambios menores del botón de tema y su preferencia. La rama no altera los sitios actualmente desplegados.
