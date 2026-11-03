# Banco de Automatismos

Editor y simulador de esquemas eléctricos para aprender automatismos: mando y potencia, instalaciones de vivienda, neumática y electroneumática, electrónica, motores y variadores, y PLC programable en contactos. Inspirado en CADe_SIMU.

**Úsalo en el navegador:** https://gerardsalamana.github.io/banco-automatismos/

No hace falta instalar nada. Funciona en el ordenador y en el móvil.

## Qué se puede hacer

- Dibujar esquemas arrastrando componentes y tirando cables de borne a borne. Al terminar cada cable pregunta qué conductor es (fase, neutro, tierra, mando) y, al simular, marca en rojo los colores que no tocan.
- Simular: pulsadores, contactores, temporizadores, protecciones que disparan, motores que giran, cilindros que salen y entran, LEDs que se queman sin su resistencia…
- Planos de varias hojas unidas por conectores de hoja.
- Exportar a PDF (una página por hoja, con cajetín), DXF para CAD, SVG y PNG.
- Cuentas de usuario: con una cuenta, los planos se guardan en la nube y se abren desde cualquier ordenador.

## Cuentas de usuario (para quien administra la web)

Las cuentas funcionan con Firebase, de Google, en su plan gratuito. Firebase envía los correos de confirmación y de recuperación de contraseña, así que no hace falta ningún servidor de correo.

Mientras `config.js` tenga `window.BANCO_FIREBASE = null`, la web funciona igual, sin cuentas, guardando los planos en el navegador.

Para activarlas, una sola vez:

1. Entra en https://console.firebase.google.com con tu cuenta de Google y crea un proyecto (por ejemplo `banco-automatismos`). Google Analytics no hace falta.
2. **Authentication** → Comenzar → **Sign-in method** → activa **Correo electrónico/contraseña**.
3. **Authentication** → **Settings** → **Authorized domains** → añade `gerardsalamana.github.io`.
4. **Firestore Database** → Crear base de datos → modo producción → la ubicación que prefieras (por ejemplo `europe-west`).
5. En Firestore, pestaña **Reglas**: borra lo que haya, pega el contenido de `firestore.rules` y pulsa **Publicar**.
6. **Configuración del proyecto** (rueda dentada) → **Tus apps** → botón `</>` (Web) → ponle un nombre → **Registrar app**. Copia el objeto `firebaseConfig` que aparece.
7. Pega ese objeto en `config.js`:

```js
window.BANCO_FIREBASE = {
  apiKey: "…",
  authDomain: "…firebaseapp.com",
  projectId: "…",
  appId: "…"
};
```

Esos datos son públicos por diseño (van en cualquier web que use Firebase). La seguridad la ponen las reglas de `firestore.rules`: cada usuario solo puede leer y escribir sus propios planos, y solo después de confirmar el correo.

Opcional: en **Authentication → Templates** puedes cambiar el remitente y el texto de los correos.

## Archivos

- `index.html` — el programa entero.
- `config.js` — configuración de Firebase.
- `firestore.rules` — reglas de seguridad de la base de datos.
