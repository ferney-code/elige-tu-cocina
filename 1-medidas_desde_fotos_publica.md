# Medidas desde fotos: skill de ensayo ficticio

Versión pública de ensayo del 28 de octubre de 2026. Documento único para guardar y reutilizar, no instalado como skill. Se omite una ruta temporal del entorno de origen y se generaliza una descripción de fallo de ejecución; no se alteran los algoritmos.

> **No usar para fabricar, cortar ni certificar medidas.** Esta entrega conserva material recuperado y añade una prueba sintética ejecutada. No están disponibles las fotos originales 4 y 5 ni el plano citado. No se ha verificado ninguna dimensión física de una cocina.

## Qué se recuperó y de dónde

La fuente señalada es una conversación de trabajo externa. Su lectura directa actual no expuso el contenido ni el archivo de la tarjeta. No se publica aquí el enlace a esa conversación. Por tanto, este documento **no es una copia verificada de aquella tarjeta**.

El contenido técnico se recuperó del borrador relacionado **ENSAYO COCINAS - entrega completa**, fechado el 27 de octubre de 2026, leído nuevamente el 28 de octubre. Su HTML conserva la sangría que no conserva bien la representación de texto plano. El borrador describe sus propias fuentes y lagunas. Se recuperaron siete bloques Python completos. No hay archivos originales independientes con los que verificar su identidad byte a byte. Las afirmaciones históricas de ejecución o exactitud son afirmaciones del borrador, no comprobaciones de esta entrega.

Se conserva abajo el contenido recuperado, incluido su inventario, método, resultados declarados y código. Los títulos históricos "resultados reales", "ejecutado" y "nunca ejecutado" describen **lo que decía la fuente el 27 de octubre**. Hoy sí se ejecutaron partes de la versión B con datos sintéticos. Las salidas históricas no se volvieron a obtener de las fotos originales.

## Cómo usar esta skill sin adivinar

Recibe este Markdown como documento de instrucciones y depósito de código. No presupongas que leerlo instala Python, guarda scripts o aporta imágenes. Trabaja en una carpeta separada y conserva las dos versiones de `vps.py` en carpetas distintas.

Antes de calcular, identifica la tarea: ensayo ficticio o medición de una foto real. Para el ensayo ficticio, usa solamente el procedimiento sintético de esta entrega. Para una foto real, pide el archivo original y una referencia física comprobable; no reutilices los puntos de las fotos 4 y 5 sobre otra imagen. Una imagen parecida, una captura reescalada o la cocina publicada para otro ensayo no sustituyen esos originales.

Para interpretar los datos:

- `u` es la columna de un píxel, creciente hacia la derecha; `v` es la fila, creciente hacia abajo. El origen de coordenadas de imagen es la esquina superior izquierda.
- `w`, `h` son ancho y alto en píxeles. No son dimensiones físicas. No cambies resolución, recorte o giro después de anotar coordenadas.
- Un punto de fuga es la intersección proyectada de líneas paralelas del espacio. `x1`, `x2` son sus columnas; `yh` es la fila del horizonte asumido. `A` corresponde al punto de fuga izquierdo y `B` al derecho, no necesariamente a "ancho" y "largo" de todos los muebles.
- `H` es la altura del plano medido sobre el piso y `hc` la altura de cámara. `Ht` y `Hb` son las alturas conocidas o supuestas de los dos extremos de una referencia vertical. En el material histórico se usaron 36 y 4,5 pulgadas como supuestos, no medidas comprobadas.
- `f` es distancia focal expresada en píxeles, no milímetros de una lente. El modelo supone direcciones ortogonales, cámara nivelada y centro principal en la columna central. No corrige distorsión de lente.
- Un inlier es un segmento aceptado por el ajuste. RANSAC busca un ajuste por muestreo; bootstrap remuestrea segmentos para estimar dispersión. Monte Carlo repite el cálculo con perturbaciones supuestas. Los percentiles 5 y 95 **no garantizan** que la medida física verdadera quede dentro.
- Las salidas están en la unidad de las alturas de referencia. En estos ejemplos son pulgadas. Para mostrar centímetros, multiplica valor y extremos del intervalo por 2,54; eso no mejora la exactitud.

Para fotos reales, comprueba primero la legibilidad y las dimensiones del archivo. Comprueba que los bordes elegidos pertenezcan al mismo plano y que ambas referencias verticales tengan alturas conocidas. Si no puedes identificar un extremo, no inventes su posición. Si falta la escala física, detén las medidas absolutas y pide esa escala. Si solo se aceptan alturas supuestas, declara los resultados condicionados a ese supuesto, no como cotas certificadas.

Después de detectar puntos de fuga, revisa que existan dos grupos válidos y que sus parámetros sean finitos. Si no hay suficientes líneas, si el modelo ortogonal no corresponde a la escena o si la detección falla, detente y solicita una foto mejor o una revisión manual. No inventes un umbral mínimo de líneas como si lo validara el proyecto.

Antes de proyectar puntos, comprueba que `v != yh`, que las dos filas de una referencia sean distintas y que `xv != u1` en `vline`. Comprueba también que `-(x1-cx)*(x2-cx)` sea positivo. El original oculta este último problema con `max(..., 1)`; que el programa devuelva un número no vuelve válida una geometría imposible. No continúes con `NaN`, infinitos, grupos vacíos o referencias ambiguas.

Al elegir medidas, usa `lenVP` solo si el borde sigue la dirección de fuga indicada y ambos puntos están al mismo nivel; `len` permite dos puntos y alturas explícitas, pero sigue midiendo la proyección sobre A o B, no cualquier distancia espacial; `vert` exige una vertical cuyo extremo inferior esté realmente en el piso. Si no se cumple, detén esa medida. Revisa las coordenadas sobre una cuadrícula y registra el rasgo visible de cada punto, no solo sus números.

Compara planos y referencias antes de etiquetar confianza. El criterio histórico de diferencia mayor al 2 % entre planos es una regla del proyecto, no una precisión física demostrada. Si las estimaciones discrepan o un extremo está oculto, informa la discrepancia o una cota inferior justificada. No conviertas automáticamente la etiqueta histórica "MEDIDA" en una aprobación para fabricar.

## Preparación reproducible y prueba realizada ahora

Se comprobó ejecución con Python **3.10.12**, NumPy **2.2.6** y OpenCV **5.0.0**, instalado mediante el paquete `opencv-python-headless==5.0.0.93`. NumPy ya estaba disponible; OpenCV se descargó e instaló con pip. Se comprobó la existencia operativa de `createLineSegmentDetector` y su uso por los scripts. No se conocen las versiones usadas por el trabajo histórico.

