# Image-Text-Learning

## 1 - Description
Two branch similarity neural network to perform image retrieval given a text query or caption on Flickr30k dataset. The two branches encodes the images and the captions into a embedding vector of length 128 which is then compared using cosine similarity to rank the output results.

## 2 - Approach
The model was built using two neural networks (one for the images and other for the captions) and was trained using the `triplet loss function`.

#### 2.1 - Dataset
Flickr30k dataset was used to train the models
- The process for getting the dataset can be found [here](http://bryanplummer.com/Flickr30kEntities/) (The dataset is also available on [kaggle](https://www.kaggle.com/hsankesara/flickr-image-dataset)).
- The splits for train, val, and test were used from [here](https://github.com/BryanPlummer/flickr30k_entities)

The dataset consists of `31,783` images out of which `1000` were used for validation and testing each.

#### 2.2 - Two Branch Architecture
![Model Architecture](/assets/model_architecture.png)

##### Image Branch
- The image branch (on the left) takes in an image of size 256 x 256. 
- The Inception network has fixed weights and not trainable. (The embedding for each image was generated before hand using the Inception V3 to save memory)

##### Caption Branch
- The caption branch (on the right) takes in stemmed captions.
- The tokenizer encodes the input into one-hot array of size 13,388 (vocabulary size).

#### 2.3 - Triplet Ranking Loss Function

![Loss Function](/assets/loss_function.png)

- `m`: margin between the positive and negative similarities (parameter).
- `d`: distance function (cosine similarity is used)
- `xi`: training image
- `yp`: positive caption for image `xi`
- `yn`: negative caption for image `xi`
- `yi`: training caption
- `xp`: positive image for caption `yi`
- `xn`: negative image for caption `yi`

More details about the ranking loss can be found [here](https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/reports/custom/15704570.pdf).

#### 2.4 - Similarity Function

![Cosine Sim](/assets/cos_sim.png)

- Close to `1` ~ High Similarity
- Close to `-1` ~ Low Similarity

#### 2.5 - Training
- The training was done using random sampling method on `batch size` of 64 and `margin` of 0.5.
- The trained models are available in the `models` folder.

## 3 - Outputs
**Ground truth image:**

![Ground Truth](/assets/gtruth.png)

**Query** The surfer is in the wave .

**Position in results:** 7th

**Output results**:

![Results](/assets/res.png)

Other results can be found in [image_text_learning_one_hot.ipynb](image_text_learning_one_hot.ipynb).

## References
- Bryan A. Plummer, et al. "Flickr30K Entities: Collecting Region-to-Phrase Correspondences for Richer Image-to-Sentence Models". IJCV 123. 1(2017): 74-93.
- Peter Young, et al. "From image descriptions to visual denotations: New similarity metrics for semantic inference over event descriptions". TACL 2. (2014): 67–78.
- Sethu Hareesh Kolluru, . "A neural architecture to learn image-text joint embedding.". - [link](https://web.stanford.edu/class/archive/cs/cs224n/cs224n.1194/reports/custom/15704570.pdf)
