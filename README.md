# CASITA

*[Read this in English](README.md)*

> Nombre sujeto a cambios.

Visor de archivos astronómicos FITS (imágenes y cubos), ligero y de código abierto,
pensado para correr bien en equipos modestos — laptops de estudiantes, no estaciones
de trabajo de investigación. Alternativa liviana a CASA Viewer / CARTA / DS9, con dos
frentes de uso que comparten un mismo núcleo: terminal (TUI) y escritorio (GUI).

**Principio rector:** el hardware del usuario nunca debe ser la razón por la que el
programa no funciona. Si un archivo es demasiado grande para la RAM disponible, el
programa sigue funcionando (más lento, por streaming), no falla ni congela el equipo.

## Estado actual

**En inicio.** Este repositorio todavía no tiene código. Se está construyendo la
Fase 1 (ver Roadmap más abajo): un visor TUI para FITS 2D en Linux.

No hay binarios ni instrucciones de instalación todavía — se agregarán aquí en cuanto
exista un MVP funcional.

## ¿Para quién es esto?

Estudiantes universitarios (no necesariamente de astronomía) que necesitan abrir e
inspeccionar archivos FITS para una tarea, laboratorio o proyecto, en hardware que no
es de investigación: 4–8 GB de RAM, GPU integrada, CPU de varios años, a veces
trabajando por SSH contra un servidor universitario igual de limitado o con conexión
intermitente.

## Roadmap

El alcance completo se dividió en fases confrontando el "alcance ideal" contra el
tiempo real disponible. La regla para decidir qué entra en el MVP: *¿un estudiante que
solo quiere ver una imagen 2D nota la ausencia de esto el día uno?* Si no, se pospone.

### Fase 1 — MVP real (en progreso)
- Lectura de FITS 2D (sin cubos todavía) y parseo de header
- Normalización de intensidad: lineal y zscale
- 1–2 colormaps (gris + viridis)
- Zoom / pan
- TUI con renderizado ANSI truecolor, navegación por teclado, funcional sobre SSH
- Lectura por streaming / memory-mapped — nunca cargar el archivo completo en RAM
- Apertura de un archivo típico en menos de 2 segundos
- Solo Linux

### Fase 2 — primera expansión
- GUI con Tauri (mouse, zoom con scroll, exportar PNG/JPEG)
- Cubos 3D: navegación por rebanadas + extracción de espectro
- Normalización log/sqrt adicional
- Windows y macOS
- Detección de terminal y degradación (Kitty/Sixel) en el TUI

### Fase 3 — mejoras posteriores (sin fecha)
- Más colormaps
- Manejo robusto de headers no estándar
- Empaquetado más pulido (instaladores nativos, firmas, etc.)

### Fuera de alcance indefinidamente
WCS (coordenadas celestes reales), formato HDF5 propietario de CARTA, calibración o
reducción de datos, regiones/mediciones fotométricas, colaboración en tiempo real,
integración con archivos científicos remotos, soporte de cubos rotados.

## Arquitectura (planeada)

El núcleo (parser de FITS, normalización, colormaps, lectura por streaming) vive
separado de la capa de renderizado desde el día uno, para que la Fase 2 (GUI en
Tauri) pueda reusarlo sin reescribirlo:

```
casita/
├── core/         # parser FITS, normalización, mmap reader — sin UI
├── tui/          # Fase 1: interfaz de terminal (ratatui/crossterm)
└── gui-tauri/    # Fase 2: interfaz de escritorio (aún no iniciado)
```

## Requerimientos no funcionales

| Categoría | Objetivo |
|---|---|
| Tamaño de instalador | GUI <50 MB · TUI (binario único) <20 MB |
| RAM en reposo | <150 MB con un archivo mediano abierto |
| Dependencias externas | Ninguna en tiempo de ejecución más allá del WebView del sistema (GUI) |
| Conectividad | 100% offline |
| Privacidad | Sin telemetría, sin cuentas, sin conexión no solicitada |
| Licencia | GNU AGPLv3 |

## Instalación

_Próximamente — en cuanto exista un binario de la Fase 1._

## Licencia

[GNU AGPLv3](https://www.gnu.org/licenses/agpl-3.0.html) — copyleft fuerte, incluye el
caso de uso por red.

## Contribuir

Este proyecto está en construcción activa y el proceso es parte del objetivo (portafolio
con código limpio y documentado). Issues y PRs son bienvenidos, especialmente sobre las
preguntas abiertas del documento de requerimientos (tamaños de archivo reales que usan
estudiantes, terminales más comunes, si los cubos son un caso frecuente para un usuario
no-astrónomo).
