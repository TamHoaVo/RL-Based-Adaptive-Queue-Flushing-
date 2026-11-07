# RL-Based Adaptive Queue Flushing for Real-Time Video

This project tests whether tabular Q-learning can improve video frame delivery
during wireless congestion by deciding when to keep or selectively flush
queued frames.

## What’s Done

- Built a Python simulator for a 30 FPS video stream with a finite router queue
  and a 150 ms playback deadline.
- Implemented three policies: FIFO, a Gecko-inspired threshold heuristic,
  and a Q-learning agent with 54 states and two actions (keep or flush).
- Trained the agent for 600 episodes and evaluated all three policies on the
  same held-out traces under stable bandwidth, fluctuating bandwidth, and a
  sudden bandwidth drop.
- Added outputs for mean delay, P95 delay, frame loss, and on-time delivery,
  plus a training reward plot and a policy comparison plot.

## Results So Far

| Scenario | FIFO on time | Gecko-inspired on time | Q-learning on time |
|---|---:|---:|---:|
| Stable | 100.00% | 100.00% | 100.00% |
| Fluctuating | 50.27% | 87.25% | 78.68% |
| Sudden drop | 64.38% | 77.92% | 72.37% |

Q-learning reduced tail delay and improved on-time delivery compared with FIFO
in both congested scenarios, at the cost of higher frame loss. The
Gecko-inspired heuristic achieved the highest on-time delivery in both scenarios.

## What Needs Improvement

1. **Test frame importance.** Train one agent with the importance input and one
   without it. Keep the flush rule the same and report on-time delivery
   separately for important and normal frames.
2. **Expand the test conditions.** Vary the video bitrate, queue capacity, and
   playback deadline one at a time while reusing the same bandwidth traces.
3. **Tune the reward and repeat training.** Compare different penalties for
   frame loss and deadline misses, then report how results vary across
   training seeds.
4. **Add an adaptive Gecko baseline.** The current baseline uses a fixed delay
   threshold. Implement a sender-side comparison of flush and keep outcomes
   at congestion alerts to better match the paper’s online A/B approach.

