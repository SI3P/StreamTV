# Stream TV
Stream TV is a multimodal application that allows you to play and control a network stream using the <a href="https://github.com/caprica/vlcj">vlcj framework</a>.

## Features:
- Play a video stream.
- Manage the play-list with your smartphone.
- Control the player with speech, gestures and gesticulations.
- Configure input modalities.

<img src="screenshot/stv.jpg?raw=true" width="400"/>

## Server (Desktop)

The Multimodal Server Architecture takes multimodal inputs and merge them into a semantic frame using different grammars.

- The interpreter controls the life cycle of each modality and realizes the multimodal fusion.
- Each mode recognizes the input signals and notifies the results through an event.
- The recognized sequences are placed in a multidimensional stream to be integrated into a single multimodal frame.
- The application receives the frames and generates a feedback for the user.
  
<img src="screenshot/server.jpg?raw=true" width="500"/>
 
### Speech recognition
Recognizes utterances using a vocabulary and a grammar that allow to build complex commands. It makes use of a probabilistic model and speech pattern representations in the form of statistical models (HMM's).

#### <a target="_blank" href="https://www.google.com/url?sa=t&rct=j&q=&esrc=s&source=web&cd=1&cad=rja&uact=8&ved=2ahUKEwj184Tb347fAhV9AxAIHespAdEQFjAAegQIBxAB&url=http%3A%2F%2Fcmusphinx.github.io%2Fwiki%2Ftutorialsphinx4%2F&usg=AOvVaw2l0Xwk5SxF7nmOmLPsNobO">Sphinx4</a>
A pure Java speech recognition library that provides a quick and easy API to convert the speech recordings into text. 
LiveSpeechRecognizer returns recognition results such as recognized utterance, list of words with time stamps, recognition lattice and confidence values. The words contained in the hypothesis with the highest probability are evaluated in chronological order to rebuild the full command.

<img src="screenshot/jsgf.png" width="500"/>

### Face recognition

Returns a boolean value representing the active presence of at least one user, determined by taking into account the number of faces detected and the state of the eyes:

1. Transform the image to grayscale format and apply Haar-cascade classifier (<a target="_blank" href="https://en.wikipedia.org/wiki/Viola%E2%80%93Jones_object_detection_framework">Viola - Jones object detection framework</a>).
2. Clip and filter regions of interest, coloring pixels white if brightness is greater than a threshold.
3. Erosion + dilation to remove discontinuous white areas.
4. If the white area exceeds 70% of the boundary then the eye is open. Confidence = Max(%L,%R).

<img src="screenshot/face.jpg?raw=true" width="500"/>

### Gesture recognition
1. Apply an Haar-cascade classifier and threshold image in YCrCb color space.
2. Find the convex hull: the smallest convex set that contains the contour points.
3. Compare the convex hull polygon with the contour to identify the convexity defects.
4. Discard points with either shallow depths or too wide angle.

<img src="screenshot/gesture.jpg?raw=true" width="500"/>

#### COG (Center of Gravity)
Moment M expresses how a force F operates at some distance d along a rigid bar from a fixed fulcrum: 

<img src="https://latex.codecogs.com/gif.latex?M=F\cdot&space;d" title="M=F\cdot d" />

<img src="https://latex.codecogs.com/gif.latex?My&space;=&space;m1x1&space;&plus;&space;m2x2&space;&plus;&space;...&space;mnxn" title="My = m1x1 + m2x2 + ... mnxn" />

<img src="https://latex.codecogs.com/gif.latex?Mx&space;=&space;m1y1&space;&plus;&space;m2y2&space;&plus;&space;...&space;mnyn" title="Mx = m1y1 + m2y2 + ... mnyn" />

<img src="https://latex.codecogs.com/gif.latex?C(entroid)&space;=&space;(\frac{My}{m},\frac{Mx}{m})" title="C(entroid) = (\frac{My}{m},\frac{Mx}{m})" />

point -> pixel:

<img src="https://latex.codecogs.com/gif.latex?m(p,q)&space;=&space;\sum_{i=1}^{n}I(x,y)x^{p}y^{q})" title="m(p,q) = \sum_{i=1}^{n}I(x,y)x^{p}y^{q})" />

<img src="https://latex.codecogs.com/gif.latex?COG&space;=&space;\left(\frac{m(1,0)}{m(0,0)};\frac{m(0,1)}{m(0,0)}\right)" title="COG = \left(\frac{m(1,0)}{m(0,0)};\frac{m(0,1)}{m(0,0)}\right)" />

<img src="https://latex.codecogs.com/gif.latex?\tan&space;(2\theta)=\frac{2m(1,1)}{m(2,0)-m(0,2)}" title="\tan (2\theta)=\frac{2m(1,1)}{m(2,0)-m(0,2)}" />

References: <a target="_blank" href="http://fivedots.coe.psu.ac.th/~ad/jg/nui055/index.html">Hand and Finger Detection - Dr. Andrew Davison</a>

#### Label tips
- <b>Index</b> range: 60°  ~ 120°
- <b>Thumb</b> range: 120° ~ 200°

### Motion recognition (optical flow)

1. Transform the image to gray-scale format and capture the feature pixels (corners).
2. Compare pairs of points and generate a list of vectors representing the directions of the movements.
3. Eliminate too short or too long vectors generated by errors and noise.
4. Build a histogram of directions and use the largest list to calculate the COG.
<img src="https://latex.codecogs.com/gif.latex?confidence(v)=\frac{y(x(v))))}{\sum_{i=0}^{i<18}y(i*20))}-\frac{300}{y(x(v))))}" title="confidence(v)=\frac{y(x(v))))}{\sum_{i=0}^{i<18}y(i*20))}-\frac{300}{y(x(v))))}" />

References: <a target="_blank" href="http://fivedots.coe.psu.ac.th/~ad/jg/nui03/index.html">Motion Detection - Dr. Andrew Davison</a>

### Multimodal fusion
- <i>Complementarity</i>: the speech and gesture modalities can be used simultaneously (sequential & synergistic).
- <i>Specialization</i>:  modalities are also specialized on different types of information.
- <i>Redundancy</i>:      cooperation between modalities to speed up the interaction.
- <i>Equivalence</i>:     users can choose which modes to use.

<img src="screenshot/fusion.jpg?raw=true" width="250"/>

#### Linearization
Analyze the multidimensional stream according to the order imposed by the size of the unimodal streams. The longest and temporally valid sequence is linearly combined with those that fall in the time window: [Sentence.Start, Sentence.End + Delta(integration)].

<img src="screenshot/linearization.jpg?raw=true" width="500"/>

#### Multimodal parser
<b>Chomsky normal form:</b>
1. remove useless productions;
2. remove unit productions;
3. remove multiple productions;
4. review semantic rules.

<b>CYK (Cocke–Younger–Kasami) alghoritm:</b>
1. sentence recognition;
2. generation of derivation tree;
3. semantic rules application.

## Client (Android)