Fuentes de distribución y referencia: [Python](https://www.python.org/downloads/), [NumPy](https://pypi.org/project/numpy/), [OpenCV sin interfaz gráfica](https://pypi.org/project/opencv-python-headless/). La instalación realizada fue una descarga de paquetes, no una compra. No se instaló nada en el teléfono o computador del destinatario. El paquete sin interfaz gráfica basta para los scripts recuperados: leen, dibujan y escriben archivos, no abren ventanas.

La versión 3.10 reproduce el entorno usado aquí, pero Python.org ya la marca fuera de soporte; no es una recomendación para nuevos sistemas de producción. Una versión mantenida requiere repetir las pruebas de compatibilidad. En otro equipo con Python 3.10 y pip disponibles, estos comandos preparan un entorno separado. No se ejecutaron en el equipo del destinatario. Si el sistema o la arquitectura no ofrece una distribución compatible, detente y registra el error; no prometas que la misma instalación funciona en todo dispositivo.

```bash
python3 -m venv .venv
. .venv/bin/activate
python -m pip install numpy==2.2.6 opencv-python-headless==5.0.0.93
python -c "import sys,numpy,cv2; print(sys.version); print(numpy.__version__,cv2.__version__)"
```

En Windows, la activación equivalente del entorno es `.venv\Scripts\activate` desde cmd. Si Python no está instalado, instala una distribución oficial adecuada al sistema antes de continuar. No escribas contraseñas ni claves dentro de este documento.

Guarda desde los bloques de esta entrega:

```text
ensayo/
  version_A/
    vp_nivelado.py
    grid.py
    vps.py         # bloque A; NO bloque B
    meas45.py      # conserva su ruta histórica en el original
  version_B/
    vp_nivelado.py # mismo bloque común
    grid.py        # mismo bloque común
    vps.py         # bloque B; NO bloque A
    meas.py
    ejemplo.py
    prueba_ficticia.py # código nuevo de esta entrega, más abajo
```

Copiar bloques no significa alterar el algoritmo. Conserva UTF-8 y la sangría. No copies delimitadores de Markdown ni las etiquetas INICIO/FIN al interior de un `.py`. No guardes ambos `vps.py` sobre el mismo archivo. Los módulos estándar no requieren instalación separada.

Hay un fallo de ejecución **comprobado ahora** en el comando histórico `VPJSON=vps.json python3 -I ejemplo.py`: `-I` excluye la carpeta local de búsqueda y `ejemplo.py` no la añade, por lo que aparece `ModuleNotFoundError: No module named 'meas'`. No se modificó el original. Para la ruta B en una carpeta de confianza, usa `python ejemplo.py` sin `-I`, con `VPJSON` definido, o crea y documenta un lanzador separado que añada explícitamente la carpeta. Aun sin `-I`, el ejemplo histórico necesita una clave `5` con datos auténticos de aquella foto; el JSON sintético usa `sintetica` y no debe hacerse pasar por esa foto.

Para la prueba ficticia, entra en `version_B` y ejecuta:

```bash
python prueba_ficticia.py
```

La prueba crea `sintetico.json`, `escena_ficticia.png`, `detectados.json` y `cuadricula_ficticia.png` en esa carpeta. Si ya hay archivos con esos nombres, usa una carpeta nueva para no sobrescribir trabajo propio. El código completo de la prueba está en esta entrega. No necesitas descargar otra foto ni un script externo.

El ensayo separa dos verificaciones. Primero aporta parámetros de cámara ideales inventados y proyecta puntos con geometría conocida para comprobar `meas.py`: altura de cámara 60 pulgadas, largo 40 pulgadas, altura 30 pulgadas. Después dibuja una escena de líneas sintética y ejecuta `vps.py` B y `grid.py`. La detección de la imagen tiene su propio error de rasterización y no se usa como si fuera idéntica a los parámetros ideales de la primera verificación. No se hizo una validación de punta a punta de medidas físicas desde una foto real.

Se ejecutaron 3000 perturbaciones de Monte Carlo. Los valores base coincidieron con 60, 40 y 30 dentro de una tolerancia aritmética de `1e-9`. Los intervalos fueron: cámara 58,2254 a 61,7808; largo 36,7821 a 43,2974; altura 28,9949 a 31,0186 pulgadas. La tolerancia comprueba cálculo ideal, no precisión de medición.

La detección de líneas terminó sin error: `x1=-494,2421`, `x2=1491,8868`, `yh=203,2681`; frente a los ideales -500, 1500 y 200, los errores fueron +5,7579, -8,1132 y +3,2681 píxeles. Se generó una cuadrícula de 1000 x 800 píxeles. No se establece con ello un umbral universal de aceptación.

El código recuperado de `vert` toma perturbaciones independientes para la misma fila inferior en numerador y denominador. Esto se conserva, pero limita la interpretación de sus intervalos. El generador aleatorio de `meas.py` se crea al importar: cambiar el orden de llamadas cambia sus intervalos. La prueba sintética se debe ejecutar en un proceso nuevo para comparar el registro.

No se ejecutaron `meas45.py` sobre sus originales, el ejemplo histórico con la foto 5 ni `analizar()` sobre una cocina. La ruta temporal incrustada en `meas45.py` no es portable. Para un ensayo real habría que crear una copia de trabajo con esa ruta cambiada, documentar ese único cambio y conservar el original. No se recuperaron las fotos 1 a 5, el plano, recortes históricos ni el archivo citado como paquete de continuación. Esos faltantes impiden reproducir y certificar los resultados históricos, aunque el ensayo ficticio sí corre.

## Material histórico recuperado

Lo que sigue conserva el texto técnico del borrador y sus siete scripts, no sus cabeceras de correo ni datos de cuenta. Las afirmaciones y etiquetas temporales son de esa fuente. Las instrucciones actuales anteriores prevalecen sobre comandos históricos fallidos y sobre cualquier afirmación histórica de validación.

AVISO PRINCIPAL: ESTA ENTREGA NO ESTÁ COMPLETA. No se pueden reproducir los resultados de las fotos 4 y 5 sin las imágenes originales. Detalle en la sección 1.


### 1. INVENTARIO


1.1 DISPONIBLE Y LEÍDO (texto real)
- vp_nivelado.py: leído del archivo adjunto en esta conversación. Es la versión que se usó como dependencia en el chat de origen, según el correo; no hay hash para comprobar que sean idénticos byte a byte.
- grid.py: código copiado del correo de origen (heredoc de la llamada a Bash que SÍ se ejecutó en el chat de origen).
- vps.py versión A: copiada del mismo correo, también ejecutada en el chat de origen sobre las fotos 4 y 5.
- meas45.py: copiado del correo, ejecutado en el chat de origen. Contiene los puntos elegidos a ojo de las fotos 4 y 5 y define las clases y funciones de medición.
- vps.py versión B, meas.py y ejemplo.py: versiones "consolidadas" copiadas del correo, escritas para una Skill. El intento de ejecutarlas falló en el chat de origen (según el borrador, no se completó el intento de ejecución). NUNCA SE EJECUTARON.
- Resultados numéricos de las fotos 4 y 5 y salidas de vps.py y meas45.py, tal como aparecen en el correo.
- Tamaños en píxeles de las 5 imágenes del paquete original (ver 3.1).

1.2 NO DISPONIBLE (impide reproducir)
- Las imágenes: foto 4 (1143x621) y foto 5 (1441x1001). No están en esta conversación ni en la carpeta de trabajo. Sin ellas no se pueden reejecutar ni verificar las medidas.
- Fotos 1 a 3 y sus resultados: el correo dice que se midieron en otro chat, pero ese contenido no está en el correo y el enlace compartido no se pudo abrir. NO HAY resultados de las fotos 1 a 3 en este material. No se inventan.
- El plano acotado de la foto 4 (isla 350,52 x 182,88 cm = 138 x 72 pulgadas): el chat de origen no lo vio. Solo se conoce el número que reportaba el paquete.
- paquete_continuar_medidas_generales.txt: adjunto citado en el chat de origen; no se pudo descargar ahí y tampoco está aquí.
- Los recortes con cuadrícula (g4.png, g5.png, g4a, g4b, g4c, g5a, g5b, g5c, g5d) y las lecturas de esas imágenes: no se conservaron. Solo quedan los números de los puntos escritos en meas45.py.
- Qué rasgo exacto de la foto 4 corresponde a cada referencia de escala (400,510) y (392,498): no quedó registrado. Para la foto 5, las referencias (625,901) y (544,685) están descritas en meas45.py como "isla" y "encimera derecha".
- Versiones de Python, OpenCV y NumPy usadas: NO REGISTRADAS.
- Salida de "python3 vp_nivelado.py foto.jpg" (función analizar) sobre las fotos: no está registrada.


### 2. MÉTODO PASO A PASO


Objetivo: sacar medidas aproximadas con rango de error desde una foto o render de cocina. Sirve para cotizar y planear; NO sirve para cortar material.

Paso 0. Identificar las fotos por tamaño en píxeles y copiarlas a una carpeta de trabajo como f4.jpg y f5.jpg.

Paso 1. Detectar segmentos de línea (vp_nivelado.py, función segmentos): se convierte la imagen a gris y se usa el detector LSD de OpenCV (cv2.createLineSegmentDetector con cv2.LSD_REFINE_STD). Se descartan segmentos más cortos que 2 % del ancho de la imagen.

Paso 2. Agrupar en dos puntos de fuga horizontales (función clusters): se excluyen segmentos casi verticales (a menos de 6 grados de 90) y casi horizontales (a menos de 1,5 grados de 0 o 180). Se hace RANSAC con 3000 iteraciones por punto de fuga, dos veces seguidas: el primer grupo se retira antes de buscar el segundo. Un segmento es "inlier" de un punto de fuga si su desviación angular respecto a la dirección hacia ese punto es menor que 1 grado. El puntaje de un punto de fuga es la suma de los largos de sus inliers. Se fijan semillas aleatorias: random.seed(2) y np.random.seed(2).

Paso 3. Refinar cada punto de fuga (vps.py): mínimos cuadrados ponderados por el largo del segmento, usando el autovector de menor autovalor de la matriz de rectas normalizadas (coordenadas centradas en el centro de la imagen y escaladas por K=1000). Se estima la incertidumbre sd_x1 y sd_x2 con 150 remuestreos bootstrap de los inliers. El horizonte yh es el promedio de las dos coordenadas y de los puntos de fuga, ponderado por el número de inliers de cada uno (n1, n2). El punto de fuga izquierdo es el de menor x. vdesv es la desviación angular media ponderada de las líneas casi verticales (si está cerca de 0, la cámara está nivelada). Se asume cámara nivelada: el horizonte es la fila yh, y los dos puntos de fuga están sobre esa fila.

Paso 4. Modelo de cámara (meas45.py y meas.py, clase Cam). Datos: cx = ancho/2 (más una sacudida en Monte Carlo), yh, x1 (punto de fuga izquierdo) y x2 (derecho).
- Distancia focal en píxeles: f = sqrt( max( -(x1 - cx) * (x2 - cx), 1 ) ). Supone que los dos puntos de fuga corresponden a direcciones perpendiculares y el punto principal está en la columna central.
- Direcciones en el plano del piso, en coordenadas (x, z): A = (x1 - cx, f) normalizado; B = (x2 - cx, f) normalizado.
- Un punto de la imagen (u, v) que está sobre un plano horizontal a altura H, con la cámara a altura hc, se lleva al plano: z = (hc - H) * f / (v - yh); x = (u - cx) / f * z. (Función P.)
- Dado un punto (u1, v1) y otra columna u2, la fila v2 del punto que está sobre la misma recta que se va al punto de fuga xv es: v2 = v1 + (yh - v1) * (u2 - u1) / (xv - u1). xv = x1 si la dirección es 'A' y x2 si es 'B'. (Función vline.)

Paso 5. Escala con referencias estándar. Se buscan dos puntos sobre la misma vertical: arriba un borde de mesón a Ht = 36 pulgadas y abajo la parte baja del gabinete o zócalo a Hb = 4,5 pulgadas. Con v_arriba = vt y v_abajo = vb (filas de la imagen), la altura de cámara se calcula con: hc = ( Ht * (vb - yh) - Hb * (vt - yh) ) / (vb - vt). (Función hc_ref.) Con varias verticales se promedia hc; hc_spread es la diferencia entre la mayor y la menor altura calculada.
Supuesto de unidades: todo queda en pulgadas porque Ht y Hb están en pulgadas. Si el diseñador usó otras alturas, todas las medidas escalan en la misma proporción.

Paso 6. Puntos a ojo. Se generan recortes ampliados con cuadrícula numerada en coordenadas de la imagen original (grid.py) y se leen a ojo las coordenadas de píxel de esquinas de isla, bordes de mesón, zócalo, pilastras, plafón y encimeras. Error asumido de lectura: +-2 px (+-2,5 px en la foto 4), ya incluido en los rangos. Es la mayor fuente de error.

Paso 7. Tipos de medida (meas45.py y meas.py, función run):
- 'lenVP' (u1, v1, H, u2, dir): largo sobre una recta que va hacia un punto de fuga, en un plano a altura H. Se calcula v2 con vline, se llevan ambos puntos al plano con P y se proyecta la diferencia sobre la dirección A o B. Resultado: valor absoluto del producto punto.
- 'len' (u1, v1, H1, u2, v2, H2, dir): largo entre dos puntos libres, cada uno con su altura, proyectado sobre A o B.
- 'vert' (v_arriba, v_piso): altura de una vertical que llega al piso. Fórmula: altura = hc * (vb - vt) / (vb - yh), con vb = fila del piso.

Paso 8. Incertidumbre por Monte Carlo (función run). N = 3000 repeticiones. En cada una se sacuden: cx con desviación 0,03 * ancho; x1 y x2 con desviación max(sd_x1, 3) y max(sd_x2, 3) píxeles; cada coordenada de píxel leída con desviación sig (2,0 por defecto; 2,5 en la foto 4); Ht con desviación 0,4 pulgadas; Hb con desviación 1,0 pulgada. Se reporta el valor base (sin sacudidas) y los percentiles 5 y 95. Generador aleatorio: np.random.default_rng(5), creado una sola vez al importar el módulo, de modo que los números dependen del orden de las llamadas a run.

Paso 9. Medir cada dimensión horizontal en DOS planos (altura del mesón, H=36, y base, H=4,5). Criterio registrado en el proyecto: si difieren más de 2 %, no se declara como medida limpia.

Paso 10. Etiquetas y criterios, tal como se aplicaron en los resultados:
- MEDIDA: coincide con una cota conocida (foto 4, ancho de isla, +2 % frente al plano) o es consistente entre referencias; en la altura al plafón de la foto 4 se llamó MEDIDA aunque solo tiene una referencia.
- MEDIDA CON RESERVA: los dos planos difieren 3 % (foto 5, largo de isla: 85,3 y 82,8 pulgadas), por encima del 2 % del método.
- CONFIANZA MEDIA: el resultado depende de la referencia elegida (foto 5, torre izquierda: 101,7 con solo la referencia de la isla y 104,9 con solo la de la encimera derecha).
- NO CONFIABLE: rango amplio (foto 4, pasillo +-22 %); los dos planos difieren 12 % y el punto de fuga izquierdo es débil con solo 30 líneas (foto 5, fondo de isla); o se supone una alineación que no se ve (foto 5, distancia a la encimera derecha).
- NO MEDIBLE: un extremo está tapado (foto 4, largo de isla, silla tapa el extremo izquierdo); solo sirve como cota inferior.
- La altura de cámara se entrega como dato auxiliar.
NOTA: los únicos criterios numéricos que quedaron en el proyecto son el 2 % entre planos y los rangos observados. Los umbrales "menos de 10 %" y "20 % o más" que aparecen en un borrador de Skill que redacté el 26/10 son una aproximación mía, NO un criterio registrado en el trabajo original.


### 3. RESULTADOS REALES (solo fotos 4 y 5; no hay datos de las fotos 1 a 3)

Los resultados son los que declaró Claude en el chat de origen. No hay verificación independiente ni medición física.

3.1 Identificación de las imágenes del paquete (solo por tamaño, según el chat de origen)
- Foto 1: 1450x751, cocina de madera clara con nevera empotrada e isla grande.
- Foto 2: 1584x1224, dibujo en líneas "Access by Cabinet Joint", con isla y dos taburetes.
- Foto 3: 2448x1584, dibujo en líneas "Chief Architect Five-12", con techo inclinado.
- Foto 4: 1143x621, cocina nocturna con isla de mesón claro, 4 sillas blancas, ventanas altas y vinoteca de vidrio a la derecha. Archivo usado: f4.jpg.
- Foto 5: 1441x1001, cocina blanca y gris con columna verde, nevera de acero, isla con tres colgantes, fondo de azulejo negro y blanco. Archivo usado: f5.jpg.
(El chat de origen no verificó que las imágenes sean exactamente las del paquete original; coinciden en tamaño.)

3.2 Salida de vps.py (versión A) sobre las fotos 4 y 5
4 {'w': 1143, 'h': 621, 'off': [0, 0], 'x1': -97.3, 'y1': 309.83, 'x2': 1134.78, 'y2': 309.57, 'yh': 309.7, 'n1': 177, 'n2': 189, 'f': 613.78, 'sd_x1': 0.62, 'sd_x2': 0.61, 'vdesv': -0.08}
5 {'w': 1441, 'h': 1001, 'off': [0, 0], 'x1': -758.99, 'y1': 445.36, 'x2': 3555.56, 'y2': 442.65, 'yh': 443.05, 'n1': 30, 'n2': 173, 'f': 2048.04, 'sd_x1': 4.59, 'sd_x2': 10.74, 'vdesv': -0.05}

3.3 Salida de meas45.py (unidades: pulgadas; formato valor base (percentil 5 a percentil 95))

=== FOTO 5 (cocina blanca, isla 3 colgantes)
hc 57.7 (56.3-59.1)
hc_spread 1.8 (0.2-4.4)
isla_largo_plano_mesón(H36) 85.3 (79.8-91.1)
isla_largo_plano_base(H4.5) 82.8 (80.2-85.4)
isla_fondo_plano_mesón(H36) 42.4 (39.3-45.7)
isla_fondo_plano_base(H4.5) 37.5 (35.6-39.4)
torre_izq_altura_hasta_puertas 103.3 (100.7-106.1)
pasillo_isla_a_frente_encimera_der(A) 101.0 (95.6-106.8)
solo ref isla:
hc 56.8 (55.2-58.5)
hc_spread 0.0 (0.0-0.0)
isla_largo_plano_mesón(H36) 81.7 (75.3-89.0)
torre_izq_altura_hasta_puertas 101.7 (98.6-105.0)
solo ref encimera der:
hc 58.6 (56.4-60.7)
hc_spread 0.0 (0.0-0.0)
isla_largo_plano_mesón(H36) 88.8 (80.0-97.6)
torre_izq_altura_hasta_puertas 104.9 (100.9-108.9)

=== FOTO 4 (cocina nocturna)
hc 61.2 (59.2-63.4)
hc_spread 1.4 (0.2-5.6)
isla_ancho_extremo(B,mesón H36) 73.5 (65.5-81.5)
isla_largo_COTA_INFERIOR(A,hasta x=300) 118.5 (107.2-131.2)
pasillo_isla_a_encimera_ventanas(B) 67.1 (52.7-82.8)
altura_plafón_pilastra_izq 114.5 (109.8-119.5)

(En el chat de origen la salida usaba el guion largo en los rangos; aquí se escribió con guion normal.)

3.4 Tabla final declarada (pulgadas; rango 5 a 95 %)

FOTO 4
- Ancho de isla (extremo, a nivel del mesón): 73,5 (65,5 a 81,5). MEDIDA. El plano reportado por el paquete da 72 pulgadas (+2 %); el plano no fue visto.
- Altura hasta el borde inferior del plafón de madera: 114,5 (109,8 a 119,5). MEDIDA, con una sola referencia.
- Largo de isla: mayor o igual a 118,5 (107 a 131). NO MEDIBLE (silla tapa el extremo izquierdo). El plano da 138.
- Pasillo entre la isla y la encimera de las ventanas: 67 (53 a 83). NO CONFIABLE (+-22 %).
- Altura de cámara: 61 (59 a 63). Dato auxiliar.

FOTO 5
- Largo de isla: 83 a 85 (80 a 91). MEDIDA CON RESERVA (planos 85,3 y 82,8; difieren 3 %).
- Altura de torre izquierda hasta las puertas, sin cornisa: 103 (101 a 106). CONFIANZA MEDIA (99 a 109 según la referencia usada).
- Fondo de isla: 37,5 a 42,4. NO CONFIABLE (planos difieren 12 %; punto de fuga izquierdo débil, 30 líneas).
- Distancia de la isla a la encimera derecha: 101 (96 a 107). NO CONFIABLE (supone alineación de las hileras).
- Altura de cámara: 57,7 (56 a 59). Dato auxiliar.

3.5 Puntos de píxel elegidos a ojo (copiados de meas45.py; las imágenes no están disponibles, no se pueden revisar)
Foto 5 (sig = 2,0), referencias de escala (v_arriba, v_abajo): (625, 901) y (544, 685).
- isla_largo_plano_mesón(H36): lenVP, (u1=520, v1=625, H=36, u2=1036, dir B)
- isla_largo_plano_base(H4.5): lenVP, (520, 901, 4.5, 1034, B)
- isla_fondo_plano_mesón(H36): lenVP, (520, 625, 36, 362, A)
- isla_fondo_plano_base(H4.5): lenVP, (520, 901, 4.5, 375, A)
- torre_izq_altura_hasta_puertas: vert, (v_arriba=140, v_piso=826)
- pasillo_isla_a_frente_encimera_der(A): len, (1034, 822, 4.5, 1042, 697, 4.5, A)
Foto 4 (sig = 2,5), referencias de escala (v_arriba, v_abajo): (400, 510) y (392, 498).
- isla_ancho_extremo(B,mesón H36): lenVP, (647, 478, 36, 828, B)
- isla_largo_COTA_INFERIOR(A,hasta x=300): lenVP, (647, 478, 36, 300, A)
- pasillo_isla_a_encimera_ventanas(B): len, (828, 416, 36, 830, 383, 36, B)
- altura_plafón_pilastra_izq: vert, (122, 525)

3.6 Comandos grid.py usados en el chat de origen (argumentos: foto salida x0 y0 x1 y1 escala paso)
- f4.jpg g4.png 0 0 1143 621 1 50
- f5.jpg g5.png 0 0 1441 1001 1 50
- f5.jpg g5a.png 340 580 640 960 3 20
- f5.jpg g5b.png 940 560 1240 740 4 20
- f4.jpg g4a.png 560 360 860 621 3 20
- f4.jpg g4b.png 0 60 420 540 2 20
- f4.jpg g4c.png 240 350 560 470 4 10
- f5.jpg g5c.png 960 780 1100 880 6 10
- f5.jpg g5d.png 1030 520 1230 570 6 10


### 4. CÓDIGO COMPLETO (versiones reales, sin omisiones)

Dónde está cada cosa: los archivos EJECUTADOS en el chat de origen son vp_nivelado.py, grid.py, vps.py (versión A) y meas45.py. Las versiones B (vps.py, meas.py, ejemplo.py) fueron escritas para una Skill y NUNCA se ejecutaron.
El texto del código viene de una transcripción recuperada del chat de origen (correo). No hay forma de confirmar byte a byte que no se perdiera algún carácter en esa transcripción.


### vp_nivelado.py

```python
import cv2, numpy as np, sys, json, random
random.seed(2); np.random.seed(2)
def segmentos(path,minfrac=0.02):
    im=cv2.imread(path); g=cv2.cvtColor(im,cv2.COLOR_BGR2GRAY); h,w=g.shape
    s=np.asarray(cv2.createLineSegmentDetector(cv2.LSD_REFINE_STD).detect(g)[0]).reshape(-1,4)
    L=np.hypot(s[:,2]-s[:,0],s[:,3]-s[:,1]); m=L>minfrac*w; return im,s[m],L[m],w,h
def ang_res(vp,s):
    mid=np.stack([(s[:,0]+s[:,2])/2,(s[:,1]+s[:,3])/2],1); d=np.stack([s[:,2]-s[:,0],s[:,3]-s[:,1]],1); d/=np.hypot(d[:,0],d[:,1])[:,None]
    t=vp-mid; t/=np.hypot(t[:,0],t[:,1])[:,None]; return np.degrees(np.arccos(np.clip(np.abs((d*t).sum(1)),0,1)))
def clusters(s,L,w,th=1.0,it=3000):
    ang=np.degrees(np.arctan2(s[:,3]-s[:,1],s[:,2]-s[:,0]))%180
    idx=np.where((np.abs(ang-90)>6)&(np.abs(ang%180)>1.5)&(np.abs(ang-180)>1.5))[0]
    ls=np.array([np.cross([s[i,0],s[i,1],1.],[s[i,2],s[i,3],1.]) for i in range(len(s))])
    out=[]; rest=list(idx)
    for _ in range(2):
        best=(0,None,[])
        for _ in range(it):
            if len(rest)<2: break
            i,j=random.sample(rest,2); vp=np.cross(ls[i],ls[j])
            if abs(vp[2])<1e-9: continue
            v=vp[:2]/vp[2]; r=ang_res(v,s[rest]); inl=[rest[k] for k in np.where(r<th)[0]]; sc=L[inl].sum()
            if sc>best[0]: best=(sc,v,inl)
        out.append(best); rest=[k for k in rest if k not in set(best[2])]
    return out,ang
def mejor_x(s,L,idx,yh,xs):
    best=(1e18,None)
    for x in xs:
        r=ang_res(np.array([x,yh]),s[idx]); c=(L[idx]*np.minimum(r,3.0)**2).sum()
        if c<best[0]: best=(c,x)
    return best
def analizar(path,yh_fijo=None):
    im,s,L,w,h=segmentos(path); cl,ang=clusters(s,L,w)
    (_,v1,i1),(_,v2,i2)=[c for c in cl]
    if v1[0]>v2[0]: (v1,i1),(v2,i2)=(v2,i2),(v1,i1)
    vert=np.abs(ang-90)<3; vd=float(np.average(ang[vert]-90,weights=L[vert])) if vert.any() else None
    print(f"{path.split('/')[-1][:8]} {w}x{h} seg={len(s)} inl={len(i1)},{len(i2)} VP libres={v1.round(0)},{v2.round(0)} verticales desv={vd:.3f}°")
    rows=[]
    for yh in range(int(h*0.25),int(h*0.75),10):
        c1,x1=mejor_x(s,L,i1,yh,np.arange(-8*w,-0.3*w,0.02*w)) if v1[0]<w/2 else mejor_x(s,L,i1,yh,np.arange(0.3*w,8*w,0.02*w))
        c2,x2=mejor_x(s,L,i2,yh,np.arange(w*1.3,8*w,0.02*w)) if v2[0]>w/2 else mejor_x(s,L,i2,yh,np.arange(-8*w,-0.3*w,0.02*w))
        rows.append((yh,c1+c2,x1,x2))
    rows=np.array(rows); k=np.argmin(rows[:,1]); yh,c,x1,x2=rows[k]
    f2=-((x1-w/2)*(x2-w/2)+(yh-h/2)**2)
    print(f"  horizonte nivelado óptimo yh={yh:.0f} (centro={h/2:.0f}); VP=({x1:.0f},{yh:.0f}),({x2:.0f},{yh:.0f}); f^2={f2:.0f} f={np.sqrt(f2) if f2>0 else float('nan'):.0f}px")
    # coste con yh forzado al centro (cámara sin cabeceo)
    kc=np.argmin(np.abs(rows[:,0]-h/2)); print(f"  coste en yh óptimo={rows[k,1]:.0f}; coste con yh=centro={rows[kc,1]:.0f}")
    return dict(yh=float(yh),x1=float(x1),x2=float(x2),f=float(np.sqrt(f2)) if f2>0 else None,w=w,h=h)
if __name__=='__main__':
    for p in sys.argv[1:]: print(json.dumps(analizar(p)))
```



### grid.py

```python
import cv2,sys
p,out,x0,y0,x1,y1,sc,step=sys.argv[1],sys.argv[2],*map(int,sys.argv[3:9])
im=cv2.imread(p); c=im[y0:y1,x0:x1].copy(); c=cv2.resize(c,None,fx=sc,fy=sc,interpolation=cv2.INTER_CUBIC)
for x in range((x0//step+1)*step,x1,step):
    X=int((x-x0)*sc); cv2.line(c,(X,0),(X,c.shape[0]),(0,0,255),1); cv2.putText(c,str(x),(X+2,12),cv2.FONT_HERSHEY_SIMPLEX,0.4,(0,0,255),1)
for y in range((y0//step+1)*step,y1,step):
    Y=int((y-y0)*sc); cv2.line(c,(0,Y),(c.shape[1],Y),(255,0,0),1); cv2.putText(c,str(y),(2,Y-2),cv2.FONT_HERSHEY_SIMPLEX,0.4,(255,0,0),1)
cv2.imwrite(out,c)
```



### vps.py (VERSIÓN A, EJECUTADA en el chat de origen; uso: python3 -I vps.py CARPETA, lee CARPETA/f4.jpg y CARPETA/f5.jpg y escribe CARPETA/vps.json)

```python
import sys,json,numpy as np,cv2,random
sys.path.insert(0,sys.argv[1]); import vp_nivelado as V
out={}
for k in ('4','5'):
    p=f'{sys.argv[1]}/f{k}.jpg'; random.seed(2); np.random.seed(2)
    im,s,L,w,h=V.segmentos(p); cl,ang=V.clusters(s,L,w)
    (_,v1,i1),(_,v2,i2)=cl
    if v1[0]>v2[0]: (v1,i1),(v2,i2)=(v2,i2),(v1,i1)
    c=np.array([w/2,h/2]);K=1000.
    def refine(idx):
        M=np.zeros((3,3))
        for i in idx:
            a=np.array([(s[i,0]-c[0])/K,(s[i,1]-c[1])/K,1.]);b=np.array([(s[i,2]-c[0])/K,(s[i,3]-c[1])/K,1.])
            l=np.cross(a,b);l/=np.hypot(l[0],l[1]);M+=L[i]*np.outer(l,l)
        v=np.linalg.eigh(M)[1][:,0];return np.array([v[0]/v[2]*K+c[0],v[1]/v[2]*K+c[1]])
    r1=refine(i1);r2=refine(i2)
    bs=np.array([(*refine(list(np.random.choice(i1,len(i1)))),*refine(list(np.random.choice(i2,len(i2))))) for _ in range(150)])
    yh=(r1[1]*len(i1)+r2[1]*len(i2))/(len(i1)+len(i2))
    f=np.sqrt(max(-(r1[0]-w/2)*(r2[0]-w/2),1))
    vert=np.abs(ang-90)<3
    d=dict(w=w,h=h,off=[0,0],x1=float(r1[0]),y1=float(r1[1]),x2=float(r2[0]),y2=float(r2[1]),yh=float(yh),n1=len(i1),n2=len(i2),f=float(f),sd_x1=float(bs[:,0].std()),sd_x2=float(bs[:,2].std()),vdesv=float(np.average(ang[vert]-90,weights=L[vert])))
    out[k]=d;print(k,{a:(round(b,2) if isinstance(b,float) else b) for a,b in d.items()})
json.dump(out,open(sys.argv[1]+'/vps.json','w'),indent=1)
```



### meas45.py (EJECUTADO en el chat de origen; la ruta de vps.json es la de la carpeta temporal de aquel entorno y hay que cambiarla por la ruta propia)

```python
import numpy as np, json, sys
rng=np.random.default_rng(5)
VP=json.load(open('RUTA_LOCAL_DEL_PROYECTO/vps.json'))
class Cam:
    def __init__(s,k,cxs=0.,j1=0.,j2=0.):
        d=VP[k]; s.cx=d['w']/2+cxs; s.yh=d['yh']; s.x1=d['x1']+j1; s.x2=d['x2']+j2
        s.f=np.sqrt(max(-(s.x1-s.cx)*(s.x2-s.cx),1))
        A=np.array([s.x1-s.cx,s.f]); B=np.array([s.x2-s.cx,s.f]); s.A=A/np.linalg.norm(A); s.B=B/np.linalg.norm(B)
    def P(s,u,v,H,hc): z=(hc-H)*s.f/(v-s.yh); return np.array([(u-s.cx)/s.f*z,z])
    def vline(s,u1,v1,u2,dr): xv=s.x1 if dr=='A' else s.x2; return v1+(s.yh-v1)*(u2-u1)/(xv-u1)
def hc_ref(c,vt,vb,Ht,Hb): return (Ht*(vb-c.yh)-Hb*(vt-c.yh))/(vb-vt)
def run(k,refs,meas,N=3000,sig=2.0):
    def one(jit):
        g=(lambda s:rng.normal(0,s)) if jit else (lambda s:0.0)
        d=VP[k]; c=Cam(k,g(0.03*d['w']),g(max(d['sd_x1'],3)),g(max(d['sd_x2'],3)))
        hl=[hc_ref(c,vt+g(sig),vb+g(sig),36+g(.4),4.5+g(1.0)) for (vt,vb) in refs]; hc=np.mean(hl); o={'hc':hc,'hc_spread':max(hl)-min(hl)}
        for n,(t,a) in meas.items():
            if t=='lenVP': # (u1,v1,H,u2,dir): mismo plano, 2.º punto sobre la recta al punto de fuga
                u1,v1,H,u2,dr=a; u1+=g(sig);v1+=g(sig);u2+=g(sig); v2=c.vline(u1,v1,u2,dr)
                o[n]=abs((c.P(u2,v2,H,hc)-c.P(u1,v1,H,hc))@(c.A if dr=='A' else c.B))
            elif t=='len': # (u1,v1,H1,u2,v2,H2,dir)
                u1,v1,H1,u2,v2,H2,dr=a
                o[n]=abs((c.P(u2+g(sig),v2+g(sig),H2,hc)-c.P(u1+g(sig),v1+g(sig),H1,hc))@(c.A if dr=='A' else c.B))
            elif t=='vert': # (vt,vb) vb en el piso
                vt,vb=a; o[n]=hc*((vb+g(sig))-(vt+g(sig)))/((vb+g(sig))-c.yh)
        return o
    base=one(False); S=[one(True) for _ in range(N)]; out={}
    for n in base:
        a=np.array([s[n] for s in S]); out[n]=(base[n],np.percentile(a,5),np.percentile(a,95))
    return out
def show(o):
    for n,v in o.items(): print(f"{n:44s}{v[0]:8.1f} ({v[1]:.1f}–{v[2]:.1f})")
print('=== FOTO 5 (cocina blanca, isla 3 colgantes)')
m5={
 'isla_largo_plano_mesón(H36)':('lenVP',(520,625,36,1036,'B')),
 'isla_largo_plano_base(H4.5)':('lenVP',(520,901,4.5,1034,'B')),
 'isla_fondo_plano_mesón(H36)':('lenVP',(520,625,36,362,'A')),
 'isla_fondo_plano_base(H4.5)':('lenVP',(520,901,4.5,375,'A')),
 'torre_izq_altura_hasta_puertas':('vert',(140,826)),
 'pasillo_isla_a_frente_encimera_der(A)':('len',(1034,822,4.5,1042,697,4.5,'A')),
}
show(run('5',[(625,901),(544,685)],m5))
print('solo ref isla:'); show(run('5',[(625,901)],{'isla_largo_plano_mesón(H36)':m5['isla_largo_plano_mesón(H36)'],'torre_izq_altura_hasta_puertas':m5['torre_izq_altura_hasta_puertas']}))
print('solo ref encimera der:'); show(run('5',[(544,685)],{'isla_largo_plano_mesón(H36)':m5['isla_largo_plano_mesón(H36)'],'torre_izq_altura_hasta_puertas':m5['torre_izq_altura_hasta_puertas']}))
print('\n=== FOTO 4 (cocina nocturna)')
m4={
 'isla_ancho_extremo(B,mesón H36)':('lenVP',(647,478,36,828,'B')),
 'isla_largo_COTA_INFERIOR(A,hasta x=300)':('lenVP',(647,478,36,300,'A')),
 'pasillo_isla_a_encimera_ventanas(B)':('len',(828,416,36,830,383,36,'B')),
 'altura_plafón_pilastra_izq':('vert',(122,525)),
}
show(run('4',[(400,510),(392,498)],m4,sig=2.5))
```



### vps.py (VERSIÓN B, CONSOLIDADA, NUNCA EJECUTADA; uso previsto: python3 -I vps.py salida.json clave=foto.jpg [clave=foto2.jpg ...])

```python
# uso: python3 -I vps.py salida.json clave=foto.jpg [clave=foto2.jpg ...] (vp_nivelado.py en la misma carpeta)
import sys,os,json,random,numpy as np,cv2
sys.path.insert(0,os.path.dirname(os.path.abspath(__file__))); import vp_nivelado as V
out={}
for arg in sys.argv[2:]:
    k,p=arg.split('=',1); random.seed(2); np.random.seed(2)
    im,s,L,w,h=V.segmentos(p); cl,ang=V.clusters(s,L,w)
    (_,v1,i1),(_,v2,i2)=cl
    if v1[0]>v2[0]: (v1,i1),(v2,i2)=(v2,i2),(v1,i1)
    c=np.array([w/2,h/2]);K=1000.
    def refine(idx): # punto de fuga por mínimos cuadrados ponderados por largo
        M=np.zeros((3,3))
        for i in idx:
            a=np.array([(s[i,0]-c[0])/K,(s[i,1]-c[1])/K,1.]);b=np.array([(s[i,2]-c[0])/K,(s[i,3]-c[1])/K,1.])
            l=np.cross(a,b);l/=np.hypot(l[0],l[1]);M+=L[i]*np.outer(l,l)
        v=np.linalg.eigh(M)[1][:,0];return np.array([v[0]/v[2]*K+c[0],v[1]/v[2]*K+c[1]])
    r1=refine(i1);r2=refine(i2)
    bs=np.array([(*refine(list(np.random.choice(i1,len(i1)))),*refine(list(np.random.choice(i2,len(i2))))) for _ in range(150)])
    yh=(r1[1]*len(i1)+r2[1]*len(i2))/(len(i1)+len(i2))
    f=np.sqrt(max(-(r1[0]-w/2)*(r2[0]-w/2),1)); vert=np.abs(ang-90)<3
    d=dict(w=w,h=h,off=[0,0],x1=float(r1[0]),y1=float(r1[1]),x2=float(r2[0]),y2=float(r2[1]),yh=float(yh),n1=len(i1),n2=len(i2),f=float(f),
           sd_x1=float(bs[:,0].std()),sd_x2=float(bs[:,2].std()),vdesv=float(np.average(ang[vert]-90,weights=L[vert])) if vert.any() else None)
    out[k]=d; print(k,{a:(round(b,2) if isinstance(b,float) else b) for a,b in d.items()})
json.dump(out,open(sys.argv[1],'w'),indent=1)
```



### meas.py (CONSOLIDADO, NUNCA EJECUTADO; uso previsto: VPJSON=vps.json python3 -I mi_medicion.py)

```python
# Biblioteca de medición. Uso: VPJSON=vps.json python3 -I mi_medicion.py (ver ejemplo en SKILL.md)
import os,json,numpy as np
rng=np.random.default_rng(5)
VP=json.load(open(os.environ['VPJSON']))
class Cam:
    def __init__(s,k,cxs=0.,j1=0.,j2=0.):
        d=VP[k]; s.cx=d['w']/2+cxs; s.yh=d['yh']; s.x1=d['x1']+j1; s.x2=d['x2']+j2
        s.f=np.sqrt(max(-(s.x1-s.cx)*(s.x2-s.cx),1))
        A=np.array([s.x1-s.cx,s.f]); B=np.array([s.x2-s.cx,s.f]); s.A=A/np.linalg.norm(A); s.B=B/np.linalg.norm(B)
    def P(s,u,v,H,hc): z=(hc-H)*s.f/(v-s.yh); return np.array([(u-s.cx)/s.f*z,z]) # (x,z) en el plano horizontal a altura H
    def vline(s,u1,v1,u2,dr): xv=s.x1 if dr=='A' else s.x2; return v1+(s.yh-v1)*(u2-u1)/(xv-u1)
def hc_ref(c,vt,vb,Ht,Hb): return (Ht*(vb-c.yh)-Hb*(vt-c.yh))/(vb-vt) # altura de cámara con dos puntos sobre la misma vertical
def run(k,refs,meas,N=3000,sig=2.0,Ht=36.,Hb=4.5,sdHt=.4,sdHb=1.0):
    """refs: lista de (v_arriba,v_abajo) de una vertical con arriba a Ht (mesón) y abajo a Hb (zócalo/parte baja del gabinete).
    meas: nombre -> ('lenVP',(u1,v1,H,u2,dir)) | ('len',(u1,v1,H1,u2,v2,H2,dir)) | ('vert',(v_arriba,v_piso)). dir 'A'=hacia VP izq., 'B'=hacia VP der."""
    def one(jit):
        g=(lambda s:rng.normal(0,s)) if jit else (lambda s:0.0)
        d=VP[k]; c=Cam(k,g(0.03*d['w']),g(max(d['sd_x1'],3)),g(max(d['sd_x2'],3)))
        hl=[hc_ref(c,vt+g(sig),vb+g(sig),Ht+g(sdHt),Hb+g(sdHb)) for (vt,vb) in refs]; hc=np.mean(hl); o={'hc':hc,'hc_spread':max(hl)-min(hl)}
        for n,(t,a) in meas.items():
            if t=='lenVP':
                u1,v1,H,u2,dr=a; u1+=g(sig);v1+=g(sig);u2+=g(sig); v2=c.vline(u1,v1,u2,dr)
                o[n]=abs((c.P(u2,v2,H,hc)-c.P(u1,v1,H,hc))@(c.A if dr=='A' else c.B))
            elif t=='len':
                u1,v1,H1,u2,v2,H2,dr=a
                o[n]=abs((c.P(u2+g(sig),v2+g(sig),H2,hc)-c.P(u1+g(sig),v1+g(sig),H1,hc))@(c.A if dr=='A' else c.B))
            elif t=='vert':
                vt,vb=a; o[n]=hc*((vb+g(sig))-(vt+g(sig)))/((vb+g(sig))-c.yh)
        return o
    base=one(False); S=[one(True) for _ in range(N)]; out={}
    for n in base:
        a=np.array([s[n] for s in S]); out[n]=(base[n],np.percentile(a,5),np.percentile(a,95))
    return out
def show(o):
    for n,v in o.items(): print(f"{n:44s}{v[0]:8.1f} ({v[1]:.1f}–{v[2]:.1f})")
```



### ejemplo.py (CONSOLIDADO, NUNCA EJECUTADO; se corre con meas.py y vps.json en la misma carpeta)

```python
from meas import *
m5={'isla_largo_plano_mesón(H36)':('lenVP',(520,625,36,1036,'B')),'isla_largo_plano_base(H4.5)':('lenVP',(520,901,4.5,1034,'B')),
    'torre_izq_altura_hasta_puertas':('vert',(140,826))}
show(run('5',[(625,901),(544,685)],m5))
```



### 5. DEPENDENCIAS, COMANDOS, DATOS DE ENTRADA, EJEMPLO Y SALIDAS ESPERADAS


5.1 Dependencias
- Python 3 (se invocaba como python3 -I). Versión exacta: NO REGISTRADA.
- numpy. Versión: NO REGISTRADA.
- OpenCV (import cv2). Versión: NO REGISTRADA. El código usa cv2.createLineSegmentDetector y cv2.LSD_REFINE_STD, así que hace falta una versión de OpenCV que los incluya. Aviso: el detector LSD no estuvo disponible en algunas versiones de OpenCV; no se comprobó con qué versión se hicieron las pruebas.
- Biblioteca estándar: sys, json, random, os.
- Comando de instalación: NO REGISTRADO en el proyecto. Sugerencia mía, no probada aquí: pip install numpy opencv-python

5.2 Datos de entrada
- f4.jpg (foto 4, 1143x621) y f5.jpg (foto 5, 1441x1001), en la misma carpeta que vp_nivelado.py. Las imágenes no están disponibles en este material.
- Alturas de referencia: Ht = 36 pulgadas (mesón), Hb = 4,5 pulgadas (zócalo o parte baja del gabinete). Supuesto, no medido.

5.3 Ejecución registrada en el chat de origen (con S = carpeta de trabajo)
1) Guardar vp_nivelado.py, grid.py, vps.py (versión A) y meas45.py en S. Copiar las fotos como S/f4.jpg y S/f5.jpg.
2) python3 -I S/vps.py S    (genera S/vps.json e imprime la salida de la sección 3.2)
3) python3 -I S/grid.py S/f4.jpg S/g4.png 0 0 1143 621 1 50    (y los demás recortes de la sección 3.6, para leer coordenadas a ojo)
4) Editar en meas45.py la ruta de vps.json para que apunte a S/vps.json.
5) python3 -I S/meas45.py    (imprime la salida de la sección 3.3)
Verificación de comportamiento: la salida de vps.py y meas45.py sobre las imágenes originales debería coincidir con las secciones 3.2 y 3.3. No se ha podido comprobar aquí (sin imágenes). Las cifras pueden variar un poco si la versión de OpenCV es otra, porque la detección de líneas LSD podría cambiar.

