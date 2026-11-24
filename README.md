# BoomBassTik · Actualizaciones

**BoomBassTik** es una app de música para Android, sin anuncios, hecha para uso personal de familia y amigos.
Este repositorio **no tiene el código de la app**: solo guarda la **última versión publicada** para que cada teléfono con BoomBassTik se actualice solo.

> La app no está en Google Play. Por eso este repositorio cumple el papel de "tienda": la app lo revisa y, si hay algo nuevo, se actualiza.

---

## Contenido

1. [¿Qué hay aquí?](#qué-hay-aquí)
2. [Instalar BoomBassTik por primera vez](#instalar-boombasstik-por-primera-vez)
3. [Cómo funciona la actualización automática](#cómo-funciona-la-actualización-automática)
4. [Si ya tenías la app antes de la 1.5.0](#si-ya-tenías-la-app-antes-de-la-150)
5. [Publicar una versión nueva](#publicar-una-versión-nueva)
6. [Formato de `version.json`](#formato-de-versionjson)
7. [Seguridad](#seguridad)
8. [Problemas frecuentes](#problemas-frecuentes)

---

## ¿Qué hay aquí?

| Archivo | Para qué sirve |
|---|---|
| `version.json` | Dice cuál es la última versión, sus novedades y dónde está su APK. La app lo lee para saber si hay algo nuevo. |
| `apk/BoomBassTik-arm64-v8a.apk` | La app para la gran mayoría de teléfonos (cualquiera de los últimos ~8 años). |
| `apk/BoomBassTik-armeabi-v7a.apk` | La app para teléfonos Android antiguos o muy básicos. |
| `apk/BoomBassTik-x86_64.apk` | La app para emuladores de Android en computador (pruebas). |
| `README.md` | Este documento. |

La carpeta `apk/` tiene solo **la última versión**. Las anteriores quedan en el historial del repositorio.

---

## Instalar BoomBassTik por primera vez

Se necesita **Android 7.0 o más nuevo**.

1. Desde el teléfono, descarga el APK:
   - **Casi todos los teléfonos:** [BoomBassTik-arm64-v8a.apk](https://github.com/Grexon-hack-2/boombasstik-updates/raw/main/apk/BoomBassTik-arm64-v8a.apk)
   - **Si ese no instala** ("la app no es compatible"): [BoomBassTik-armeabi-v7a.apk](https://github.com/Grexon-hack-2/boombasstik-updates/raw/main/apk/BoomBassTik-armeabi-v7a.apk)
2. Abre el archivo descargado (desde la notificación o desde la app **Archivos → Descargas**).
3. Si Android avisa que el navegador no puede instalar apps, toca **Configuración**, activa **Permitir de esta fuente** y vuelve atrás.
4. Toca **Instalar**.
5. Si aparece **Google Play Protect** («Análisis recomendado» o «App desconocida»), toca **Más detalles** y luego **Instalar sin analizar** (en algunos teléfonos dice **Instalar de todas formas**). Pasa porque la app no viene de Google Play, no porque tenga algo malo. Puede aparecer también en la primera actualización.

Listo. Desde aquí, las versiones nuevas llegan solas.

---

## Cómo funciona la actualización automática

```text
  BoomBassTik (en el teléfono)                       Este repositorio
  ────────────────────────────                       ────────────────
  1. Al abrir la app y cada 6 horas  ── pregunta ──▶  version.json
  2. ¿Hay una versión mayor?          ◀── responde ──
  3. Descarga el APK de su procesador ◀── descarga ── apk/BoomBassTik-<procesador>.apk
  4. Comprueba la huella SHA-256 (si no coincide, lo borra)
  5. Lo instala cuando nadie está escuchando música
  6. Al abrir de nuevo: "¡Ya tienes la 1.x!" con las novedades
```

**Lo que ve cada persona:**

| Momento | Qué pasa |
|---|---|
| **La primera actualización** | Aparece un aviso con las novedades y el botón **Actualizar ahora**. Android pide **una sola vez** el permiso de *instalar apps desconocidas* para BoomBassTik, y luego confirmar la instalación. |
| **De ahí en adelante (Android 12 o más nuevo)** | Todo es automático: se descarga en segundo plano y se instala sola cuando la música está en pausa y la app está en segundo plano. Nunca corta una canción. |
| **Android 11 o anterior** | Se descarga sola, pero Android exige confirmar cada instalación con un toque (es una regla de Android, no de la app). |
| **Con datos móviles** | No descarga sola para no gastar datos: muestra **Descargar** con el tamaño. Con Wi-Fi descarga sin preguntar. |
| **Arreglo urgente** | Si la versión está marcada como urgente (por ejemplo, YouTube dejó de funcionar), descarga de inmediato, también con datos. |
| **Después de actualizar** | Al abrir la app aparece una vez **"¡Ya tienes la 1.x!"** con las novedades. |

La app solo **descarga** desde este repositorio: no envía ningún dato tuyo.

---

## Si ya tenías la app antes de la 1.5.0

Desde la **1.5.0**, BoomBassTik se firma con una llave propia (antes usaba la de pruebas).
Android no deja actualizar una app si la firma cambia, así que **una sola vez** hay que:

1. Desinstalar BoomBassTik (mantén presionado el ícono → **Desinstalar**).
2. Instalarla de nuevo con los pasos de [Instalar BoomBassTik por primera vez](#instalar-boombasstik-por-primera-vez).

Tus favoritos e historial de esa instalación anterior se pierden. Después de esto, no vuelve a ser necesario.

---

## Publicar una versión nueva

*(Solo para quien mantiene la app.)*

Todo se hace desde el PC donde se compila BoomBassTik, con `publicar.bat` (carpeta `D:\Claude\BoomBastik-etapa1`).

1. Haz doble clic en **`publicar.bat`**.
2. Responde:
   - **Nombre de la versión**, por ejemplo `1.5.1` (Enter usa la siguiente automáticamente).
   - **Novedades**, una por línea; deja una línea vacía para terminar. Son las que verá la gente.
   - **¿Es urgente?** Solo `s` si es un arreglo importante (YouTube dejó de funcionar, la app se cierra, etc.).
3. El script, solo:
   - sube el número de versión;
   - compila la app;
   - **verifica que cada APK esté firmado con la llave de BoomBassTik** (si no, no publica nada);
   - reemplaza los APK de `apk/` por los nuevos, calcula sus huellas SHA-256 y escribe `version.json`;
   - lo sube a GitHub con un commit normal (el historial se conserva).
4. Los teléfonos la encuentran en las próximas horas (al abrir la app o en su revisión de cada 6 horas).

**Sin preguntas:** crea `proxima-version.txt` en la misma carpeta y `publicar.bat` lo usa:

```text
version=1.5.1
urgente=no
Arreglos en el Modo DJ
La búsqueda por voz entiende mejor
```

**Requisitos del PC:** el proyecto compilando bien (`construir.bat`), la llave en `D:\Claude\BoomBassTik-firma` y [Git](https://git-scm.com) con acceso a este repositorio. Sin Git, el script deja todo listo en `D:\Claude\boombasstik-updates` para subirlo a mano.

**Reglas que no se rompen:**

- El número de versión **siempre sube**. Los teléfonos solo instalan versiones mayores a la suya.
- Nunca subas un APK firmado con otra llave: los teléfonos lo rechazarían.

---

## Formato de `version.json`

```json
{
  "schema": 1,
  "versionCode": 10501,
  "versionName": "1.5.1",
  "publishedAt": "2026-11-25",
  "urgent": false,
  "minSdk": 24,
  "notes": [
    "Arreglos en el Modo DJ",
    "La búsqueda por voz entiende mejor"
  ],
  "apks": {
    "arm64-v8a":   { "file": "apk/BoomBassTik-arm64-v8a.apk",   "size": 24117248, "sha256": "…" },
    "armeabi-v7a": { "file": "apk/BoomBassTik-armeabi-v7a.apk", "size": 21495808, "sha256": "…" },
    "x86_64":      { "file": "apk/BoomBassTik-x86_64.apk",      "size": 25165824, "sha256": "…" }
  }
}
```

| Campo | Significado |
|---|---|
| `schema` | Versión de este formato. Hoy es `1`. |
| `versionCode` | Número entero que siempre sube. Se calcula como `mayor × 10000 + menor × 100 + parche` (1.5.1 → `10501`). Es lo que la app compara. |
| `versionName` | El nombre que ve la gente. |
| `publishedAt` | Fecha de publicación (AAAA-MM-DD). |
| `urgent` | `true` = se descarga de inmediato, también con datos móviles. |
| `minSdk` | Versión mínima de Android (API). `24` = Android 7.0. |
| `notes` | Novedades, una por elemento. Se muestran antes y después de actualizar. |
| `apks` | Un APK por tipo de procesador, con su ruta en este repositorio, su tamaño en bytes y su huella SHA-256. La app elige el de su procesador. |

`publicar.bat` escribe este archivo solo. No hace falta editarlo a mano.

---

## Seguridad

- **Firma propia.** Cada APK está firmado con la llave de BoomBassTik. Android solo acepta una actualización si tiene **la misma firma** que la app instalada; nadie más puede publicar una versión que los teléfonos acepten.
  Huella SHA-256 del certificado:
  `54:2A:0D:13:66:4E:D1:E9:F0:DC:01:A0:38:F8:6C:BA:31:26:35:DA:10:6D:12:B5:3A:45:1A:37:14:AB:CC:9C`
- **Huella de cada descarga.** La app comprueba el SHA-256 de cada APK descargado contra `version.json`. Si no coincide (descarga dañada o cambiada), lo borra y no lo instala.
- **La llave nunca se sube aquí.** Vive solo en el PC de quien mantiene la app (`D:\Claude\BoomBassTik-firma`), con una copia de respaldo.

> ⚠️ **Guarda una copia de la llave fuera del PC** (USB, Google Drive o similar). Si se pierde, ya no se pueden publicar actualizaciones y todos tendrían que desinstalar e instalar de nuevo.

---

## Problemas frecuentes

<details>
<summary><b>No me llega la actualización</b></summary>

- Abre BoomBassTik: revisa al abrirse y cada 6 horas.
- Revisa que tengas internet. Con datos móviles verás el botón **Descargar** en el inicio en vez de descargarse sola.
- Si tienes una versión anterior a la 1.5.0, mira [Si ya tenías la app antes de la 1.5.0](#si-ya-tenías-la-app-antes-de-la-150).
</details>

<details>
<summary><b>Dice "No se pudo actualizar"</b></summary>

- Toca **Reintentar** en el inicio.
- Si el mensaje habla de **firma distinta**, desinstala la app e instálala de nuevo (una sola vez).
- Si habla de **espacio**, libera espacio en el teléfono e intenta otra vez.
</details>

<details>
<summary><b>Al actualizar apareció Google Play Protect</b></summary>

Es normal con apps que no vienen de Google Play. Toca **Más detalles → Instalar sin analizar**. No toques **No instalar app**: cancelaría la actualización (la app la vuelve a ofrecer después).
</details>

<details>
<summary><b>Toqué "Más tarde"</b></summary>

El aviso vuelve la próxima vez que abras la app. Mientras tanto, en el inicio queda **"Versión 1.x disponible · Actualizar"**.
</details>

<details>
<summary><b>Android dice "Por tu seguridad, tu teléfono no puede instalar apps desconocidas"</b></summary>

Toca **Configuración**, activa **Permitir de esta fuente** para BoomBassTik (o para el navegador, si es la primera instalación) y vuelve atrás.
</details>

<details>
<summary><b>Se puso a actualizar mientras escuchaba música</b></summary>

No debería: solo se instala sola con la música en pausa y la app en segundo plano, o cuando tocas **Instalar ya**. Si pasa, avísale a quien mantiene la app.
</details>

---

<sub>BoomBassTik · uso personal, sin anuncios. El audio proviene de YouTube; esta app no está afiliada a YouTube ni a Google.</sub>
