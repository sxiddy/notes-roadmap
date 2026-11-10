Status: PARTIAL 11-11-2026 (stopped early: weekly usage limit). 3 of 13 class options fully listed, 5 counted, 5 not opened.

# VTOP content inventory (Winter Semester 2026-27 courses)

Read-only pass over VTOP "Course Page" in the owner's signed-in browser on 11-11-2026. Nothing was downloaded,
opened or changed. No student or faculty names are recorded (uploaders are F1, F2 ... per table).

## What VTOP shows (and does not)
- One table per course and class type: semester of upload, material title, module tag, uploader, upload date.
- Type is only "Document" or "Web Link". VTOP does not show pdf/ppt/doc, file size or page count. Those are
  unknown until the owner downloads.
- The uploads are from earlier runs of the course (2025-26 and Fall 2026-27) by many faculty, so the same module
  appears 10-20 times in different versions. Volume below is uploads, not unique lectures.
- Organisation: by module tag (0 = general, 1-5 = syllabus module; lab uses experiment numbers). Not by lecture.
- "Course Syllabus Modules" and "Course Book" tabs exist for every course (only BACSE103's syllabus was read).

## Summary
Counts marked "about" come from the page count at 10 rows per page (the rows were not read).

| Course | Class | Type (see SUBJECT-TYPES.md) | Materials listed | Upload semester | Past papers / question banks listed | Read |
|---|---|---|---|---|---|---|
| BACSE103 Computation Structures | ETH | Systems and hardware | 107 | Fall 2025-26 | 4 question-bank files (Qbank, Sample Question Bank, Question Bank module 1, Question bank) | full |
| BACSE103 Computation Structures | ELA | Systems and hardware | 19 | Fall 2025-26 | Qbank, "Assessment-4 questions", lab cycle sheet, OpenMP manual programs | full |
| BACSE104 Structured and OOP | ETH | Programming | 181 (3 are web links) | Winter 2025-26 | D2-CAT1Key, D2-CAT2QP, Quiz 1 and Quiz 2 links, Sample Programs (Folder), C ebook | full |
| BACSE104 Structured and OOP | ELA | Programming | unknown | | unknown | not opened |
| BACSE105 Data Structures and Algorithms | ETH | Programming | about 161-170 | not read | unknown | count only |
| BACSE105 Data Structures and Algorithms | ELA | Programming | unknown | | unknown | not opened |
| BACSE106 Operating Systems | ETH | Systems and hardware | about 91-100 | Winter 2025-26 (first row) | unknown | count only |
| BACSE106 Operating Systems | ELA | Systems and hardware | unknown | | unknown | not opened |
| BAESP101 Spanish Level I | ETH | Language | 0 ("No data available in table") | | none | full |
| BAESP101 Spanish Level I | ELA | Language | unknown | | unknown | not opened |
| BAMAT205 Discrete Mathematics and Linear Algebra | TH | Mathematics | about 161-170 | Summer 2025-26 (first row) | unknown | count only |
| BAPHY105 Engineering Physics | ETH | Physics with lab | about 181-190 | Fall 2026-27 (first row) | unknown | count only |
| BAPHY105 Engineering Physics | ELA | Physics with lab | unknown | | unknown | not opened |

The dropdown lists only these 13 Winter options plus the current Fall courses (Applied Chemistry, Problem Solving
using Python, Basic Engineering, Technical English Communication, Multivariable Calculus and Differential
Equations). There are no further Winter courses below.

## Suggested tier and build order (no demand numbers exist; reasons are effort and risk only)
| Order | Course | Tier | Theory / lab split | Why here |
|---|---|---|---|---|
| 1 | BAPHY105 Engineering Physics | Lite, then Full | ETH + ELA | Closest to the existing shell (Chemistry, Basic Engineering); most material; uploads are from the current semester. Proves the pipeline before any new code. |
| 2 | BACSE104 Structured and OOP | Lite | ETH + ELA | Needs the code-block and code-question work that BACSE105 reuses. CAT papers are listed. C and C++ (titles: pointers, dynamic memory, structures, file handling, templates, STL, exception handling). |
| 3 | BACSE105 Data Structures and Algorithms | Lite + step visualisers | ETH + ELA | Reuses everything from order 2; adds the shared step player. |
| 4 | BACSE103 Computation Structures | Lite + logic tools | ETH + ELA | Fully inventoried; modules map cleanly (M1 13, M2 19, M3 33, M4 16, M5 17 uploads). Needs the simulator core. |
| 5 | BACSE106 Operating Systems | Lite + simulators | ETH + ELA | Reuses the step player and Gantt renderer from orders 3-4. |
| 6 | BAMAT205 Discrete Maths and Linear Algebra | Lite | theory only | Needs the notation decision (KaTeX at build time) first. |
| 7 | BAESP101 Spanish Level I | Lite | ETH + ELA | No material on VTOP: cannot start until the owner supplies a source. |

## BACSE103 syllabus (from the "Course Syllabus Modules" tab)
1. Digital Systems and Combinational Circuits: number systems; fixed and floating point; logic gates; K-map
   simplification; adder, carry look-ahead adder; encoder, decoder; multiplexer, demultiplexer.
2. Sequential Circuits: flip-flops; Mealy and Moore finite state machines; registers; synchronous and
   asynchronous counters with timing sequences.
3. Computer Architectures and Arithmetic Algorithms: Von Neumann and Harvard; Booth and modified Booth
   multiplication; restoring and non-restoring division; instruction set architecture, formats, types,
   addressing modes; instruction cycle; single-cycle and multicycle datapath.
4. Memory and IO Peripherals: memory organisation, hierarchy and design; cache mapping; replacement algorithms;
   programmed and interrupt-driven IO; DMA; shared memory architectures.
5. Advanced Architecture: microcontroller introduction; ARM versions and instruction set; stacks and
   subroutines; instruction-level and thread-level parallelism.
Lab (ELA) titles point to C basics, Arduino pin configuration, microcontroller work and OpenMP programs.

## Missing or needing the owner
1. Finish this inventory (about 15 minutes of browsing): BACSE104 ELA, BACSE105 ETH rows + ELA, BACSE106 ETH rows
   + ELA, BAESP101 ELA, BAMAT205 rows, BAPHY105 ETH rows + ELA, and the syllabus tab of each course.
2. Spanish has no theory material on VTOP. The owner must name a source (faculty handouts, prescribed textbook).
3. Downloads need approval. Suggested: pick ONE faculty's complete set per course (the owner's own slot faculty
   once allotted, otherwise the most complete set) instead of all 100-190 uploads; the rest are duplicates.
