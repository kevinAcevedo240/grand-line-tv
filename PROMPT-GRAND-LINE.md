# GRAND LINE · Launcher de One Piece para Android TV / Google TV

> Cómo usarlo: no tienes que descargar nada. Abre tu asistente (Claude Code, Codex o Gemini CLI) en una carpeta
> vacía y escríbele: `Lee https://github.com/kevinAcevedo240/grand-line-tv/blob/main/PROMPT-GRAND-LINE.md y síguelo.`
> Él trae el repo CleanYourTV y las dos imágenes por su cuenta.

---

Mi TV es Android TV o Google TV y está en la misma red Wi-Fi que este computador. Quiero reemplazar el launcher
por **GRAND LINE**: una pantalla de inicio de One Piece que se sienta como un TV de edición especial, no como un
tema genérico con un logo pegado. Abajo está el diseño completo, ya probado en un TV real. Respétalo al detalle.

## 0. Antes de diseñar

1. Trabaja dentro del repo CleanYourTV. Si no está en esta carpeta, clónalo con
   `git clone https://github.com/Gathin23/cleanyourtv` y entra en él. Si el TV todavía no se ha limpiado, sigue
   primero su `PROMPT.md` completo (backup, debloat y launcher), usando sus scripts para conservar el backup y la
   ruta de restauración. Si ya está hecho, salta este punto.
2. Conéctate al TV con las herramientas del repo y dime: modelo, versión de Android, RAM y resolución.
3. Hazme estas preguntas todas juntas, antes de empezar:
   - ¿Cómo quieres que te salude el TV? (por defecto: "nakama")
   - ¿Qué apps usas más, y en qué orden? (máximo 8 en el carrusel)
   - ¿En qué idioma quieres la interfaz? (por defecto: español)
   - Nivel de efectos: completo, equilibrado o rendimiento. Si el TV tiene 2 GB de RAM o menos, recomiéndame rendimiento.

## 1. Archivos de referencia

Descarga estas dos imágenes en una carpeta `grandline/` dentro del repo (con `curl -fsSL -o`):

| Archivo | Enlace | Para qué es |
|---|---|---|
| `onepiece-logo.png` | https://raw.githubusercontent.com/kevinAcevedo240/grand-line-tv/main/imagenes/onepiece-logo.png | El logo oficial, con fondo transparente. Va abajo a la derecha. |
| `thousand-sunny.png` | https://raw.githubusercontent.com/kevinAcevedo240/grand-line-tv/main/imagenes/thousand-sunny.png | El barco Thousand Sunny, con fondo transparente. Va en el mar, al fondo a la izquierda. |

Conviértelos a WebP optimizado dentro de la app. Fuera de estas dos imágenes, **todo se dibuja por código**:
no descargues otras imágenes de internet.

## 2. Concepto visual

- Ambiente: altamar, aventura, pergamino envejecido y un paisaje vivo que cambia con la hora del día.
- Paleta de interfaz: azul océano profundo `#0B2545`, dorado berry `#E9B949`, rojo sombrero de paja `#C1121F`,
  pergamino `#F2E3C6`, blanco espuma `#F7F9FB`.
- Tipografías libres (OFL), empaquetadas dentro de la app, sin depender de internet:
  - **Pirata One**: saludo, hora, fecha, nombres de las apps y el título de la app enfocada.
  - **Rye**: "WANTED", "VIVO O EN EL SOFÁ", "MARINE" y los botones de abajo.
  - **Nunito**: textos de interfaz y frases.
- Todo debe leerse a 3 metros: textos grandes y buen contraste.

## 3. Layout (diseña en 1920 × 1080, escala a la resolución real, margen seguro del 5 %)

### Arriba a la izquierda: saludo y hora
- Saludo según la hora, en Pirata One dorado: "¡Buenos días, nakama!" (5:00–11:59), "¡Buenas tardes, nakama!"
  (12:00–18:59) y "Noche de travesía, nakama" (19:00–4:59).
