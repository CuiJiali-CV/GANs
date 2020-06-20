<h1 align="center">Beyond The FRONTIER</h1>

<p align="center">
    <a href="https://www.tensorflow.org/">
        <img src="https://img.shields.io/badge/Tensorflow-1.13-green" alt="Vue2.0">
    </a>
    <a href="https://github.com/CuiJiali-CV/">
        <img src="https://img.shields.io/badge/Author-JialiCui-blueviolet" alt="Author">
    </a>
    <a href="https://github.com/CuiJiali-CV/">
        <img src="https://img.shields.io/badge/Email-cuijiali961224@gmail.com-blueviolet" alt="Author">
    </a>
    <a href="https://www.stevens.edu/">
        <img src="https://img.shields.io/badge/College-SIT-green" alt="Vue2.0">
    </a>
</p>

[Paper Here](https://arxiv.org/abs/1707.04993)
<br /><br />

## Back Ground

* #####  The video generation is much harder for the following reasons:

  - First, since the video is a spatio-temporal recording of visual information of objects performing various actions, the generative model needs to learn the plausible physical motion models of objects in addition to learning their appearance models.
  - Second, the time dimension brings in a huge amount of variations. Consider different videos have the same motion pattern, but played in different speed. The different speed pattern results in a different video, although the appearances of the video is the same.

* ##### Video clip is a point in the latent space unnecessarily increases the complexity of the problem.

  * First, the video of the same action with different execution are represented by different points in the latent space.
  * Second, this assumption forces every generated video clip to have the same length.

  

<br /><br />

## Solutions

#### As a video are about the objects (content) performing actions(motion), the latent space of images should be further decomposed into two subspaces, where the deviation of a point in the first subspace(content) leads content changes in a video clip and the deviation in the second subspace(motion) results in temporal motions.

- In this way, the video of an action with different execution speeds will only result in difference in the motion space.
- Also, decomposing motion and content allows to control the changing of the video. Consider **changing the content representation while fixing the motion space**, we could have the video of different objects performing the same motion pattern. **By changing the motion space while fixing the content representation**, we could have the video of the same objects performing different motion pattern.



## Process

- ##### Overall

  - it generates a video clip by sequentially generating video frames. At each time step, an image generative network maps a random vector to an image. The random vector consists of two parts where the first is sampled from a content subspace and the second is sampled from a motion subspace. 
  - Since content in a short video clip usually remains the same, it models the content space using a Gaussian distribution and use the same realization to generate each frame in a video clip. On the other hand, sampling from the motion space is achieved through a recurrent neural network where the network parameters are learned during training.

  












## Author

```javascript
var iD = {
  name  : "Jiali Cui",
  
  bachelor: "Harbin Institue of Technology",
  master : "Stevens Institute of Technology",
  
  Interested: "CV, ML"
}
```
