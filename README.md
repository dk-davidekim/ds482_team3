# Team 3

## Method
### Facial Recognition and Attribute Analysis
We used the [deepface](https://github.com/serengil/deepface) package. We use this package for both facial verification `f(image_1, image_2) -> similarity_score` as well as attribute analysis for findind the race of people in images `f(image_1) -> {'white': 0.1, 'middle eastern': 0.34, ...}'. Information can be found on their GitHub

### Image cloaking
To cloak images, we use the [`fawkes`](`https://sandlab.cs.uchicago.edu/fawkes/') tool. This tool takes pictures and returns a cloaked version. The cloaked version looks similar to human eyes, but degrades the performance of facial recognition tools.

### Data
The data we used was the [VGGFace2](https://www.robots.ox.ac.uk/~vgg/data/vgg_face2/) Dataset. This dataset is available as a [torrent](https://academictorrents.com/details/535113b8395832f09121bc53ac85d7bc8ef6fa5b) and it around 40Gb. A crucial part of this project is to have a racially and genedered balanced dataset. This is not one of those datasets. We could not find a large and publially available dataset that was both racially and gender balanced, that also had labeled individuals with multiple pictures of every individual. This dataset contains the gender of individuals, labeled individuals, and multiple instances of every individual. We describe in the data preprocessing section how we bridged the final gap.

### Data Preprocessing
1. Racial and Gender Balancing - We wanted a dataset that was balanced between races and gender. We decided to use a curated dataset of 100 individuals of every race (races included in the `deepface` attribute detection), balanced with 50 males and 50 females. We looped through every individual in the training set (~9500 individuals) and ran the `deepface` attribute detection to get a set of confidence scores for every instance of that individual. We sum the confidence scores for each image and the the maximum to determine the race for every individual. We continued our loop until we found 50 instances of every group (white male, white female, Indian male, Indian female, ...). This underscored how unbalanced the original dataset was in that it took around 400 individuals to get our 50 white males, and just over 8000 individuals to get our 50 black females. Finally, we randomly sample 5 images from each of our individuals and this is our curated dataset.  

2. Cloaking
For each of our images, we use `fawkes` to cloak the image.

### Testing
Now that we have the curated dataset and have cloacked it, for each individual we run the following 
1. Select a random non cloaked image called `base`. For every other non cloaked image `other_i` and the cloaked version of it `other_cloaked_i`, we calculate the effectiveness of the cloacking with `\sum_{i} (v(base, other_i) - v(base, other_cloaked_i))` where `v` is the verification function of `deepface` that provides a distance between the two scores. A large score means that `fawkes` is effective at cloaking this individual.'
2. Now that we have scores for every individual, we can group along either race, gender or both, and compare the efficacy of `fawkes`