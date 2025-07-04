# 👜 Backpack App - Resumen de Componentes

Este documento resume la arquitectura de la aplicación Android **Backpack**, sus componentes principales, funciones y la forma en que se comunican entre sí.

---

## 📁 Componentes Principales

| Componente                     | Función Principal                                                         | Comunicación / Dependencias                                                                                                             |
| ------------------------------ | ------------------------------------------------------------------------- | --------------------------------------------------------------------------------------------------------------------------------------- |
| `MainActivity.kt`              | Punto de entrada de la app. Inicializa Firebase, ViewModels y navegación. | Usa `AuthViewModel` y `MochilaViewModel` con `ViewModelProvider`. Carga el `NavGraph`. Controla `navController`. Maneja Google Sign-In. |
| `NavGraph.kt`                  | Define las rutas de navegación entre pantallas.                           | Llama a: `LoginScreen`, `RegisterScreen`, `MochilaListScreen`, `MochilaDetailsScreen`, `MapScreen`. Inyecta ViewModels.                 |
| `AuthViewModel.kt`             | Gestiona autenticación (login, registro, Google Sign-In), sesión local.   | Firebase Auth, `SharedPreferences`, observado por `MainActivity`.                                                                       |
| `MochilaViewModel.kt`          | Escucha y expone en tiempo real la lista de mochilas desde Firebase.      | Firebase Realtime Database. Observado por `MochilaListScreen` y `MochilaDetailsScreen`.                                                 |
| `Mochila.kt`                   | Modelo de datos con ID, latitud y longitud.                               | Usado por `MochilaViewModel` y pantallas de mochilas.                                                                                   |
| `LoginScreen.kt`               | Login con email/contraseña o Google.                                      | Usa `AuthViewModel`, `navController`, `MainActivity.signInWithGoogle()`.                                                                |
| `RegisterScreen.kt`            | Registro de usuario nuevo.                                                | Usa `AuthViewModel`, `navController`.                                                                                                   |
| `MochilaListScreen.kt`         | Muestra lista de mochilas.                                                | Observa `MochilaViewModel`, navega a detalles.                                                                                          |
| `MochilaDetailsScreen.kt`      | Muestra detalles de una mochila.                                          | Usa `mochilaId`, consulta a `MochilaViewModel`, navega a mapa.                                                                          |
| `MapScreen.kt`                 | Muestra ubicación en Google Maps.                                         | Lee Firebase directamente usando `mochilaId`. Usa `GoogleMapView`.                                                                      |
| `AppTheme.kt`, `Typography.kt` | Definen tema visual y estilos de texto.                                   | Aplicado globalmente en `MainActivity`.                                                                                                 |
| `AndroidManifest.xml`          | Declara permisos, configuración de API y actividad principal.             | Permisos: Internet, ubicación. Incluye API KEY para Maps y configuración de Google Sign-In.                                             |

---

## 🔄 Flujo General de Comunicación

```
MainActivity
 ├── Inicia Firebase y ViewModels
 ├── Observa authState de AuthViewModel
 ├── Carga NavGraph con navController
 └── Maneja Google Sign-In

AuthViewModel
 ├── Login, registro, sesión local
 └── Firebase Auth y SharedPreferences

MochilaViewModel
 └── Escucha Firebase Realtime Database ("mochilas/") y expone lista reactiva

NavGraph
 └── Controla navegación entre:
     - LoginScreen → usa AuthViewModel
     - RegisterScreen → usa AuthViewModel
     - MochilaListScreen → usa MochilaViewModel
     - MochilaDetailsScreen → usa MochilaViewModel
     - MapScreen → lee Firebase directamente

Pantallas (Screens)
 └── Observan ViewModels y navegan usando NavController
```

---

## 📚 Extras

* Arquitectura: MVVM
* Navegación: Jetpack Navigation Compose
* UI: Jetpack Compose + Material 3
* Base de datos: Firebase Realtime Database
* Autenticación: Firebase Auth + Google Sign-In
* Almacenamiento de sesión: SharedPreferences

---