5.4 Ejecución prevista con las versiones B (NO PROBADA)
- python3 -I vps.py vps.json 4=f4.jpg 5=f5.jpg
- VPJSON=vps.json python3 -I ejemplo.py
Salida esperada de ejemplo.py: no registrada. Si las versiones B son equivalentes a las A, debería reproducir las líneas de la foto 5 que corresponden a isla_largo_plano_mesón, isla_largo_plano_base y torre_izq_altura_hasta_puertas, pero el valor exacto depende del orden de llamadas del generador aleatorio de meas.py (ejemplo.py solo llama a run una vez, a diferencia de meas45.py). Es una expectativa, no un resultado.

5.5 Información no registrada (resumen)
- Versiones de Python, OpenCV y NumPy; comando de instalación original; hash o fecha del vp_nivelado.py usado; salida de vp_nivelado.py por sí solo; qué rasgos exactos de la foto 4 usan las referencias (400,510) y (392,498).


### 6. QUÉ SE PROBÓ, QUÉ NO, LIMITACIONES Y PENDIENTES


6.1 Se ejecutó realmente (en el chat de origen, el 26/10/2026, según el correo)
- vp_nivelado.py (importado por vps.py), vps.py versión A, grid.py y meas45.py, sobre las fotos 4 y 5. Salidas en las secciones 3.2 y 3.3.
- Se identificaron las 5 imágenes por tamaño en píxeles.

