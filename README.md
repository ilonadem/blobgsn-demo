## BlobGSN: Generative Scene Networks with Mid-Level Blob Representations
**Unconstrained Scene Generation with Locally Conditioned Radiance Fields and Mid-Level Blob Representations**<br>
Allen Zhang, David Fang, Ilona Demler<br>

### [Project Page](https://ilonadem.github.io/blobgsn-demo/) | [Data](#datasets)

## Abstract 
We combined BlobGAN with Generative Scene Networks (GSN) to generate editable 3D scenes. Namely, we use Gaussian "blobs" as input to generating a 2-D floorplan that is then used to locally condition a radiance field that represents a 3D scene. The Gaussian blobs represent objects in a scene; by moving, shifting, scaling, removing, and adding the blobs in the latent space we are able to make corresponding changes in the rendered scene. The result is a customizable and editable 3D scene, and a self-suprevised way of identifying and representing the objects in a scene.


## Related Work

Generative adversarial networks (GANs) have wide applications in the realm of scene representations. BlobGAN and GSN both use similar GAN architectures, which we predict will allow them to work well together in combination.

### BlobGAN
BlobGAN presents an unsupervised way to train a mid-level representation for a generative model of scenes. Scenes are modeled as a collection of spatial, alpha-composited “blobs” of features which are placed onto a feature grid and then decoded into an image by a GAN (such as StyleGAN2). The blob parameters encode the position, size, and style of each “entity” in the scene. In experiments, researchers find that editing blobs in the latent space corresponds to edits in the rendered output. Their underlying generator/discriminator architecture is based off of StyleGAN2.

### Generative Scene Networks (GSN)
GSN is also a generative model that learns to create novel indoor scenes. It relies on generating a local latent scene floorplan from a global latent code in order to locally condition NeRFs. This local latent floorplan essentially decomposes the scene from one large radiance field into many smaller local radiance fields. Their underlying generator/discriminator architecture is also based on StyleGAN2.

## Motivation
Current 3D representation research mainly focuses on single isolated objects, such as faces, simple cars, mugs, or small uncluttered scenes. Scenes on the scales of rooms, streets, and cityscapes are only now starting to gain interest in this field, along with a variety of NeRF derivatives used to represent them. However, GANs in this space are still uncommon, as creating novel 3D scenes on this scale is difficult. With a GSN and BlobGAN combination, we hope to couple the mid-scale room generation of GSN with the scene editing capabilities of BlobGAN, and possibly expand to larger-scale scenes.

A model that achieves our goals would allow for a variety of uses. Considering the hype around GANs such as DALL-E and Stable Diffusion, it could be used for generative art in the form of customizable 3D rooms. Our model could also possibly be applied large-scale scenes such as streets and cities, although we predict that this will be quite difficult. The most important application however could be scene understanding. If our blobs can represent objects in a room, that would mean that our model is capable of unsupervised object segmentation in 3D scenes, which could be useful in downstream applications such as autonomous vehicles.

## Proposed Architecture
We propose using the output from the BlobGAN generator as the input latent floorplan for GSN. We hope that the model learns a correspondence between blobs in the floorplan and actual rendered entities, whether that be rooms, walls, or smaller objects such as couches and tables. The discriminator remains the same from GSN (which is also based off StyleGAN so it should also be similar to the BlobGAN discriminator). 

 ![](./figs/architecture.png)

## Results
These results are from a model trained for 2 days on the Replica dataset with a batch size of 4 and 10 blobs.

### Scene Walkthroughs

### Blob Editing

With our blob representation, we can edit blobs in the forms of moving, resizing, adding, removing, and rotating blobs. Such edits lead to changes in the scene. In our case, we hypothesize that these blobs represent large-scale objects such as rooms and walls. Thus, when we move blobs, we can see walls and even whole rooms moving in our scene.

<p align="center">
  <img src="./gifs/moving_blobs_down.gif" width="50%" />
 <br>
  Moving a blob towards the camera.
 <br>
 &nbsp; <br>
 <img src="./gifs/moving_blobs_away.gif" width="50%" />
 <br>
  Moving a blob away from the camera.
  <br>
 &nbsp; <br>
 <img src="./gifs/moving_blobs_left.gif" width="50%" />
 <br>
  Moving a blob to the left of the camera.
  <br>
 &nbsp; <br>
 <img src="./gifs/moving_blobs_right.gif" width="50%" />
 <br>
  Moving a blob to the right of the camera.
</p>

We can manipulate multiple blobs at the same time and resize blobs to make their objects more prominent. For example, we can effectively spawn in a blob to "create" a new wall/room in the scene.

<p align="center">
  <img src="./gifs/moving_blobs_double.gif" width="50%" />
 <br>
  Moving two blobs at the same time.
 <br>
 &nbsp; <br>
 <img src="./gifs/moving_blobs_spawn.gif" width="50%" />
 <br>
  Spawning a blob in the middle of the scene.
</p>

## Datasets
We use the Replica and Vizdoom datasets provided by the Generative Scene Networks authors. They contain scenes and sequences of rgb and depths frames, along with camera parameters.

Dataset | Size | Download Link
--- | :---: | :---:
Vizdoom | 2.4 GB | [download](<https://docs-assets.developer.apple.com/ml-research/datasets/gsn/vizdoom.zip>)
Replica | 11.0 GB | [download](<https://docs-assets.developer.apple.com/ml-research/datasets/gsn/replica.zip>)

Datasets can be downloaded by running the following scripts:  
**VizDoom**<br>
```
python scripts/download_vizdoom.py
```
**Replica**<br>
```
python scripts/download_replica.py
```

## Code Acknowledgements
Our code builds off of existing work:
- [BlobGAN](https://github.com/dave-epstein/blobgan)
- [Generative Scene Networks](https://apple.github.io/ml-gsn/)
