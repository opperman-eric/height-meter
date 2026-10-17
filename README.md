# Height Meter

A single-file mobile web app. Open `index.html` for manual entry. iPhone sensor access requires an HTTPS host and permission granted from the Enable iPhone level button. No dependencies, server, accounts or data collection.

## Host on Render

Push these files to your Git repository. In Render create a **Static Site**, select that repository, leave the build command empty, and set the publish directory to `.`. Open its HTTPS URL in iPhone Safari. Use Share → Add to Home Screen to save it like an app.

## Using the level

Hold the screen plane vertically and sight along a long edge toward the earpiece end. The phone can be rolled onto either side; aiming toward the bottom end is not supported. Check that a horizontal edge reads approximately 0° and a 45° slope approximately 45° before field use. Optional zero calibration corrects small offsets and resets on reload or a new measurement. Physical iPhone verification is still required.

Height and step length are centimetres; output is metres. “Step length” means one footfall, not a two-step stride. Eye height is entered height minus the head-to-eye offset. The default 12 cm offset is only an estimate; measure it. All inputs are retained in localStorage where available.

For level ground and a vertical object:

    distance = steps × step_length
    little_distance = eye_height / tan(angle)
    height = (distance + little_distance) × tan(angle)
           = eye_height + distance × tan(angle)

The sensor derives elevation of the device's physical +Y axis from beta, and checks screen-plane verticality with beta/gamma. Screen orientation does not change the sighting axis. Manual input supports readings from the iOS Measure app; this web app does not read the Measure app directly.

Example: eye height 1.63 m, 20 steps of 0.70 m, angle 45° → 15.63 m. Sloping ground and leaning objects require a different method. This app provides estimates.

## Calculation trace and capture controls

The calculation trace displays input parameters, SI conversions, the tangent factor, auxiliary and complete triangle calculations, and a direct-form residual check. Intermediate values display 12 significant figures; computation uses JavaScript floating-point precision. The reported height is rounded to 0.01 m, which does not imply centimetre accuracy.

Capture uses the on-screen control. Safari and iOS Home Screen web apps do not provide a supported API for mapping physical volume buttons to arbitrary JavaScript actions. Apple AVCaptureEventInteraction is a native camera-capture API, not a browser API.

## Timed angle acquisition

The capture control starts a cancellable countdown (default 5 s; adjustable from 3 to 30 s). It can be started with any recent sensor orientation; a valid aiming orientation is required only at capture time. A short activation tone and short ticks in the last three seconds precede two ascending confirmation tones after the angle is saved. A low tone indicates failure and the previously captured angle is retained.

Web Audio is initialized from the capture-button gesture. If audio initialization fails, the countdown does not start. Media volume must be audible; physical iPhone audio output still requires device verification. Keep the page visible: hiding it or resetting the observation cancels the countdown. Stale readings, invalid angles, nonvertical screen orientation, interrupted audio, or a timer delayed more than 750 ms reject capture rather than saving a potentially incorrect observation.

Validation: automated headless Edge checks covered starting before aiming, successful timed capture, invalid orientation rejection, stale sensor rejection, cancellation, reset, and absence of JavaScript errors. Sensor and audio were simulated; physical iPhone testing remains necessary.