6.2 NO se ejecutó
- vps.py versión B, meas.py y ejemplo.py: nunca corrieron (error de tiempo agotado en la herramienta). Quedan como código sin probar.
- En esta conversación (27/10/2026) no se ejecutó nada, porque no hay imágenes.
- No se corrió vp_nivelado.py como script independiente ni su función analizar.
- Las fotos 1 a 3 no se midieron en este material.

6.3 Limitaciones
- Validación débil: el método solo se comparó con una cota conocida en una cocina (foto 4, ancho de isla: 73,5 contra 72 pulgadas del plano, no visto). En las demás medidas no hay respuesta con la que compararlas.
- Los puntos se eligen a ojo sobre recortes con cuadrícula; es la mayor fuente de error.
- La escala depende de que el mesón mida 36 pulgadas y el zócalo 4,5. Si no, todo escala igual.
- El punto de fuga de la foto 5 izquierdo es débil (30 líneas) y el derecho tiene incertidumbre mayor (sd 10,74 px).
- En la foto 4 el apoyo de altura de cámara usó una sola pareja de referencias en un solo plano; en la foto 5 las dos referencias difirieron 3 %.
- Se asume cámara nivelada, dos puntos de fuga ortogonales y punto principal centrado. No se modela distorsión de lente.
- Los resultados son los declarados por Claude, sin medición física. Sirven para cotizar y planear, no para cortar material.
- Los números pueden cambiar si la versión de OpenCV es otra.