4. File formats and sizes are unknown until download. Slides-only versus full notes cannot be told from titles.
5. Past papers: only BACSE103 (question banks) and BACSE104 (CAT1 key, CAT2 paper) list any. For the rest, unknown.
6. Lab manuals: BACSE103 ELA lists a lab cycle sheet and OpenMP manual programs; other labs not opened.
7. The Fall course BAMAT101 (Multivariable Calculus) and BACSE101 (Python, lab only) have no site yet; they fit
   the Mathematics and Programming playbooks if the owner wants them.

## Material lists

### BACSE103 Computation Structures, ETH (107 materials, fully read)

Source semester of the uploads: Fall Semester 2025-26. Dates 04-08-2025 to 19-05-2026. Uploaders: 16 faculty (names left out on purpose).

Per module tag: 0 (Document): 2, 1 (Document): 20, 2 (Document): 19, 3 (Document): 33, 4 (Document): 16, 5 (Document): 17

| # | Title | Module tag (type) | Uploader | Date |
|---|---|---|---|---|
| 1 | M5_Advanced Architecture | 5 (Document) | F1 (SENSE) | 19-05-2026 |
| 2 | M4_Memory and I/O Peripherals | 4 (Document) | F1 (SENSE) | 19-05-2026 |
| 3 | M3_Computer Architectures and Arithmetic Algorithms | 3 (Document) | F1 (SENSE) | 19-05-2026 |
| 4 | M2_Sequential Circuits | 2 (Document) | F1 (SENSE) | 19-05-2026 |
| 5 | M1_Digital Systems and Combinational Circuits | 1 (Document) | F1 (SENSE) | 19-05-2026 |
| 6 | Qbank | 0 (Document) | F2 (SCOPE) | 23-11-2025 |
| 7 | Microprocessor and Microcontroller | 5 (Document) | F3 (SCOPE) | 14-11-2025 |
| 8 | Advanced Architecture | 5 (Document) | F3 (SCOPE) | 14-11-2025 |
| 9 | Advanced Architecture | 5 (Document) | F3 (SCOPE) | 14-11-2025 |
| 10 | Module 5 - Advanced Architecture | 5 (Document) | F4 (SENSE) | 14-11-2025 |
| 11 | Module 4 - Memory and I/O Peripherals | 4 (Document) | F4 (SENSE) | 14-11-2025 |
| 12 | Module-5 Parallel-processing-(ILP-and-TLP) | 5 (Document) | F5 (SCOPE) | 13-11-2025 |
| 13 | Module-5-Stacks-and-Subroutines | 5 (Document) | F5 (SCOPE) | 13-11-2025 |
| 14 | Module-5 Introduction to Microprocessor and Microcontroller | 5 (Document) | F5 (SCOPE) | 13-11-2025 |
| 15 | Module-5-ARM Stacks and subroutine | 5 (Document) | F5 (SCOPE) | 13-11-2025 |
| 16 | Module-4 Memory and Input Output Organization | 4 (Document) | F5 (SCOPE) | 13-11-2025 |
| 17 | Parallel processing (ILP and TLP) | 5 (Document) | F6 (SCOPE) | 12-11-2025 |
| 18 | Parallel processing Techniques: Instruction Level parallelism-updated | 5 (Document) | F7 (SCORE) | 10-11-2025 |
| 19 | Memory and IO Peripherals | 4 (Document) | F3 (SCOPE) | 09-11-2025 |
| 20 | ARM, Stack and Subroutines | 5 (Document) | F6 (SCOPE) | 09-11-2025 |
| 21 | Stacks and Subroutines | 5 (Document) | F7 (SCORE) | 08-11-2025 |
| 22 | Introduction to microprocessor and microcontroller | 5 (Document) | F6 (SCOPE) | 06-11-2025 |
| 23 | Module-4 Memory | 4 (Document) | F5 (SCOPE) | 04-11-2025 |
| 24 | Single cycle Datapath design, Multicycle Datapath design | 3 (Document) | F7 (SCORE) | 02-11-2025 |
| 25 | Parallel processing Techniques: Instruction Level parallelism | 5 (Document) | F7 (SCORE) | 02-11-2025 |
| 26 | ARM architecture | 5 (Document) | F7 (SCORE) | 02-11-2025 |
| 27 | Mod-5-Microcontroller | 5 (Document) | F7 (SCORE) | 02-11-2025 |
| 28 | I/O Fundamentals- programmed I/O and Interrupt driven I/O, Direct Memory Access | 4 (Document) | F7 (SCORE) | 02-11-2025 |
| 29 | Shared memory architectures | 4 (Document) | F7 (SCORE) | 02-11-2025 |
| 30 | I/O fundamentals, DMA, Inetrrupt | 4 (Document) | F6 (SCOPE) | 01-11-2025 |
| 31 | Cache Replacement | 4 (Document) | F6 (SCOPE) | 01-11-2025 |
| 32 | Memory Design | 4 (Document) | F6 (SCOPE) | 01-11-2025 |
| 33 | Direct Memory Access(DMA) | 4 (Document) | F7 (SCORE) | 26-10-2025 |
| 34 | Virtual memory | 4 (Document) | F7 (SCORE) | 26-10-2025 |
| 35 | Mod-4-Cache memory | 4 (Document) | F7 (SCORE) | 26-10-2025 |
| 36 | Mod-4-Memory Design | 4 (Document) | F7 (SCORE) | 26-10-2025 |
| 37 | Cache Memory (Mapping Techniques) | 4 (Document) | F6 (SCOPE) | 15-10-2025 |
| 38 | Memory hierarchy and organization | 4 (Document) | F6 (SCOPE) | 15-10-2025 |
| 39 | Von Neumann machine and Harvard architecture, Multiplication, Division, Instruction set Instruction formats, Instruction types, addressing modes | 3 (Document) | F3 (SCOPE) | 06-10-2025 |
| 40 | Flip Flops, Sequential circuit design and Analysis: Finite State Machines-Mealy and Moore, Registers, and Counters: Synchronous and Asynchronous, | 2 (Document) | F3 (SCOPE) | 06-10-2025 |
| 41 | Floating Point | 3 (Document) | F8 (SCOPE) | 06-10-2025 |
| 42 | Booth's Multiplier | 3 (Document) | F8 (SCOPE) | 06-10-2025 |
| 43 | Instruction Formats | 3 (Document) | F8 (SCOPE) | 06-10-2025 |
| 44 | Computer Architectures and Arithmetic Algorithms | 3 (Document) | F8 (SCOPE) | 06-10-2025 |
| 45 | Module 03 - Computer Architectures and Arithmetic Algorithms | 3 (Document) | F4 (SENSE) | 03-10-2025 |
| 46 | Module 02 - Sequential Circuits (FSM, Counters, Shift Register) | 2 (Document) | F4 (SENSE) | 03-10-2025 |
| 47 | Booth and modified booth | 3 (Document) | F6 (SCOPE) | 02-10-2025 |
| 48 | Division | 3 (Document) | F6 (SCOPE) | 02-10-2025 |
| 49 | Asynchronous counter | 2 (Document) | F6 (SCOPE) | 02-10-2025 |
| 50 | Registers | 2 (Document) | F6 (SCOPE) | 02-10-2025 |
| 51 | CS_module3_part2 | 3 (Document) | F9 (SCORE) | 01-10-2025 |
| 52 | CS_module3_part1 | 3 (Document) | F9 (SCORE) | 01-10-2025 |
| 53 | Single and multi cycle datapath | 3 (Document) | F6 (SCOPE) | 01-10-2025 |
| 54 | IAS Computer and types | 3 (Document) | F6 (SCOPE) | 01-10-2025 |
| 55 | Instruction cycle | 3 (Document) | F6 (SCOPE) | 01-10-2025 |
| 56 | CS_module2_part2 | 2 (Document) | F9 (SCORE) | 01-10-2025 |
| 57 | Module2_part1 | 2 (Document) | F9 (SCORE) | 01-10-2025 |
| 58 | Mod-3-Binary division | 3 (Document) | F7 (SCORE) | 29-09-2025 |
| 59 | Mod-3-Instrcution types | 3 (Document) | F7 (SCORE) | 29-09-2025 |
| 60 | Mod-3- addressing modes problem | 3 (Document) | F7 (SCORE) | 29-09-2025 |
| 61 | M0d-3-Modified Booth algorithm | 3 (Document) | F7 (SCORE) | 29-09-2025 |
| 62 | instruction cycle | 3 (Document) | F6 (SCOPE) | 25-09-2025 |
| 63 | Data Path | 3 (Document) | F6 (SCOPE) | 25-09-2025 |
| 64 | addressing modes | 3 (Document) | F6 (SCOPE) | 25-09-2025 |
| 65 | Module-3-Computer Architectures and Arithmetic Algorithms | 3 (Document) | F5 (SCOPE) | 25-09-2025 |
| 66 | Module-3-Computer Architectures and Arithmetic Algorithms | 3 (Document) | F5 (SCOPE) | 25-09-2025 |
| 67 | RISC vs CISC | 3 (Document) | F6 (SCOPE) | 24-09-2025 |
| 68 | ISA | 3 (Document) | F6 (SCOPE) | 24-09-2025 |
| 69 | Restoring and Non-restoring division | 3 (Document) | F6 (SCOPE) | 19-09-2025 |
| 70 | Module 3- up to Division | 3 (Document) | F5 (SCOPE) | 17-09-2025 |
| 71 | Mod-3-Booth Multiplication | 3 (Document) | F7 (SCORE) | 17-09-2025 |
| 72 | Mod-3-part-2 | 3 (Document) | F7 (SCORE) | 17-09-2025 |
| 73 | IEEE 754 | 3 (Document) | F6 (SCOPE) | 17-09-2025 |
| 74 | Module 1 | 1 (Document) | F9 (SCORE) | 17-09-2025 |
| 75 | Module-2 Sequential circuits | 2 (Document) | F5 (SCOPE) | 13-09-2025 |
| 76 | Von Neumann and Harvard architecture | 3 (Document) | F6 (SCOPE) | 12-09-2025 |
| 77 | Mod-3-Part-1 | 3 (Document) | F7 (SCORE) | 10-09-2025 |
| 78 | Mod-2-Shift Registers | 2 (Document) | F7 (SCORE) | 03-09-2025 |
| 79 | Shift Registers | 0 (Document) | F10 (SCOPE) | 01-09-2025 |
| 80 | Mod counters | 2 (Document) | F7 (SCORE) | 31-08-2025 |
| 81 | Mod-2-Counters | 2 (Document) | F7 (SCORE) | 31-08-2025 |
| 82 | Flip Flops | 2 (Document) | F7 (SCORE) | 31-08-2025 |
| 83 | Counters | 2 (Document) | F10 (SCOPE) | 29-08-2025 |
| 84 | Module-2 Flip-flops | 2 (Document) | F5 (SCOPE) | 19-08-2025 |
| 85 | Multiplexer. Demultiplexes, Representation of data | 1 (Document) | F5 (SCOPE) | 19-08-2025 |
| 86 | Digital Systems and Combinational Circuits | 1 (Document) | F3 (SCOPE) | 15-08-2025 |
| 87 | Mod-2-Design a sequential circuit | 2 (Document) | F7 (SCORE) | 15-08-2025 |
| 88 | module 1 new | 1 (Document) | F11 (SCOPE) | 13-08-2025 |
| 89 | Module-2 Sequential circuits-Flip-Flops | 2 (Document) | F5 (SCOPE) | 13-08-2025 |
| 90 | Module-1 Digital Systems and Combinational circuits | 1 (Document) | F5 (SCOPE) | 13-08-2025 |
| 91 | Binary Parallel Adder and Look ahead Carry Adder Binary Subtractor | 1 (Document) | F7 (SCORE) | 12-08-2025 |
| 92 | Module 02 - Sequential Circuits (Flip Flops) | 2 (Document) | F4 (SENSE) | 11-08-2025 |
| 93 | Module 01 - Digital Systems and Combinational with Additional Content | 1 (Document) | F4 (SENSE) | 11-08-2025 |
| 94 | Module-2-Sequential Circuits | 2 (Document) | F7 (SCORE) | 11-08-2025 |
| 95 | Module-2-part-2 | 1 (Document) | F7 (SCORE) | 08-08-2025 |
| 96 | Mod-1-part-1 | 1 (Document) | F7 (SCORE) | 08-08-2025 |
| 97 | Question Bank- module 1 | 1 (Document) | F6 (SCOPE) | 08-08-2025 |
| 98 | Module 2 Flip Flops | 2 (Document) | F11 (SCOPE) | 08-08-2025 |
| 99 | Combinational Circuits | 1 (Document) | F10 (SCOPE) | 08-08-2025 |
| 100 | Sample Question Bank | 1 (Document) | F12 (SCOPE) | 08-08-2025 |
| 101 | Question bank | 1 (Document) | F13 (SCOPE) | 08-08-2025 |
| 102 | Module1 | 1 (Document) | F14 (SENSE) | 07-08-2025 |
| 103 | mmodule1_lec1 | 1 (Document) | F15 (SCORE) | 06-08-2025 |
| 104 | Module 1 | 1 (Document) | F13 (SCOPE) | 05-08-2025 |
| 105 | Module-1-Part-3 | 1 (Document) | F16 (SCOPE) | 04-08-2025 |
| 106 | Module-1-part-2 | 1 (Document) | F16 (SCOPE) | 04-08-2025 |
| 107 | MOdule 1-Part-1 | 1 (Document) | F16 (SCOPE) | 04-08-2025 |

