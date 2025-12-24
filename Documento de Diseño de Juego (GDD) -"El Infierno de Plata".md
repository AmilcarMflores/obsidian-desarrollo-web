- **Versión de Preproducción: 1.0** 
- **Proyecto:** Videojuego Estudiantil
- **Contacto:** Amilcar Mamani Flores
- **Fecha:** 9 de noviembre de 2025
---
### 1. PLANIFICACIÓN Y PREPRODUCCIÓN
Definimos el concepto central, la audiencia, la historia y las mecánicas base del juego, siguiendo las directrices de la fase de preproducción.

#### 1.1. Idea del Juego (Concepto)

- **Título Provisional:** El Infierno de Plata
- **Género:** Aventura 2D, Plataformas y Supervivencia Atmosférica (Combina "Aventura" con elementos de "Simulación" de supervivencia).
- **Referencia Histórica:** El cuadro "La Llegada del Virrey Morcillo a Potosí" (1716).
- **Concepto Central:** Un minero _mitayo_, Santiago, es forzado a trabajar en el Cerro Rico de Potosí como parte de una _mita_ mortal impuesta para celebrar la llegada del Virrey. El juego es una aventura de descenso y escape donde el jugador no lucha, sino que sobrevive.
- **Referencias de Tono y Mecánicas:** _Limbo_ (estética y puzzles), _This War of Mine_ (gestión de moral/recursos), _El Viejo y el Mar_ (temática de perseverancia), _La Divina Comedia_ (estructura de niveles).

#### 1.2. Público Objetivo

- Jugadores que disfrutan de experiencias narrativas, atmosféricas y con un alto componente emocional.
- Interesados en juegos de plataformas y puzzles que priorizan el ingenio sobre el combate.
- Audiencia (16+) debido a la temática opresiva y oscura.

#### 1.3. Guion e Historia

Siguiendo el proceso de "Cómo crear el guion de un videojuego":

1. **Género:** Aventura/Supervivencia. La narrativa es lineal y ambiental.
    
2. **Mundo del Juego:** El Cerro Rico de Potosí en 1716. No es un mundo realista, sino una representación alegórica del infierno: opresivo, claustrofóbico, oscuro y peligroso. La única "luz" es la promesa de la familia y el rumor de una salida.
    
3. **Personaje Principal:** **Santiago**. (Ver sección 2.1).
    
4. **Trama (Logline):** Tras ser separado de su familia y arrojado a la mina, Santiago debe descender a través de los peligrosos "círculos" del Cerro Rico, gestionando su resistencia y su miedo, para encontrar una legendaria salida de ventilación conocida como "El Suspiro" antes de que la montaña lo consuma.
    
5. **Diálogo:** Minimalista. La narrativa se cuenta a través de:
    
    - **Voz en Off (Opcional):** Pensamientos internos de Santiago (estilo _Cowboy Bebop_ / _Hemingway_).
        
    - **Ambiental:** Susurros de otros mineros, gritos de capataces.
        
    - **Visual:** El entorno cuenta la historia (derrumbes, herramientas abandonadas).
        
6. **Objetivos y Misiones:** El objetivo principal es uno: **Escapar**. Los objetivos secundarios son micro-desafíos: "Encuentra aceite para la lámpara", "Supera la zona de gases", "Evita al Capataz".

#### 1.4. Jugabilidad y Mecánicas

La jugabilidad se centra en la **perseverancia** y la **exploración sigilosa**.

- **Mecánica Central 1: La Barra de Resistencia (Simulación)**
    - No hay barra de "Vida". Hay una barra de **Resistencia/Moral**.
    - **Disminuye con:** El tiempo, el esfuerzo (correr, escalar), la oscuridad total y el miedo (cerca de enemigos o peligros).
    - **Se recupera con:** El consumible principal: **Hojas de Coca**. Encontrar agua o refugios seguros la restaura mínimamente.
    - _Resultado:_ Si la barra llega a cero, Santiago colapsa y el juego termina.

- **Mecánica Central 2: Luz y Oscuridad (Gestión)**
    - El jugador gestiona un recurso limitado (aceite de lámpara o fósforos).
    - La oscuridad total es un enemigo: drena la Resistencia rápidamente. El jugador debe decidir cuándo gastar luz y cuándo avanzar a ciegas.

