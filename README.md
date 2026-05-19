# 💪 Rutina del Gym — Web compartida con tu gym buddy

Una web para registrar tus entrenos, los de tu compañero, y comparar progreso semana / mes / año. Funciona en celular y compu, se sincroniza en tiempo real entre ambos.

## ✨ Qué incluye

- 5 días de rutina (Martes a Sábado) con todos tus ejercicios
- Registro de sets, reps, peso y comentarios por sesión
- Diagrama de músculos trabajados (principal y secundario)
- Link directo a video de cada ejercicio en YouTube
- Alternativas si la máquina está ocupada
- Comparación de progreso entre tú y tu compañero por semana, mes o año
- Sincronización en tiempo real en la nube (Firebase, gratis)

---

## 🚀 Pasos para tenerlo en línea

Vas a hacer 3 cosas:

1. Crear un proyecto de Firebase (la base de datos en la nube — gratis)
2. Pegar los datos de Firebase en el `index.html`
3. Subir el `index.html` a GitHub Pages

Tiempo total: ~15 minutos.

---

### Paso 1️⃣ — Crear el proyecto de Firebase

1. Entra a https://console.firebase.google.com/ (logueate con Google)
2. Click en **"Agregar proyecto"** (o "Add project")
3. Ponle un nombre, ej: `rutina-gym`
4. Desactiva Google Analytics (no lo necesitas) → **Crear proyecto**
5. Cuando termine, click en **Continuar**

#### Crear la base de datos

1. En el menú izquierdo, click en **Build → Firestore Database**
2. Click en **"Crear base de datos"** (Create database)
3. Elige **"Iniciar en modo de prueba"** (Start in test mode) — *ojo: esto deja la base abierta 30 días, después la cerramos abajo*
4. Elige la región más cercana (ej: `us-east1` o `southamerica-east1`)
5. Click **Habilitar**

#### Obtener las credenciales de Firebase

1. En el menú izquierdo, click en el **ícono de engranaje ⚙️** arriba → **Configuración del proyecto**
2. Baja hasta la sección **"Tus apps"**
3. Click en el ícono **`</>`** (Web)
4. Ponle un apodo, ej: `gym-web` → **Registrar app**
5. Te va a mostrar un bloque de código con un objeto `firebaseConfig` que se ve así:

```js
const firebaseConfig = {
  apiKey: "AIzaSyXXXXXXXXXXXXXXXXXXXX",
  authDomain: "rutina-gym-abc.firebaseapp.com",
  projectId: "rutina-gym-abc",
  storageBucket: "rutina-gym-abc.appspot.com",
  messagingSenderId: "123456789",
  appId: "1:123456789:web:abc123"
};
```

**Copia ese objeto completo.**

---

### Paso 2️⃣ — Pegar las credenciales en index.html

1. Abre el archivo `index.html` con cualquier editor de texto (VS Code, Notepad, Sublime, lo que tengas)
2. Busca la sección que dice `FIREBASE CONFIG — REEMPLAZA ESTOS VALORES` (está cerca del inicio del script)
3. Reemplaza el objeto `firebaseConfig` con el tuyo
4. **(Opcional)** Cambia la variable `SALA` por un nombre único de ustedes:
   ```js
   const SALA = "jazmin-y-buddy"; // o lo que quieras, sin espacios
   ```
   ⚠️ Tú y tu gym buddy deben usar **exactamente la misma SALA** para compartir datos.

5. Guarda el archivo.

---

### Paso 3️⃣ — Subir a GitHub Pages

#### A) Crear el repositorio

1. Entra a https://github.com (crea cuenta si no tienes)
2. Click en **"+" arriba a la derecha → New repository**
3. Nombre: `rutina-gym` (o lo que quieras)
4. Marca **Public** ✅
5. Marca **Add a README file** ✅
6. Click **Create repository**

#### B) Subir tu index.html

1. En el repo recién creado, click en **"Add file" → "Upload files"**
2. Arrastra tu `index.html` editado
3. Click **Commit changes**

