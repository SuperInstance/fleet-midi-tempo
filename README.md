# fleet-midi-tempo

_Tempo and time feel — how fast and how swung?_

_One of 16 ternary MIDI agents in the [Live Paradigm Fleet](https://github.com/SuperInstance/sailor-workspace)._

---

## Philosophy — Why Ternary?

The Live Paradigm treats musical gestures as ternary operations. Where binary logic
gives yes/no, ternary gives **approve/reject/observe** — a richer cognitive substrate
that maps naturally to music theory, emotional tension, and conversational flow.

This agent implements **ternary decomposition for tempo**.

## Architecture

Position in the fleet pipeline:

```
🎤 Voice → OpenSMILE (25 features) → Ghost Track (T-0..T-4 CR predictions)
  → tminus-dispatcher (cue scheduling) → Fleet Conductor (routing)
  → tempo (port 2163)
```

## API Reference

| Method | Path | Description |
|--------|------|-------------|
| GET | /health | Health check + agent identity |
| POST | /agent with `{"type":"probe"}` | Liveness probe for fleet-conductor |
| POST | /agent | Process musical data, return ternary analysis |
| POST | / | Direct query with JSON body |

### Response Format

```json
{
  "status": "ok",
  "agent": "fleet-midi-tempo",
  "port": 2163,
  "ternary_vector": [0, 0, 0],
  "ternary_invariant": 0,
  "closed_gesture": false
}
```

## Ternary Logic

| Position | +1 | 0 | -1 |
|----------|------|------|------|
| ternary[0] | fast (≥120 BPM) | moderate (80-119 BPM) | slow (<80 BPM) |

## Educational Supplement

Tempo is the pulse of music, measured in beats per minute (BPM). It's the most
fundamental rhythmic parameter — everything else (groove, feel, accent) builds on it.

### BPM Ranges
- Slow (-1): <80 BPM — ballads, laments, spacious
- Moderate (0): 80-119 BPM — walking tempo, natural
- Fast (+1): ≥120 BPM — dance, excitement, urgency

### Time Feel
- Straight: Even eighth notes, no swing
- Swung: Uneven eighth notes, 2:1 or 3:2 ratio
- Pulse: Minimal subdivision, downbeat emphasis

## Fleet Integration

- **Port**: 2163
- **Roles**: tempo
- **Conductor ID**: `tempo`
- **Protocol**: HTTP POST to `/2163/agent` with JSON body, 5s timeout
- **Conservation Law**: Σ(Δ_midi) = 4 × Σ(ternary) — closed gestures return to start

## Starting

Local development:

```bash
python3 engine.py --port 2163
```

Or via the fleet start script:

```bash
./scripts/start-fleet-agents.sh
```

## Credits

**Part of the Live Paradigm Fleet** — A ternary cognitive architecture for musical AI.
GitHub: github.com/SuperInstance
Fleet conductor: [sailor-workspace](https://github.com/SuperInstance/sailor-workspace)