6.4 Pendientes
1) Conseguir las imágenes de las fotos 4 y 5 (y las 1 a 3) para reejecutar y verificar.
2) Ejecutar de verdad las versiones B (vps.py, meas.py, ejemplo.py) y comprobar que reproducen los números de 3.2 y 3.3.
3) Recuperar los resultados de las fotos 1 a 3 del chat anterior; no están en este material.
4) Armar la tabla única de las fotos 1 a 5 (opcional, solo posible con los datos de 3).
5) Registrar versiones de Python, OpenCV y NumPy.
6) Decidir umbrales numéricos claros para MEDIDA, CON RESERVA y NO CONFIABLE (hoy solo está el 2 % entre planos).
7) Confirmar que la Skill "medidas-desde-fotos" propuesta el 26/10 se guardó (no se pudo comprobar desde aquí); cualquier umbral numérico que contenga además del 2 % es una aproximación mía.
8) Declarar siempre los límites de 6.3 al entregar medidas.

FIN DE LA ENTREGA (INCOMPLETA: faltan imágenes, fotos 1 a 3, plano de la foto 4, versiones de dependencias y ejecución de las versiones B).


## Código nuevo: prueba_ficticia.py

Escrito para este ensayo. No es código original recuperado.

```python
"""Prueba nueva y sintética. No usa ni certifica fotos de cocinas reales.
Ejecutar dentro de la carpeta que contiene los scripts de la versión B.
"""
from pathlib import Path
import json, math, os, subprocess, sys
import cv2
import numpy as np

BASE = Path(__file__).resolve().parent
os.chdir(BASE)

def ejecutar(args, entorno=None):
    p = subprocess.run(args, text=True, capture_output=True, env=entorno)
    print('COMANDO:', ' '.join(map(str, args)))
    print('CODIGO_SALIDA:', p.returncode)
    print(p.stdout)
    if p.stderr:
        print('STDERR:', p.stderr)
    return p

# Cámara y geometría inventadas, conocidas por construcción.
d = dict(w=1000, h=800, off=[0, 0], x1=-500., y1=200.,
         x2=1500., y2=200., yh=200., n1=0, n2=0,
         f=1000., sd_x1=0., sd_x2=0., vdesv=0.)
Path('sintetico.json').write_text(json.dumps({'sintetica': d}, indent=2))
os.environ['VPJSON'] = str(BASE/'sintetico.json')
sys.path.insert(0, str(BASE))
from meas import Cam, hc_ref, run
c = Cam('sintetica')
hc = 60.

def proyectar(x, z, altura):
    return (500 + 1000*x/z, 200 + 1000*(hc-altura)/z)

vt = proyectar(0, 100, 36)[1]
vb = proyectar(0, 100, 4.5)[1]
a = proyectar(-10, 100, 36)
b = proyectar(-10+40/math.sqrt(2), 100+40/math.sqrt(2), 36)
piso = proyectar(0, 100, 0)[1]
alto = proyectar(0, 100, 30)[1]
medidas = {
    'largo_ficticio_40_pulgadas': ('lenVP', (*a, 36, b[0], 'B')),
    'altura_ficticia_30_pulgadas': ('vert', (alto, piso))
}
o = run('sintetica', [(vt, vb)], medidas, N=3000)
assert abs(hc_ref(c, vt, vb, 36, 4.5)-60) < 1e-9
assert abs(c.vline(*a, b[0], 'B')-b[1]) < 1e-9
assert abs(o['hc'][0]-60) < 1e-9
assert abs(o['largo_ficticio_40_pulgadas'][0]-40) < 1e-9
assert abs(o['altura_ficticia_30_pulgadas'][0]-30) < 1e-9
assert all(np.isfinite(v).all() for v in o.values())
print('ENTRADAS FICTICIAS:', dict(refs=[(vt,vb)], inicio=a, fin=b, alto=alto, piso=piso))
print('RESULTADOS SINTETICOS:', json.dumps(o, indent=2))
print('APROBADO: valores base 60, 40 y 30 pulgadas, tolerancia 1e-9.')
# La tolerancia anterior comprueba aritmética sintética, no precisión física.
# Los intervalos de Monte Carlo dependen de las perturbaciones del original.

imagen = np.full((800, 1000, 3), 255, dtype=np.uint8)
# Dos familias de rectas proyectadas, más verticales. No es una cocina.
for vp in [(-500, 200), (1500, 200)]:
    for x in range(-300, 1301, 80):
        ok, p1, p2 = cv2.clipLine((0,0,1000,800), vp, (x, 1200))
        if ok:
            cv2.line(imagen, p1, p2, (0,0,0), 2, cv2.LINE_AA)
for x in range(100, 1000, 150):
    cv2.line(imagen, (x,250), (x,760), (80,80,80), 2)
assert cv2.imwrite('escena_ficticia.png', imagen)
p = ejecutar([sys.executable, '-I', 'vps.py', 'detectados.json', 'sintetica=escena_ficticia.png'])
if p.returncode == 0:
    det = json.loads(Path('detectados.json').read_text())['sintetica']
    print('ERRORES DETECCION (px):', {k:det[k]-d[k] for k in ['x1','x2','yh']})
else:
    print('DETECCION NO COMPLETADA: no sustituir por los datos ideales.')
p = ejecutar([sys.executable, '-I', 'grid.py', 'escena_ficticia.png',
              'cuadricula_ficticia.png', '0','0','1000','800','1','100'])
if p.returncode == 0:
    assert cv2.imread('cuadricula_ficticia.png').shape == (800,1000,3)
    print('CUADRICULA: dimensiones comprobadas 1000x800.')
print('FIN PRUEBA FICTICIA. No valida cocinas, escala real ni exactitud física.')
```

