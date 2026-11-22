<div align="center">

# 🦎 Xoloty

### Plataforma integral de neurodiversidad

Una app móvil inclusiva que adapta la experiencia digital a personas con **autismo, TDAH, dislexia y daltonismo**, con una mascota ajolote que acompaña en cada paso.

![Power Apps](https://img.shields.io/badge/Power%20Apps-742774?style=for-the-badge&logo=powerapps&logoColor=white)
![Power Automate](https://img.shields.io/badge/Power%20Automate-0066FF?style=for-the-badge&logo=powerautomate&logoColor=white)
![SharePoint](https://img.shields.io/badge/SharePoint-0078D4?style=for-the-badge&logo=microsoftsharepoint&logoColor=white)
![OpenAI](https://img.shields.io/badge/OpenAI-412991?style=for-the-badge&logo=openai&logoColor=white)

🏆 **Ganadora de la hackatón interna** · clasificada a la hackatón final

</div>

---

## 📖 ¿Qué es Xoloty?

Xoloty nace de una idea simple: **la tecnología debería adaptarse a las personas, no al revés.**

Muchas apps asumen una sola forma de leer, comunicarse o concentrarse. Xoloty ofrece herramientas pensadas para distintos perfiles neurodivergentes en un solo lugar, con un diseño suave (paleta lavanda/pastel) y un ajolote como guía.

La app está construida con el ecosistema **Microsoft 365**: Power Apps como interfaz, Power Automate como backend, SharePoint como base de datos y la API de OpenAI para las funciones con IA.

---

## ✨ Funciones

### 🔐 Cuentas y acceso
- **Login** validado contra la lista de usuarios en SharePoint.
- **Registro en 3 pasos** (datos personales → ubicación → correo y contraseña) en una sola pantalla.
- **Contraseña segura** con indicadores visuales de requisitos (mínimo 8 caracteres, una mayúscula y un número) y botón para mostrar/ocultar.
- **Recuperar y cambiar contraseña por código al correo**: se envía un código de 5 dígitos con vencimiento de 15 minutos y cuenta regresiva. Se puede iniciar desde el Login o desde Ajustes.
- **Correo con diseño Xoloty** (HTML) para el código de verificación.

### 🧩 Autismo: comunicación con pictogramas
- Selección de pictogramas por categorías (Personas, Acciones...) usando la librería **ARASAAC**.
- Un flujo de **IA** convierte los pictogramas elegidos en una frase clara, en modo **afirmativo** o **pregunta**.
- **Texto a voz** para que un tutor o docente escuche el mensaje.

### 📖 Dislexia: lector de pantalla
- **Pegar texto** o **subir un archivo** (`.txt` y `.pdf`).
- **Lectura en voz alta** oración por oración, con **velocidad ajustable**.
- **Resaltado de palabras** mientras la voz lee.
- Botones **A− / A+** para cambiar el tamaño de la letra.

### ⚙️ Ajustes
- Perfil de usuario con fotos de perfil predefinidas.
- Cambio de contraseña y cierre de sesión.

> Los perfiles de **TDAH** y **Daltonismo** forman parte de la visión del proyecto (modo enfoque, micro-rutinas, filtros de paleta, doble codificación color + icono). Consulta el [roadmap](#-roadmap).

---

## 🏗️ Arquitectura

```
┌──────────────────────┐
│      Power Apps      │  ← Interfaz (pantallas, formularios, lógica)
└──────────┬───────────┘
           │ llama
           ▼
┌──────────────────────┐        ┌───────────────────────┐
│    Power Automate    │ ─────► │   API de OpenAI       │
│  (flujos / backend)  │        │ (frases, voz, PDF)    │
└──────────┬───────────┘        └───────────────────────┘
           │ lee / escribe
           ▼
┌──────────────────────┐        ┌───────────────────────┐
│  Listas SharePoint   │        │  OneDrive (archivos)  │
└──────────────────────┘        └───────────────────────┘
```

### Flujos de Power Automate

| Flujo | Qué hace |
|---|---|
| `GenerarMensajeIA` | Recibe la frase de pictogramas y el modo (afirmativo/pregunta) y devuelve el mensaje armado por IA (`gpt-4.1-mini`). |
| `GenerarVoz` | Convierte el mensaje generado en audio. |
| `LeerTexto` | Convierte texto a audio con velocidad configurable (lector de dislexia). |
| `SubirArchivoTexto` | Extrae el texto de un `.txt` o un `.pdf` (los PDF se procesan vía HTTP a OpenAI). |
| `EnviarCodigo` | Genera el código de verificación, lo guarda con su vencimiento y lo envía por correo en HTML. |
| `VerificarCodigo` | Valida el código y que no haya expirado. |
| `CambiarClave` | Actualiza la contraseña en la lista de usuarios. |

### Listas de SharePoint

| Lista | Uso |
|---|---|
| `Usuarios` | Cuentas y datos de perfil (nombres, correo, contraseña, código de recuperación y su expiración, etc.). |
| `xoloty_pictogramas` | Catálogo de pictogramas (`Title`, `Categoria`, `IDArasaac`). |
| `Xoloty_Archivos` | Lista auxiliar (con adjuntos) usada como selector para subir archivos. |

---

## 🚀 Puesta en marcha

> Xoloty es una app de **Power Apps**, así que no se ejecuta con `npm start`. Estos son los pasos para montarla en tu propio entorno.

### Requisitos
- Cuenta de **Microsoft 365** con acceso a Power Apps, Power Automate y SharePoint.
- Licencia que permita la acción **HTTP** en Power Automate (por ejemplo, la prueba de 90 días).
- Una **API key de OpenAI** con créditos disponibles.

### Pasos

1. **Clona el repositorio**
   ```bash
   git clone https://github.com/Parappa-hub/Xoloty.git
   ```
2. **Crea el sitio y las listas de SharePoint** (`Usuarios`, `xoloty_pictogramas`, `Xoloty_Archivos`) con las columnas descritas arriba.
3. **Importa los flujos** de Power Automate y configura las conexiones (SharePoint, OneDrive, Outlook y la acción HTTP hacia OpenAI).
4. **Agrega tu API key de OpenAI** dentro de los flujos. 🔒 **Nunca la subas al repositorio.**
5. **Importa la app** en Power Apps y vuelve a conectar los orígenes de datos y los flujos.
6. **Prueba** el login, el registro y las funciones de cada perfil.

---

## 🎨 Identidad visual

- **Mascota:** ajolote 🦎
- **Paleta:** lavanda y tonos pastel
- **Principios:** interfaces limpias, poco ruido visual, tarjetas suaves y navegación simple (Inicio · Perfil · Ajustes)

---

## 🗺️ Roadmap

- [x] Login, registro en 3 pasos y recuperación de contraseña
- [x] Autismo: pictogramas + IA + voz
- [x] Dislexia: lector de pantalla con subida de archivos y resaltado
- [x] Pantalla de Ajustes
- [ ] TDAH: modo enfoque, micro-rutinas con recompensas y recordatorios de pausa
- [ ] Daltonismo: filtros de paleta, alto contraste y doble codificación color + icono
- [ ] Lectura guiada con reconocimiento de voz
- [ ] Guiones sociales y termómetro sensorial (Autismo)
- [ ] Almacenar contraseñas con hash en lugar de texto plano

---

## 🙏 Créditos y licencias

- **Pictogramas:** [ARASAAC](https://arasaac.org) · autor: Sergio Palao · origen: Gobierno de Aragón · licencia [CC BY-NC-SA](https://creativecommons.org/licenses/by-nc-sa/4.0/). Su uso comercial no está permitido sin autorización.
- **IA:** API de OpenAI.
- **Plataforma:** Microsoft Power Platform y Microsoft 365.

---

## 👥 Equipo

<!-- Agrega aquí a las personas del equipo con su rol y su GitHub -->

| Nombre | Rol |
|---|---|
| _Tu nombre_ | _Tu rol_ |

---

## 📄 Licencia

<!-- Elige una licencia (por ejemplo MIT) y agrégala como archivo LICENSE -->

Pendiente de definir.

---

<div align="center">

Hecho con 💜 para que todas las mentes tengan su lugar.

</div>
