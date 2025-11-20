# Fireflies Effect Documentation

## Overview

The Fireflies effect simulates realistic firefly bioluminescence with species-specific flash patterns. Each firefly independently follows a state machine that controls its flash behavior, creating natural-looking animations inspired by real firefly species.

## User Controls

The effect provides four main parameters for customization:

### Speed (0-255)
Controls how fast the flash patterns play. Higher values make patterns execute faster.
- **Low (0-100)**: Very slow, meditative flashing
- **Medium (100-200)**: Natural firefly timing
- **High (200-255)**: Rapid, energetic flashing

### Fireflies (0-255, labeled as "Intensity")
Determines the number of active fireflies (1-32 fireflies).
- Maps internally: `1 + (intensity >> 3)`
- **Low intensity**: 1-8 fireflies (sparse)
- **Medium intensity**: 9-16 fireflies (moderate)
- **High intensity**: 17-32 fireflies (dense swarm)

### Activity (0-255, Custom1)
Controls how frequently fireflies trigger their flash patterns. Higher values mean more frequent flashing.
- **Low (0-64)**: Infrequent, occasional flashes
- **Medium (64-128)**: Moderate activity, balanced
- **High (128-255)**: Very active, constant flashing

### Species (0-31, Custom2)
Selects the flash pattern behavior. See species list below.

