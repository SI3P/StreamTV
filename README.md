# Stream TV
A Multimodal Streaming Application:
- play a video stream;
- manage a play-list with your smartphone;
- control the player with speech, gestures and gesticulations;
- configure input modalities.

<img src="screenshot/stv.jpg?raw=true" height="300"/>


## Server (Desktop)

The Multimodal Server Architecture takes multimodal inputs and merge them into a semantic frame using different grammars:

- the interpreter controls the life cycle of each modality and realizes the multimodal fusion;
- each mode recognizes the input signals and notifies the results through an event;
- the recognized sequences are placed in a multidimensional stream to be integrated into a single multimodal frame;
- the application receives the frames and generates a feedback for the user.
  
<img src="screenshot/server.jpg?raw=true" height="200"/>
 
### Speech Recognition
Recognizes utterances using a vocabulary and a grammar that allow to build complex commands. It makes use of a probabilistic model and speech pattern representations in the form of statistical models (HMM's).

#### Sphinx4
A pure Java speech recognition library that provides a quick and easy API to convert the speech recordings into text. 
LiveSpeechRecognizer returns recognition results such as recognized utterance, list of words with time stamps, recognition lattice and confidence values. The words contained in the hypothesis with the highest probability are evaluated in chronological order to rebuild the full command.

<img src="screenshot/jsgf.png" height="100"/>

### Face Recognition

Returns a boolean value representing the active presence of at least one user, determined by taking into account the number of faces detected and the state of the eyes:

- transform the image to grayscale format and apply Haar-cascade classifier (<a href="https://en.wikipedia.org/wiki/Viola%E2%80%93Jones_object_detection_framework">Viola - Jones object detection framework</a>)
- clip and filter regions of interest, coloring pixels white if brightness is greater than a threshold
- erosion + dilation to remove discontinuous white areas
- if the white area exceeds 70% of the boundary then the eye is open. Confidence = Max(%L,%R)

<img src="screenshot/face.jpg?raw=true" height="200"/>

### Gesture Recognition

## Client (Android)