### BACSE103 Computation Structures, ELA (19 materials, fully read)

Source semester of the uploads: Fall Semester 2025-26. Dates 02-08-2025 to 09-07-2026. Uploaders: 9 faculty (names left out on purpose).

Per module tag: 0 (Document): 2, 1 (Document): 3, 1,2 (Document): 1, 1,2,3 (Document): 1, 10 (Document): 4, 2 (Document): 1, 4,5 (Document): 1, 6 (Document): 1, 8 (Document): 2, 9 (Document): 2, 9,10,11,12 (Document): 1

| # | Title | Module tag (type) | Uploader | Date |
|---|---|---|---|---|
| 1 | ML workflow | 0 (Document) | F1 (SCOPE) | 09-07-2026 |
| 2 | Qbank | 0 (Document) | F2 (SCOPE) | 23-11-2025 |
| 3 | Aurdino pin configuration | 8 (Document) | F3 (SCORE) | 03-11-2025 |
| 4 | Microcontroller | 6 (Document) | F4 (SCORE) | 01-11-2025 |
| 5 | factorial program | 9 (Document) | F3 (SCORE) | 30-10-2025 |
| 6 | open MP lab cycle sheet | 10 (Document) | F3 (SCORE) | 30-10-2025 |
| 7 | open MP manual programs | 10 (Document) | F3 (SCORE) | 27-10-2025 |
| 8 | basics of c programming | 10 (Document) | F3 (SCORE) | 27-10-2025 |
| 9 | open MP | 10 (Document) | F3 (SCORE) | 27-10-2025 |
| 10 | OpenMP Program for (Largest & Smallest, Matrix Operation, Performance Analyzer, Synchronization Constructs) | 9,10,11,12 (Document) | F5 (SENSE) | 17-10-2025 |
| 11 | coursematerial-microcontroller | 8 (Document) | F6 (SCOPE) | 14-10-2025 |
| 12 | Open MP | 9 (Document) | F1 (SCOPE) | 18-09-2025 |
| 13 | Assessment-4 questions | 4,5 (Document) | F4 (SCORE) | 31-08-2025 |
| 14 | 2 ex | 2 (Document) | F7 (SCOPE) | 27-08-2025 |
| 15 | Ex 1 | 1 (Document) | F7 (SCOPE) | 12-08-2025 |
| 16 | Lab Experiment-1 | 1 (Document) | F8 (SCOPE) | 04-08-2025 |
| 17 | Assessment-1-Combinational Logic Circuits | 1 (Document) | F4 (SCORE) | 02-08-2025 |
| 18 | Module 1 | 1,2 (Document) | F7 (SCOPE) | 02-08-2025 |
| 19 | Module 1 | 1,2,3 (Document) | F9 (SCOPE) | 02-08-2025 |