## Registro de la ejecución ficticia

Las rutas locales del intérprete se presentan como `python`; no cambia los resultados.

```text
ENTRADAS FICTICIAS: {'refs': [(440.0, 755.0)], 'inicio': (400.0, 440.0), 'fin': (642.5293301326966, 387.08450978922986), 'alto': 500.0, 'piso': 800.0}
RESULTADOS SINTETICOS: {
  "hc": [
    60.0,
    58.22538884642014,
    61.78084505622637
  ],
  "hc_spread": [
    0.0,
    0.0,
    0.0
  ],
  "largo_ficticio_40_pulgadas": [
    39.99999999999997,
    36.782112141496555,
    43.29739276127198
  ],
  "altura_ficticia_30_pulgadas": [
    30.0,
    28.994919082117004,
    31.01864156354773
  ]
}
APROBADO: valores base 60, 40 y 30 pulgadas, tolerancia 1e-9.
COMANDO: python -I vps.py detectados.json sintetica=escena_ficticia.png
CODIGO_SALIDA: 0
sintetica {'w': 1000, 'h': 800, 'off': [0, 0], 'x1': -494.24, 'y1': 202.48, 'x2': 1491.89, 'y2': 204.16, 'yh': 203.27, 'n1': 72, 'n2': 64, 'f': 993.06, 'sd_x1': 2.85, 'sd_x2': 3.07, 'vdesv': 0.01}

ERRORES DETECCION (px): {'x1': 5.75787130621336, 'x2': -8.113245754841046, 'yh': 3.268051295519342}
COMANDO: python -I grid.py escena_ficticia.png cuadricula_ficticia.png 0 0 1000 800 1 100
CODIGO_SALIDA: 0

CUADRICULA: dimensiones comprobadas 1000x800.
FIN PRUEBA FICTICIA. No valida cocinas, escala real ni exactitud física.
```

