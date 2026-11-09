# Aurelia 1.3
<img width="1280" height="760" alt="aurelia-1 2-a" src="https://github.com/user-attachments/assets/5239b581-5bd1-488a-ae7d-42c7d3c31e18" />


Secuenciador generativo MIDI de 4 voces para REAPER (JSFX). Cubre estocástica, serial, fractal, de autómatas, espacial, euclídea, de restricciones e interválica: 14 motores en total.

## Voces y modos

Cada voz sale por su propio canal MIDI (MIDI OUT) y tiene uno de estos catorce modos. El mando **TIPO** de cada modo nuevo elige el submodelo.

- **ESTOCÁSTICO**: nubes de notas como en la música estocástica de Xenakis. Los ataques siguen un proceso de Poisson (intervalos exponenciales) y las alturas, una ley de probabilidad: uniforme, lineal de intervalos de Xenakis, Gauss, Cauchy, logística, coseno hiperbólico, arcoseno o exponencial. Tiene deriva de densidad y de registro, paseo aleatorio, microtonos, nubes de varias notas, glissandi y secciones.
- **GENDYN**: la síntesis dinámica estocástica de Xenakis llevada a escala de notas. Un polígono de N puntos, en el que cada punto es una nota y cada tramo su duración, se transforma en cada ciclo:
  - **ORDEN 1** son paseos aleatorios simples (S.709).
  - **ORDEN 2** son paseos de segundo orden, como en GENDY3.
  - Tiene **barreras reflectantes**, las seis **leyes** de Gendy (lineal, Cauchy, logística, coseno hiperbólico, arcoseno, exponencial) y periodo libre o fijo.
  - **GLISS** interpola entre puntos con pitch bend, como la forma de onda poligonal.
  - **SECC/ACTIVO** reproduce la arquitectura de secciones que suenan o callan.
- **TURING**: Turing Machine de Music Thing.
  - Registro de 16 bits y DAC de 8 bits.
  - El mando **BLOQUEO**: arriba, bucle fijo; centro, azar; abajo, bucle invertido al doble de largo.
  - Longitud de 2 a 16 y salida de gates por bit, como el expansor Pulses.
- **BLOOM**: secuenciador fractal de Qu-Bit.
  - Un **tronco** de hasta 32 pasos genera hasta 7 **ramas**. Cada rama deriva de la anterior por transposición, inversión, retrógrado o mutación.
  - **CAMINO** elige uno de los 128 recorridos del árbol.
  - **MUTAR** reescribe el tronco al final de cada vuelta.
  - **ORDEN**: adelante, atrás, péndulo o azar.
  - Silencios, ratchets y slides por paso.
  - **NUEVO** crea un tronco nuevo y **DESH.** recupera el anterior.

