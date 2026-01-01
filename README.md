# DGT Mainchess (Web Serial)

Proyecto estatico para visualizar y seguir un tablero DGT conectandolo al navegador mediante la API Web Serial. Incluye tres variantes de interfaz (raiz, v2 y v3) que leen el estado del tablero, muestran la posicion con Chessground y en las versiones nuevas validan jugadas con chess.js.

## Requisitos
- Tablero DGT conectado por USB y drivers FTDI instalados.
- Navegador Chromium (Chrome, Edge, Brave...) con soporte de Web Serial; requiere ejecutarse sobre https o localhost (abrir el archivo directamente no funciona).
- Permitir el acceso al puerto serie cuando el navegador lo solicite.

## Puesta en marcha local
1) Arranca un servidor estatico en la raiz del repo, por ejemplo: `python -m http.server 8000`.
2) Abre `http://localhost:8000/` y elige la variante que quieras probar (`/index.html`, `/v2/` o `/v3/`).
3) Conecta el tablero y pulsa el boton "Cargar posicion del tablero DGT" para conceder permisos y sincronizar.
4) En la variante `/v3`, usa los selectores superiores para alternar entre modo claro/oscuro, elegir el diseño de tablero y cambiar la paleta de piezas sin necesidad de recargar la pagina.

## Estructura rapida
- `index.html`: demo basica; lee la posicion inicial, resalta ultimos movimientos y muestra el FEN.
- `v2/`: interfaz con validacion de jugadas usando chess.js y seleccion de color inicial.
- `v3/`: version apaisada con tablero principal, tablero auxiliar oculto y sincronizacion de turnos. Incluye selector de tema claro/oscuro, estilos de tablero (madera, pizarra, esmeralda) y tres gamas de piezas (clasicas, neón, esmeralda).
- `lib/Board.js`: envoltorio sobre Web Serial para hablar con el tablero DGT (reset, numero de serie, version, posicion, lector continuo).
- `lib/Command.js`: constantes y decodificacion del protocolo DGT, incluyendo piezas y mapeo de casillas.
- `plugins/chessground/`: assets locales de Chessground (CSS y JS minificado) para poder usarlo sin conexion CDN en la version basica.

## Publicar en GitHub Pages
- No hay build: basta con publicar tal cual el contenido estatico. Puedes usar Settings > Pages > Deploy from branch apuntando a `main` y la carpeta `/` (o `/v2`, `/v3` si quieres fijar una variante).
- GitHub Pages sirve sobre https, por lo que Web Serial funcionara; el usuario tendra que aceptar el acceso al dispositivo desde el navegador.
- Si usas otra plataforma, asegurate de servir sobre https y mantener las rutas relativas tal cual.

## Desarrollo
- Sin dependencias npm: las librerias llegan via CDN (v2/v3) o estan dentro de `plugins/`.
- Si necesitas ampliar funcionalidad, anade pruebas manuales conectando un tablero real porque el flujo depende del hardware.
- Anade una licencia cuando decidas la distribucion.