- Debajo, la hora muy grande (unos 140 px) en Pirata One blanco. A su derecha, una línea vertical fina dorada y
  el día de la semana ("Viernes") y la fecha ("30 de octubre") en Pirata One dorado, en dos líneas.

### Arriba a la derecha: Log Pose y Wi-Fi
- **Log Pose** (la brújula de One Piece): pedestal de bronce en perspectiva 3D (un cilindro corto con su cuello,
  sin que el borde café de la base se salga del dibujo), el brazalete de cuero que pasa por detrás y por delante
  con costuras, y una esfera de vidrio con la esfera de la brújula.
  - La aguja es una vista aparte, para girar sin redibujar el instrumento.
  - Cada pocos segundos se mueve sola un poquito, como si tambaleara buscando una isla.
  - Al cambiar de app en el carrusel, "busca": gira un poco y se asienta.
  - Al enfocarla, da una vuelta.
  - Es enfocable: con OK abre la **Bitácora** (ver sección 6).
- **Wi-Fi como ojo de buey** (porthole): aro de bronce con remaches, vidrio oscuro de mar y las ondas del Wi-Fi
  en color pergamino. Muestra el estado de la conexión y con OK abre los ajustes de red.
- El foco de estos controles es un aro dorado con un brillo suave, dibujado dentro del margen de la vista.

### Centro: el carrusel coverflow de carteles WANTED (el protagonista)
- Cada app es un cartel de **WANTED** (ver sección 4). La app enfocada queda centrada.
- Posiciones a cada lado del centro: 330, 585 y 790 px. Escalas: 1.0 (centro), 0.82, 0.66 y 0.52. Oscurecido de
  las laterales: 38 %, 62 % y 80 %, como si se alejaran hacia atrás.
- En "equilibrado" y "completo" las laterales giran en Y unos 22°. En "rendimiento" el carrusel es plano.
- La app enfocada sube un poco y tiene un brillo dorado suave.
- Transición entre apps de unos 300 ms con ease-out, sin saltos.
- **Encima** del cartel enfocado: el nombre de la app en Pirata One dorado (unos 64 px) y debajo su frase en
  Nunito itálica blanca ("Se busca vivo o en el sofá"). Para que se lean usa un drop shadow sutil y pequeño
  detrás de las letras, **nunca** un óvalo o círculo oscuro, y que la sombra no tape el cartel.
- Animación de entrada de menos de 1 segundo: el carrusel llega deslizándose como una ola.

### Abajo al centro: botones de pergamino
- Tres botones tipo píldora: **ENTRADAS**, **AJUSTES** y **TODAS LAS APPS**, con su ícono.
- Mismo papel de los carteles (textura de pergamino, borde café quemado), texto en Rye en mayúsculas.
- ENTRADAS abre un panel de pergamino con las entradas HDMI. AJUSTES abre los ajustes del sistema. TODAS LAS APPS
  abre una cuadrícula con todas las apps instaladas, en mini carteles con el mismo estilo.

### Abajo a la derecha: el logo
- `onepiece-logo.png`, de unos 360 px de ancho, sin fondo ni recuadro.

## 4. El cartel WANTED (lo más parecido posible al original)

Dibújalo en un espacio de 600 × 840 y escálalo. Pintado una sola vez en un bitmap por app.

1. **"WANTED"** en Rye, ocupando todo el ancho arriba.
2. **Retrato**: un marco horizontal ancho con el logo o banner de la app. El logo va **completo dentro del
   marco, sin salirse**, sobre su propio color de fondo (léelo de las esquinas del logo).
3. **"VIVO O EN EL SOFÁ"** en Rye, en lugar de "DEAD OR ALIVE".
4. El **nombre de la app** grande en Pirata One, como si fuera el nombre del pirata.
5. La **recompensa** con el símbolo de berry (una B cruzada por dos trazos verticales) y la cifra con puntos de
   miles y el guion final, como en el anime: `฿ 3.000.000.000-`.