#### C) Activar GitHub Pages

1. En el repo, click en **Settings** (arriba a la derecha)
2. En el menú izquierdo, click en **Pages**
3. En **"Source"**, selecciona **Deploy from a branch**
4. En **"Branch"**, selecciona **main** y carpeta **/ (root)** → **Save**
5. Espera 1–2 minutos. Refresca esa página.
6. Arriba va a aparecer un mensaje verde con tu URL, algo como:
   ```
   https://tu-usuario.github.io/rutina-gym/
   ```

¡Esa es tu web! Mándale ese link a tu gym buddy y listo.

---

### Paso 4️⃣ (Importante) — Cerrar la base de datos

El "modo de prueba" de Firebase deja la base abierta a internet por 30 días. Pasado eso, deja de funcionar. Para evitar eso, cambia las reglas:

1. En Firebase Console → **Firestore Database → Reglas (Rules)**
2. Reemplaza el contenido con:

```
rules_version = '2';
service cloud.firestore {
  match /databases/{database}/documents {
    // Permite leer y escribir en cualquier sala — sin auth, simple.
    // No es súper seguro: cualquiera que descubra tu SALA podría editar.
    // Para uso entre ustedes dos está bien si el nombre de SALA es difícil de adivinar.
    match /salas/{sala}/{document=**} {
      allow read, write: if true;
    }
  }
}
```

3. Click **Publicar**.

> **Sobre seguridad:** Como no tienes login, la "seguridad" depende de que el nombre de tu SALA sea difícil de adivinar. Usa algo como `mi-rutina-2026-abc123` en lugar de `gym`. Si en algún momento te preocupa más la privacidad, dime y agregamos login con Google.

---

## 📱 Cómo usarla

1. Abre la URL en tu celu (te conviene agregarla a la pantalla de inicio — en iPhone: botón compartir → "Añadir a pantalla de inicio". En Android: menú de Chrome → "Agregar a la pantalla principal")
2. La primera vez ingresa tu nombre
3. Tu gym buddy hace lo mismo desde su celu, abriendo el mismo link
4. Cuando él termine su sesión, automáticamente te aparece en tu pantalla (en tiempo real)
5. En "Ver progreso" pueden comparar quién progresó más cada semana 😈

---

## ❓ Preguntas frecuentes

**¿Cuesta algo?**
No. Firebase tiene un nivel gratuito (Spark plan) que permite 50.000 lecturas y 20.000 escrituras al día. Ustedes dos ni se acercarán a eso.

**¿Mi gym buddy necesita instalar algo?**
No. Solo abre el link en su celu.

**¿Y si pierdo el celu?**
Los datos están en la nube (Firebase). Abre el link en otro dispositivo, elige tu nombre y todo sigue ahí.

**¿Puedo cambiar la rutina?**
Sí, edita la sección `ROUTINE` dentro del `index.html`. Cada ejercicio tiene `name`, `video`, `primary`, `secondary` y `alts`.

**¿Y si quiero kg en vez de libras?**
Busca `lb` en el archivo y reemplázalo por `kg`. Son ~6 lugares.

**¿Cómo agrego más usuarios (no solo dos)?**
Ya funciona — pueden agregar hasta los que quieran, todos comparten la misma SALA.

---

## 🐛 Problemas comunes

**"Modo local" arriba en amarillo**
Significa que las credenciales de Firebase no se pegaron bien. Revisa el `firebaseConfig` en `index.html`.

**Mi gym buddy no ve mis datos**
Asegúrense de tener el **mismo valor de `SALA`** en sus copias del `index.html`. Si subiste a GitHub Pages, ambos están viendo el mismo archivo, así que esto debería pasar automáticamente.

**Error "Missing or insufficient permissions"**
Las reglas de Firestore están bloqueando. Revisa el Paso 4 — pega las reglas y publícalas.

---

¡A entrenar! 💪
