# Looper MIDI Sync Proposals

## Kemper as MIDI Master

### Phase 1 — Beat Scanner for the Looper

**Effort:** Low · **Impact:** Medium

At the moment we cannot read an estimated BPM from a recorded loop sample. Enabling the beat scanner for the looper would close that gap.

With MIDI transport sync (play, stop, pause) and phase quantization added on top, the full **Kemper as MIDI Master** feature becomes viable.

#### Suggested flow

```mermaid
flowchart TD
    A["User starts loop recording"]
    B["<b>Audio Recording + Beat Scanner</b><br/>Tempo estimation during recording"]
    C["User finishes loop recording"]
    D["<b>Beat Count Estimation</b><br/>Estimated BPM + exact loop duration"]
    E["<b>Tempo Refinement</b><br/>BPM correction with loop length info"]
    F["<b>Synchronized Loop Playback</b><br/>MIDI Clock + MIDI Transport + Phase Alignment"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```


### Phase 2 — Smart Looper Mode

**Effort:** High(Experimental) · **Impact:** High (potential product differentiator)

Introduce a smart looper mode that listens for repeating guitar phrases, records them automatically, and holds a silent, phase-aligned loop ready to go. One footswitch press starts playback whenever the musician wants. Loop recording start and stop no longer need to be timed manually. 

#### Suggested flow

```mermaid
flowchart TD
    A["Musician arms smart looper mode"]
    B["<b>Phrase Listening</b><br/>Detect repeating guitar phrases"]
    C["<b>Automatic Capture</b><br/>Record phrase without manual start/stop"]
    D["<b>Silent Prep</b><br/>Phase-aligned loop held ready"]
    E["Musician presses footswitch"]
    F["<b>Synchronized Playback</b><br/>Loop starts phase-aligned"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
```


## Kemper as MIDI Slave

**Effort:** High · **Impact:** High

Synchronize the looper to an external MIDI Clock master (e.g. Ableton Live). Recording and playback boundaries follow the external musical grid, and long-term phase drift is corrected via a digital PLL.

When the master tempo changes, the recorded loop is time-stretched with **pitch held fixed** (WSOLA for live tracking). After stretch, loop length is re-locked to an integer beat count so phase stays aligned with the MIDI grid.

#### Suggested flow

```mermaid
flowchart TD
    A["DAW MIDI Clock + Transport"]
    B["MIDI RX + timestamping"]
    C["Clock recovery / digital PLL"]
    D["Transport + musical grid"]
    E["Tempo ratio<br/>master BPM vs loop BPM"]
    F["Quantization scheduler"]
    G["Pitch-preserving stretch<br/>WSOLA / OLA"]
    H["Timestamped DSP commands"]
    I["Looper audio engine"]
    J["Playback phase feedback"]

    A --> B
    B --> C
    C --> D
    D --> E
    E --> F
    F --> G
    G --> H
    H --> I
    I --> J
    J --> F
    J --> C
```
