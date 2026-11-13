# Multi-session architecture

Use a Qt supervisor with one streaming worker process per workstation for the first implementation. Moonlight Qt stores the active session in `Session::s_ActiveSession` and gates execution with a semaphore of capacity one. The pinned moonlight-common-c library also keeps connection callbacks, negotiated formats, ports, and encryption state in process-wide variables. Isolating workers preserves the existing protocol implementation and lets the supervisor recover an individual failed stream. The costs are a per-worker runtime and decoder allocation, an IPC protocol, and additional Windows rendering integration.

The first prototype should keep two windowed workers connected to separate hosts and verify that disconnecting or crashing one leaves the other streaming. Then compare a Windows child window for each SDL surface with a compositor using shared D3D11 textures. The child-window approach can reuse SDL rendering but needs explicit focus, DPI, resize and native window lifetime tests. Shared textures give the Qt shell direct control of presentation but add GPU synchronization and resource-management work. Select the presentation path using measured switching latency and touch/input behavior.

The supervisor owns saved workstation labels, tab order, selected tab, connection state, and pairing data. Each worker receives only the configuration for its host over private local IPC. Use one writer for persistent settings. Never send pairing keys on the command line. A worker crash or supervisor exit must revoke input and close the owned process tree. When switching tabs, have the old worker send release and neutral events for held inputs, drop capture, and acknowledge the handoff before the new worker accepts input. SDL focus and its optional background-gamepad behavior alone cannot enforce this. Apply the gate to keyboard, pointer, touch, and controller events. A watchdog must fail closed if IPC is lost.

## Background profiles and audio

Live Background keeps video reception, decoding and the session active for all workers. Keep the active surface visible and preserve a ready frame for each background worker. Measure whether the chosen Windows surface or compositor changes decoder or presentation behavior when a tab is hidden.

For Limited Data, start by reducing background presentation work and measure host-to-client bytes independently of decoder and GPU load. The client uses bitrate and frame-rate settings at stream setup. In-session bandwidth or FPS reduction remains unverified. Verify Sunshine protocol support before promising a network reduction. Keep the audio path running when the selected mode allows it.

All audible is the default target. Give each worker a single output path and apply audio policy independently from input. Active station only and background stations muted have the same audible result under exclusive tab selection unless a distinct control is defined. Both modes must preserve the stream, and a background alert must play once when the selected policy allows it.

## Acceptance sequence

1. Two independent streams survive a worker failure.
2. Three connected tabs restore their names and order, and switch without reauthentication. Verify input handoff with held keys/buttons, controller axes and touchscreen gestures. Verify individual disconnect and reconnect.
3. Measure Live Background switching against 250 ms and Limited Data switching against 500 ms. Measure per-host bandwidth, decode and encoder load, CPU, GPU, memory, and background audio latency.
4. Run sustained three-host streaming on Onyx with existing Cortex workloads active. Exercise an individual host and network interruption, capture recovery times, and confirm unaffected streams and host workloads.
