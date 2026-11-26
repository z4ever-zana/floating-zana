name: zana.apk
on:
  workflow_dispatch:
  push:
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Siapkan proyek
        run: |
          if [ -f FloatingAI.zip ]; then unzip -q FloatingAI.zip; fi
          if [ -d FloatingAI ]; then cd FloatingAI; fi
          echo "PROJECT_DIR=$(pwd)" >> $GITHUB_ENV
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: 8.7
      - name: Build APK
        working-directory: ${{ env.PROJECT_DIR }}
        run: gradle assembleDebug --no-daemon
      - uses: actions/upload-artifact@v4
        with:
          name: FloatingAI-APK
          path: ${{ env.PROJECT_DIR }}/app/build/outputs/apk/debug/app-debug