- **Mecánica Central 3: Sigilo y Puzzles (Acción/Aventura)**
    - **No hay combate.** Santiago no puede luchar.
    - **Sigilo:** Evitar los conos de visión (luz de las lámparas) de los Capataces.
    - **Puzzles de Entorno:** Usar la física del juego (empujar vagonetas, soltar rocas) para crear caminos o distraer/eliminar enemigos indirectamente.

#### 1.5. Física del Videojuego

- **Gravedad 2D:** Se ajustará para que los saltos se sientan "pesados" y "con esfuerzo", reflejando el agotamiento de Santiago.
- **Colisiones:** Detección estándar de colisiones 2D (personaje-suelo, personaje-pared).
- **Física de Objetos (RigidBody 2D):** Aplicada a objetos clave para puzzles (vagonetas, rocas, palancas) para que reaccionen al ser empujados por el jugador.
---
### 2. DISEÑO DE PERSONAJES INCLUYENDO ENEMIGOS, OBJETOS

#### 2.1. Personaje Jugable (PJ)

- **Nombre:** Santiago
- **Concepto:** Minero _mitayo_. No es un héroe de acción, es un hombre común (padre, esposo) tratando de sobrevivir. Su animación de movimiento refleja fatiga.
- **Habilidades (Mecánicas):**
    - Moverse (Caminar/Correr cansado)
    - Saltar (Corto y pesado)
    - Escalar (Cornisas y escaleras)
    - Empujar / Jalar (Objetos de puzzle)
    - Usar Ítem (Consumir Coca / Encender Lámpara)

#### 2.2. Personajes No Jugables (NPC)

- **Enemigos (Hostiles - IA):**
    - **Capataz (Básico):** El enemigo principal. Patrulla rutas fijas. Su "visión" es el cono de luz de su lámpara. Si detecta al jugador, lo persigue. Un toque significa _Game Over_.
    - **Capataz Mayor (Mini-Jefe):** Un enemigo más grande y astuto que guarda una salida de nivel. No puede ser evadido; debe ser "derrotado" usando un puzzle de entorno (ej. provocando un derrumbe sobre él).

- **NPC Interactivos (Neutrales):**
    - **Minero Moribundo:** (Como el descrito en el texto de NPC). Un PNJ que pide ayuda (agua o coca). El jugador debe tomar una decisión moral: gastar sus valiosos recursos para ayudarlo (sin recompensa mecánica) o dejarlo (el "peso" de _Bebop_).
    - **Viejos Mineros (Informativos):** Al inicio del juego, PNJ de ambiente cuyos susurros (texto en pantalla) introducen la trama y la leyenda de "El Suspiro".

#### 2.3. Objetos

- **Consumibles:** Hojas de Coca (recupera Resistencia), Aceite (recarga lámpara), Fósforos (luz temporal).
- **Objetos de Puzzle:** Vagonetas (para empujar, bloquear), Rocas (para soltar), Palancas, Puertas de madera (rompibles).
---
### 3. DISEÑO DE ESCENARIOS Y ANIMACIÓN

#### 3.1. Diseño de Escenarios (Mundo)

- **Estilo Visual:** 2D minimalista. Primer plano (incluyendo PJ y enemigos) en **siluetas negras** (estilo _Limbo_).
- **Fondos:** Pintados con texturas atmosféricas y _parallax scrolling_ (varias capas de fondo moviéndose a diferentes velocidades) para dar profundidad.
- **Paleta de Colores:** Opresiva. Predominan los ocres, rojos oscuros, grises fríos y el negro absoluto.
- **Estructura de Niveles (Los "Círculos" de Dante):**
    1. **Círculo 1: El Limbo (Superior):** Túneles principales. Introducción al sigilo y capataces.
        
    2. **Círculo 2: Los Vapores (Gases):** Zonas bajas. Nubes de gas tóxico. Mecánica de "aguantar la respiración".
        
    3. **Círculo 3: La Oscuridad (Profundo):** Zonas sin vetas. Oscuridad total. Mecánica de gestión de luz.
        
    4. **Círculo 4: El Caos (Derrumbes):** Zonas inestables. Puzzles de tiempo/carreras.
        
    5. **Círculo 5: El Corazón (El Tío):** Nivel final. Puzzle ambiental con la efigie del "Tío" de la mina.

