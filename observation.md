# 2D feature Tracking Observations

In the next section I will suggest a top 3 of detector and descriptor combinations, to do so it is important to define in which context this algorithm is supose to be used.
This algorithm will be embedded in a vehicle with limited ressources, will perform in a dynamic environment and with potential risk for peoples health.

Which means our algorithm needs to be lightweight to fit the limited ressources and fast enough to preserv validity of data in time. Also a high number of keypoint will prioritized for redundancy to ensure trustable data.

## TOP 3 combinations

1. FAST/BRIEF - FAST/ORB
2. FAST/BRISK
3. ORB/BRISK

### FAST/BRIEF - FAST/ORB

The decision between these two seems to be a trade off between frame rate or redundancy but they offer a the best frame rate and redundancy accross all combinations considering our KPI.

### FAST/BRISK 

This one is a little bit slower than the previous ones.

### ORB/BRISK

Is still a decent solution but with a lower frame rate and less matches thus less redundancy.
