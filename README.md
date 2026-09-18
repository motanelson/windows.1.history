 The architecture of Windows 1.00 (1985) was much simpler than that of Windows 95, and the concept of "switching between programs" was also quite different.

1. The Windows 1.00 layer

Windows 1.00 was not yet a standalone operating system like modern versions of Windows. It was a graphical environment that ran on top of MS-DOS.

In simplified terms:

┌─────────────────────────────┐
│     Windows Applications    │
├─────────────────────────────┤
│        Windows 1.00         │
│                             │
│  USER     GDI     KERNEL    │
├─────────────────────────────┤
│           MS-DOS            │
├─────────────────────────────┤
│            BIOS             │
├─────────────────────────────┤
│           Hardware          │
└─────────────────────────────┘

The three fundamental components were:

KERNEL — basic services of the Windows environment;

USER — windows, menus, keyboard/mouse input, etc.;

GDI — text and graphics rendering.


This established the foundation of the architecture that would evolve over many years.


---

2. What memory did Windows 1.00 use?

Windows 1.00 was designed for 8086/8088 machines and operated using the MS-DOS memory model.

The 8086 had an address space of:

1 MB = 1,048,576 bytes

However, not all of that 1 MB was available to Windows.

A simplified representation:

Address
FFFFF  ┌─────────────────────┐
       │ BIOS / ROM          │
       ├─────────────────────┤
       │ Hardware / Video    │
       ├─────────────────────┤
       │                     │
       │ Memory available    │
       │ for DOS/Windows     │
       │                     │
       ├─────────────────────┤
       │ MS-DOS              │
       └─────────────────────┘
00000

On a PC with 640 KB of conventional RAM, a portion of that memory was occupied by DOS, Windows itself, and the applications.  Therefore, one should not interpret "the 8086 has 1 MB" as "Windows had 1 MB available."


---

3. How did memory work on the 8086?

This is where a huge difference compared to Windows 95 appears.

The 8086 used 64 KB segments.

An address could be represented as:

1234:5678

The first value was the segment, and the second was the offset.

The physical address was calculated roughly as:

segment × 16 + offset

For example:

1234:5678

1234 × 16 = 12340h
12340h + 5678h = 179B8h

Therefore, Windows 1.00 was essentially working with the segmented memory model of the 8086/MS-DOS.


---

4. How did Windows 1.00 switch between programs?

There is a very important distinction here.

Windows 1.00 introduced a graphical interface for running applications, but it lacked the modern process/preemptive multitasking system associated with modern Windows NT versions.

Windows 1.x primarily used cooperative multitasking.

In other words, an application had to yield control back to Windows.

Conceptually:

Windows
   │
   ├── executes Program A
   │
   │    Program A works
   │
   ├── A yields control
   │
   ├── executes Program B
   │
   │    Program B works
   │
   └── B yields control

The application was not simply interrupted by the system at any moment, as would happen in a modern preemptive system.


---

5. So, could Windows 1.00 have multiple programs "open"?

Yes.

You could have several applications loaded, but the way it worked was quite different from modern Windows.

Windows kept track of applications and their resources, and could switch execution between them when they yielded control.

It was more like this:

┌───────────────┐
        │    Windows    │
        └───────┬───────┘
                │
        decides who runs
                │
       ┌────────┼────────┐
       ▼        ▼        ▼
    APP A     APP B     APP C

There wasn't the strict separation between processes and memory spaces found in modern NT systems.


---

6. What if a program got stuck?

This was one of the issues with the cooperative model.

If an application didn't return control to Windows, it could prevent other applications from getting a chance to run.

Simply put:

Windows
   │
   ▼
Program A
   │
   │  "I'm going to keep running..."
   │
   ▼
Program A
   │
   └── doesn't return control
             ↓
       other programs
       don't get to run

This is a fundamental difference compared to modern systems.


---

7. And the window? Was the window the program?

No.

This is interesting because today we think:

> “I have a window = I have a process.”



In Windows 1.00, that association isn't correct.

There was a distinction between:

Application
   │
   └── window
        ├── client area
        ├── title
        └── menus

USER handled window and event management.

GDI handled drawing.


---


8. And how was the program loaded?

A Windows 1.x application used the NE (New Executable) format, which would also be used by several later versions of 16-bit Windows.

The file contained information about its segments.

For example:

PROGRAMA.EXE
│
├── Header
│
├── CODE segment
│
├── DATA segment
│
├── other segments
│
└── resources

Windows read this information and loaded the necessary segments into memory.


---

9. And Windows 1.00 itself?

It was also composed of modules, rather than simply being one huge monolithic file.

Conceptually:

WIN.COM
   │
   ▼
Windows
   │
   ├── KERNEL
   ├── USER
   ├── GDI
   └── other components

These components used the available conventional memory and interacted directly with DOS services and the hardware.


---

10. The major difference compared to Windows 95

We can place the two side by side:

WINDOWS 1.00

Application
    ↓
KERNEL / USER / GDI
    ↓
MS-DOS
    ↓
BIOS
    ↓
Hardware

8086
segmented memory
16-bit
cooperative multitasking

Whereas in Windows 95:

WINDOWS 95

Win32 Application
      ↓
Win32 API
      ↓
32/16-bit components
      ↓
VxD / DOS
      ↓
Hardware

386+
virtual memory
32-bit + 16-bit
much more advanced multitasking

Thus, Windows 1.00 is essentially a graphical and execution layer placed on top of MS-DOS,
