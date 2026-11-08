# Campus GAR

Portal de recluta y desarrollo de agentes de Grupo Asesores RAM: expediente de cada candidato desde ADT hasta agente activo, ficha visual de avance, score de ADT con PSP y factores Knock Out, cartas compromiso, expediente de conexión, recursos por etapa, vista por generación y kardex imprimible.

- **Página:** `index.html`, publicada con GitHub Pages en `servicioramgnp.github.io/campus`.
- **Datos:** una hoja de Google con Apps Script (`Code.gs`). Los archivos (PSP, CV, cartas y recursos) se guardan en una carpeta de Drive.
- **Acceso:** cada gerente entra con su PIN. Su nombre y su rol se toman de la hoja *Usuarios*.

El repositorio solo contiene la página. Ningún dato de candidatos vive en GitHub.

---

## 1. Crear el backend (hoja de Google + Apps Script)

1. En Google Drive crea una hoja de cálculo nueva llamada **Campus GAR - Datos**.
2. Abre **Extensiones › Apps Script**. Borra lo que trae `Código.gs`, pega todo el contenido de `Code.gs` y guarda.
3. En el selector de funciones elige **setup** y pulsa **Ejecutar**. Autoriza los permisos que pide Google (hoja y Drive).
   - Se crean las hojas *Personas*, *Config*, *Recursos*, *Usuarios* y *Registro*.
   - Se crea la carpeta de Drive **Campus GAR - Archivos**.
4. Abre la hoja **Usuarios**:
   - Cambia el PIN `1234` del administrador.
   - Agrega un renglón por gerente:

   | nombre | rol | pin | admin | activo |
   | --- | --- | --- | --- | --- |
   | Moisés | Gerente de Desarrollo | (tu PIN) | TRUE | TRUE |
   | … | Gerente de Atracción de Talento | … | FALSE | TRUE |
   | … | Gerente de Capacitación | … | FALSE | TRUE |
   | … | Gerente Comercial | … | FALSE | TRUE |
   | … | Director de Agencia | … | FALSE | TRUE |

   - Escribe el rol exactamente como aparece arriba: así funciona el filtro "Mi área".
   - **admin = TRUE** permite cambiar la configuración, subir recursos y eliminar expedientes.
   - Para quitarle el acceso a alguien, pon **activo = FALSE**.
5. Publica el backend: **Implementar › Nueva implementación › Tipo: Aplicación web**.
   - *Ejecutar como:* **Yo**
   - *Quién tiene acceso:* **Cualquier usuario**. La URL responde a quien la tenga, pero sin un PIN válido no entrega ningún dato.
   - Pulsa **Implementar** y copia la URL que termina en `/exec`.

## 2. Conectar la página

Abre `index.html` y pega la URL en la línea de configuración, al inicio del script:

```js
const API_URL="https://script.google.com/macros/s/XXXXXXXX/exec";
```

Sin esa URL, la página abre en *vista de ejemplo* con dos candidatos ficticios y no guarda nada.

## 3. Crear el repositorio y publicarlo

1. En GitHub, con la cuenta **servicioramgnp**, crea un repositorio nuevo llamado **campus**. Debe ser público para usar GitHub Pages gratis; la página no contiene datos.
2. Sube `index.html` (ya con tu `API_URL`) y este `README.md` a la rama `main`.
3. Ve a **Settings › Pages › Build and deployment**: *Source:* Deploy from a branch · *Branch:* `main` / `(root)` · **Save**.
4. En uno o dos minutos Campus queda en **https://servicioramgnp.github.io/campus/**.

## 4. Compartir los archivos

Comparte la carpeta de Drive **Campus GAR - Archivos** con la cuenta de Google de cada gerente, con permiso de *Lector*. Así pueden abrir el PSP, el CV, las cartas y los recursos desde Campus. Los documentos personales de conexión no se suben: solo se registra si están entregados.

---

## Mantenimiento

- **Cambiar metas, comisiones o documentos:** en Campus, pestaña *Configuración*, con un usuario admin. Las metas 2027 se capturan ahí en enero.
- **Actualizar el backend:** después de editar `Code.gs`, ve a **Implementar › Gestionar implementaciones › ✏️ › Versión: Nueva versión › Implementar**. La URL no cambia.
- **Actualizar la página:** sube el nuevo `index.html` al repositorio y conserva tu `API_URL`.
- **Respaldo:** la hoja *Personas* tiene un renglón por expediente, en formato JSON. Descarga una copia de la hoja de vez en cuando (*Archivo › Descargar*).
- **Registro:** la hoja *Registro* anota quién registró, eliminó o subió archivos, y cuándo.

## Límites a tener en cuenta

- La lista se actualiza sola cada minuto. Si dos gerentes editan la misma sección de la misma persona al mismo tiempo, se queda el último cambio.
- La fotografía se reduce a 240 px para que quepa en la hoja.
- Archivos de hasta 10 MB.
- La lectura del PSP con Claude no está incluida en esta versión.
