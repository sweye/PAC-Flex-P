# PAC / Flex-P

**PAC** stands for **Perceptual Audio Codec**.

PAC is an experimental audio codec project focused on making audio files smaller than FLAC while staying perceptually close to the original WAV.

The project started as **Flex-P**, a prototype perceptual codec. The public format name is currently planned as:

```text
PAC = Perceptual Audio Codec
.pac = file extension
Flex-P = prototype / research branch name
```

## Status

PAC is currently experimental.

It is not a finished production codec yet. It can encode and decode test audio in a research notebook, but it still needs more testing, a cleaner command-line encoder/decoder, and more robust codec internals.

## Goal

PAC is designed to sit between FLAC and lossy codecs like MP3/AAC:

```text
WAV  = original uncompressed audio
FLAC = lossless compression, larger but exact
PAC  = perceptual compression, smaller than FLAC, near-transparent
MP3/AAC/Opus = much smaller, more mature lossy codecs
```

PAC is **not lossless**. It is perceptual. That means it may remove or reduce details that are hard for humans to hear.

## Current Best Result

On a full test track using the **STFT Balanced2** preset:

```text
WAV:  18.103 MB
FLAC: 10.928 MB
PAC:   8.319 MB

PAC vs FLAC: 23.88% smaller
SNR: 36.60 dB
Correlation: 0.999891
```

The strongest current setting is:

```text
Transform: STFT
Preset: Balanced2
```

The MDCT path is experimental and currently lower quality.

## Current Pipeline

```text
WAV input
↓
PCM16 test slice / full track
↓
STFT frequency transform
↓
Mid/Side stereo
↓
masking-inspired quantization
↓
bass and stereo protection
↓
deadzone zeroing
↓
symbol stream
↓
entropy compression
↓
.pac / .flex payload
↓
decode back to WAV
↓
compare against original and FLAC
```

## Why STFT Right Now?

STFT is currently the working transform path.

The MDCT path is theoretically better for a real codec, but the current implementation is too crude and loses quality. For now:

```text
Best current quality/compression: STFT Balanced2
Future goal: proper MDCT with block switching
```

## File Format Idea

A PAC file should eventually contain:

```text
magic bytes: PAC
version
sample rate
channels
bit depth
transform mode
block size
hop size
codec preset
metadata
compressed audio payload
```

## Planned Features

- Clean command-line encoder and decoder
- Real `.pac` container
- Better entropy coding
- Better psychoacoustic masking
- MDCT rewrite with proper overlap/windowing
- Block switching for transients
- ABX listening tests
- Multi-song benchmark suite
- Optional Discord preview workflow
- Future player app / web decoder

## Repo Structure

```text
pac-flexp/
├── README.md
├── LICENSE
├── requirements.txt
├── notebooks/
│   └── README.md
├── src/
│   └── pac/
│       ├── __init__.py
│       ├── codec.py
│       ├── symbols.py
│       └── metrics.py
├── tests/
│   └── README.md
├── examples/
│   └── README.md
└── docs/
    └── format.md
```

## Warning

PAC is not ready for real distribution yet.

Do not claim it universally beats FLAC. Current evidence shows it can beat FLAC on some tested files while keeping high correlation and strong listening quality.

The honest claim is:

> PAC / Flex-P is an experimental perceptual audio codec that has beaten FLAC in size on selected test audio while staying very close to the original waveform.

## License

MIT License.
