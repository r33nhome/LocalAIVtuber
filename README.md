# Local AI Vtuber (Ein Tool für KI-Vtubers, das vollständig lokal und offline läuft)

Vollständiges Demo- und Setup-Video: https://youtu.be/Yl-T3YgePmw?si=n-vaZzClw0Q833E5

- Chatbot, Übersetzung und Text-to-Speech (TTS), alles komplett kostenlos und lokal auf deinem PC.
- Unterstützt Sprachausgabe in Deutsch, Englisch, Japanisch, Spanisch, Französisch, Russisch und mehr, angetrieben von RVC, Silero und Voicevox.
- Beinhaltet ein speziell trainiertes KI-Modell, um generische Chatbot-Antworten zu vermeiden und den Charakter beizubehalten.
- Weboberfläche über Gradio UI.
- Plugin-Unterstützung, um einfach weitere Anbieter (z.B. für andere KIs oder Stimmen) hinzuzufügen.

<table>
  <tr>
    <td><img src="https://github.com/0Xiaohei0/VtuberChess/assets/24196833/6433bc1f-cdec-423f-b190-b7330497d28e" /></td>
    <td><img src="https://github.com/0Xiaohei0/VtuberChess/assets/24196833/5521eff5-4b36-4b13-9961-f4d7af8daded" /></td>
  </tr>
</table>

## Verbesserte Installationsanleitung für Windows

Diese Anleitung wurde überarbeitet, um eine reibungslosere Installation unter Windows zu gewährleisten. Sie basiert auf der Annahme, dass du diesen Fork (eine verbesserte Version) des Projekts verwendest, der bereits eine reparierte `requirements.txt` enthält.

### Phase 1: Vorbereitung (Die Basis-Software)

Bevor du startest, stelle sicher, dass die folgende Software installiert ist:

