# Android Video Streaming App

A Netflix-style Android app built with Kotlin and Jetpack Compose, backed by a Ktor mock REST API.

## Features

- **Home**: featured banner and horizontally scrolling poster rows
- **Detail**: video info; movies get a Play button, TV shows get an episode list
- **Favorites**: saved locally with Room and kept across app restarts
- **My Netflix**: profile, recently viewed (from the API) and local favorites
- **Player**: fullscreen landscape playback with Media3 ExoPlayer
- Loading, error and retry states on every screen

## Tech stack

| Layer | Tools |
|---|---|
| UI | Jetpack Compose, Material 3, Navigation Compose, Coil |
| Architecture | MVVM (ViewModel + StateFlow), Repository pattern, Hilt |
| Data | Retrofit + OkHttp + Gson, Kotlin Coroutines, Room + Flow |
| Media | Media3 ExoPlayer |
| Backend | Ktor 3 (Netty), kotlinx.serialization |
| Build | Gradle version catalog, KSP, R8 |

## Architecture

```
Compose Screen  <- StateFlow -  ViewModel  ->  Repository  ->  Retrofit (network) / Room (local)
                         dependencies provided by Hilt
```

## Results

Enabling R8 code and resource shrinking, with keep rules for Gson/Retrofit reflection, cut the release APK from **15.1 MB to 2.3 MB (−85%)**. Every flow was verified on the minified build.

## Project layout

```
app/        Android client (com.laioffer.netflix)
backend/    Ktor mock API: GET /home, POST /videoDetail, GET /profile, /media/*.mp4
```

## Run locally

1. Start the backend: open `backend/` in IntelliJ IDEA and run `Application.kt`, or run `./gradlew run` there. It listens on port 8080 and adds 300–500 ms of simulated latency to each request.
2. Open the project root in Android Studio and run the `app` configuration on an emulator. The app reaches the host machine at `http://10.0.2.2:8080/`.
