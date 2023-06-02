# Assignment4 Report
***12011408 徐春骥***
### Implement
1. step 1:  ***get positive features***

Handling Noise: For each positive example, add noise to the image. This introduces variations in the positive examples, which can help improve the robustness and generalization of the HoG features.

Feature Concatenation: The function stores both the HoG features of the original image and the HoG features of the noisy image in the feats array. This design decision allows the model to learn from both the clean and noisy versions of the positive examples.

2. step 2: 
