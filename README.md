# CletaEats - Android Client

<em>A modern food delivery mobile application built with Jetpack Compose and Kotlin, featuring role-based access control for Customers, Delivery Drivers, Restaurants, and Administrators.</em>

---

## Table of Contents

- [CletaEats - Android Client](#cletaeats---android-client)
  - [Table of Contents](#table-of-contents)
  - [Overview](#overview)
  - [Architecture](#architecture)
  - [Features](#features)
  - [Technology Stack](#technology-stack)
  - [Screens \& Navigation](#screens--navigation)
  - [Project Structure](#project-structure)
  - [Ecosystem](#ecosystem)
  - [Getting Started](#getting-started)
    - [Prerequisites](#prerequisites)
    - [Installation](#installation)
    - [Configuration](#configuration)
    - [Usage](#usage)
  - [License](#license)
  - [Contact](#contact)

---

## Overview

**CletaEatsApp** is the Android mobile client for the CletaEats food delivery ecosystem. This application provides a comprehensive interface for managing food orders, deliveries, and restaurant operations with a modern Material Design 3 UI.

The app supports four distinct user roles with specialized interfaces:
- **Customers**: Browse restaurants, create orders, track deliveries
- **Delivery Drivers (Repartidores)**: Manage assigned deliveries, update order status
- **Restaurants**: Handle incoming orders, update preparation status
- **Administrators**: Access analytics, manage complaints, register new restaurants

This application communicates with:
- **CletaEats Backend**: https://github.com/isaacmendezr/CletaEatsBackend

---

## Architecture

**Framework:** Android Native with Jetpack Compose  
**Language:** Kotlin  
**Architecture Pattern:** MVVM (Model-View-ViewModel) + Clean Architecture  
**Dependency Injection:** Hilt/Dagger  
**State Management:** Kotlin StateFlow with Coroutines  
**UI Framework:** Jetpack Compose with Material Design 3  
**Network Layer:** Retrofit + Moshi + OkHttp  
**Navigation:** Jetpack Navigation Compose  
**Persistence:** DataStore + Local JSON files (offline-first)

**Architectural Layers:**
- **Data Layer**: Repository pattern with dual-source strategy (REST API + local storage)
- **Domain Layer**: Business logic encapsulated in repository methods
- **Presentation Layer**: ViewModels with reactive StateFlow
- **UI Layer**: Declarative Compose UI with Material Design 3

---

## Features

| Category | Description |
| :-------- | :----------- |
| 🔐 **Authentication** | Multi-role login/registration with field validation (cedula, email, phone, address) |
| 🍽️ **Restaurant Browsing** | Responsive grid/list layout with image loading and search capabilities |
| 🛒 **Order Management** | Create orders with multi-combo selection, automatic cost calculation (subtotal + transport + 13% IVA) |
| 🚴 **Delivery Tracking** | Real-time order status updates with visual indicators (preparing, in-transit, delivered) |
| 👤 **Profile Management** | Edit user information with role-specific fields and password change |
| 📊 **Admin Dashboard** | Comprehensive reports: revenue analytics, order statistics, complaint management |
| 💬 **Feedback System** | Customer reviews and delivery driver complaint submission |
| 📱 **Responsive Design** | Adaptive layouts for tablets/foldables using WindowSizeClass |
| 🎨 **Modern UI** | Material You dynamic theming, smooth animations, and intuitive navigation |
| 🔄 **Offline Support** | Local data persistence with automatic sync fallback mechanism |

---

## Technology Stack

**Core Dependencies:**
- **Jetpack Compose BOM** - Modern declarative UI toolkit
- **Material3** - Material Design 3 components with dynamic theming
- **Hilt** - Dependency injection framework
- **Kotlin Coroutines** - Asynchronous programming
- **Lifecycle ViewModel** - Lifecycle-aware state management
- **Navigation Compose** - Type-safe navigation

**Networking:**
- **Retrofit** - REST API client
- **Moshi** - JSON serialization with polymorphic adapters
- **OkHttp** - HTTP client with logging interceptor
- **Gson** - Local JSON storage

**UI & Media:**
- **Coil** - Async image loading with caching
- **Material Icons Extended** - Comprehensive icon set
- **Adaptive Layout** - Responsive UI components

**Storage:**
- **DataStore Preferences** - Persistent user session management

**Utilities:**
- **Timber** - Logging framework

**Configuration:**
- **Min SDK:** 24 (Android 7.0 Nougat)
- **Target SDK:** 36
- **Compile SDK:** 36
- **Java Version:** 17
- **Build Tools:** Gradle 8.x, AGP 8.10.1

---

## Screens & Navigation

**Navigation Routes:**

| Route | Screen | User Role | Description |
|-------|--------|-----------|-------------|
| `login` | LoginScreen | All | User authentication entry point |
| `register` | RegistroScreen | All | Multi-role user registration |
| `restaurants/{clienteId}` | RestauranteListadoScreen | Customer | Browse available restaurants |
| `restaurant_details/{clienteId}/{restauranteJson}` | RestauranteDetallesScreen | Customer | Select combos and create order |
| `orders` | ClienteOrdenesScreen | Customer | View order history and status |
| `repartidor_orders/{repartidorId}` | RepartidorOrdenesScreen | Driver | Manage assigned deliveries |
| `restaurante_orders/{restauranteId}` | RestauranteOrdenesScreen | Restaurant | Handle incoming orders |
| `reports` | ReportesScreen | Admin | Analytics dashboard |
| `profile` | PerfilScreen | All | Edit user profile |
| `repartidor_quejas` | RepartidorQuejasScreen | Admin/Driver | Complaint management |
| `register_restaurant` | RestauranteRegistroScreen | Admin | Register new restaurants |
| `feedback/{pedidoJson}` | FeedbackScreen | Customer | Submit order feedback |

**Security Features:**
- Role-based access control with `RequireRole` composable
- Automatic redirect to login for unauthorized access
- Session persistence with DataStore

---

## Project Structure

```sh
CletaEatsApp/
├── app/
│   ├── build.gradle.kts
│   └── src/main/
│       ├── AndroidManifest.xml
│       ├── java/com/example/cletaeatsapp/
│       │   ├── CletaEatsApplication.kt        # Hilt application class
│       │   ├── MainActivity.kt                 # Main entry with drawer navigation
│       │   ├── data/
│       │   │   ├── model/                      # Domain entities
│       │   │   │   ├── Cliente.kt
│       │   │   │   ├── Repartidor.kt
│       │   │   │   ├── Restaurante.kt
│       │   │   │   ├── Pedido.kt
│       │   │   │   ├── Admin.kt
│       │   │   │   └── UserType.kt             # Sealed class for roles
│       │   │   ├── network/                    # API layer
│       │   │   │   ├── CletaEatsApiService.kt  # Retrofit interface
│       │   │   │   ├── CletaEatsNetwork.kt     # Network configuration
│       │   │   │   └── MoshiAdapters.kt        # JSON polymorphic adapters
│       │   │   └── repository/
│       │   │       └── CletaEatsRepository.kt  # Data abstraction layer
│       │   ├── di/
│       │   │   └── NetworkModule.kt            # Hilt dependency injection
│       │   ├── ui/
│       │   │   ├── components/                 # Reusable composables
│       │   │   │   ├── CommonComposables.kt
│       │   │   │   ├── OrdenCard.kt
│       │   │   │   └── RestauranteCard.kt
│       │   │   ├── navigation/
│       │   │   │   └── NavGraph.kt             # App navigation logic
│       │   │   ├── screens/                    # Main screens
│       │   │   │   ├── LoginScreen.kt
│       │   │   │   ├── RegistroScreen.kt
│       │   │   │   ├── RestauranteListadoScreen.kt
│       │   │   │   ├── RestauranteDetallesScreen.kt
│       │   │   │   ├── ClienteOrdenesScreen.kt
│       │   │   │   ├── RepartidorOrdenesScreen.kt
│       │   │   │   ├── RestauranteOrdenesScreen.kt
│       │   │   │   ├── ReportesScreen.kt
│       │   │   │   ├── PerfilScreen.kt
│       │   │   │   ├── RepartidorQuejasScreen.kt
│       │   │   │   ├── RestauranteRegistroScreen.kt
│       │   │   │   └── FeedbackScreen.kt
│       │   │   └── theme/                      # Material3 theming
│       │   │       ├── Color.kt
│       │   │       ├── Type.kt
│       │   │       └── Theme.kt
│       │   ├── utils/
│       │   │   └── CommonUtils.kt              # Utility functions
│       │   └── viewmodel/                      # Presentation layer
│       │       ├── LoginViewModel.kt
│       │       ├── RegistroViewModel.kt
│       │       ├── RestauranteViewModel.kt
│       │       ├── ClienteOrdenViewModel.kt
│       │       ├── RepartidorOrdenViewModel.kt
│       │       ├── RestauranteOrdenViewModel.kt
│       │       ├── ReportesViewModel.kt
│       │       ├── PerfilViewModel.kt
│       │       ├── RepartidorQuejasViewModel.kt
│       │       ├── RestauranteRegistroViewModel.kt
│       │       └── FeedbackViewModel.kt
│       └── res/
│           ├── values/
│           │   ├── strings.xml
│           │   ├── colors.xml
│           │   └── themes.xml
│           └── xml/
│               └── network_security_config.xml
├── gradle/
│   ├── libs.versions.toml                      # Version catalog
│   └── wrapper/
├── build.gradle.kts                             # Root build configuration
├── settings.gradle.kts
└── README.md
```

---

## Ecosystem

The CletaEatsApp is part of an integrated food delivery ecosystem:

| Component | Description | Repository |
|------------|-------------|-------------|
| 📱 **CletaEatsApp** | Android mobile client (Kotlin + Jetpack Compose) | [github.com/isaacmendezr/CletaEatsApp](https://github.com/isaacmendezr/CletaEatsApp) |
| 💻 **CletaEatsBackend** | REST API backend (Spring Boot + PostgreSQL) | [github.com/isaacmendezr/CletaEatsBackend](https://github.com/isaacmendezr/CletaEatsBackend) |

---

## Getting Started

### Prerequisites

- **Android Studio** Iguana 2024.2.1 or later
- **JDK** 17 or higher
- **Android SDK** API 24-36
- **Gradle** 8.x (included via wrapper)
- **CletaEatsBackend** running locally or accessible remotely

### Installation

1. Clone the repository:
   ```sh
   git clone https://github.com/isaacmendezr/CletaEatsApp.git
   cd CletaEatsApp
   ```

2. Open the project in Android Studio:
   ```sh
   # Open Android Studio and select "Open an Existing Project"
   # Navigate to the CletaEatsApp directory
   ```

3. Sync project with Gradle files:
   - Android Studio will automatically prompt to sync
   - Or manually: **File → Sync Project with Gradle Files**

4. Wait for dependency download and indexing to complete

### Configuration

**Backend Connection:**

Edit `CletaEatsNetwork.kt` to set the backend URL:

```kotlin
// For Android Emulator (default):
private const val BASE_URL = "http://10.0.2.2:8080/"

// For physical device on same network:
private const val BASE_URL = "http://YOUR_LOCAL_IP:8080/"

// For production:
private const val BASE_URL = "https://your-backend-domain.com/"
```

**Network Security:**

For development with HTTP (cleartext), ensure `network_security_config.xml` allows it:

```xml
<?xml version="1.0" encoding="utf-8"?>
<network-security-config>
    <base-config cleartextTrafficPermitted="true" />
</network-security-config>
```

⚠️ **Important:** Disable cleartext in production builds!

### Usage

1. **Start the backend server:**
   ```sh
   # See CletaEatsBackend repository for instructions
   # Ensure it's running on http://localhost:8080 (or configured URL)
   ```

2. **Run the Android app:**
   - Select an emulator or connected device
   - Click **Run** (▶️) in Android Studio
   - Or use command line:
     ```sh
     ./gradlew installDebug
     ```

3. **Test with default credentials:**
   
   The backend provides seeded data. Login with:
   
   **Admin:**
   ```
   Username: admin
   Password: admin123
   ```
   
   **Customer Example:**
   ```
   Cédula: 123456789
   Password: password
   ```
   
   **Delivery Driver Example:**
   ```
   Cédula: 987654321
   Password: password
   ```

4. **Build release APK:**
   ```sh
   ./gradlew assembleRelease
   # APK location: app/build/outputs/apk/release/app-release.apk
   ```

---

## License

This project is part of an academic assignment. All rights reserved.

## Contact

**Developer:** Isaac Méndez  
**Repository:** https://github.com/isaacmendezr/CletaEatsApp

---

**Note:** This application requires the CletaEatsBackend server to be running. Offline functionality is limited to viewing cached data. For full functionality, ensure backend connectivity.
