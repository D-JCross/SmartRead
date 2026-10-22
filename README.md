# SmartRead

**SmartRead** is a Windows desktop PDF reader built in **C++17 with Qt 6**. It combines a traditional, page-based PDF viewing experience with offline text-to-speech narration so users can listen to a document while following along visually.

The project began as an AI-assisted C++ graphical user interface assignment. Its guiding idea is simple: **keep the PDF looking like a PDF**, make the reading experience nicer.

> \*\*Project status:\*\* SmartRead has been developed and packaged for Windows. The feature descriptions below document the design and development requirements discussed throughout the project; availability and reliability of individual behaviors should be checked against the specific release being downloaded. In particular, word-highlight synchronization after repeated click-to-read actions was an area of active debugging.

## Features and intended behavior

### Original PDF viewing

* Open **text-based PDF documents** without first converting them to a different format.
* Render the original pages rather than substituting a plain-text-only reading view.
* Navigate PDF content while keeping narration and on-page reading position connected.

### Read aloud

* Speak extracted PDF text using **Qt TextToSpeech** and installed Windows speech voices.
* Provide **Play, Pause, Resume, Stop**, and a **reading-speed control**.
* Allow a reader to click a word on a PDF page and start narration at that position.
* Follow spoken words using **one yellow word-highlight rectangle**, with scrolling and continuation of speech across page boundaries.

### Two reading modes

|Mode|Purpose|
|-|-|
|**Story Mode**|Focus on narrative content; attempt to skip recurring page headers, footers, and page numbers. Start at a prologue when found, or at the first detected narrative chapter even if it is not labeled Chapter 1. Announce chapter titles.|
|**Read Everything**|Read the PDF's extracted text without Story Mode's narrative filtering.|

Story Mode relies on information available in the PDF's text and layout. PDFs are inconsistent, so chapter recognition and removal of repeated page furniture should be understood as **best-effort**, not guaranteed for every book.

## Technology stack

|Component|Technology|
|-|-|
|Language|C++17|
|UI|Qt 6 Widgets|
|PDF rendering and text|Qt PDF (`QPdfDocument`, `QPdfSelection`)|
|Speech|Qt TextToSpeech, Windows-installed voices|
|Build configuration|CMake|
|Windows development|Qt Creator, MinGW, Ninja/CMake|
|Primary target|Windows 64-bit|

The development environment discussed for this project used **Qt Creator 20.0.2**, **Qt 6.12.0**, and the **MinGW 13.1.0 64-bit** kit. These are documented development versions, not proof that every other Qt 6 configuration has been tested.

## Download and run on Windows

**For users who just want to launch SmartRead, use the complete Windows deployment package. You do not need Qt Creator or a C++ compiler.**

1. Download the packaged SmartRead ZIP from this repository or its GitHub Releases page, **if a release has been published**.
2. Extract the **entire archive** to a folder.
3. Open the extracted folder and run **`SmartRead.exe`**.
4. Keep the accompanying DLLs and plugin folders beside the executable. **Do not move the executable out of its deployment folder by itself.**

A deployed Windows package may contain `Qt6Core.dll`, `Qt6Gui.dll`, `Qt6Widgets.dll`, `Qt6Pdf.dll`, `Qt6TextToSpeech.dll`, related Qt/MinGW runtime files, FFmpeg libraries, and plugin directories such as `platforms/`, `imageformats/`, `styles/`, `multimedia/`, and `texttospeech/`. The exact files depend on the built application and Qt deployment results.

**Security note:** SmartRead may be unsigned. Verify the origin of any downloaded executable before running it.

## Build from source

### Requirements

* Windows 64-bit
* Qt Creator with a compatible **Qt 6 MinGW 64-bit kit**
* Qt modules used by the source, including **Widgets**, **Pdf**, and **TextToSpeech** (and any additional modules listed in `CMakeLists.txt`)
* CMake and a compatible C++17 compiler; Ninja is suitable when configured through Qt Creator

### Recommended: Qt Creator

1. Locate the **original source project** and open its `CMakeLists.txt` in Qt Creator.
2. Choose the installed Qt 6 MinGW 64-bit kit.
3. Allow CMake to configure the project.
4. Select a **Release** configuration when preparing a distributable build, or **Debug** while developing.
5. Build and run SmartRead from Qt Creator.

The source project discussed during development contained these core files:

```text
CMakeLists.txt
main.cpp
mainwindow.h
mainwindow.cpp
mainwindow.ui       # if retained by the source version
```

Additional resources or modules may be present in the actual repository. **The deployed executable directory is not automatically the complete source project.** A repository should include the original C++ source and build configuration so another developer can inspect and rebuild the application.

### Create a standalone Windows deployment

