# sntrup761

Post-quantum Key Encapsulation Mechanism (KEM) for [SimpleGo](https://github.com/saschadaemgen/SimpleGo) - the world's first native C implementation of the SimpleX Messaging Protocol on dedicated hardware.

## What is sntrup761?

Streamlined NTRU Prime with parameter set 761. A lattice-based post-quantum KEM designed to resist attacks from both classical and quantum computers. The same algorithm used by OpenSSH (since 8.5) and SimpleX Chat (since v5.7, mandatory and not disableable).

## Source

Extracted from [PQClean](https://github.com/PQClean/PQClean) (round3 tag), directory `crypto_kem/sntrup761/clean/`. PQClean provides clean, portable, tested reference implementations of post-quantum cryptographic algorithms. All symbols are prefixed with `PQCLEAN_SNTRUP761_CLEAN_` to prevent namespace collisions.

## Key Sizes

| Artifact | Size |
|----------|------|
| Public Key | 1,158 bytes |
| Secret Key | 1,763 bytes |
| Ciphertext | 1,039 bytes |
| Shared Secret | 32 bytes |

## Why this fork?

SimpleGo uses sntrup761 as part of its hybrid post-quantum Double Ratchet encryption. Every ratchet step combines classical X448 key agreement with sntrup761 KEM, making both the initial key exchange and all subsequent message keys quantum-resistant.

This repository gives SimpleGo full source control over its post-quantum dependency. No risk of upstream changes breaking the build, no dependency on external tag stability.

## Platform Integration

For ESP32-S3 (SimpleGo's target hardware), two platform-specific replacements are needed:

- `randombytes()` - mapped to `esp_fill_random()` (hardware RNG)
- `sha512()` - mapped to `mbedtls_sha512()` (hardware SHA accelerator)

These wrappers live in the SimpleGo main repository, not here. This repository contains only the pure algorithmic source.

## Performance on ESP32-S3 (240 MHz, -O2)

| Operation | Expected Time |
|-----------|---------------|
| Key Generation | 50-80 ms |
| Encapsulation | 5-8 ms |
| Decapsulation | 5-8 ms |

Key generation runs as background pre-computation between ratchet steps. Perceptible latency per direction change is only encap + decap (~12-18 ms).

## License

The sntrup761 implementation from PQClean is **Public Domain**. No patent issues, no licensing restrictions.

## Related Projects

- [SimpleGo](https://github.com/saschadaemgen/SimpleGo) - Native C implementation of the SimpleX Messaging Protocol on ESP32-S3
- [GoRelay](https://github.com/saschadaemgen/GoRelay) - SMP-compatible relay server in Go
- [SimpleDB](https://github.com/cannatoshi/SimpleDB) - SimpleX SQLite database decryption and export tool
- [PQClean](https://github.com/PQClean/PQClean) - Original upstream repository

## Author

Sascha Dämgen - [IT and More Systems](https://simplego.dev), Recklinghausen, Germany