- **ANALOGIQUE A**: las cadenas de Markov de Xenakis en *Analogique A* (1958), según las reconstrucciones de Di Scipio y Carvalho:
  - Cada **pantalla** (A–H) combina altura (f0/f1), dinámica (g0/g1) y densidad (d0/d1), y dura **ΔT**.
  - Las alturas salen de las seis regiones de la obra (de E1 a A6). Las densidades son de 1, 3 o 9 sucesos por celda.
  - Los ataques caen en quintillos, cuatrillos o tresillos de la pantalla.
  - **MODELO GTPM** usa la matriz 8×8 de la obra. **ACOPLADO** usa las tres cadenas de dos estados con las matrices alfa/beta, que se eligen según el estado de otro parámetro.
  - **PROTOC.** encadena secciones de 30 pantallas, en equilibrio o perturbadas hacia una pantalla. **PERTURB**/**DESTINO** permite perturbar a mano.
- **SUBSEQ**, a partir de la librería *Subsequence*:
  - Un grafo de acordes con pesos (**FUNC.**, **MODAL**, **CROMÁT.** con P/L/R, **PEDAL**) y **GRAVED.** hacia la tónica.
  - Conducción de voces automática.
  - Ritmo euclídeo que se reconstruye en cada ciclo y se aclara con la **TENSIÓN** de la sección.
  - Secciones congeladas (**FORMA** FIJA o AABA) e inercia melódica de Narmour (**INERCIA**): tras un salto la melodía cambia de dirección; tras un paso, sigue.
  - **PAPEL**: acordes, bajo, melodía o arpegio.
- **MARKOV**, como los dispositivos Max for Live:
  - Cadenas de orden variable (1–4) aprendidas de la **ENTRADA** MIDI, de lo que tocan las otras **VOCES** o de un corpus al **AZAR**.
  - La cadena de ritmo es independiente (**ORDEN R**).
  - Notas absolutas o intervalos, con **TEMPER** (exploración), **VENTANA**, **CONGELA** y **REINIC.** (romper la cadena).
  - Lo aprendido se guarda con el proyecto.

- **SERIAL** (serial / combinatoria):
  - **PR1**: Project 1 de G. M. Koenig. Alturas, ritmo y dinámica tienen cada uno un nivel de regularidad de 1 a 7. En 1 la serie suena en orden y en grupos repetidos; en 7 es azar sin repeticiones inmediatas.
  - **PR2**: Project 2. Cada parámetro tiene su método de selección: ALEA, SERIE, SECUENC., GRUPO, TENDENC. (máscara que se abre y cierra en **TEND** compases) o RATIO.
  - **MISHA**: la serie leída con formas P, R, I, RI, CICLO o AZAR, más **SHIFT** por vuelta, como el Eventide Misha.
  - **ROTAR**, **PERMUTAR** (permutaciones en orden lexicográfico) y **COMBINAR** (combinaciones de la serie): las operaciones de OpenMusic.
- **FRACTAL**:
  - **NØRGÅRD**: la serie infinita de Per Nørgård, con intervalo e inicio ajustables. Es autosemejante: cada 2, 4, 8… notas reaparece la serie entera.
  - **1/F**: ruido rosa por el algoritmo de dados de Voss (**DADOS**), con mezcla de ruido blanco.
  - **L-SYSTEM**: gramáticas de Lindenmayer FIBONACCI, THUE-MORSE, CANTOR (sonido/silencio) y PLANTA (ramas con pila), con **ITER** generaciones.
  - **RIT FRAC** alarga notas según otra lectura de la serie de Nørgård.
- **AUTÓMATA**:
  - **OTOMATA**: células que viajan por una rejilla de 9×9. Suenan al chocar con una pared y giran al cruzarse.
  - **ECA-256**: cuatro autómatas elementales de 8 células con reglas REGLA + fila × VARIA; cada nacimiento es una nota.
  - **WOLFRAM**: una fila de 16 células que se lee como número binario, como número de células vivas o por la célula central.
- **ESPACIAL** (geométrica):
  - **RENÉ**: rejilla cartesiana 4×4 con relojes X e Y, recorridos (FILAS, ZIGZAG, RELOJ XY, DIAGONAL, AZAR) y páginas Z, como el René de Make Noise.
  - **KRIA**: disparo, nota, octava y duración son bucles de longitud distinta (polimetría por parámetro), como el Kria de monome.
  - **ORCA**: líneas de operadores reloj (D), incremento (I) y azar (R), en el espíritu de Orca.
- **EUCLÍDEO** (distribución):
  - **TOUSSAINT**: ritmo E(k,n) con rotación. Un segundo anillo decide cuándo avanza la melodía de **NOTAS** alturas.
  - **T-1**: como el Torso T-1, con repeticiones (**REPEAT**), **ARMONÍA** y **CAOS**.
  - **GRIDS**: mapa de baterías al estilo de Mutable Instruments, con **X/Y/DENS**. Con **NOTAS** = 1 saca bombo, caja y charles GM (36/38/42); apaga ESC y ARM.
  - **ANULAR**: dos anillos combinados con OR, AND, XOR o A→NOTA, como en Hemisphere.
- **REGLAS** (restricciones):
  - **OMCLOUDS**: búsqueda adaptativa de OpenMusic. La frase se corrige sola en cada vuelta hacia las reglas: intervalos permitidos, salto máximo, sin repeticiones y triada en tiempo fuerte. **TEMP** la mantiene viva.
  - **SITUATION**: acordes con intervalos verticales permitidos y movimiento horizontal mínimo.
  - **CONTRAP.**: contrapunto de primera especie contra la voz guía: consonancias, sin quintas ni octavas paralelas, preferencia por el movimiento contrario. Va encima, debajo o libre. Está en el espíritu de las restricciones de bach/cage.
  - **MUS.MOUSE**: el *Music Mouse* de Laurie Spiegel. Un cursor errante mueve dos pares de voces en movimiento paralelo, contrario u oblicuo, en acorde o arpegiadas.
- **INTERVÁLICO** (relativo):
  - **MISHA**: una secuencia de intervalos (no de notas) leída con formas P/R/I/RI.
  - **SAMCHILL**: como el teclado Samchillian, cada nota sale de la anterior por un intervalo. Prefiere los pequeños (**PESO**, **MAX INT**).
  - **FUENTE = ENTRADA**: tocas los intervalos con el teclado, cada tecla respecto a **REF** (D4 = sube un tono). MISHA los graba en su secuencia; SAMCHILL los toca al momento. Pon la transposición en OFF.
  - **BORDE**: REBOTE o ENVOLVER al salir del registro.

## Salida CC

Junto al canal de cada voz hay un botón **CC** y un selector de número:
- Con **CC** apagado, la voz manda notas.
- Con **CC** encendido, la voz manda un controlador MIDI continuo por su canal, en vez de notas. El selector elige el número de controlador (0–127). Haz clic para abrir la lista; el menú marca los más comunes: 1 modulación, 7 volumen, 10 panorama, 11 expresión, 71 resonancia, 74 corte del filtro.
- El valor del CC es la altura que habría sonado (0–127), ya ajustada a escala y armonía. Así cualquier motor sirve como secuenciador de modulación: GENDYN como deriva lenta del filtro, TURING como sample & hold, 1/F como expresión humana…
- Para modular el sintetizador de otra voz, pon la voz CC en el **mismo canal** que esa voz.

## Presets

Arriba, junto al nombre, el selector **PRESET** carga una de las 256 configuraciones completas:
- Flechas o rueda para recorrerlas; clic para el menú por categorías.
- Hay 8 categorías de 32: Xenakis y estocástica, aleatoria e indeterminación, serialismo, minimalismo y proceso, Berlin School y secuencial, generativa y algorítmica, contemporánea y espectral, ritmo/euclídeo/modulación.
- Cada preset fija los modos, parámetros, escala, rejilla, armonía y canales de las cuatro voces: 1–4 por defecto, el 10 para la batería GM y el canal de la voz modulada para los CC. Después reinicia las voces.
- La ayuda explica en qué obra o técnica se inspira el preset.
- El preset también se puede cambiar por automatización (parámetro «Preset cargado»). Al abrir un proyecto no se vuelve a cargar: se respetan los ajustes guardados.

## Entrada MIDI

En el panel derecho, sección **ENTRADA MIDI**:
- **TRANSPONER**: la última nota que tocas transporta las cuatro voces y la escala respecto a **REF** (C4 por defecto), y se queda así.
- **MANTENER**: transporta solo mientras mantienes la tecla.
- **TÓNICA**: cambia la tónica sin cambiar de octava.
- **CANAL** elige el canal de entrada (OMNI = todos) y **THRU** deja pasar la entrada a la salida.
- Las notas que entran alimentan siempre el corpus del modo MARKOV y, con FUENTE = ENTRADA, los modos INTERVÁLICO.

## Cuantización entre voces

Cada voz tiene tres interruptores. Con los tres apagados, la voz es independiente.

- **ESC** ajusta las alturas a la escala común (panel derecho: TÓNICA y ESCALA, 27 escalas).
- **RIT** ajusta ataques y duraciones a la rejilla común (REJILLA y FUERZA).
- **ARM** limita la distancia armónica con la voz guía, o con todas (DIST ARM, GUÍA, contra).
  - La distancia es la de Tenney (log₂ de numerador × denominador del intervalo justo): unísono 0, quinta 2,6, cuarta 3,6, sexta mayor 3,9, tercera mayor 4,3, tercera menor 4,9… Cada octava suma 1.
  - Cada nota se mueve a la más cercana que cumpla el límite.

## Manejo 

- Knobs: arrastra en vertical (Mayús = fino), usa la rueda, o haz clic derecho o Ctrl+clic para volver al valor inicial.
- Selectores: flechas `<` `>`, rueda, o clic en el centro para abrir el menú.
- La ayuda del panel derecho explica el control sobre el que está el ratón.
- **MEMORIA**: clic o teclas 1–0 para recuperar; mantén pulsado 0,6 s para guardar. Se guardan con el proyecto.

El glissando (GENDYN, GLISS, SLIDE) y los microtonos usan pitch bend. Ajusta **BEND** al mismo rango que tu sintetizador.
