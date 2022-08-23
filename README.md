# similar_fabric Description

### Objectives:  
Develop a feature extraction model that can identify whether two cloths are similar

### Methods:  
Use simese networkr that connects 3 CNN models together, and then use the Euclidean distance to calculate the Triplet loss
  
![image](https://user-images.githubusercontent.com/86472351/153794971-e409a701-dd05-4d8f-95f8-1b9e0c3a1a6a.png)
  
As can be seen from the above figure, the import image is divided into anchor, positive and negative. The loss calculation principle is as follows::  
  
![image](https://user-images.githubusercontent.com/86472351/153796035-ed101d2c-4b3f-48af-b07a-59acd86e48bc.png)  
  
### Outcome: 
85%-90% classification accuracy of similar fabrics.
However, the data set is too small, and so it shows overfitting problem. In the future, it should be retrained as more fabric data available.
After testing,  we found that the ResNet50 pre-trained model of keraas performed better than AlexNet and VGG16. Therefore, should avoid using VGG16 or AlexNet in the future.
  
# similar_fabric Program description

### Dataset Link (包含fabric_data、說明PPT):  
https://drive.google.com/drive/folders/1TdWUdJYUOgRTYbXSxCeTnCEPWCn7JB0H?usp=sharing

### Main program:  
simese.ipynb: The main program for training deep learning models (detailed operations are in the comments)
Main steps: read in image files and do preprocessing -> recombine image vectors into triplet dataset -> construct simese model using ResNest50 -> train -> test

### Required folders for programs operation:  
fabric_data: Similar fabric information provided by LittleKing, each folder in it is classified according to similar fabrics
model: Store the eigenvalue extraction model generated by simese, Res_model_emb_c_1.h5 is the first version of the official model
fabric_array: Used to store the content of simese.ipynb preliminary processing image file
triplet: Used to store the triplet dataset constructed by simese.ipynb
temp: Temporary folder for storing simese.ipynb for data argumentation
  
  
# similar fabric model update method
1. Add new fabric types to fabric_data dataset:  
Folders are filed by similar fabric types (each folder name is the code name of a similar fabric)
  
![image](https://user-images.githubusercontent.com/86472351/153983251-e412270a-1cb9-4275-bced-f755b3b5ca3c.png)  
  
![image](https://user-images.githubusercontent.com/86472351/153983698-5057908c-d1da-4690-b70e-55caa33c5f34.png)  

2. Modify the simese.ipynb path:  
os_base = 'Path where all the above folders are stored'  
data_file = 'fabric_data'   

3. After running, you will get two model results siamese_triplet, embedding_model, and save them with the extension of .h5 
The above two models can be read with: model = load_model('xxxx.h5')
siamese_triplet: to continue previous training ， history = model.fit(x=train_x, shuffle=True, batch_size=batch,validation_split=.2, epochs=5)  
embedding_model: The model used to extract features to compare the similarity

4. embedding_model替換至 Weaverbird 主程式的方法:
覆蓋weverbird後端 ML_models資料夾下的Res_model_emb_c_1.h5 model，若名稱有修改，需到主程式修改model讀檔名稱  
覆蓋完成後，定期跑everbird後端的similar fabric主程式即更新完成  (主程式內容請參照該程式內註解)