## Integridad de los bloques de esta copia pública

Hashes SHA-256 del texto UTF-8 del bloque sin el salto final añadido al guardarlo. Sirven para comprobar la extracción de esta copia pública, no la identidad con archivos originales perdidos. En meas45.py se reemplazó únicamente una ruta temporal del entorno de origen por RUTA_LOCAL_DEL_PROYECTO/vps.json; no se cambió el algoritmo. Sustituye esa ruta por la ubicación de tu propio JSON antes de ejecutarlo.

- `vp_nivelado.py` (A/común): `6d723a4444109c405f28031f47a0037da18f38f149df3ae478c6e60df473a5df`.
- `grid.py` (A/común): `1dca878f41df4fccf01fb0e77c2efd6ef2d86548ba6870e731e6affdf480c98d`.
- `vps.py` (A/común): `c3bc1416c5ce2ed38247eff52d1ca2dce42d26907d31fffa911c0a7843d32c1b`.
- `meas45.py` (A/común): `1d1a59a265687bd733cf27e6a9e014fdaf05946151bc604d401eda06f08f4afb`.
- `vps.py` (B): `648e3e1366033e9562a9820a85c8780a3bb29119af4c458784318ecef10c98e3`.
- `meas.py` (B): `92767a0c177d92b1c1f715edd5b6ba6e4fe923dc4c9b67da7994ed671766e424`.
- `ejemplo.py` (B): `479cff84731fd16c926e7d01904d299ed2c752b420e13fd3ec5fdfa198f7405d`.

FIN DEL DOCUMENTO
