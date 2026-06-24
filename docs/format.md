PAC Format Draft
PAC = Perceptual Audio Codec
Proposed extension
```text
.pac
```
Container Layout
```text
PAC magic bytes
header length
JSON or binary header
compressed audio payload
```
Header Fields
```text
version
sample_rate
channels
bit_depth
original_frames
transform_mode
block_size
hop
preset
entropy_backend
metadata
```
Current Prototype Notes
The prototype currently uses STFT and symbol coding. MDCT is experimental.
The decoded WAV is not the compressed file. It is only the reconstructed playback file.
The `.pac` / `.flex` file is the actual compressed audio.