6. Abajo a la izquierda, letra muy pequeña en tres líneas: "ESTE CARTEL ES PURA FICCIÓN. CUALQUIER PARECIDO CON
   TU MARATÓN DEL DOMINGO ES PURA COINCIDENCIA." Abajo a la derecha, **"MARINE"** en Rye.
7. **Las S de los lados**: a cada lado del bloque de texto van **dos** curvas en S largas, una sobre otra y
   conectadas, como trazos de pluma (finos en las puntas, más gruesos en el medio y con una bolita en cada
   extremo). Deja un margen claro: no pueden tocar la B de los berries ni la V de "VIVO".
8. **Papel**: pergamino con textura, bordes quemados hacia adentro, ligeramente desgarrados (usa una semilla
   aleatoria fija para que cada cartel se rompa siempre igual) y una sombra realista detrás.

Recompensas y frases por defecto (la más usada vale más):

| App | Recompensa | Frase |
|---|---|---|
| Netflix | ฿ 3.000.000.000- | Se busca vivo o en el sofá |
| YouTube | ฿ 1.111.000.000- | Visto por última vez en «un video más» |
| Disney+ | ฿ 1.032.000.000- | Navega entre princesas y piratas |
| Prime Video | ฿ 930.000.000- | Entregas desde el Nuevo Mundo |
| Crunchyroll | ฿ 500.000.000- | Reincidente: maratones de anime |
| YouTube Music | ฿ 330.000.000- | Canta a todo pulmón en cubierta |
| Play Store | ฿ 66.000.000- | Mercader de la Gran Ruta |
| Ajustes | ฿ 1- | Solo un grumete |

Las apps sin cartel propio usan un WANTED genérico con su ícono. Detecta las apps instaladas automáticamente.

## 5. El paisaje (sin fotos de fondo: todo dibujado por código y en movimiento)

Cambia solo según la hora: **mañana** 5:00–11:59, **tarde** 12:00–18:59, **noche** 19:00–4:59.

| | Cielo (de arriba a abajo) | Luz | Mar (del horizonte hacia abajo) |
|---|---|---|---|
| Mañana | `#1F5FA8` → `#5EA7E0` → `#A9D6F2` → `#E9F5FB` | Sol blanco con brillo cálido, arriba a la derecha | `#9FD4EA` → `#4EA3CF` → `#1F6AA6` → `#0B3A6A` |
| Tarde | `#0B2545` → `#3A3D76` → `#9C4F78` → `#F08A4B` → `#FFCF73` | Sol de atardecer grande y dorado, bajo, a la derecha | `#D8875A` → `#7B4F74` → `#283C6E` → `#071A33` |
| Noche | `#02060F` → `#071A36` → `#163463` → `#25497D` | Luna con brillo frío y estrellas | `#2C4F80` → `#173562` → `#0A1F3F` → `#030B18` |

Capas, de atrás hacia adelante:

1. Cielo con degradado, sol o luna con su halo, y nubes finas y lejanas (capa quieta).
2. **Nubes cúmulo realistas**: cada una hecha con decenas de "puffs" suaves, base plana y difuminada, más
   iluminada del lado de la luz y con un blur ligero aplicado **una sola vez** al pintarla. Nada de círculos
   sobrepuestos con bordes duros. Cada nube se desplaza muy despacio (unos 50 px por minuto); las lejanas, más lento.
