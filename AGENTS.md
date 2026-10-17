

AGENTS.md — SmartRead

How I want AI to work

I decide what it should do, test the results, and decide whether a change is good enough. AI is a tool I use to write code, troubleshoot problems, and improve the app. I expect it to follow my instructions and did not directly allow it to change any files on its own.



My main rule: If something works, don't break it trying to fix something else.



What I'm building

SmartRead is a Windows PDF reader that can read books aloud. I want it to feel like a normal PDF viewer, with the original pages visible, not a separate wall of extracted text.



The features I want preserved are:



Click any word, including at the bottom of a page, and start reading from that word.

Highlight the word being spoken with one yellow rectangle, not a different highlighting style.

Scroll smoothly with the narration and continue onto the next page.

Keep Play, Pause, Resume, Stop, and reading-speed controls working.

Keep Story Mode for skipping repeated headers, footers, and page numbers, alongside Read Everything mode.



Technical conventions

I use C++17, Qt 6 Widgets, Qt PDF, Qt TextToSpeech, CMake, Qt Creator, and MinGW on Windows. The main source files are main.cpp, mainwindow.h, mainwindow.cpp, and CMakeLists.txt.



Instructions for code changes:



Give me full file paths to prevent confusion.

When changing code, provide the complete replacement, not snippets or patches.

Put the code directly in the chat, and include the exact Windows path for each file.

Make sure nothing is cut off. Check the includes, declarations, matching header, and closing braces before sending anything. I've had incomplete files cause hundreds of compiler errors.

Fix what I actually reported. Don't rewrite or change anything but the task we are focused on. 

Keep edits as small as possible.

Figure out why before guessing. I will provide current source and compiler output, ask specific questions when needed, and explain the likely cause before replacing code. If I say to diagnose first, don't send new code yet.

Don't claim it works without proof. If code hasn't been compiled or tested in my Qt environment, say that. A plausible fix isn't the same thing as a verified fix.

Respect earlier working versions. If I say version 1.4 worked better, use that actual code as a reference. Don't reconstruct a different version and call it the original. Preserve working versions.



What my testing taught me:



The hardest bug was the word highlight jumping to random parts of a PDF. The voice still read from the word I clicked correctly. After testing, I found that the first click worked, but problems appeared when I clicked a second time. That mattered because it pointed toward speech-session handling, not a general failure to start narration.



I wanted the AI to pay attention to those details instead of repeatedly replacing large sections of code. One fix changed the highlighting style I liked; other replacements broke clicking or scrolling. I also ran into truncated .cpp files. Those experiences are why I insist on targeted debugging and complete source files.