1.  **Python 3.10:** Lade es von der [offiziellen Python-Webseite](https://www.python.org/downloads/release/python-3100/) herunter.
    - **WICHTIG:** Setze bei der Installation den Haken bei **"Add Python 3.10 to PATH"**.

2.  **NVIDIA CUDA Toolkit (Optional, für NVIDIA-GPUs):** Wenn du eine starke NVIDIA-Grafikkarte hast, installiere das [CUDA Toolkit 12.1 oder neuer](https://developer.nvidia.com/cuda-toolkit-archive).
3.  **Visual Studio:** Installiere die kostenlose [Visual Studio Community Edition](https://visualstudio.microsoft.com/downloads/).
    - **WICHTIG:** Wähle bei der Installation die Komponente **"Desktopentwicklung mit C++"** aus.
4.  **Git:** Installiere [Git für Windows](https://git-scm.com/download/win).

### Phase 2: Projekt-Installation

1.  **Projekt klonen:** Öffne die Windows-Kommandozeile (`cmd`) und führe diesen Befehl aus, um das Projekt herunterzuladen:
    ```    git clone https://github.com/DEIN-GITHUB-NAME/LocalAIVtuber.git
    ```
    *(Ersetze `DEIN-GITHUB-NAME` durch deinen tatsächlichen GitHub-Benutzernamen)*.
    Wechsle danach in den neuen Ordner: `cd LocalAIVtuber`

2.  **Virtuelle Umgebung erstellen:**
    ```
    python -m venv venv
    .\venv\Scripts\activate
    ```
    *(Falls ein Fehler auftritt, dass Skripte nicht ausgeführt werden können, öffne PowerShell als Administrator und führe `Set-ExecutionPolicy RemoteSigned` aus und bestätige mit 'J'.)*

3.  **Manuelle Installation von `pyopenjtalk` (WICHTIGSTER SCHRITT):**
    - Ein benötigtes Paket (`pyopenjtalk`) kann nicht automatisch gebaut werden. Wir installieren es daher manuell.
    - Gehe auf diese Webseite: [Gohlke's Python Libraries](https://www.lfd.uci.edu/~gohlke/pythonlibs/#pyopenjtalk)
    - Drücke `Strg + F` und suche nach `pyopenjtalk`.
    - Lade die Datei herunter, die exakt so heißt: **`pyopenjtalk‑0.3.3‑cp310‑cp310‑win_amd64.whl`**
    - Speichere diese `.whl`-Datei direkt in deinem Projektordner (`LocalAIVtuber`).
    - Führe in deiner aktiven Kommandozeile diesen Befehl aus:
      ```
      pip install pyopenjtalk-0.3.3-cp310-cp310-win_amd64.whl
      ```

4.  **Restliche Pakete installieren:**
    - Die `requirements.txt` in diesem Fork ist bereits repariert. Führe einfach aus:
      ```
      pip install -r requirements.txt
      ```

### Phase 3: Erste Konfiguration (Wichtige Fixes)

Bevor du das Programm startest, müssen wir zwei Dinge manuell anpassen, um eine gute Erfahrung auf Deutsch zu gewährleisten.

1.  **Deutsche Spracheingabe reparieren:**
    - Standardmäßig versteht das Programm nur Englisch. Um das zu beheben:
    - Öffne die Datei: `plugins\VoiceInput\VoiceInput.py` mit einem Texteditor.
    - Finde die Zeile (ca. Zeile 21): `self.model = whisper.load_model("small.en")`
    - Ändere sie zu: `self.model = whisper.load_model("medium")`
    - Speichere die Datei.

2.  **Sprachausgabe (TTS) aktivieren:**
    - Standardmäßig sind alle Sprachausgabe-Plugins deaktiviert.
    - Öffne die Datei `modules.json` mit einem Texteditor.
    - Ändere die Zeile `"silero": false,` zu `"silero": true,`.
    - Speichere die Datei.

### Phase 4: Programmstart

1.  **Programm starten:**
    ```    python main.py
    ```
    - Beim ersten Start werden die KI-Modelle (wie das `medium`-Whisper-Modell) heruntergeladen. Dies kann je nach Internetverbindung einige Minuten dauern. Sei geduldig und beobachte die Kommandozeile.

2.  **Weboberfläche öffnen:**
    - Sobald du die Nachricht `Running on local URL: http://127.0.0.1:7860` siehst, ist das Programm bereit.
    - Öffne deinen Webbrowser und gehe zu: **http://127.0.0.1:7860**

### Hinweise

#### Programm neu starten
Um das Programm später erneut zu starten, führe einfach diese Befehle im Projektordner aus:
.\venv\Scripts\activate
python main.py
Generated code
#### LLM auf der GPU ausführen (für bessere Performance)
Wenn du eine starke NVIDIA-GPU hast, kannst du die KI-Berechnungen beschleunigen. Installiere `llama-cpp-python` mit CUDA-Unterstützung mit diesen Befehlen:
*In der Kommandozeile (cmd):*
set CMAKE_ARGS=-DGGML_CUDA=ON
pip install llama-cpp-python --force-reinstall --no-cache-dir --verbose
Generated code
*In PowerShell:*
$env:CMAKE_ARGS ="-DGGML_CUDA=ON"
pip install llama-cpp-python --force-reinstall --no-cache-dir --verbose
Generated code
## Zukünftige Pläne (Projekt-Roadmap)
- [x] Chat-Input von Streaming-Plattformen abrufen
- [x] Lokales LLM verbessern (Feinabgestimmtes Modell verfügbar)
- [x] Plugins für Cloud-Anbieter schreiben (Azure TTS, Elevenlabs, ChatGPT, Whisper...)
- [x] GPU-Unterstützung
- [x] VTube Studio-Integration
- [ ] KI Spiele spielen und kommentieren lassen (kann derzeit Schach und "Keep Talking and Nobody Explodes")
- [ ] KI singen lassen

## FAQ:
- **NameError: name '_in_projection' is not defined**
  - Du kannst `gpt_sovits` und `rvc` nicht gleichzeitig in der `modules.json` aktivieren. Einige ihrer Module haben Konflikte.

- **UnboundLocalError: local variable 'response' referenced before assignment**
  - Wenn du dieses Repository geklont hast, fehlen dir möglicherweise Modelldateien für `gpt-sovits`. Diese findest du in der ZIP-Datei im [Releases-Bereich des Original-Projekts](https://github.com/0Xiaohei0/LocalAIVtuber/releases). Ersetze den Ordner `plugins\gpt_sovits\models` mit dem aus der ZIP-Datei.

- **Wie nutze ich den YouTube-Chat-Abruf?**
  - Kopiere die `youtube_video_id` aus der URL deines Streams.
  ![image](https://github.com/0Xiaohei0/LocalAIVtuber/assets/24196833/942b9811-46bc-40f9-a7df-7938d0070513)
  - Drücke dann auf "Start Fetching Chat".
  ![image](https://github.com/0Xiaohei0/LocalAIVtuber/assets/24196833/96b8a971-00e8-4930-a9b4-897b3ddf27bf)
