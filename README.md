# Pareto Core web · 2.1

Versión independiente para móvil y computador, basada en las funciones del iPhone hasta la compilación 30. Incluye Escáner, Análisis, detalles por área y problema, barras de situaciones, Pareto avanzado, historial con filtros y edición completa, tareas con importancia personal, exportación Excel. Sin IA.

## Abrir en este equipo

La configuración pública actual ya está en `config.js`, excluida de Git. Desde esta carpeta:

```sh
python3 -m http.server 8012 --bind 127.0.0.1
```

Abre **http://127.0.0.1:8012/**. Usa tu cuenta existente de Supabase; los datos son los mismos del iPhone. Hace falta internet para acceder a Supabase. No abras `index.html` directamente: el exportador necesita cargar la plantilla mediante HTTP.

En otro equipo, copia `config.example.js` a `config.js` y completa únicamente la URL y la clave pública del mismo proyecto. No introduzcas claves secretas ni `service_role`. Los estilos, gráficos y cliente Supabase están incluidos localmente; no dependen de un CDN.

## Subir a GitHub y publicar

1. Crea tu repositorio y sube **el contenido de esta carpeta** a su raíz, incluyendo `assets`, `js`, `vendor`, `tests`, `.nojekyll` y `.github`. No subas el proyecto iPhone ni el resto de la carpeta Pareto_Core_v2. No incluyas el `config.js` local.
2. En **Settings → Secrets and variables → Actions → Variables**, crea `SUPABASE_URL` y `SUPABASE_ANON_KEY` con los dos valores públicos de `config.js`.
3. En **Settings → Pages → Source**, elige **GitHub Actions**.
4. En **Actions → Publicar Pareto Core → Run workflow**, ejecuta la publicación. Primero se verifican los cálculos y luego se prepara y publica el sitio. El enlace aparece al terminar. Las siguientes actualizaciones se publican ejecutando nuevamente el flujo.

Este flujo está preparado; no se ha publicado nada desde esta entrega. Las variables indicadas son configuración pública de navegador y aparecerán en el sitio, como corresponde al cliente Supabase. La privacidad de los registros sigue dependiendo de las políticas RLS existentes.

Si prefieres otro alojamiento estático, sube `index.html`, `styles.css`, `js`, `assets`, `vendor` y un `config.js` con los valores públicos. Todas las rutas son relativas, compatibles con una subcarpeta de GitHub Pages. No se requiere compilación ni servidor de aplicación.

## Uso y privacidad

- En escritorio (desde 768 px), se abre Acciones con las pendientes como contenido principal. La navegación superior muestra Acciones, Nueva acción y Escáner; Análisis está oculto. Nueva acción tiene su propia pantalla para escribir y valorar la tarea. En móvil se conservan las pestañas inferiores de Escáner, Acciones y Nueva acción, y el inicio en Escáner. Cambiar de pestaña o redimensionar no descarta borradores. Tras guardar una acción se vuelve a la lista; un error mantiene los campos para reintentar. El historial aparece únicamente al solicitarlo.
- La interfaz web recupera el fondo azul oscuro y los acentos verde esmeralda originales. Los informes Excel conservan su formato salvia.
- La renovación visual compacta filas, presenta la importancia como un selector discreto y usa un icono de papelera con nombre accesible para eliminar. El progreso se mantiene en una franja secundaria. Escáner y acceso comparten tipografía, campos y espaciado; las funciones y los formularios siguen disponibles sin nuevos pasos.
- Los gráficos sencillos muestran intensidad de fricción 1–5. El Pareto avanzado distribuye carga; el de problemas usa solo la carga clasificada. La importancia personal se usa únicamente en tareas y acciones.
- Hoy, Semana, 30 y 90 días usan la zona horaria del navegador; las semanas empiezan el lunes. El historial general incluye todos los registros; detalles, gráficos y Excel usan el período abierto.
- La sesión se restaura mediante el almacenamiento del cliente Supabase. «Recordar mi correo» guarda solo el correo como preferencia; desactivarlo elimina ese dato, independientemente de la sesión. Pareto no guarda contraseñas ni copias de registros en el navegador.
- La sesión web no utiliza Keychain; esa protección pertenece a la app nativa. Cierra sesión en equipos compartidos. El contenido se cubre al ocultar la pestaña, aunque las miniaturas del navegador o del sistema no están bajo control total de una web.
- La edición guarda solo al confirmar. Al borrar se pide confirmación. Los cambios actualizan detalles, cálculos, sugerencias e historial sin alterar fecha ni respaldo original.
- No hay escrituras sin conexión. Las respuestas de otra cuenta o solicitudes anteriores no se aplican a la pantalla actual.

## Excel

Exporta desde Análisis para obtener Áreas, Situaciones y Datos con filtros independientes. Áreas no incluye tarjetas ni etiqueta de período. Situaciones tiene una barra por registro, incluso textos repetidos; el área permanece en su columna. Filtrar Áreas o Situaciones también filtra su gráfico; filtrar Datos no recalcula los otros informes.

También puedes usar «Descargar Excel», discretamente junto a Actualizar en Acciones. Descarga el mismo informe de fricciones para el período elegido en Análisis (Semana al entrar), incluso si todavía no has abierto ese apartado. El período se indica al mantener el cursor sobre el botón; no exporta la lista de tareas. Con Análisis oculto de la navegación, se utiliza Semana al iniciar la sesión.

Las celdas de texto no se convierten en fórmulas. Se conservan los tonos salvia, el nivel original cuando existe y los separadores suaves. Excel limita las alturas de fila a 409,5 puntos: textos con muchísimos saltos pueden requerir consultar la celda completa. El orden inicial mantiene colores por nivel; Excel conserva el formato de cada punto del gráfico al reordenar manualmente las filas, por lo que el número y la longitud de la barra siguen siendo la referencia del nivel.

## Pruebas

```sh
node --test tests/core.test.cjs
```

Para pruebas de navegador, instala Playwright en tu entorno de desarrollo y su Chromium. No se utiliza en la web publicada:

```sh
node tests/browser.cjs
node tests/edge-browser.cjs
node tests/static.cjs
node tests/layout.cjs
node tests/actions-export.cjs
```

Puedes indicar su ubicación mediante `PLAYWRIGHT_MODULE`. Para la comprobación adicional con WebKit instalado, usa `TEST_WEBKIT=1`. Todas estas pruebas interceptan Supabase y usan datos simulados; no modifican tu cuenta. Consulta `VERIFICATION.md` para los resultados y los pendientes de dispositivo.

## Organización

`js/domain.js` contiene vocabulario y cálculos; `service.js`, operaciones filtradas por propietario; `state.js`, estado e invalidación de solicitudes; `charts.js`, gráficos accesibles; `excel.js`, exportación; `app.js`, interacción. La plantilla Excel se conserva en `assets/excel-template.json`. Supabase JS está fijado a **2.117.3**, con su licencia en `vendor`.

La entrega no modifica la web anterior ni la aplicación iPhone, no necesita migraciones y no incluye IA, publicación automática ni ZIP.
