# Groove Unified Syntax (GUS)
**Version:** 1.2.1

GUS is a compact notation for describing the felt structure of a groove.

It captures pulse count, pulse shape, anchor, and modifiers. It does not capture instrumentation, dynamics, exact subdivisions, or exact MIDI/audio placement.

## Syntax

```text
GUS ::= <Pulse> [<PulseShape>] [<Anchor>] [<Modifier>...]
```

Example:

```text
P4 A↓ N(1)<
```

## Symbols

### Pulse

Use `P` to name the number of felt pulses in the cycle.

- `P2`, `P3`, `P4`, `P5`, `P6`, `P8`, `P16`

### Pulse Shape

Add a ratio when the pulses have uneven felt spans.

```text
P6[3:3:3:3:2:2]
```

The ratio values must match the pulse count. Ratios describe felt weight, not exact timing.

### Anchor

Use `A` to name the groove's structural gravity.

- `A↓`: Downbeat-anchored.
- `A↑`: Upbeat-anchored.
- `A↔`: Backbeat-anchored.
- Omit `A` for neutral or floating grooves.

### Modifiers

Use modifiers to name plain omissions, syncopated omissions, neighbor hits, and upbeats.

```text
O(<pulse-list>)
S(<pulse-list>)<timing?>
N(<pulse-list>)<timing?>
U(<pulse-list>)
```

#### Plain omission: O

Use `O(...)` when the named pulse is absent with no displacement.

- `O(4)`: Pulse 4 is absent with no displacement.
- `O(2,4)`: Pulses 2 and 4 are absent with no displacement.

#### Omitted pulse syncopation: S

Use `S(...)` when the named pulse is omitted and displaced by neighbor hits.

- `S(4)<`: Pulse 4 is omitted; a neighboring hit before it is present.
- `S(4)>`: Pulse 4 is omitted; a neighboring hit after it is present.
- `S(4)<>`: Pulse 4 is omitted; neighboring hits before and after it are present.
- `S(2,4)<>`: Pulses 2 and 4 are omitted; each has neighbor hits before and after.
- `S(2,3,4,1)<`: Pulses 2, 3, 4, and 1 are omitted with preceding neighbor hits in that order.

#### Neighbor hits on present pulses: N

Use `N(...)` when the named pulse is present and has a neighbor hit.

- `N(1)<`: Pulse 1 is present and has an anticipatory hit before it.
- `N(1)>`: Pulse 1 is present and has a trailing hit after it.
- `N(1)<>`: Pulse 1 is present and has neighbor hits before and after it.
- `N(1,3)<`: Pulses 1 and 3 are present and have anticipatory neighbor hits.

#### Upbeats in P4: U

Use `U(...)` to mark `&` upbeats inside a `P4` feel.

- `U(1)`: `1&` is present.
- `U(2)`: `2&` is present.
- `U(3)`: `3&` is present.
- `U(4)`: `4&` is present.
- `U(1,2,3,4)`: All four upbeats are present.

`U(...)` cannot take `<`, `>`, or `<>`, and it cannot be wrapped inside `S(...)` or `N(...)`.

## Rules

- A GUS expression must contain exactly one `P` token.
- Positions refer to felt pulse numbers, not exact beats or subdivisions.
- Use `O(...)` only when the named pulse is absent with no displacement.
- Use `S(...)` only when the named pulse is omitted and displaced by neighbor hits.
- Use `N(...)` only when the named pulse is present.
- Use `U(...)` only for `&` upbeats in a `P4` feel.
- Use `P8` when upbeats become primary felt pulses.
- Use `P16` when sixteenth-note positions become primary felt pulses.
- Omit neutral values by default.
- Read tokens left to right.

## Live Shorthand

Use shorthand for fast capture while listening.

- `v`: `A↓`
- `^`: `A↑`
- `<`: neighbor hit before the named pulse
- `>`: neighbor hit after the named pulse
- `<>`: neighbor hits before and after the named pulse

Example:

```text
4vN(1)< = P4 A↓ N(1)<
```

## Examples

### Straight groove

Hit pattern: `1, 2, 3, 4`

```text
P4 A↓
```

### Straight groove with missing pulse

Hit pattern: `1, 2, 3`

```text
P4 A↓ O(4)
```

### Straight groove with upbeat anticipation

Hit pattern: `1, 2, 3, 4, 4&`

```text
P4 A↓ U(4)
```

### Metal groove / Gallop groove

Hit pattern: `1, 1&, 1a, 2, 2&, 2a, 3, 3&, 3a, 4, 4&`

```text
P8 A↓ N(1,2,3,4)<
```

### Reggaeton groove / "It's My Life"

Hit pattern: `1, 1a, 2&, 3, 3a, 4&`

```text
P4 A↓ U(2,4) S(2,4)<
```

### 8-pulse action groove / "Basara Deshi"

Hit pattern: `1, 1&, 2, 2&, 3, 3&, 4, 4&`

```text
P8 A↓
```

### 16-pulse sixteenth grid

Hit pattern: `1, 1e, 1&, 1a, 2, 2e, 2&, 2a, 3, 3e, 3&, 3a, 4, 4e, 4&, 4a`

```text
P16 A↓
```

### 6-pulse uneven action groove

Hit pattern: `1, 1a, 2&, 3e, 4, 4&`

```text
P6[3:3:3:3:2:2] A↓
```

### Axel F groove (original)

Hit pattern: `1, 2, 2a, 3e, 3&, 4, 4&`

```text
P4 A↓ S(3)<> U(3,4) 
```

### Axel F groove (no anticipation)

Hit pattern: `1, 2, 2a, 3e, 3&, 4`

```text
P4 A↓ S(3)<> U(3) 
```

### Balearic groove

Hit pattern: `1, 1e, 2&, 3&`

```text
P4 A↓ N(2)< U(2,3) O(4)
```

### Snake charmer groove

Hit pattern: `1, 1&, 1a, 2, 2&, 3, 3&, 3a, 4, 4&`

```text
P8 A↓ U(2,6)
```