For the Qt 6.12.0 MinGW setup used during development, one approach is:

1. Build SmartRead in **Release** mode.
2. Copy the resulting `SmartRead.exe` into an empty deployment folder.
3. In a Windows terminal, change to that deployment folder and run:

```bat
"C:\\Qt\\6.12.0\\mingw\_64\\bin\\windeployqt.exe" --release --compiler-runtime SmartRead.exe
```

4. Confirm the required Qt and speech plugins and runtime libraries were copied.
5. Test the deployed application **outside Qt Creator**, ideally on a Windows computer without the development environment installed.
6. ZIP the entire working deployment folder and distribute it as one package.

`windeployqt` handles many dependencies, but it does **not** guarantee a fully working package on its own. Check the actual output and include any runtime dependencies required by the chosen Qt build. Also review the licenses and distribution requirements of bundled third-party components.

## How SmartRead works

SmartRead's design combines four main tasks:

1. **PDF presentation:** Qt PDF renders the original document pages.
2. **Text extraction and selection:** PDF text and word positions make it possible to choose a starting point for reading and locate highlights.
3. **Narration:** Qt TextToSpeech sends text through an installed Windows voice and exposes speech state/progress information.
4. **Reading coordination:** Application logic connects the current spoken position to the corresponding document word, highlights it, and moves through subsequent text and pages.

The reading modes control which text is selected for narration. Story Mode attempts to follow the structure of a book, while Read Everything takes a less selective approach.

### A challenging synchronization bug

An important issue discovered during development involved **click-to-read and highlighting becoming out of sync**. The spoken narration could start at the correct word, but the yellow rectangle sometimes jumped to a different location after the user clicked a second word. This distinction helped narrow the investigation: the problem was not necessarily PDF text selection or speech starting at the wrong word, but potentially how the active speech request and word-position updates were coordinated.

## Limitations

* **Text-based PDFs:** Scanned pages and image-only PDFs are not currently documented as supported. OCR was not established as an implemented feature.
* **PDF layout differences:** Text extraction order, word coordinates, unusual typography, and book structure can affect narration, chapter detection, and highlighting.
* **Windows speech:** Voices depend on what is installed and available through the Windows/Qt speech backend.
* **Platform support:** Windows is the supported distribution target. Source portability to Linux or macOS does not mean compatible builds have been produced or tested.
* **Version differences:** Features and bugs may differ between development builds. Consult the actual released binary and source code for the precise implementation.

## Development process and AI assistance

SmartRead was built for an assignment that **required AI-assisted development**. Rather than handing control of the project to an AI agent integrated into the IDE, I used **ChatGPT in a separate conversation** and worked directly in **Qt Creator**.

That workflow let me:

* Decide which features SmartRead should have and how the reading experience should feel.
* Review suggested approaches and full replacement source files before making changes.
* Build and test changes myself in Qt Creator.
* Report compiler errors and specific behaviors back to ChatGPT for further diagnosis.
* Compare new versions against earlier working versions instead of accepting unrelated rewrites.

I found that detailed instructions were more helpful than broad requests. One lesson was to **preserve what already works while fixing a specific problem**. At times, a change intended to repair synchronization altered unrelated functionality or produced incomplete source files. That made debugging, testing, maintaining working versions, and checking complete files especially important.

Using a separate chat was a **workflow preference for visibility and deliberate review**, not a claim that IDE-integrated AI tools cannot offer approval or edit controls. I remained responsible for making design decisions, applying changes, checking results, and deciding what belonged in the final application.

The project demonstrates practical C++ GUI development as well as the judgment required to use AI-generated code critically.

## Repository and distribution notes

During deployment, a GitHub Desktop working copy was maintained at:

```text
C:\\Users\\Admin\\Documents\\SmartRead\\SmartRead
```

This path was used for a **copied deployed application**, not necessarily the original Qt Creator source tree. Keep the distinction clear when organizing this repository.

A complete submission should provide:

* Original C++ source files and CMake configuration.
* Documentation such as this README and `AGENTS.md`.
* Genuine application screenshots, if provided.
* A tested, complete Windows distributable package (preferably as a ZIP or GitHub Release).
* Assignment reflection and demonstration material as required by the course.

Some executable, DLL, and plugin files were ignored by Git during development. Before publishing a Windows package, confirm that every necessary runtime file is actually included in the distributed ZIP. **Do not assume a successful Git push contains ignored files.** It is reasonable to keep binaries out of the source tree while distributing them separately as a release artifact.

## Author

**Cross Dzikiewicz**  
Computer Science student — SUNY Broome Community College

## License

No license for the original SmartRead source code has been specified. Do not assume the application is open source merely because its files are visible on GitHub. Qt and all other third-party libraries remain under their respective licenses and redistribution conditions.