### BACSE104 Structured and Object-Oriented Programming, ETH (181 materials, fully read)

Source semester of the uploads: Winter Semester 2025-26. Dates 06-12-2025 to 01-05-2026. Uploaders: 20 faculty (names left out on purpose).

Per module tag: 0 (Document): 4, 0 (Web Link): 1, 1 (Document): 63, 2 (Document): 43, 2 (Web Link): 2, 3 (Document): 27, 4 (Document): 21, 5 (Document): 20

| # | Title | Module tag (type) | Uploader | Date |
|---|---|---|---|---|
| 1 | STL | 5 (Document) | F1 (SCORE) | 01-05-2026 |
| 2 | Module 5 | 5 (Document) | F2 (SCORE) | 20-04-2026 |
| 3 | Module 4b | 4 (Document) | F2 (SCORE) | 20-04-2026 |
| 4 | STL | 5 (Document) | F3 (SCORE) | 16-04-2026 |
| 5 | STL | 5 (Document) | F3 (SCORE) | 16-04-2026 |
| 6 | templates | 5 (Document) | F3 (SCORE) | 16-04-2026 |
| 7 | exception handling eg | 5 (Document) | F3 (SCORE) | 16-04-2026 |
| 8 | exception handling | 5 (Document) | F3 (SCORE) | 16-04-2026 |
| 9 | Module 5- Exception handling in C++ | 5 (Document) | F4 (SCOPE) | 13-04-2026 |
| 10 | Additional topics in C++ | 0 (Document) | F5 (SCOPE) | 13-04-2026 |
| 11 | Module 5 Exception Handling | 5 (Document) | F5 (SCOPE) | 13-04-2026 |
| 12 | Module 5 | 5 (Document) | F6 (SCOPE) | 10-04-2026 |
| 13 | Templates and STL (Overall) | 5 (Document) | F4 (SCOPE) | 10-04-2026 |
| 14 | Module 5 Template and STL | 5 (Document) | F5 (SCOPE) | 09-04-2026 |
| 15 | Module 5- STL continued... | 5 (Document) | F4 (SCOPE) | 08-04-2026 |
| 16 | Module 5 - STL | 5 (Document) | F4 (SCOPE) | 08-04-2026 |
| 17 | Module 5 - Generic functions and Classes | 5 (Document) | F4 (SCOPE) | 08-04-2026 |
| 18 | Strings | 4 (Document) | F4 (SCOPE) | 08-04-2026 |
| 19 | Module 4 Polymorphism | 4 (Document) | F5 (SCOPE) | 07-04-2026 |
| 20 | Module 5 - Templates & STL | 5 (Document) | F7 (SCOPE) | 06-04-2026 |
| 21 | Quiz 2 Link | 0 (Web Link) | F8 (SCOPE) | 06-04-2026 |
| 22 | Run Time Polymorphism | 4 (Document) | F9 (SCOPE) | 06-04-2026 |
| 23 | Exception Handling | 5 (Document) | F7 (SCOPE) | 01-04-2026 |
| 24 | Module 5 | 5 (Document) | F10 (SCOPE) | 01-04-2026 |
| 25 | Module 4 Final | 4 (Document) | F10 (SCOPE) | 30-03-2026 |
| 26 | Module 4- Polymorphism-function and Operator Overloading | 4 (Document) | F4 (SCOPE) | 27-03-2026 |
| 27 | Virtual Destructors | 4 (Document) | F1 (SCORE) | 25-03-2026 |
| 28 | Templates | 5 (Document) | F1 (SCORE) | 25-03-2026 |
| 29 | Exception Handling | 5 (Document) | F1 (SCORE) | 25-03-2026 |
| 30 | Polymorphism | 4 (Document) | F1 (SCORE) | 25-03-2026 |
| 31 | Polymorphism | 4 (Document) | F1 (SCORE) | 25-03-2026 |
| 32 | Module 4: Ambiguity in multipath Inheritance and Constructors under Inheritance | 4 (Document) | F4 (SCOPE) | 25-03-2026 |
| 33 | Inheritance | 4 (Document) | F2 (SCORE) | 13-03-2026 |
| 34 | Introduction to OOP & C++ | 3 (Document) | F2 (SCORE) | 13-03-2026 |
| 35 | Dynamic Memory Allocation, Structure, Union and File Handling | 2 (Document) | F2 (SCORE) | 13-03-2026 |
| 36 | Module 3 :- Lambda Expression | 3 (Document) | F4 (SCOPE) | 13-03-2026 |
| 37 | Inheritance and Constructors | 4 (Document) | F7 (SCOPE) | 12-03-2026 |
| 38 | Inheritance - Part1 | 4 (Document) | F7 (SCOPE) | 12-03-2026 |
| 39 | Module 4 | 4 (Document) | F6 (SCOPE) | 10-03-2026 |
| 40 | Structures and Pointers, Union | 2 (Document) | F7 (SCOPE) | 10-03-2026 |
| 41 | Structures and Functions | 2 (Document) | F7 (SCOPE) | 10-03-2026 |
| 42 | Structures | 2 (Document) | F7 (SCOPE) | 10-03-2026 |
| 43 | Module 4 - Inheritance - part2 | 4 (Document) | F7 (SCOPE) | 10-03-2026 |
| 44 | Module 3 - Lambda Functions | 3 (Document) | F7 (SCOPE) | 10-03-2026 |
| 45 | Module 3 except Lambda Functions | 3 (Document) | F7 (SCOPE) | 10-03-2026 |
| 46 | inheritance | 4 (Document) | F3 (SCORE) | 09-03-2026 |
| 47 | constructors and destructor | 3 (Document) | F3 (SCORE) | 09-03-2026 |
| 48 | functions | 3 (Document) | F3 (SCORE) | 09-03-2026 |
| 49 | class and object | 3 (Document) | F3 (SCORE) | 09-03-2026 |
| 50 | features of OOP | 3 (Document) | F3 (SCORE) | 09-03-2026 |
| 51 | introduction to POP OOP (C C++) | 3 (Document) | F3 (SCORE) | 09-03-2026 |
| 52 | Files | 2 (Document) | F3 (SCORE) | 09-03-2026 |
| 53 | Module 4-inheritance | 4 (Document) | F10 (SCOPE) | 09-03-2026 |
| 54 | Friend function | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 55 | Polymorphism | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 56 | Inline functions | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 57 | Inheritance | 4 (Document) | F1 (SCORE) | 08-03-2026 |
| 58 | Call By Reference | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 59 | Static data members & Member functions | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 60 | Copy constructor | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 61 | Constructor | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 62 | Class & Object | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 63 | Introduction to OOP | 3 (Document) | F1 (SCORE) | 08-03-2026 |
| 64 | Module 4- Inheritance in C++ | 4 (Document) | F4 (SCOPE) | 06-03-2026 |
| 65 | Module 2 Unions | 2 (Document) | F4 (SCOPE) | 06-03-2026 |
| 66 | Module 3 | 3 (Document) | F10 (SCOPE) | 03-03-2026 |
| 67 | Module 4 Inheritance in CPP | 4 (Document) | F5 (SCOPE) | 03-03-2026 |
| 68 | Module 3- static, friend, this | 3 (Document) | F4 (SCOPE) | 25-02-2026 |
| 69 | Module 4: Single inheritance | 4 (Document) | F5 (SCOPE) | 24-02-2026 |
| 70 | MODULE 3 | 3 (Document) | F5 (SCOPE) | 23-02-2026 |
| 71 | Quiz 1 link | 2 (Web Link) | F8 (SCOPE) | 23-02-2026 |
| 72 | Constructors and Destructors | 3 (Document) | F4 (SCOPE) | 20-02-2026 |
| 73 | default arguments | 3 (Document) | F4 (SCOPE) | 20-02-2026 |
| 74 | Module 3 | 3 (Document) | F6 (SCOPE) | 19-02-2026 |
| 75 | Classes and Objects | 3 (Document) | F4 (SCOPE) | 18-02-2026 |
| 76 | Name spaces, Default arguments and inline functions | 3 (Document) | F4 (SCOPE) | 18-02-2026 |
| 77 | Introduction to OOPs and C++ | 3 (Document) | F4 (SCOPE) | 18-02-2026 |
| 78 | C Files | 2 (Document) | F4 (SCOPE) | 18-02-2026 |
| 79 | Files | 2 (Document) | F5 (SCOPE) | 16-02-2026 |
| 80 | Command Line Arguments | 2 (Document) | F1 (SCORE) | 15-02-2026 |
| 81 | File Handling | 2 (Document) | F1 (SCORE) | 15-02-2026 |
| 82 | Union | 2 (Document) | F1 (SCORE) | 15-02-2026 |
| 83 | Structure | 2 (Document) | F1 (SCORE) | 15-02-2026 |
| 84 | Files and Command Line Args | 2 (Document) | F7 (SCOPE) | 10-02-2026 |
| 85 | Module 2 Updated | 2 (Document) | F10 (SCOPE) | 10-02-2026 |
| 86 | C Files | 2 (Document) | F4 (SCOPE) | 10-02-2026 |
| 87 | D2-CAT2QP | 0 (Document) | F11 (SCOPE) | 03-02-2026 |
| 88 | D2-CAT1Key | 0 (Document) | F11 (SCOPE) | 03-02-2026 |
| 89 | strings | 1 (Document) | F12 (SCORE) | 30-01-2026 |
| 90 | c basics | 1 (Document) | F12 (SCORE) | 30-01-2026 |
| 91 | arrays | 1 (Document) | F12 (SCORE) | 30-01-2026 |
| 92 | pointers | 2 (Document) | F12 (SCORE) | 30-01-2026 |
| 93 | functions arrays storage classes | 2 (Document) | F12 (SCORE) | 30-01-2026 |
| 94 | pointers arrays functions | 2 (Document) | F12 (SCORE) | 30-01-2026 |
| 95 | function pointers | 2 (Document) | F12 (SCORE) | 30-01-2026 |
| 96 | Sample Programs (Folder) | 2 (Web Link) | F13 (SCORE) | 28-01-2026 |
| 97 | functions | 1 (Document) | F13 (SCORE) | 28-01-2026 |
| 98 | Pointers | 2 (Document) | F13 (SCORE) | 28-01-2026 |
| 99 | Module 2- Pointers | 2 (Document) | F2 (SCORE) | 25-01-2026 |
| 100 | structures in C | 2 (Document) | F4 (SCOPE) | 24-01-2026 |
| 101 | Pointers | 2 (Document) | F1 (SCORE) | 23-01-2026 |
| 102 | Storage class | 1 (Document) | F1 (SCORE) | 23-01-2026 |
| 103 | pointers | 2 (Document) | F14 (SCORE) | 22-01-2026 |
| 104 | Module 2 | 2 (Document) | F6 (SCOPE) | 21-01-2026 |
| 105 | structure | 2 (Document) | F3 (SCORE) | 21-01-2026 |
| 106 | array of pointers and pointer to array | 2 (Document) | F3 (SCORE) | 21-01-2026 |
| 107 | function poiners | 2 (Document) | F3 (SCORE) | 21-01-2026 |
| 108 | pointers | 2 (Document) | F3 (SCORE) | 21-01-2026 |
| 109 | storage class | 1 (Document) | F3 (SCORE) | 21-01-2026 |
| 110 | strings | 1 (Document) | F3 (SCORE) | 21-01-2026 |
| 111 | functions | 1 (Document) | F3 (SCORE) | 21-01-2026 |
| 112 | type casting | 1 (Document) | F3 (SCORE) | 21-01-2026 |
| 113 | arrays | 1 (Document) | F3 (SCORE) | 21-01-2026 |
| 114 | branching and looping | 1 (Document) | F3 (SCORE) | 21-01-2026 |
| 115 | Module 1 | 1 (Document) | F2 (SCORE) | 20-01-2026 |
| 116 | Module 2: Structures and Union | 2 (Document) | F5 (SCOPE) | 20-01-2026 |
| 117 | Module 2: Dynamic Memory Allocation | 2 (Document) | F5 (SCOPE) | 20-01-2026 |
| 118 | Dynamic Memory allocation | 2 (Document) | F4 (SCOPE) | 19-01-2026 |
| 119 | C - Arrays | 1 (Document) | F15 (SCORE) | 19-01-2026 |
| 120 | C - Basics | 1 (Document) | F15 (SCORE) | 19-01-2026 |
| 121 | DMA | 2 (Document) | F16 (SCOPE) | 16-01-2026 |
| 122 | Dynamic Memory Allocation | 2 (Document) | F7 (SCOPE) | 14-01-2026 |
| 123 | Pointers and Arrays | 2 (Document) | F7 (SCOPE) | 14-01-2026 |
| 124 | Pointers | 2 (Document) | F7 (SCOPE) | 14-01-2026 |
| 125 | Storage Classes | 1 (Document) | F7 (SCOPE) | 14-01-2026 |
| 126 | Strings | 1 (Document) | F7 (SCOPE) | 14-01-2026 |
| 127 | Functions | 1 (Document) | F7 (SCOPE) | 14-01-2026 |
| 128 | Arrays | 1 (Document) | F7 (SCOPE) | 14-01-2026 |
| 129 | Control Structures - Branching and Looping | 1 (Document) | F7 (SCOPE) | 14-01-2026 |
| 130 | Operators | 1 (Document) | F7 (SCOPE) | 14-01-2026 |
| 131 | Introduction | 1 (Document) | F7 (SCOPE) | 14-01-2026 |
| 132 | Module 2 | 2 (Document) | F10 (SCOPE) | 13-01-2026 |
| 133 | Module 2 Pointers | 2 (Document) | F5 (SCOPE) | 12-01-2026 |
| 134 | Functions | 1 (Document) | F1 (SCORE) | 12-01-2026 |
| 135 | Storage class | 1 (Document) | F1 (SCORE) | 12-01-2026 |
| 136 | Strings | 1 (Document) | F1 (SCORE) | 12-01-2026 |
| 137 | Array | 1 (Document) | F1 (SCORE) | 12-01-2026 |
| 138 | Module 1 | 1 (Document) | F6 (SCOPE) | 11-01-2026 |
| 139 | Pointers and arrays | 2 (Document) | F16 (SCOPE) | 09-01-2026 |
| 140 | Pointers | 2 (Document) | F16 (SCOPE) | 09-01-2026 |
| 141 | Pointers and arrays | 2 (Document) | F4 (SCOPE) | 09-01-2026 |
| 142 | Module 2- Pointers in C | 2 (Document) | F4 (SCOPE) | 09-01-2026 |
| 143 | functions | 1 (Document) | F14 (SCORE) | 08-01-2026 |
| 144 | arrays and strings | 1 (Document) | F14 (SCORE) | 08-01-2026 |
| 145 | Module 1 Fundamentals of C Programming | 1 (Document) | F5 (SCOPE) | 05-01-2026 |
| 146 | mod 2 | 2 (Document) | F17 (SCOPE) | 31-12-2025 |
| 147 | mod 1 | 1 (Document) | F17 (SCOPE) | 25-12-2025 |
| 148 | Functions | 1 (Document) | F4 (SCOPE) | 19-12-2025 |
| 149 | Strings | 1 (Document) | F4 (SCOPE) | 19-12-2025 |
| 150 | Module 1 | 1 (Document) | F10 (SCOPE) | 19-12-2025 |
| 151 | Module1 | 1 (Document) | F18 (SCOPE) | 16-12-2025 |
| 152 | Strings | 1 (Document) | F16 (SCOPE) | 16-12-2025 |
| 153 | IntroductIon to C | 1 (Document) | F1 (SCORE) | 16-12-2025 |
| 154 | Arrays | 1 (Document) | F4 (SCOPE) | 15-12-2025 |
| 155 | Type conversion | 1 (Document) | F4 (SCOPE) | 15-12-2025 |
| 156 | ARRAYS | 1 (Document) | F16 (SCOPE) | 15-12-2025 |
| 157 | Do while | 1 (Document) | F16 (SCOPE) | 14-12-2025 |
| 158 | For Loop | 1 (Document) | F16 (SCOPE) | 14-12-2025 |
| 159 | While | 1 (Document) | F16 (SCOPE) | 14-12-2025 |
| 160 | C Programming-basics | 1 (Document) | F13 (SCORE) | 13-12-2025 |
| 161 | sample code | 1 (Document) | F14 (SCORE) | 12-12-2025 |
| 162 | Class 6 (Theory -11.12.25) | 1 (Document) | F8 (SCOPE) | 11-12-2025 |
| 163 | Looping statements in C | 1 (Document) | F4 (SCOPE) | 11-12-2025 |
| 164 | Module 1 | 1 (Document) | F10 (SCOPE) | 11-12-2025 |
| 165 | C lang Introduction | 1 (Document) | F3 (SCORE) | 11-12-2025 |
| 166 | Structure of C Program, Compiler Vs Interpreter, Variables, Reserved words, Data types | 1 (Document) | F19 (SCOPE) | 11-12-2025 |
| 167 | First Day (Introductory Class) | 0 (Document) | F19 (SCOPE) | 11-12-2025 |
| 168 | Operators and Type Conversion | 1 (Document) | F5 (SCOPE) | 10-12-2025 |
| 169 | Control structures | 1 (Document) | F4 (SCOPE) | 10-12-2025 |
| 170 | Fundamentals of C Part 1 | 1 (Document) | F5 (SCOPE) | 09-12-2025 |
| 171 | Switch case | 1 (Document) | F16 (SCOPE) | 09-12-2025 |
| 172 | Module1-Part1 | 1 (Document) | F20 (SCOPE) | 09-12-2025 |
| 173 | If---else | 1 (Document) | F16 (SCOPE) | 09-12-2025 |
| 174 | C ebook 1 | 1 (Document) | F14 (SCORE) | 09-12-2025 |
| 175 | class 4 (Theory - 8.11.25-Monday) | 1 (Document) | F8 (SCOPE) | 08-12-2025 |
| 176 | Operators in C | 1 (Document) | F4 (SCOPE) | 08-12-2025 |
| 177 | I/O Operations in C | 1 (Document) | F4 (SCOPE) | 08-12-2025 |
| 178 | Fundamentals of C | 1 (Document) | F4 (SCOPE) | 08-12-2025 |
| 179 | Operators | 1 (Document) | F16 (SCOPE) | 08-12-2025 |
| 180 | Tokens, Keywords, Identifiers | 1 (Document) | F16 (SCOPE) | 08-12-2025 |
| 181 | Class Notes 2 (6.12.25) | 1 (Document) | F8 (SCOPE) | 06-12-2025 |

## Not read (see item 1 above)
BACSE104 ELA, BACSE105 ETH/ELA, BACSE106 ETH/ELA, BAESP101 ELA, BAMAT205 TH, BAPHY105 ETH/ELA: open Course Page,
pick the course, set "Show 100 entries", and list the rows here in the same table shape.
