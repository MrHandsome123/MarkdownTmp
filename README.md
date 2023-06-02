# Assignment4 Report
***12011408 徐春骥***
### Implement
1. step 1:  ***get positive features***

This function should return all positive training examples (faces) from
    36x36 images in 'train_path_pos'. Each face should be converted into a
    HoG template according to 'feature_params'.

Handling Noise: For each positive example, add noise to the image. This introduces variations in the positive examples, which can help improve the robustness and generalization of the HoG features.

Feature Concatenation: The function stores both the HoG features of the original image and the HoG features of the noisy image in the feats array. This design decision allows the model to learn from both the clean and noisy versions of the positive examples.

2. step 2: ***get_random_negative_features***

This function should return negative training examples (non-faces) from any
    images in 'non_face_scn_path'. Images should be loaded in grayscale because
    the positive training data is only available in grayscale (use
    load_image_gray()).
    
Sample Mining: The function is designed to mine a specified number of negative examples from the non-face images. It is not crucial for the function to find exactly num_samples non-face features, as some images may be too small to provide enough samples. The implementation allows for sampling some number of features from each image to collect a sufficient number of negative examples.

Image Processing and Cropping: The function applies certain image processing techniques to the non-face images before feature extraction. It randomly scales the image by a factor between 0.7 and 1.0 (rs), allowing variations in the image scale. It then uses a sliding window approach to crop sub-images of size win_size x win_size from the processed image. The sliding window moves in steps equal to the window size to cover the entire image. This design decision enables the function to extract features from different regions of the non-face images.

Feature Extraction: For each cropped sub-image, the function computes the HoG features using vlfeat.hog.hog() and flattens the feature matrix to obtain a feature vector. The resulting feature vectors are stored in the feats array.

Data Collection and Limitation: The function collects the feature vectors from all the processed sub-images of each non-face image and concatenates them into the feats array. However, since the number of sub-images can be large, the implementation limits the total number of extracted features to num_samples. If the number of collected features exceeds num_samples, the function randomly shuffles the feats array and selects the first num_samples samples. This design decision ensures that the function returns a subset of features that matches the desired number of non-face samples.

3. step 3: **train classifier**

LinearSVC

4. step 4: **mine hard negs**

SVM Prediction: After computing the HoG features, the function uses the provided SVM object to predict whether each feature vector corresponds to a face or not.

False-Positive Filtering: The implementation checks the SVM's predictions for each feature vector. If the prediction exceeds a decision threshold, indicating a false positive, the feature vector is considered a hard negative example. Only the feature vectors corresponding to false positives are retained and stored in the feats array.

5. step 5: **run detector**

Sliding Window: The function uses a sliding window approach to traverse the HoG features and extract bounding boxes with their corresponding HoG features. The sliding window iterates over the feature grid with a step size of step_size. For each window position, the HoG features within the window are flattened and classified using the trained SVM classifier. Bounding boxes are created based on the window position and scaling factor and stored in cur_bboxes. The corresponding classification confidences are stored in cur_confidences.

Non-Maximum Suppression: After processing all sliding windows for each scale, the function performs non-maximum suppression on the detected bounding boxes and confidences. The non_max_suppression_bbox() function is called to remove duplicate detections and keep only the most confident ones. The top topk detections with the highest confidences are selected using np.argsort().

Valid Bounding Boxes: The function filters out invalid bounding boxes that do not pass the non-maximum suppression using the is_valid_bbox variable.

### Result Analyze
![图片](https://github.com/MrHandsome123/MarkdownTmp/assets/101486329/26c6cc29-3f1a-4fbc-babc-b83b7be9dc26)

