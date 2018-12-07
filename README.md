# Stream TV
A Multimodal Streaming Application:
- play a video stream;
- manage a play-list with your smartphone;
- control the player with speech, gestures and gesticulations;
- configure input modalities.

<img src="screenshot/stv.jpg?raw=true" height="300"/>


## Server (Desktop)

The Multimodal Server Architecture takes multimodal inputs and merge them into a semantic frame using different grammars.

- The interpreter controls the life cycle of each modality and realizes the multimodal fusion.
- Each mode recognizes the input signals and notifies the results through an event.
- The recognized sequences are placed in a multidimensional stream to be integrated into a single multimodal frame.
- The application receives the frames and generates a feedback for the user.
  
<img src="screenshot/server.jpg?raw=true" height="200"/>
 
### Speech Recognition
Recognizes utterances using a vocabulary and a grammar that allow to build complex commands. It makes use of a probabilistic model and speech pattern representations in the form of statistical models (HMM's).

#### Sphinx4
A pure Java speech recognition library that provides a quick and easy API to convert the speech recordings into text. 
LiveSpeechRecognizer returns recognition results such as recognized utterance, list of words with time stamps, recognition lattice and confidence values. The words contained in the hypothesis with the highest probability are evaluated in chronological order to rebuild the full command.

<img src="screenshot/jsgf.png" height="100"/>

### Face Recognition

Returns a boolean value representing the active presence of at least one user, determined by taking into account the number of faces detected and the state of the eyes:

1. Transform the image to grayscale format and apply Haar-cascade classifier (<a href="https://en.wikipedia.org/wiki/Viola%E2%80%93Jones_object_detection_framework">Viola - Jones object detection framework</a>).
2. Clip and filter regions of interest, coloring pixels white if brightness is greater than a threshold.
3. Erosion + dilation to remove discontinuous white areas.
4. If the white area exceeds 70% of the boundary then the eye is open. Confidence = Max(%L,%R).

<img src="screenshot/face.jpg?raw=true" height="200"/>

### Gesture Recognition
1. Apply an Haar-cascade classifier and threshold image in YCrCb color space.
2. Find the convex hull: the smallest convex set that contains the contour points.
3. Compare the convex hull polygon with the contour to identify the convexity defects.
4. Discard points with either shallow depths or too wide angle.

#### COG (Center of Gravity)
Moment M expresses how a force F operates at some distance d along a rigid bar from a fixed fulcrum: 
M=F\cdot d    My=mx   Mx=my
My = m1x1 + m2x2 + ... mnxn
Mx = m1y1 + m2y2 + ... mnyn
C(entroid) = (\frac{My}{m},\frac{Mx}{m})

From points to pixels:
$$m(p,q) = \sum_{i=1}^{n}I(x,y)x^{p}y^{q}})$$
COG = \left(\frac{m(1,0)}{m(0,0)};\frac{m(0,1)}{m(0,0)}\right)
\tan (2\theta)=\frac{2m(1,1)}{m(2,0)-m(0,2)}

## Client (Android)