#### 3.2. Animación

- **Estilo:** Animación 2D fluida, enfocada en el peso y la emoción.
- **Animaciones (Santiago):**
    - Idle (Respiración cansada)
    - Caminar (lento, arrastrando los pies)
    - Correr (con esfuerzo, jorobado)
    - Salto (pesado)
    - Escalar (con esfuerzo)
    - Empujar/Jalar
    - Usar Ítem (llevarse la mano a la boca para mascar coca)
    - Muerte (colapso por agotamiento)

- **Animaciones (Capataz):**
    - Patrullar (caminata decidida)
    - Detectar (sobresalto)
    - Perseguir (correr)
    - Atacar (golpe de látigo o agarre)

- **Animaciones (Entorno):** Polvo flotante, goteo de agua, parpadeo de antorchas, caída de pequeñas rocas.
---
### 4. ANIMACIÓN, INTEGRACIÓN (IA y Pruebas)

#### 4.1. Integración de la Inteligencia Artificial (IA)

Basado en el texto "Cómo diseñar la inteligencia artificial":

- **Comportamiento (Capataz Básico):** Se usará una **Máquina de Estados Finitos (FSM)**.
    1. **Estado PATRULLA (Defecto):** Moverse entre puntos A y B. El "cono de visión" (trigger de colisión) está activo.
        
    2. **Estado ALERTA (Detección):** Si el PJ entra en el cono -> Detenerse 1 seg, reproducir sonido de alerta. Transición a PERSECUCIÓN.
        
    3. **Estado PERSECUCIÓN:** Aumentar velocidad de movimiento. Moverse hacia la `ultima_posicion_vista` del PJ.
        
    4. **Estado ATAQUE:** Si `distancia_al_PJ < 1m` -> Reproducir animación de ataque (Game Over).
        
    5. **Estado BÚSQUEDA:** Si llega a `ultima_posicion_vista` y no ve al PJ -> Pausar 3 seg, "mirar" (animación) -> Volver a PATRULLA.

#### 4.2. Plan de Pruebas (Testing)

- Probar la jugabilidad (que los saltos se sientan bien, que la barra de resistencia sea justa).
- Probar la IA (que los capataces no se atasquen, que el sigilo funcione).
- Probar Puzzles (asegurar que todos los puzzles tengan solución).
- Control de Calidad (QA) de colisiones y física.
---
### 5. DISEÑO DE MENUS, SONIDO, CREDITOS

#### 5.1. Diseño de Menús (Interfaz de Usuario - UI/UX)

- **Estilo:** Minimalista, diegético (integrado en el mundo).
- **Menú Principal:** Una silueta de Santiago mirando el Cerro Rico desde lejos. Opciones de texto simples: "Empezar Mita" (Nuevo Juego), "Continuar Descenso" (Cargar), "Ajustes", "Salir".
- **HUD (In-Game):** Mínimo.
    - **Barra de Resistencia:** Podría ser una viñeta oscura que crece en la pantalla, o la propia animación de Santiago (más encorvado, más lento).
    - **Icono de Ítem:** Un pequeño ícono de "Hoja de Coca" y "Lámpara" en una esquina.

#### 5.2. Diseño de Sonido y Música

- **Música:** Minimalista y atmosférica. El instrumento principal será una **zampoña o quena solitaria**. La música solo aparecerá en momentos clave de emoción o tensión.
- **Sonido Ambiental (Diegético):** Es el audio principal.
    - Viento constante en los túneles.
    - Goteo de agua.
    - Caída de rocas distantes.
    - Madera crujiendo.

- **Efectos de Sonido (SFX):**
    - **Santiago:** Respiración (normal, agitada, tosiendo), pasos, latido del corazón (en tensión).
    - **Enemigos:** Pasos pesados, gritos, chasquido de látigo.

#### 5.3. Créditos

- Scroll vertical simple sobre un fondo de la mina, con la música melancólica principal.