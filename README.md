STM32 Switch Recorder/Player System
📌 Overview
This firmware implements a record-and-playback system for an STM32 microcontroller that:

Records switch press sequences (timing and duration)

Plays back recorded sequences by controlling LEDs

Provides visual feedback through LEDs

Offers serial debug output via UART

🛠 Hardware Requirements
MCU: STM32F103C6T6 (or compatible)

Switches:

SW1 (PA0): Record control

SW2 (PA1): Play control

SW3-SW7 (PA2-PA6): General input switches

LEDs:

PC13: Onboard LED (activity indicator)

PB3: Record status LED

PB4: Play status LED

PB5-PB8: LEDs 3-7 (corresponding to switches 3-7)

Serial: USART1 (115200 baud) for debugging

🔧 Key Features
Three Operation Modes:

Idle: Waiting for user input

Recording: Capturing switch presses (max 100 events)

Playback: Replaying recorded sequence

Visual Feedback:

Record LED (solid on during recording)

Play LED (solid on during playback)

Onboard LED blinks on any switch press

Individual LEDs mirror switch states during recording

Serial Debug Output:

System status messages

Detailed event log with timestamps

Recording/playback duration

🚀 Usage Instructions
1. Recording Mode
Press and release SW1 (Record button)

Record LED turns on solid

System ready to capture switch presses

Press any combination of SW3-SW7

Corresponding LED lights while pressed

Onboard LED blinks on each press

Press SW1 again to stop recording

Record LED turns off

Event log prints to serial

2. Playback Mode
Ensure a sequence has been recorded

Press and release SW2 (Play button)

Play LED turns on solid

Recorded sequence plays back on LEDs

System automatically stops after playback

Or press SW2 again to stop early

3. Serial Monitoring
Connect to USART1 (115200 baud) to see:

System startup message

Recording start/stop notifications

Playback start/stop notifications

Detailed event log with timing information

⚙ Technical Details
Data Structures
c
typedef struct {
    uint8_t switchNum;   // Which switch (3-7)
    uint32_t pressTime;  // Milliseconds since recording start
    uint32_t duration;   // Press duration in ms
} SwitchEvent;

typedef enum {
    STATE_IDLE,
    STATE_RECORDING,
    STATE_PLAYING
} SystemState;
Key Parameters
MAX_EVENTS: 100 (maximum recordable events)

DEBOUNCE_DELAY: 50ms (switch debouncing)

UART_BAUDRATE: 115200

Memory Usage
Fixed array stores up to 100 events

Each event uses 12 bytes (3x uint32_t)

Total storage: ~1.2KB for events

📝 Notes
The onboard LED (PC13) blinks on any switch press

Record/Play LEDs remain solid during their respective modes

Only SW3-SW7 presses are recorded (SW1/SW2 are control switches)

Playback requires at least one recorded event

Serial output provides detailed timing information

🐛 Known Issues
Fixed-size event buffer (will stop recording after 100 events)

No non-volatile storage (recordings lost on power cycle)

Limited error handling for edge cases

🔮 Possible Enhancements
Add flash memory storage for recordings

Implement multiple recording banks

Add playback speed control

Include error handling for buffer overflows

Add LED patterns for system status

📊 Serial Output Example
System Ready
Recording started at 1024 ms
Recording stopped. Duration: 5043 ms. Recorded 5 events.

[6500ms] Event Log (5 events):
[6500ms] Event 1: SW3 at 120ms (duration 250ms)
[6500ms] Event 2: SW4 at 800ms (duration 400ms)
[6500ms] Event 3: SW5 at 2000ms (duration 100ms)
[6500ms] Event 4: SW3 at 3500ms (duration 500ms)
[6500ms] Event 5: SW6 at 4800ms (duration 200ms)

Playback started at 7000 ms
Playback stopped. Duration: 4800 ms.
