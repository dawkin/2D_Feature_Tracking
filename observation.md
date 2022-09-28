# 2D feature Tracking Observations

## Data Buffer

The data buffer need to be optimized in order to store only the needed data at each iteration. In the following example only two frame are kept inside the buffer to perform the matching between two frame. The implementation is done using the queue model, the frames are added to the end of the queue and removed from the front.

```
dataBuffer.push_back(frame);

while(dataBuffer.size() > dataBufferSize){
    dataBuffer.erase(dataBuffer.begin());
}
```
## Keypoints
The given number of keypoints in the following section is the number of keypoints found on the car and not the entire frame.

The top detector in term of keypoints number the best algorithm is the BRISK and in term of speed the best one is the FAST.

### SHI-TOMASI

distribution of neighbouring size: all small

Computation time(min, max) : 19,5002-42,0977 ms

    frame 1 :
        n = 125
    frame 2 :
        n = 118
    frame 3 :
        n = 123
    frame 4 :
        n = 120
    frame 5 :
        n = 120
    frame 6 :
        n = 113
    frame 7 :
        n = 114
    frame 8 :
        n = 123
    frame 9 :
        n = 111
    frame 10 :
        n = 112

### HARRIS

distribution of neighbouring size: all small

Computation time(min, max) : 24,6506-59,2363 ms

    frame 1 :
        n = 17
    frame 2 :
        n = 14
    frame 3 :
        n = 18
    frame 4 :
        n = 21
    frame 5 :
        n = 26
    frame 6 :
        n = 43
    frame 7 :
        n = 18
    frame 8 :
        n = 31
    frame 9 :
        n = 26
    frame 10 : 
        n = 34

### FAST

distribution of neighbouring size: all small

Computation time(min, max) : 0,930502-2,09801 ms

    frame 1 :
        n = 149
    frame 2 :
        n = 152
    frame 3 :
        n = 150
    frame 4 :
        n = 155 
    frame 5 :
        n = 149
    frame 6 :
        n = 149
    frame 7 :
        n = 156
    frame 8 :
        n = 150
    frame 9 :
        n = 138
    frame 10 :
        n = 143
		
		
### BRISK

distribution of neighbouring size: Small to Big

Computation time(min, max) : 40,2603-69,8223 ms

	
	frame 1 : 
		n = 264
	frame 2 :
		n = 282
	frame 3 :
		n = 282
	frame 4 :
		n = 277
	frame 5 :
		n = 297
	frame 6 :
		n = 279
	frame 7 :
		n = 289
	frame 8 :
		n = 272
	frame 9 :
		n = 266
	frame 10 :
		n = 254
		
### ORB

distribution of neighbouring size: varying from medium size to big size

Computation time(min, max) : 6,81303-19,0001 ms
	
	frame 1 : 
		n = 92
	frame 2 :
		n = 102
	frame 3 :
		n = 106
	frame 4 :
		n = 113
	frame 5 :
		n = 109
	frame 6 :
		n = 125
	frame 7 :
		n = 130
	frame 8 :
		n = 129
	frame 9 :
		n = 127
	frame 10 :
		n = 128
		
### AKAZE

distribution of neighbouring size: From small to medium size

Computation time(min, max) : 131,699-242,478 ms
	
	frame 1 : 
		n = 166
	frame 2 :
		n = 157
	frame 3 :
		n = 161
	frame 4 :
		n = 155
	frame 5 :
		n = 163
	frame 6 :
		n = 164
	frame 7 :
		n = 173
	frame 8 :
		n = 173
	frame 9 :
		n = 177
	frame 10 :
		n = 179
		

### SIFT 

distribution of neighbouring size: From very small to very big

Computation time(min, max) : 152,286-324,673 ms
	
	frame 1 : 
		n = 138
	frame 2 :
		n = 132
	frame 3 :
		n = 124
	frame 4 :
		n = 137
	frame 5 :
		n = 134
	frame 6 :
		n = 140
	frame 7 :
		n = 137
	frame 8 :
		n = 148
	frame 9 :
		n = 159
	frame 10 :
		n = 137


## Descriptors and Matchers

List of implemented descriptor extractors: BRISK, BRIEF, ORB, FREAK, AKAZE, SIFT

The perfomances of the detectors and descriptors combinations will be addressed in the next section the following will look at matchers performances. The first detector will be the HARRIS detector since it provide a small amount of point with the BRIEF then the second one, the BRIF detector which gives the biggest amount of point to analyse the impact of the number of point to match.

HARRIS-BRIEF-BF-NN: 18 points -> 0,094 ms
HARRIS-BRIEF-BF-KNN: 18 points -> 0,094 ms
HARRIS-BRIEF-FLANN-NN: 18 points -> 0,327 ms
HARRIS-BRIEF-FLANN-KNN: 18 points -> 0,335 ms

BRISK-BRIEF-BF-NN: 282 points -> 4,325 ms
BRISK-BRIEF-BF-KNN: 282 points -> 6,139 ms
BRISK-BRIEF-FLANN-NN: 282 points -> 5,14 ms
BRISK-BRIEF-FLANN-KNN: 282 points -> 4,448 ms

The conclusion is that for a small amount of keypoints the FLANN approach is not worth and must only be used when the number of keypoints is growing. FLANN is best when combined with KNN approach.

## Perfomances

Results are stored in the spreadsheets under multiple format.

In the next section I will suggest a top 3 of detector and descriptor combinations, to do so it is important to define in which context this algorithm is supose to be used.
This algorithm will be embedded in a vehicle with limited ressources, will perform in a dynamic environment and with potential risk for peoples health.

Which means our algorithm needs to be lightweight to fit the limited ressources and fast enough to preserv validity of data in time. Also a high number of keypoint will prioritized for redundancy to ensure trustable data.

### TOP 3 combinations

1. FAST/BRIEF - FAST/ORB
2. FAST/BRISK
3. ORB/BRISK

#### FAST/BRIEF - FAST/ORB

The decision between these two seems to be a trade off between frame rate or redundancy but they offer a the best frame rate and redundancy accross all combinations considering our KPIs.

#### FAST/BRISK 

This one is a little bit slower than the previous ones.

#### ORB/BRISK

Is still a decent solution but with a lower frame rate and less matches thus less redundancy.
