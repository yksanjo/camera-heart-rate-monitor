# camera-heart-rate-monitor

[![License: MIT](https://img.shields.io/badge/License-MIT-yellow.svg)](https://opensource.org/licenses/MIT) [![GitHub stars](https://img.shields.io/github/stars/yksanjo/camera-heart-rate-monitor?style=social)](https://github.com/yksanjo/camera-heart-rate-monitor/stargazers) [![GitHub forks](https://img.shields.io/github/forks/yksanjo/camera-heart-rate-monitor.svg)](https://github.com/yksanjo/camera-heart-rate-monitor/network/members) [![GitHub issues](https://img.shields.io/github/issues/yksanjo/camera-heart-rate-monitor.svg)](https://github.com/yksanjo/camera-heart-rate-monitor/issues)
[![Last commit](https://img.shields.io/github/last-commit/yksanjo/camera-heart-rate-monitor.svg)](https://github.com/yksanjo/camera-heart-rate-monitor/commits/main)

Quick python script which allows you to detect your heart rate by simply placing your finger on your camera.

# How does it work?
## Idea
I noticed you can see your heart beat when you put your finger on your phone camera and hold it upto the light. The easiest way to detect this is by seeing changes in average intensity in the frame.

## Implementation
1. Fetch stream of camera frames
2. Convert to greyscale, compute average intensity of each frame -> $I = [I_1, ..., I_2]$
3. Compute first derivative of this array, i.e. $dI = I[1:] - I[:-1]$
4. Find longest sequence of itensities, $K$, where derivative is relatively flat (not much intensity change) $K = I[i:i+k]$
5. Apply gaussian blur to remove noise from $K$.
6. Peaks in $K$ indicate heart beats, so take derivative again
     - We apply a gaussian blur to $K$ to remove noise before computing $dK$.
7. When $dK$ crosses $0$ from above, this indicates a heart beat
