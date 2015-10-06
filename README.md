# subscribe for a new user.
This API describe how to subcribe for the new user.

### 1. POST {hostname}/OcrREST/user/signup

####    
     <form method="POST" enctype="multipart/form-data"
 		action="{hostname}/OcrREST/user/signup">
		File to upload: <input type="file" name="fileUpload"><br /> 
		Email: <input type="text" name="email"><br /> 
		Login: <input type="text" name="login"><br />
		Phone: <input type="text" name="phone"><br /> 
		Address: <input type="text" name="address"><br /> 
		Client: <input type="text" name="clientName"><br /> 
		<input type="submit" value="Upload"> Press here to upload the file!
	</form>
###Parameters : form-data
```
--email         Required
--fileUpload    Required
--login         Optional
--phone         Optional
--address       Optional
--clientName    Optional
```
##### Status Code 200

    HTTP/1.1 200 OK
    Content-Type: application/json; charset=UTF-8
Response body : 

    {
        "success"         :  true,
        "message"      :  "An email with a activation link has been sent to the provided address.",
        "errorsDetail" : []
    }

##### Status Code 411
    HTTP/1.1 411 Length Required
    Content-Type: application/json; charset=UTF-8
Response body : 

    {
        "success" : false,
        "message": "KO",
        "errorsDetail" : [
              "email" : "the email is required",
              "fileUpload" : "the file upload is required",
        ]
    }
Notes : returns when email or file upload is missing.

##### Status Code 403
    HTTP/1.1 403 Forbidden
    Content-Type: application/json; charset=UTF-8
    
Response body : 
    {
        "success" : false,
        "message": "KO",
        "errorsDetail" : [
              "email" : "the email is not valid.",
              "fileUpload" : "the upload file is not in the format pdf"
        ]
    }
Notes : return when an email is not valide or a file upload is not in format pdf.
##### Status Code 409
    HTTP/1.1 410 Forbidden
    Content-Type: application/json; charset=UTF-8
Response body : 
    
    {
        "success" : false,
        "message": "KO",
        "errorsDetail" :[
         "email" :     "the email joe@yahoo.com existed already in the database.",
         "invariant" : "the invariant xyz extracted from the invoice existed already in the database"
        ]
    }
Notes : return when an email is used or an invariant existed in database.