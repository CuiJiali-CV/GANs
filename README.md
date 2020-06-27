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

- ##### Details

  - ![Image text](https://github.com/CuiJiali-CV/GANs/raw/master/imgs/mocogan/process.png)
  - For a video, the content vector, <img src="https://latex.codecogs.com/gif.latex?Z_c" title="Z_c" />, is sampled once and fixed. Then, a series of random variables <img src="https://latex.codecogs.com/gif.latex?(\epsilon_{(1)},\epsilon_{(2)},...,\epsilon_{(K)})" title="(\epsilon_{(1)},\epsilon_{(2)},...,\epsilon_{(K)})" />is sampled and mapped to a series of motion codes <img src="https://latex.codecogs.com/gif.latex?(Z^{(1)}_M,Z^{(2)}_M,...,Z^{(K)}_M)" title="(Z^{(1)}_M,Z^{(2)}_M,...,Z^{(K)}_M)" />,via the recurrent neural network<img src="https://latex.codecogs.com/gif.latex?R_M" title="R_M" />. A generator <img src="https://latex.codecogs.com/gif.latex?G_I" title="G_I" />produces a frame, <img src="https://latex.codecogs.com/gif.latex?\tilde{x}^k" title="\tilde{x}^k" />,using the content and the motion vectors <img src="https://latex.codecogs.com/gif.latex?[Z_c,Z_M^k]" title="[Z_c,Z_M^k]" />. The discriminators, <img src="https://latex.codecogs.com/gif.latex?D_I" title="D_I" />and <img src="https://latex.codecogs.com/gif.latex?D_V" title="D_V" />, are trained on real and fake images and videos, respectively, sampled from the training set <img src="https://latex.codecogs.com/gif.latex?v" title="v" />and the generated set <img src="https://latex.codecogs.com/gif.latex?\tilde{v}" title="\tilde{v}" />. The function <img src="https://latex.codecogs.com/gif.latex?S_1" title="S_1" />samples a single frame from a video, <img src="https://latex.codecogs.com/gif.latex?S_T" title="S_T" /> samples T consequtive frames. 

- ##### Tricks

  - Ideally, <img src="https://latex.codecogs.com/gif.latex?D_V" title="D_V" /> alone should be sufficient for training <img src="https://latex.codecogs.com/gif.latex?G_I" title="G_I" />and <img src="https://latex.codecogs.com/gif.latex?R_M" title="R_M" />, because <img src="https://latex.codecogs.com/gif.latex?D_V" title="D_V" /> provides feedback on both static image appearance and video dynamics. However, we found that using <img src="https://latex.codecogs.com/gif.latex?D_I" title="D_I" />significantly improves the convergence of the adversarial training. This is because training <img src="https://latex.codecogs.com/gif.latex?D_I" title="D_I" /> is simpler as it only needs to focus on static appearances.

  - ###### Categorical Dynamics

    - Dynamics in videos are often categorical. To model this categorical signal, we augment the input to <img src="https://latex.codecogs.com/gif.latex?R_M" title="R_N" /> with a categorical random variable, <img src="https://latex.codecogs.com/gif.latex?z_A" title="D_V" />, where each realization is a one-hot vector. We keep the realization fixed since the action category in a short video remains the same. The input to <img src="https://latex.codecogs.com/gif.latex?R_M" title="D_V" /> is then given by  <img src="https://latex.codecogs.com/gif.latex?[\begin{bmatrix}&space;z_A\\\epsilon^{(1)}&space;\end{bmatrix},...,\begin{bmatrix}&space;z_A\\\epsilon^{(K)}&space;\end{bmatrix}]" title="[\begin{bmatrix} z_A\\\epsilon^{(1)} \end{bmatrix},...,\begin{bmatrix} z_A\\\epsilon^{(K)} \end{bmatrix}]" />.
    - To relate <img src="https://latex.codecogs.com/gif.latex?z_A" title="D_V" /> to the true action category, we adopt the infoGAN learning and augment the objective function to <img src="https://latex.codecogs.com/gif.latex?F_V(D_I,&space;D_V,&space;G_I,&space;R_M)&space;&plus;&space;\lambda&space;L_I&space;(G_I,&space;Q)" title="F_V(D_I, D_V, G_I, R_M) + \lambda L_I (G_I, Q)" />where <img src="https://latex.codecogs.com/gif.latex?L_I" title="L_I" /> is a lower bound of the mutual information between the generated video clip and <img src="https://latex.codecogs.com/gif.latex?z_A" title="L_I" />, <img src="https://latex.codecogs.com/gif.latex?\lambda" title="L_I" /> is a hyperparameter,  and the auxiliary distribution <img src="https://latex.codecogs.com/gif.latex?Q" title="L_I" />(which approximates the distribution of the action category variable conditioning on the video clip) is implemented by adding a softmax layer to the last feature layer of <img src="https://latex.codecogs.com/gif.latex?D_V" title="D_V" />. We use <img src="https://latex.codecogs.com/gif.latex?\lambda = 1" title="L_I" />. We note that when labeled training data are available, we can train Q to output the category label for a real input video clop to further improve the performance.

- ##### Experiments

  - videos of the same object performing different motions by using a fixed content vector and varying motion trajectories
  - videos of different objects performing the same motion by using different content vectors and the same motion trajectory.
  - ![Image text](https://github.com/CuiJiali-CV/GANs/raw/master/imgs/mocogan/exp1.png)
  - ![Image text](https://github.com/CuiJiali-CV/GANs/raw/master/imgs/mocogan/exp2.png)
  - ![Image text](https://github.com/CuiJiali-CV/GANs/raw/master/imgs/mocogan/exp3.png)

  












## Author

```javascript
var iD = {
  name  : "Jiali Cui",
  
  bachelor: "Harbin Institue of Technology",
  master : "Stevens Institute of Technology",
  
  Interested: "CV, ML"
}
```
