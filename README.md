# Buscador de Centros de Costo — ASA

App de una sola página (`index.html`) para filtrar tu maestro de centros de costo (EMPRESA, FUNDO, AREA, Idconsumidor, Descripción, Fecha_ingreso, Fecha_baja, Cuenta_destino, Jerarquía), igual patrón que tu app de programación de mano de obra: datos en Google Sheets, hosting gratis en GitHub Pages, sin backend.

## 1. Publicar tu Google Sheet como CSV

1. Abre tu hoja maestra de CeCo en Google Sheets.
2. Ve a **Archivo → Compartir → Publicar en la web**.
3. En "Vincular", selecciona la hoja específica (no "Todo el libro").
4. En "Formato de vinculación", elige **CSV**.
5. Click en **Publicar** → confirma.
6. Copia la URL que te da Google (luce así):
   ```
   https://docs.google.com/spreadsheets/d/e/2PACX-1vXXXXXXXXXXXXXXXXXXXXX/pub?gid=0&single=true&output=csv
   ```

⚠️ Esta URL es de solo lectura y no requiere login — cualquiera con el link puede ver los datos publicados. Si el maestro tiene información sensible, no la publiques así; usa Apps Script como intermediario en su lugar (avísame y lo armamos con el mismo patrón de tu `tareo.html`).

## 2. Configurar la app

Abre `index.html` y pega la URL en la constante al inicio del `<script>`:

```js
const CONFIG = {
  SHEET_CSV_URL: "https://docs.google.com/spreadsheets/d/e/2PACX-.../pub?gid=0&single=true&output=csv"
};
```

Con esto la app carga los datos automáticamente al abrir — no depende de que el usuario final pegue nada.

(También puedes pegar/cambiar la URL desde la barra superior de la app en cualquier momento, útil para probar con otra hoja sin tocar el código.)

## 3. Publicar en GitHub Pages

Mismo flujo que usas en `programacion-asa`:

```bash
# dentro de tu repo (o uno nuevo, ej. centros-costo-asa)
git add index.html README.md
git commit -m "Buscador de centros de costo"
git push
```

Luego en GitHub: **Settings → Pages → Branch: main → / (root) → Save**.
Tu app queda en `https://<tu-usuario>.github.io/<repo>/`.

## 4. Mantenimiento

- Cada vez que actualices el maestro en Google Sheets, los cambios se reflejan solos (la app pide el CSV fresco en cada carga — sin caché).
- Si agregas o quitas columnas en la hoja, ajusta el arreglo `COLUMNS` en el `<script>` para que coincida con los encabezados exactos.
- Si el volumen de filas crece mucho (>10-15k), conviene paginar la tabla; avísame y lo agregamos.

## Columnas esperadas en la hoja

| Encabezado exacto | Uso en la app |
|---|---|
| `EMPRESA` | Filtro dropdown, primer nivel de cascada |
| `FUNDO` | Filtro dropdown, cascada sobre EMPRESA |
| `AREA` | Filtro dropdown, cascada sobre FUNDO |
| `Idconsumidor` | Búsqueda de texto |
| `Descripcion` | Búsqueda de texto |
| `Fecha_ingreso` | Filtro de rango de fechas |
| `Fecha_baja` | Filtro de estado (activo/dado de baja) |
| `Cuenta_destino` | Filtro dropdown |
| `Jerarquia` | Filtro dropdown |

Los encabezados de tu hoja deben coincidir exactamente (mayúsculas/minúsculas incluidas) con esta lista.
