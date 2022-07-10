---
uid: admin-building
title: Збірка з джерельного коду
---
<!-- markdownlint-disable MD029 ol-prefix -->

# Джерельний код

Як альтернатива використанню [binary packages](xref:admin-installing), ви можете зібрати Jellyfin з джерельного коду.

Jellyfin підтримує кілька методів створення для різних платформ, і інструкції для всіх підтримуваних платформ наведено нижче.

Усі збірки пакетів починаються з цих двох кроків:

1. Клонуйте репозиторій.

    ```sh
    git clone https://github.com/jellyfin/jellyfin.git
    cd jellyfin
    ```

2. Ініціалізуйте підмодулі.

    ```sh
    git submodule update --init
    ```

## Образ контейнеру

1. Створіть образ контейнера за допомогою Docker або Podman.

    ```sh
    docker build -t $USERNAME/jellyfin .
    ```

    or

    ```sh
    podman build -t $USERNAME/jellyfin .
    ```

2. Запустіть Jellyfin у новому контейнері за допомогою Docker або Podman зі створеного образу контейнера.

    ```sh
    docker run -d -p 8096:8096 $USERNAME/jellyfin
    ```

    or

    ```sh
    podman run -d -p 8096:8096 $USERNAME/jellyfin
    ```

## Linux чи MacOS

3. Використовуйте сценарій `build`, що входить до комплекту, для виконання збірок.

    ```sh
    ./build --help
    ./build --list-platforms
    ./build <platform> all
    ```

4. Отримані архіви можна знайти за адресою `../bin/<platform>`.

> [!ПРИМІТКА]
> Дуже ймовірно, що в майбутньому це буде виділено в окреме сховище.

## Windows

3. Установіть Dotnet SDK 6.0 з [Microsoft's Website](https://dotnet.microsoft.com/en-us/download/dotnet/6.0) і [install Git for Windows](https://gitforwindows.org/).
   Ви повинні мати Powershell 3 або новішої версії.

4. У Powershell установіть політику виконання на необмежену.

    ```powershell
    set-executionpolicy unrestricted
    ```

5. Якщо ви створюєте версію Jellyfin, новішу за 10.6.4, вам потрібно буде завантажити сценарій збірки з окремого репозиторію.

   ```powershell
   git clone https://github.com/jellyfin/jellyfin-server-windows.git windows
   ```

6. Запустіть сценарій збірки Jellyfin.

    ```powershell
    windows\build-jellyfin.ps1 -verbose
    ```

    * Прапорці `-WindowsVersion` і `-Architecture` можуть оптимізувати збірку для вашого поточного середовища; за замовчуванням — стандартна Windows x64.

    * Прапорець `-InstallLocation` дозволяє вибрати, куди зберігатимуться скомпільовані двійкові файли; типовим є `$Env:AppData\Jellyfin-Server\`.

    * Прапор `-InstallFFMPEG` автоматично витягне стабільні двійкові файли `ffmpeg`, які відповідають вашій архітектурі (наразі лише x86/x64) з [BtbN](https://github.com/BtbN/FFmpeg-Builds/releases) і розмістить їх у вашому каталозі Jellyfin.

    * Прапорець `-InstallNSSM` автоматично витягне стабільний двійковий файл `nssm`, який відповідає вашій архітектурі (наразі лише x86/x64) з [веб-сайту NSSM](https://nssm.cc/) і розмістить його у вашому каталозі Jellyfin.

7. (Необов’язково) Використовуйте [NSSM](https://nssm.cc), щоб налаштувати Jellyfin для роботи як служби.

8. Jellyfin тепер доступний у каталозі за замовчуванням або в будь-якому іншому каталозі, який ви вибрали.

    * Запустіть його з PowerShell.

        ```powershell
        &"$env:APPDATA\Jellyfin-Server\jellyfin.exe"
        ```

    * Запустіть його з CMD.

        ```cmd
        %APPDATA%\Jellyfin-Server\jellyfin.exe
        ```

> [!ПРИМІТКА]
> Дуже ймовірно, що в майбутньому це буде виділено в окреме сховище.
