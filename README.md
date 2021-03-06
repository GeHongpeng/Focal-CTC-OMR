## Research Paper Publication:
SangCTC- Improving Traditional CTC Loss Function for Optical Music Recognition (https://doi.org/10.1109/PuneCon46936.2019.9105720)

### Citation:  
J. K. Shah, A. S. Padte and P. N. Ahirao,  
"SangCTC-Improving Traditional CTC Loss Function for Optical Music Recognition (August 2019),"  
2019 IEEE Pune Section International Conference (PuneCon), Pune, India, 2019, pp. 1-5,  
doi: 10.1109/PuneCon46936.2019.9105720.
  
# Focal-CTC-OMR  
(Implementation of Part I of SangCTC)  

## Problem Statement  
Given a music sheet, usually in the form of an image, the goal of an OMR system is to use various vision algorithms to interpret the corresponding music symbols and later convert it into digital playable format of music. To solve this problem in an End-to-End manner, Convolutional Recurrent Neural Network (CRNN) architecture is used. It considers both spatial and sequential nature of this problem. CTC loss function is proved to be a favorable choice in these types of sequence problems as it trains the models directly from input images to their corresponding musical transcripts without the need for a frame-by-frame alignment between the image and the ground-truth thereby solving the purpose of End to-End training. Though traditional CTC seems to solve a major chunk of the problem, it suffers from some limitations due to Unbalanced Dataset.   
  
![python](/images/image6.PNG)  

## Unbalanced Dataset Problem 
### Data Visualization 
![python](/images/boxPlot.png)  
![python](/images/barGraph.png)  
![python](/images/waffleChart.png)  
  
### Solution: Focal CTC 
## Focal CTC Loss Function
```python
def focal_ctc(alpha=0.5,gamma=2.0,targets,logits,seq_len):
      
    #FOCAL LOSS
    #This function computes Focal Loss
    #Inputs: alpha, gamma, targets, logits, seq_len
    #Default Values: alpha=0.5 and gamma=2.0
    #Output: loss
       
    ctc_loss = tf.nn.ctc_loss(labels=targets, inputs=logits, sequence_length=seq_len, time_major=True)
    p= tf.exp(-ctc_loss)
    focal_ctc_loss= tf.multiply(tf.multiply(alpha,tf.pow((1-p),gamma)),ctc_loss) #((alpha)*((1-p)**gamma)*(ctc_loss))
    loss = tf.reduce_mean(focal_ctc_loss)
      
return loss    
```
Edit: If you are facing Exploding Gradients Problem ('nan' in loss) try clipping on ```ctc_loss```
## Experiments and Results  
![python](/images/TrainingLoss.png)  
![python](/images/ComparingSymbolErrorRate.png)  


  
  
## Sampling the Dataset  
(Scripts contain sampling and visualization codes)  
![python](/images/SampledDatasetDistributionImbalance.png)  
#### Word Cloud
![python](/images/wordcloud2.png) 

## References
1) End-to-End Neural Optical Music Recognition of Monophonic Scores (https://doi.org/10.3390/app8040606)  
2) Focal CTC Loss for Chinese Optical Character Recognition on Unbalanced Datasets (https://doi.org/10.1155/2019/9345861)  
3) Base Code (https://github.com/OMR-Research/tf-end-to-end)  
