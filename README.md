# Sistema de Asistencia - Computación en la Nube

Proyecto del curso **Computación en la Nube** del Instituto Profesional Santo Tomás.

Sistema web para el registro y seguimiento de asistencia de alumnos, con base de datos en la nube (Firebase) y opción de almacenamiento local (localStorage).

---

## Cómo ejecutar

### Opción 1: GitHub Pages (nube)

El proyecto está publicado en GitHub Pages. Accede directamente desde el navegador:

```
https://ianbmisaac.github.io/Computacion-en-la-nube-1/
```

No se necesita instalar nada. Solo abre el enlace en tu navegador.

### Opción 2: Local (sin servidor)

1. Clona el repositorio:
   ```bash
   git clone https://github.com/ianbmisaac/Computacion-en-la-nube-1.git
   ```
2. Abre `index.html` en tu navegador (doble clic o arrastra el archivo).

---

## Funcionalidades

### CRUD de usuarios
- **Crear**: Registrar un usuario con RUT (ID), nombre y email.
- **Leer**: Tabla con todos los usuarios registrados.
- **Actualizar**: Marcar asistencia (presente/ausente) por día.
- **Eliminar**: Botón de eliminar usuario en la tabla.

### Registro de asistencia
- Marca con checkboxes quiénes asistieron el día de hoy.
- Guarda el registro con fecha (formato YYYY-MM-DD).
- Puedes editar la asistencia en cualquier momento del mismo día.

### Calendario historial
- Vista mensual con navegación entre meses.
- Los días con registro de asistencia se marcan con **borde verde**.
- Al hacer clic en un día, se muestra quiénes estuvieron presentes y ausentes.

### Búsqueda
- Campo de búsqueda para filtrar usuarios por nombre, RUT o email.

---

## Base de datos: Nube vs Local

La aplicación ofrece dos modos de almacenamiento, seleccionables con botones en la parte superior:

### Modo Firebase (nube)
- **Proveedor**: Google Firebase Realtime Database.
- **Hosting**: GitHub Pages (sitio estático) + Firebase CDN.
- **Almacenamiento**: Los datos se guardan en la nube de Firebase.
- **Ventaja**: Los datos persisten y son accesibles desde cualquier dispositivo con acceso a internet.
- **Cómo funciona**: La aplicación envía y recibe datos mediante peticiones HTTP a la API de Firebase Realtime Database. Los datos se almacenan en formato JSON en los servidores de Google.

### Modo Local (localStorage)
- **Proveedor**: Almacenamiento del navegador web.
- **Hosting**: Solo funciona abriendo el archivo directamente o desde GitHub Pages.
- **Almacenamiento**: Los datos se guardan en el navegador del usuario.
- **Limitación**: Los datos solo existen en ese navegador y ese dispositivo. Si se limpia el navegador o se cambia de equipo, los datos se pierden.

### Comparación

| Característica | Firebase (nube) | localStorage |
|---|---|---|
| Persistencia entre dispositivos | Sí | No |
| Requiere conexión a internet | Sí | No |
| Acceso desde otro equipo | Sí | No |
| Velocidad de lectura | Rápido (conexión) | Instantáneo |
| Capacidad de almacenamiento | Ilimitada | ~5 MB por sitio |
| Costo | Gratuito (con límites) | Gratuito |

---

## Modelo de datos

### Firebase Realtime Database (JSON)

```
/
├── usuarios/
│   ├── {RUT}/
│   │   ├── username: "Nombre del alumno"
│   │   └── email: "correo@ejemplo.com"
│   └── ...
└── asistencia/
    ├── {YYYY-MM-DD}/
    │   ├── {RUT}: true    (presente)
    │   ├── {RUT}: false   (ausente)
    │   └── ...
    └── ...
```

### localStorage

```
asistencia_local_usuarios  → { "12345678-9": { "username": "...", "email": "..." } }
asistencia_local_registro  → { "2026-09-09": { "12345678-9": true } }
```

### Campos

| Campo | Tipo | Descripción |
|---|---|---|
| RUT | string | Identificador único del usuario (sin puntos). Ej: `12345678-9` |
| username | string | Nombre completo del alumno |
| email | string | Correo electrónico del alumno |
| asistencia | boolean | `true` = presente, `false` = ausente |

---

## Flujo de la aplicación

1. El usuario abre `index.html` y hace clic en "Registrar Asistencia".
2. Se carga `Asistencia.html` con modo Firebase por defecto.
3. Se registran alumnos (RUT + nombre + email). Los datos se guardan en Firebase o localStorage según el modo seleccionado.
4. Se marca la asistencia del día con checkboxes y se guarda.
5. El calendario muestra los días con registro (borde verde).
6. Al hacer clic en un día, se muestra el detalle: quiénes asistieron y quiénes no.

---

## Servicios utilizados

| Servicio | Uso |
|---|---|
| GitHub Pages | Hosting del sitio web (HTML/CSS/JS estáticos) |
| Firebase Realtime Database | Base de datos en la nube para usuarios y asistencia |
| Firebase SDK (CDN) | Librería JavaScript para conectar con Firebase |

### Justificación de reglas de Firebase

Las reglas de lectura y escritura están abiertas (`".read": true, ".write": true`) porque es un entorno de desarrollo/académico. En producción, se recomienda:
- Autenticación de usuarios (Firebase Auth).
- Reglas por ruta (solo el usuario autenticado puede escribir su asistencia).
- Validación de datos en servidor.

---

## Estructura del proyecto

```
Computacion-en-la-nube-1/
├── index.html              ← Página de inicio
├── css/
│   └── styles.css          ← Estilos
├── html/
│   └── Asistencia.html     ← Página principal (CRUD + asistencia)
├── Images/
│   └── Instituto-Profesional-Santo-Tomas.png  ← Logo
├── .gitignore
└── README.md
```

---

## Tecnologías

- HTML5
- CSS3
- JavaScript (ES6+)
- Firebase Realtime Database
- GitHub Pages
