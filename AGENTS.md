AGENTS.md — SmartRead

How I want AI to work

I decide what it should do, test the results, and decide whether a change is good enough. AI is a tool I use to write code, troubleshoot problems, and improve the app. I expect it to follow my instructions and did not directly allow it to change any files on its own.



My main rule: If something works, don't break it trying to fix something else.



What I'm building

SmartRead is a Windows PDF reader that can read books aloud. I want it to feel like a normal PDF viewer, with the original pages visible, not a separate wall of extracted text.



The features I want preserved are:



Click any word, and start reading from that word.

Highlight the word being spoken with one yellow rectangle.

Scroll smoothly with the narration and continue onto the next page.

Keep Play, Pause, Resume, Stop, and reading-speed controls working.

Keep Story Mode for skipping repeated headers, footers, and page numbers, alongside Read Everything mode.



Technical conventions

I use C++17, Qt 6 Widgets, Qt PDF, Qt TextToSpeech, CMake, Qt Creator, and MinGW on Windows. The main source files are main.cpp, mainwindow.h, mainwindow.cpp, and CMakeLists.txt.



Instructions for code changes:



When changing code, provide a complete working replacement,

Put the code directly in the chat, and include the exact Windows path for each file.

Make sure nothing is cut off. Check the includes, declarations, matching header, and closing braces before sending anything. I've had incomplete files cause hundreds of compiler errors.

Fix what I actually reported. Don't rewrite or change anything but the task we are focused on.

Keep edits as small as possible.

Figure out why before guessing. I will provide current source and compiler output, ask specific questions when needed, and explain the likely cause before replacing code. If I say to diagnose first, don't send new code yet.

Don't claim it works without proof. If code hasn't been compiled or tested in my Qt environment, say that. A plausible fix isn't the same thing as a verified fix.

Respect earlier working versions. If I say version 1.4 worked better, use that actual code as a reference. Don't reconstruct a different version and call it the original. Preserve working versions.



What My Testing Taught Me:



The hardest part of developing SmartRead was implementing new features without ChatGPT accidentally breaking parts of the application that already worked. I often asked for one specific change, but the AI would rewrite unrelated sections of code, sometimes introducing new bugs or removing functionality I had spent hours getting right. Because of this, I learned to regularly save backup versions of my working code before making changes. I ended up reverting to these backups quite often when AI-generated modifications caused more problems than they solved. This taught me the importance of version control, testing changes individually, and maintaining control over my project instead of blindly trusting every solution the AI provided.