3. Gaviotas, solo de día.
4. Una isla en silueta a la derecha, en el horizonte.
5. El mar con su degradado.
6. Crestas de olas más anchas que la pantalla, meciéndose.
7. El camino de luz del sol o la luna sobre el agua, en dos capas que titilan desfasadas.
8. Una **palmera** en la isla que se mece desde la base, con las hojas moviéndose como con viento.
9. **El Thousand Sunny** (`thousand-sunny.png`), lejos a la izquierda, mirando hacia la derecha (voltéalo):
   - Pequeño y algo opaco, con una bruma del color del cielo: más claro de día y más oscuro de noche.
   - Debe **estar dentro del agua**, no flotando: la quilla queda escondida bajo la línea de flotación, el borde
     inferior se funde con el mar, hay espuma donde el casco toca el agua y pequeñas olas en la proa y la popa.
   - Un reflejo tenue debajo, volteado, desvanecido y cortado en ondas.
   - Navega un poco hacia adelante y vuelve, y se balancea con el oleaje.
10. Viñeta y degradados oscuros arriba y abajo para que el texto siempre se lea.

Cada movimiento arranca en un punto aleatorio de su ciclo, para que nada se mueva al mismo tiempo.

## 6. Bitácora (ajustes del launcher, desde el Log Pose)

Un panel de pergamino arriba a la derecha, con título "Bitácora" y subtítulo "Ajustes de este launcher":

| Opción | Valores |
|---|---|
| Apps del carrusel | Ordenar › |
| Efectos | Rendimiento (carrusel plano) · Equilibrado (carrusel 3D) · Completo (3D y zoom lento en las imágenes) |
| Fondo | Paisaje (el dibujado) · Mis imágenes (fondos propios de la carpeta `wallpapers/`) |
| Paisaje en movimiento | Sí · No |
| Cambio de imagen | Cada 30 s, 45 s, 1 min, 2 min o 5 min (solo con "Mis imágenes", con crossfade de 1,5 s) |

Guarda lo que cambie en SharedPreferences: todo se ajusta sin recompilar.

## 7. Control remoto y sonido

- Navegación 100 % con el D-pad y el foco siempre visible. Izquierda/derecha mueven el carrusel, abajo baja a los
  botones, arriba sube al Log Pose y al Wi-Fi, Atrás vuelve al carrusel.
- Cada movimiento y cada clic suena con los sonidos de navegación del sistema.

## 8. Rendimiento (muchos TVs tienen 1 GB de RAM)

- Construye sobre el launcher que trae el repo, respetando su stack y sus protecciones (botones del control,
  Entradas, Home y restauración). No lo rehagas desde cero; si es imposible, explícame por qué antes.
- Carteles, paisaje y controles se pintan **una sola vez** en bitmaps. Al navegar solo se mueven y desvanecen
  (translation, rotation, scale, alpha). Nada de blur en tiempo real.
- El paisaje se anima con un solo temporizador a unos 20 pasos por segundo (se mueve tan lento que se ve igual
  que a 60). Los carteles y la aguja sí van a 60 fps.
- El paisaje solo se mueve mientras el inicio está en pantalla. Sin servicios en segundo plano.
- Objetivo: navegación fluida y apertura de apps sin retraso.

## 9. Cómo quiero que trabajes

1. Primero hazme un **prototipo HTML** en 1920 × 1080 que pueda abrir en el navegador de este computador y navegar
   con las flechas, con teclas para ver la mañana, la tarde y la noche. Muéstrame capturas de las tres horas y
   espera mi visto bueno antes de seguir.
2. Cuando lo apruebe, impleméntalo en el launcher, compílalo e instálalo por ADB con los scripts del repo, y
   déjalo como pantalla de inicio.
3. Verifícalo con capturas de pantalla del TV y confirma que sobrevive a un reinicio.
4. Antes de cada paso que toque el TV, dime qué vas a hacer. Nada irreversible: todo debe poder deshacerse con la
   restauración del repo.
5. Al terminar, apaga la depuración por red como indica el repo y explícame en español, corto, cómo volver al
   launcher original.

Si se te ocurre un detalle de One Piece que eleve el diseño sin hacerlo pesado, propónmelo antes de agregarlo.
