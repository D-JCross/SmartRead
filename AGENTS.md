# AGENTS.md — SmartRead

SmartRead is a Windows PDF reader that can read books aloud. I want it to feel like a normal PDF viewer, with the original pages visible.

### Features to Preserve

* Click any word and start reading from that word.
* Highlight the word being spoken with one yellow rectangle.
* Scroll smoothly with the narration and continue onto the next page.
* Keep Play, Pause, Resume, Stop, and reading-speed controls working.
* Keep Story Mode for skipping repeated headers, footers, and page numbers, alongside Read Everything mode.

## Technical Conventions

I use **C++17, Qt 6 Widgets, Qt PDF, Qt TextToSpeech, CMake, Qt Creator, and MinGW on Windows**.

The main source files are:

* `main.cpp`
* `mainwindow.h`
* `mainwindow.cpp`
* `CMakeLists.txt`

## Instructions for Code Changes

1. **Provide complete replacements.** When changing code, provide a complete working replacement. Put the code directly in the chat and include the exact Windows path for each file.
2. **Make sure nothing is cut off.** Check the includes, declarations, matching header, and closing braces before sending anything. I've had incomplete files cause hundreds of compiler errors.
3. **Fix only what I reported.** Don't rewrite or change anything except the task we're focused on. Keep edits as small as possible.
4. **Figure out why before guessing.** I will provide current source and compiler output. Use that information, ask specific questions when needed, and explain the likely cause before replacing code. If I say to diagnose first, don't send new code yet.
5. **Don't claim it works without proof.** If code hasn't been compiled or tested in my Qt environment, say that. A plausible fix isn't the same thing as a verified fix.
6. **Respect earlier working versions.** If I say version 1.4 worked better, use that actual code as a reference. Don't reconstruct a different version and call it the original. Preserve working versions.

## What My Testing Taught Me

The hardest part of developing SmartRead was implementing new features without ChatGPT accidentally breaking parts of the application that already worked. I often asked for one specific change, but the AI would rewrite unrelated sections of code, sometimes introducing new bugs or removing functionality I had spent hours getting right. Because of this, I learned to regularly save backup versions of my working code before making changes. I ended up reverting to these backups quite often when AI-generated modifications caused more problems than they solved. This taught me the importance of version control, testing changes individually, and maintaining control over my project instead of blindly trusting every solution the AI provided.

