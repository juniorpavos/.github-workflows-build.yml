name: Build APK
on: [push, workflow_dispatch]
jobs:
  build:
    runs-on: ubuntu-latest
    steps:
      - uses: actions/checkout@v4
      - name: Descomprimir proyecto
        run: unzip -o proyecto.zip
      - uses: actions/setup-java@v4
        with:
          distribution: temurin
          java-version: 17
      - uses: gradle/actions/setup-gradle@v4
        with:
          gradle-version: "8.9"
      - name: Descargar diccionario
        run: |
          mkdir -p app/src/main/assets
          curl -L -o app/src/main/assets/words.txt https://raw.githubusercontent.com/dwyl/english-words/master/words_alpha.txt
          curl -L -o app/src/main/assets/common.txt https://raw.githubusercontent.com/first20hours/google-10000-english/master/google-10000-english-no-swears.txt
      - name: Compilar
        run: gradle assembleDebug
      - uses: actions/upload-artifact@v4
        with:
          name: ayudante-apk
          path: app/build/outputs/apk/debug/app-debug.apk
          
