# SymbOS network daemon for the MSX Pico+ (ESP8266 WiFi)

A TCP / UDP / TLS driver for Prodatron's SymbOS Network Daemon that talks to the ESP8266
WiFi module of the MSX Pico+ through the memory-mapped UART (registers #7F06 / #7F07).
It is DRIVER=6 inside the SymbOS Network Daemon source (GPL-3.0) and also builds for the PicoVerse 2350.

Status: confirmed working on real MSX Pico+ hardware (SymbOS 4.0, MSX2, Nextor boot with the SD driver,
1 MB mapper from a PicoVerse 2350). DHCP, DNS, TCP, UDP (SNTP) and HTTPS work.

## Build (WinAPE 1.0.13)

| Wrapper | Output | Target |
|---|---|---|
| `src/Dmn-Network-_PICO.asm` | `netd-pico.exe` | MSX Pico+ (ESPDEBUG=1 for the first test, set 0 when happy) |
| `src/Dmn-Network-_ESP.asm`  | `netd-esp.exe`  | PicoVerse 2350 |

Edit the `WRITE` path in the wrapper, keep all files of `src/` in one folder together with the SymbOS
constants file (`SymbOS-Constants.asm`) from the original network daemon sources.
In the daemon set Cartridge Slot/Subslot to the slot of the MSX Pico+ (tested: slot 1, subslot 2).

## Files

- `src/Dmn-Network.asm` - patched daemon (UDP handlers, tcpsta fix, DRIVER=6 branch)
- `src/Dmn-Network-ESP.asm` - the ESP driver (transport, detection, TCP, DNS, UDP, TLS)
- `src/Dmn-Network-i18n-direct.asm` - texts
- `src/NetTest*.asm` - Net Test companion app (world clock via SNTP, weather, sun times, HTTPS check)
- `docs/README-ESP-driver.md`, `docs/README-NetTest.md` - details
- `docs/Dmn-Network-DRIVER6.diff` - changes against the original `Dmn-Network.asm`

## Notes

- The driver is based on ducasp's UNAPI_BIOS_CUSTOM_ESP and the MSX Pico BIOS; ESP firmware: ducasp/ESP8266-UNAPI-Firmware.
- Debug subline (ESPDEBUG=1) is a slot scan log, not an error.
- License: follows the SymbOS Network Daemon, GPL-3.0.
