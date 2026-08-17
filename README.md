# App-DT: Green Smart Home Digital Twin

[![Kotlin](https://img.shields.io/badge/Kotlin-2.0-7F52FF.svg?style=flat&logo=kotlin&logoColor=white)](https://kotlinlang.org/)
[![Android](https://img.shields.io/badge/Android-SDK%2035-3DDC84.svg?style=flat&logo=android&logoColor=white)](https://developer.android.com/)
[![Architecture](https://img.shields.io/badge/Architecture-Clean%20%2B%20MVVM-blue.svg?style=flat)]()
[![Dagger Hilt](https://img.shields.io/badge/DI-Dagger%20Hilt-orange.svg?style=flat)]()
[![Coroutines](https://img.shields.io/badge/Async-Coroutines%20%26%20Flow-purple.svg?style=flat)]()
[![License](https://img.shields.io/badge/License-MIT-green.svg?style=flat)](LICENSE)

> **Master's Thesis Project** developed in collaboration with **Università degli Studi di Brescia** (UNIBS) and **CNR-ISTI Pisa** under the Italian national research project **PRIN EUD4GSH**.

---

## 📖 Overview & Elevator Pitch

**App-DT** is a native Android application engineered to transform domestic energy management through a proactive **User-in-the-Loop** approach. Interfacing with a household **Digital Twin**, the system moves beyond reactive, "blind" home automation by introducing an asynchronous **"What-If" simulation layer**.

Users can formulate energy automations as *intentions*, allowing the Digital Twin to prospectively simulate total power loads, predict grid overloads, and negotiate conflicts in real-time before actual execution on physical IoT devices.

---

## 📊 Key Achievements & Validated Metrics

During rigorous empirical user testing across residential task scenarios, the application demonstrated exceptional usability and effectiveness:

| Metric | Measured Result | Benchmark / Industry Baseline |
| :--- | :---: | :--- |
| **System Usability Scale (SUS)** | **87.0 / 100** | Grade A+ (*Sector Average: 68.0*) |
| **Task Completion Rate** | **100%** | Standard home automation tasks |
| **Conflict Resolution Success** | **80%** | Resolution of runtime energy overloads via What-If dialogs |
| **Response Latency** | **< 300 ms** | Asynchronous simulation feedback via StateFlow |

---

## 🏛️ System Architecture

The application is built strictly around **Clean Architecture** and **Reactive MVVM** principles to decouple business logic from the Android UI framework.

```mermaid
graph TD
    subgraph UI ["Presentation Layer (MVVM)"]
        Activity["Activities & Fragments<br/>(ViewBinding, Material Design 3)"]
        VM["ViewModels<br/>(StateFlow / SharedFlow)"]
        Activity -->|Observes UI State| VM
    end

    subgraph Domain ["Domain Layer (Framework-Agnostic)"]
        UC1["SimulateWhatIfUseCase"]
        UC2["ResolveConflictUseCase"]
        UC3["ManageDevicesUseCase"]
        VM -->|Invokes| UC1
        VM -->|Invokes| UC2
        VM -->|Invokes| UC3
    end

    subgraph Data ["Data & Network Layer"]
        Repo["DigitalTwinRepositoryImpl"]
        Retrofit["Retrofit REST API<br/>(Moshi JSON)"]
        DataStore["DataStore Preferences<br/>(Local Cache)"]
        UC1 --> Repo
        UC2 --> Repo
        UC3 --> Repo
        Repo --> Retrofit
        Repo --> DataStore
    end

    subgraph External ["Physical / Simulated Environment"]
        DT["Digital Twin Runtime Server<br/>(PRIN EUD4GSH IoT Hub)"]
        Retrofit <-->|HTTP / REST (JSON)| DT
    end
```

### Architectural Highlights
- **Domain Layer Isolation:** All business rules and Digital Twin negotiation transactions (`SimulateWhatIfUseCase`, `CheckAuthStateUseCase`) are encapsulated in pure Kotlin UseCases.
- **Dependency Injection with Dagger Hilt:** Modularized into `NetworkModule`, `RepositoryModule`, and `DataSourceModule` for high unit-testability and mock injection.
- **Asynchronous State Streams:** UI state is managed via `StateFlow` and `SharedFlow`, guaranteeing lifecycle-aware state delivery without memory leaks.

---

## ✨ Core Features

1. **Real-Time Energy Dashboard:** Instant visualization of household power consumption, active devices, and solar/grid distribution.
2. **What-If Simulation Engine:** Test automation rules before activation to identify potential peak overloads.
3. **Dynamic Conflict Negotiation:** Interactive dialogs propose alternative execution schedules or lower-power device alternatives when grid limits are threatened.
4. **Device Management:** Full granular control of IoT smart appliances and sub-metered sockets.

---

## 📱 UI Showcase

| Real-Time Dashboard | IoT Devices Grid |
| :---: | :---: |
| <img src="media/dashboard.png" width="360" alt="Dashboard View" /> | <img src="media/devices.png" width="360" alt="Devices Grid" /> |

| What-If Automation Config | Dynamic Conflict Negotiation |
| :---: | :---: |
| <img src="media/what-if-action.png" width="360" alt="What-If Configuration" /> | <img src="media/conflict-dialog.png" width="360" alt="Conflict & Negotiation Dialog" /> |

---

## 🛠️ Tech Stack

- **Language:** Kotlin 2.0+
- **Architecture:** Clean Architecture + MVVM
- **UI Framework:** Android ViewBinding, XML layouts, Navigation Component, Material Design 3
- **Concurrency & Asynchrony:** Kotlin Coroutines, StateFlow / SharedFlow
- **Dependency Injection:** Dagger Hilt
- **Networking & Persistence:** Retrofit 2, Moshi, AndroidX DataStore Preferences
- **Build System:** Gradle (Kotlin DSL), Target SDK 35, Min SDK 34

---

## 🚀 Getting Started

### Prerequisites
- **Android Studio** (Koala / Ladybug or newer)
- **JDK 17+**
- Android Emulator or Physical Device running **Android 14+** (API 34+)

### Installation & Run
```bash
# 1. Clone the repository
git clone https://github.com/hidemet/App-DT.git

# 2. Navigate to project root
cd App-DT

# 3. Open in Android Studio and sync Gradle dependencies
# 4. Build and Run on your target device / emulator
./gradlew installDebug
```

---

## 📄 License & Academic Context

Developed by **Nicholas Dumas** as a Master's Thesis in Computer Engineering at **Università degli Studi di Brescia**.  
Released under the [MIT License](LICENSE).
