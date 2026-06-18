# Hitbox Highlight — Build Instructions (updated)

Sorry about the `gradlew.bat` error — I had included the *config* for the
Gradle wrapper but not the actual wrapper scripts/jar. That's fixed now,
with one caveat explained below. Everything here is still source code that
needs a one-time compile step; I have no way to hand you a pre-built `.jar`
directly (no internet/compiler in my sandbox — see prior messages).

## What's in this zip now
- `gradlew` and `gradlew.bat` — the actual wrapper scripts (now included)
- `gradle/wrapper/gradle-wrapper.properties` — tells the wrapper which
  Gradle version to fetch (8.10)
- **Missing:** `gradle/wrapper/gradle-wrapper.jar` — this one file is
  compiled binary, not text, so I can't write it by hand. See Option A or B
  below to get it — both take under a minute.

## Option D — No installs at all: let GitHub build it for you (recommended)
This avoids the missing-wrapper-jar issue entirely and needs nothing
installed on your computer.
1. Create a free GitHub account if you don't have one: https://github.com/signup
2. Create a new **public** repository (any name).
3. Upload every file from this zip into it (drag-and-drop works on
   github.com, or use git). Keep the folder structure intact, including
   the hidden `.github/workflows/build.yml` file.
4. Go to the **Actions** tab of your repo. A workflow called "Build Mod Jar"
   should already be running (or click "Run workflow" to start it).
5. Wait ~2 minutes for the green checkmark.
6. Click into the finished run → scroll to **Artifacts** →
   download `hitboxhighlight-jar` (it's a zip containing the real `.jar`).

## Option A — Easiest: install Gradle once, let it fix itself
1. Install JDK 21: https://adoptium.net/temurin/releases/?version=21
2. Install Gradle (any recent version): https://gradle.org/install/
   - Windows: `choco install gradle` (if you have Chocolatey), or download
     the zip from gradle.org and add its `bin` folder to PATH
   - Mac: `brew install gradle`
   - Linux: `sdk install gradle` (via SDKMAN) or your package manager
3. In the unzipped project folder, run:
   ```
   gradle wrapper
   ```
   This generates the missing `gradle-wrapper.jar` locally in seconds.
4. Now run the actual build:
   ```
   ./gradlew build        (Mac/Linux)
   gradlew.bat build      (Windows)
   ```
5. Your jar appears at `build/libs/hitboxhighlight-1.0.0.jar`

## Option B — Skip the wrapper entirely
If you did Option A and already have Gradle installed system-wide, you can
just skip `gradlew` altogether and build directly:
```
gradle build
```
This uses your installed Gradle instead of the wrapper, so the missing
wrapper jar doesn't matter at all.

## Option C — Open in IntelliJ IDEA (no command line needed)
1. Install IntelliJ IDEA Community (free) + the Fabric/Minecraft Development
   plugin (optional but helpful)
2. File → Open → select the unzipped project folder
3. IntelliJ detects the `build.gradle`, prompts to import, and will
   auto-generate the wrapper jar and download all dependencies for you
4. Use the Gradle panel on the right → Tasks → build → `build` (double-click)
5. Jar appears at `build/libs/hitboxhighlight-1.0.0.jar`

## Installing the built mod in Minecraft
1. Install Fabric Loader for Minecraft **1.21.4**: https://fabricmc.net/use/installer/
2. Download **Fabric API 0.110.5+1.21.4** from Modrinth/CurseForge, put it in
   your `mods` folder
3. Put `hitboxhighlight-1.0.0.jar` in the same `mods` folder
4. Launch via the Fabric profile

## What the mod does (recap)
Raycasts from your camera each frame, same reach + same block-occlusion
rules vanilla uses for hitting entities. If your crosshair lands on another
player within ~6 blocks with nothing blocking the view, it draws a blue
wireframe matching that player's exact bounding box. Nothing renders
through walls or for players you aren't directly looking at.

Tweak color/range/line width in:
`src/client/java/com/example/hitboxhighlight/HitboxHighlightConfig.java`
