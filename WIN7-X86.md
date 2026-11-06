# OpenTabletDriver 0.6.4.0 - Windows 7 x86 build

This tree is OpenTabletDriver v0.6.4.0, the last release targeting net6.0
(.NET 6 is the last .NET version that runs on Windows 7 SP1; .NET 8+ does not).

## Changes from upstream v0.6.4.0

- `WindowsDisplay`: `Shcore.dll` (SetProcessDpiAwareness) does not exist before
  Windows 8.1, so DPI awareness was silently never enabled on Windows 7. It now
  falls back to `user32!SetProcessDPIAware`.
- `.github/workflows/win7-x86.yml`: builds a self-contained `win-x86` package.

- Self-contained builds must NOT be single-file: Eto.Wpf builds a path from
  `typeof(object).Assembly.Location`, which is empty inside a single-file bundle
  and crashes the UI with ArgumentNullException (Parameter 'path1').

## Building

Push this tree to a GitHub repository and run the "Windows 7 x86 build" workflow
(Actions tab -> Run workflow). The zip appears under the run's artifacts.

Locally on Windows with the .NET 6 SDK:

    ./eng/windows/package.ps1 -output dist -netRuntime win-x86 -selfContained $true -singleFile $false

## Running on Windows 7

Windows 7 SP1 is required. Check Microsoft's .NET 6 requirements page for the
current prerequisite list; at the time of writing it includes the SHA-2 code
signing update (KB4474419), the servicing stack update (KB4490628) and the
Visual C++ 2015-2022 x86 redistributable.
Tablets that need WinUSB/libusb drivers (via Zadig) need a signed driver or
test-signing mode on 32-bit Windows 7.

This build is untested on real Windows 7 hardware.
