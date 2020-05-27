# MNIST-Model-Deploy :   [https://mnistwebapp.herokuapp.com/](https://mnistwebapp.herokuapp.com/)


#### On Google Colab Ipynb file

[Original file is located at Google Colab](https://colab.research.google.com/github/Vatsalparsaniya/Mnist-Model-Deploy/blob/master/__init__.ipynb) <a href="https://colab.research.google.com/github/Vatsalparsaniya/Mnist-Model-Deploy/blob/master/__init__.ipynb" target="_parent"><img src="https://colab.research.google.com/assets/colab-badge.svg" alt="Open In Colab"/></a>

[Google Drive](https://drive.google.com/open?id=1Tjjsbf2RwQZrSog8ztiz76Wos0ewFRHN)

## WebApp using Flask with Google Colab of MNIST model Deploy 

#### Handle Post request using Flask

    @app.route('/mnistprediction/', methods=['GET', 'POST'])
        def mnist_prediction():
            if request.method == "POST":
                if not request.files['file'].filename:
                    flash("No File Found")
                else:
                    f =  request.files['file']
                    f.save("uploads/"+f.filename)
                    tf_image = image.load_img("uploads/"+f.filename, 
                                    grayscale=True, 
                                    color_mode='rgb', 
                                    target_size=(28,28),
                                    interpolation='nearest'
                                    )
                    np_image = image.img_to_array(tf_image)
                    pred_img = np.reshape(np_image,(1,28,28,1))/255.0
                    pred_img = 1 - pred_img
                    predictions = mnist_model.predict(pred_img)
                    number = int(np.argmax(predictions))
                    return str(number)
                
#### Prediction Button AJAX request

      $(document).ready(function(){
          $('#predbtn').click(function () {
              var form_data = new FormData($('#upload-file-model')[0]);
              $.ajax({
                  type: 'POST',   
                  url: '/mnistprediction/',
                  data: form_data,
                  contentType: false,
                  cache: false,
                  processData: false,
                  async: true,
                  success: function (data){
                      $('#resultModel').text(' Predicted Number :  ' + data);
                      console.log(fileName)
                      $('#image_div1').attr('src','/get-mnist-image/'+fileName)
                  },
              });
          });    
      });
      
 
![MNIST Webapp](https://github.com/Vatsalparsaniya/Mnist-Model-Deploy/blob/master/static/image/mnist.PNG)


![MNIST Webapp](https://github.com/Vatsalparsaniya/Mnist-Model-Deploy/blob/master/static/image/mnist1.PNG)
