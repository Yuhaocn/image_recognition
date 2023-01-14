# image_recognition
To implement the loss function of YOLO, we will need to recreate the loss function from yolo v1 paper
 
And it will separate to several parts:
	compute_iou function – which meant to Compute the intersection over union of two set of boxes
this help us to find the bounding boxes with highest IOU for the loss calculation.
	Regression_loss function - which refers to Localization loss
 
torch.sum(torch.pow(predicted_corners - target_corners, 2)) Returns the sum of all elements from equation  
torch.sum(torch.pow(torch.sqrt(predicted_sizes) - torch.sqrt(target_sizes), 2)) Returns the sum of all elements from equation  
and multiply by l_coord in Yolo v1 paper is 5.
	Get_class_prediction_loss function - which refers to Classification loss
 
torch.sum(has_object_map* torch.sum(torch.pow(classes_pred - classes_target, 2), dim=-1)) the classification loss at each cell is the squared error of the class conditional probabilities for each class
	Get_contain_conf_loss-If an object is detected in the box, the confidence loss (measuring the objectness of the box) is:
 
torch.sum(torch.pow(box_pred_conf - box_target_conf.detach(), 2))
	Get_no_object_loss-If an object is not detected in the box, the confidence loss is:

 
torch.sum(torch.logical_not(has_object_map) * torch.pow(x, 2)) and multiply by l_noobj in Yolo v1 paper is 0.5.
For the same item, YOLO can perform repeated detections. To correct this, YOLO uses non-maximal suppression to eliminate duplications with a lower confidence level. Non-maximal suppression raises mAP by 2-3%.
The advantage of YOLO is that it is quick. This is ideal for real-time processing. One single network is used to make predictions (object locations and classifications). To enhance accuracy, it can be trained from beginning to conclusion. YOLO is more generalized. When generalizing from natural pictures to other domains such as artwork, it beats previous approaches.
The classifier is limited to a certain region using region proposal approaches. When anticipating limits, YOLO uses the entire image. YOLO has less false positives in background regions now that it has more context.
![1](https://user-images.githubusercontent.com/102940480/212444207-0a0b31fa-bcdd-4b47-a647-6eb1bde99947.png)
![5](https://user-images.githubusercontent.com/102940480/212444208-c9392ec8-6c40-44f4-8e70-098b2d126671.png)
![6](https://user-images.githubusercontent.com/102940480/212444209-46421690-abe3-40ac-93b4-f367a85619fd.png)
![2](https://user-images.githubusercontent.com/102940480/212444210-a3d585e8-3149-4681-b84c-b2e4db41b60f.png)
![3](https://user-images.githubusercontent.com/102940480/212444212-e348f542-16ba-486b-afa6-3c8716bc30ac.png)
![4](https://user-images.githubusercontent.com/102940480/212444213-5f2b7cf5-3689-4258-9ebf-7a4062ae4dec.png)
