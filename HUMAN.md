# Moonlight Multi-Session user guide

This development branch is preparing a separate Windows on ARM client. Its installer and streaming behavior still need qualification. Use your existing Moonlight client for streaming and RustDesk for fallback access while that work is underway.

The planned setup opens Cortex as the default tab and lets you keep Fyre and Greyshift connected beside it. Selecting a tab should move keyboard, mouse, controller, and touch input to that workstation. The connection status and disconnect control will belong to each tab. Saved names and tab order will be restored when the client starts.

The planned Live Background profile keeps each stream decoding and is intended for fast switching. The Limited Data profile will report the resource savings demonstrated by measurement. The planned default audio mode keeps all connected workstations audible so an alert can be heard from an inactive tab. The architecture notes describe the current limits and the required acceptance tests.