### Color
All fireflies use the same color from the first color slot. Traditional fireflies use warm yellow/amber (#FFD700, #FFA500), but any color can be used for creative effects.

## Species Patterns

### Species 0: Eastern Firefly (Photinus pyralis)
**Pattern**: Single quick flash
**Behavior**: Quick fade in → brief hold → quick fade out → long pause
**Inspiration**: The classic North American firefly
**Best for**: Realistic firefly simulation

### Species 1: Photuris
**Pattern**: Double flash
**Behavior**: Quick flash → pause → second quick flash → long pause
**Inspiration**: Photuris fireflies known for mimicking other species
**Best for**: More complex, rhythmic patterns

### Species 2: Synchronous
**Pattern**: Slow steady glow
**Behavior**: Slow smooth fade in → long hold → slow smooth fade out → pause
**Inspiration**: Great Smoky Mountains synchronous fireflies
**Best for**: Ambient, meditative lighting
**Note**: Uses easing functions for smooth curves

### Species 3: Rapid Blinker
**Pattern**: Multiple quick flashes
**Behavior**: Four rapid flashes in succession → pause
**Best for**: Energetic, attention-grabbing effects

### Species 4: J-Curve
**Pattern**: Slow rise, instant drop
**Behavior**: Smooth slow fade in → instant drop to zero → pause
**Best for**: Asymmetric, dramatic flashes
**Note**: Uses cubic easing for smooth rise

### Species 5: Triple Flash
**Pattern**: Three flashes in succession
**Behavior**: (Flash → pause) × 3 → long pause
**Best for**: Rhythmic, patterned lighting

### Species 6: Long Pulse
**Pattern**: Extended glow
**Behavior**: Medium fade in → very long hold → medium fade out → long pause
**Best for**: Gentle, sustained illumination

### Species 7: Morse Code
**Pattern**: Dot-dot-dash
**Behavior**: Short flash → pause → short flash → pause → long flash → long pause
**Best for**: Unique rhythmic pattern, communication-themed displays
**Fun fact**: Classic morse code "S" pattern (dot-dot-dash)

### Species 8: Flicker/Shimmer
**Pattern**: Random brightness variations
**Behavior**: Fade in → random brightness levels with quick transitions → pause
**Best for**: Organic, candle-like flickering effect
**Note**: Uses hardware random for unpredictable shimmer

### Species 9: Sawtooth
**Pattern**: Symmetric rise and fall
**Behavior**: Linear fade in → linear fade out (same duration) → pause
**Best for**: Geometric, predictable patterns

### Species 10: Stuttered Burst
**Pattern**: Machine gun effect
**Behavior**: Five ultra-fast flashes in rapid succession → pause
**Best for**: High-energy, staccato effects
**Note**: Uses very short timing (speedFactor >> 6) for ultra-fast execution

### Species 11: Pulse Wave
**Pattern**: Instant on, slow fade
**Behavior**: Instant full brightness → long slow fade out → pause
**Best for**: Strobe-like onset with gentle decay

### Species 12-31: Generic Random
**Pattern**: Random variation
**Behavior**: Semi-random timing with basic fade in/hold/fade out/pause
**Best for**: Unpredictable variety

## Testing Recommendations

### Realistic Firefly Simulation
Perfect for outdoor-themed installations or naturalistic displays.

**Settings:**
- **Species**: 0 (Eastern Firefly), 1 (Photuris), or 2 (Synchronous)
- **Color**: Warm yellow/amber (#FFD700 or #FFA500)
- **Speed**: 150-200 (natural timing)
- **Fireflies**: 64-128 (8-16 fireflies)
- **Activity**: 64-128 (moderate frequency)

### Dramatic/Theatrical Effect
High-impact lighting for performances or attention-grabbing displays.

**Settings:**
- **Species**: 10 (Stuttered Burst) or 3 (Rapid Blinker)
- **Color**: Bright white or saturated colors
- **Speed**: 180-220 (fast)
- **Fireflies**: 128-200 (16-25 fireflies)
- **Activity**: 150-255 (high frequency)

### Ambient Mood Lighting
Subtle, relaxing background illumination.

**Settings:**
- **Species**: 6 (Long Pulse), 8 (Shimmer), or 2 (Synchronous)
- **Color**: Soft colors (pale blue, warm amber, gentle purple)
- **Speed**: 80-150 (slow to medium)
- **Fireflies**: 32-96 (4-12 fireflies)
- **Activity**: 32-80 (low to moderate frequency)

### Playful/Whimsical
Fun, engaging patterns for interactive installations.

**Settings:**
- **Species**: 7 (Morse Code), 5 (Triple Flash), or 4 (J-Curve)
- **Color**: Vibrant, saturated colors
- **Speed**: 120-180 (moderate)
- **Fireflies**: 96-160 (12-20 fireflies)
- **Activity**: 80-150 (moderate to high)

### Forest/Garden Theme
Perfect for outdoor LED installations.

**Settings:**
- **Species**: Mix of 0, 1, 2 (use multiple segments with different species)
- **Color**: Yellow-green (#9ACD32) to amber (#FFA500)
- **Speed**: 140-180 (natural)
- **Fireflies**: 64-128 (8-16 fireflies per segment)
- **Activity**: 50-100 (sparse, natural)

### Holiday/Festive
Twinkling lights for celebrations.

**Settings:**
- **Species**: 3 (Rapid Blinker), 5 (Triple Flash), or 8 (Shimmer)
- **Color**: Traditional holiday colors or white
- **Speed**: 150-200 (lively)
- **Fireflies**: 128-255 (16-32 fireflies)
- **Activity**: 100-180 (active)

## Technical Details

### Implementation

The effect uses a struct-based approach where each firefly maintains:
- **Position**: Random LED index (0 to SEGLEN-1)
- **Brightness**: Current brightness level (0-255)
- **State**: Current phase in the flash pattern (state machine)
- **Timer**: Frame counter within current state
- **StateMax**: Duration of current state (varies by species)

### State Machine

Each species implements a unique state machine:
1. **State 0**: Firefly is off, waiting to spawn
2. **States 1+**: Active pattern phases (fade in, hold, fade out, pauses, etc.)
3. **Reset**: Returns to state 0 after pattern completes

### Brightness Calculations

Different brightness curves are used:
- **Linear**: Direct progress mapping (states with simple fade)
- **Eased**: `ease8InOutQuad()` or `ease8InOutCubic()` for smooth organic curves
- **Instant**: Full brightness immediately (pulse wave)
- **Random**: Hardware random values within range (shimmer effect)

### Memory Usage

- **Per firefly**: 7 bytes (position: 2, brightness: 1, state: 1, timer: 2, stateMax: 1)
- **Total**: 7 × (1 + intensity/8) bytes
- **Maximum**: 224 bytes at full intensity (32 fireflies)

### Performance

- **Frame rate**: Returns `FRAMETIME` (standard WLED frame timing)
- **Background fade**: Uses `SEGMENT.fade_out(245)` for smooth trails
- **Efficiency**: Only active fireflies (brightness > 0) are drawn

## Advanced Tips

### Creating Natural Randomness
Use slightly different speed and activity values across multiple segments to create more organic, less synchronized behavior.

### Layering Effects
Combine multiple segments with different species:
- Segment 1: Species 0, low activity (background)
- Segment 2: Species 3, higher activity (accents)

### Color Variation
While the effect uses a single color per segment, you can:
- Use multiple segments with different colors
- Animate the color slot externally for slowly changing hues
- Use palettes in other segments for color variety

### Synchronization Control
- **High activity + low fireflies**: Synchronized appearance
- **Low activity + high fireflies**: Scattered, independent flashing
- **Speed variations**: Use different speeds per segment for natural desynchronization

## Implementation Location

**File**: `wled00/FX.cpp`
**Function**: `mode_fireflies()` (lines 826-1211)
**Effect ID**: `FX_MODE_FIREFLIES` (218)
**Registration**: Added in `setupEffectData()` after Twinklecat effect

## Future Enhancement Ideas

- 2D support for LED matrices with positional movement
- Brightness variation per firefly (dim/bright individuals)
- Optional slow position drift (fireflies floating)
- Additional species patterns (32 slots available, 12 currently used)
- Temperature-based color variation (cooler/warmer based on activity)

---

**Version**: 1.0
**Author**: Created for WLED
**Date**: 2025
**Effect Type**: 1D Animation
**Category**: Nature/Organic Effects
