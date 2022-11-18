# Supervised_Contrastive_Learning

Extending Contrastive Learning to the Supervised Setting

## Description

* In recent years, self-supervised representation learning, which is used in a variety of image and video tasks, has significantly advanced due to the application of contrastive learning. These contrastive learning approaches typically teach a model to pull together the representations of a target image (a.k.a., the “anchor”) and a matching (“positive”) image in embedding space, while also pushing apart the anchor from many non-matching (“negative”) images. Because labels are assumed to be unavailable in self-supervised learning, the positive is often an augmentation of the anchor, and the negatives are chosen to be the other samples from the training minibatch. However, because of this random sampling, false negatives, i.e., negatives generated from samples of the same class as the anchor, can cause a degradation in the representation quality. Furthermore, determining the optimal method to generate positives is still an area of active research.

* In contrast to the self-supervised approach, a fully-supervised approach could use labeled data to generate positives from existing same-class examples, providing more variability in pretraining than could typically be achieved by simply augmenting the anchor. However, very little work has been done to successfully apply contrastive learning in the fully-supervised domain.
Paper proposes a novel loss function, called SupCon, that bridges the gap between self-supervised learning and fully supervised learning and enables contrastive learning to be applied in the supervised setting. 

* This is the first contrastive loss to consistently perform better on large-scale image classification problems than the common approach of using cross-entropy loss to train the model directly. Importantly, SupCon is straightforward to implement and stable to train, provides consistent improvement to top-1 accuracy for a number of datasets and architectures (including Transformer architectures), and is robust to image corruptions and hyperparameter variations.



### Methodology

This method is structurally similar to those used in self-supervised contrastive learning, with modifications for supervised classification. Given an input batch of data, we first apply data augmentation twice to obtain two copies, or “views,” of each sample in the batch (though one could create and use any number of augmented views). Both copies are forward propagated through an encoder network, and the resulting embedding is then L2-normalized. Following standard practice, the representation is further propagated through an optional projection network to help identify meaningful features. The supervised contrastive loss is computed on the normalized outputs of the projection network. Positives for an anchor consist of the representations originating from the same batch instance as the anchor or from other instances with the same label as the anchor; the negatives are then all remaining instances. To measure performance on downstream tasks, we train a linear classifier on top of the frozen representations.

### Key Findings

* SupCon consistently boosts top-1 accuracy compared to cross-entropy, margin classifiers (with use of labels), and self-supervised contrastive learning techniques on CIFAR-10 and CIFAR-100 and ImageNet datasets. With SupCon, we achieve excellent top-1 accuracy on the ImageNet dataset with the ResNet-50 and ResNet-200 architectures.

![image](https://user-images.githubusercontent.com/34183327/202644636-ec5bdc16-c384-41ec-b9c7-f680f85feca3.png)

* The gradient contributions from hard positives/negatives are large while those for easy positives/negatives are small. This implicit property allows the contrastive loss to sidestep the need for explicit hard mining, which is a delicate but critical part of many losses, such as triplet loss
* SupCon is also more robust to natural corruptions, such as noise, blur and JPEG compression.SupCon loss is less sensitive than cross-entropy to a range of hyperparameters. Across changes in augmentations, optimizers, and learning rates, we observe significantly lower variance in the output of the contrastive loss.

### Future Work
This work provides a technical advancement in the field of supervised classification. Supervised contrastive learning can improve both the accuracy and robustness of classifiers with minimal complexity. The classic cross-entropy loss can be seen as a special case of SupCon where the views correspond to the images and the learned embeddings in the final linear layer corresponding to the labels. We note that SupCon benefits from large batch sizes, and being able to train the models on smaller batches is an important topic for future research.

