# subscribe_user
api subscribe user
##  Users


See [conventions](../conventions.md) for some common conventions of writing Balloon REST API specs.

### 1. POST /users

#### Request 

    POST /users
    
    {
        "username"     : "joe",
        "email"        : "joe@example.com",
    }

#### Response

##### Status Code 201

    HTTP/1.1 201 Created
    Content-Location: https://www.balloon.com/api/v1/users/joe
    Content-Type: application/json; charset=UTF-8

    {See Response Body for Section 4}

##### Status Code 411

    HTTP/1.1 411 Length Required
    Content-Location: https://www.balloon.com/api/v1/users
    Content-Type: application/json; charset=UTF-8
    
    {
        "message" : "Required parameter is missing",
        "require" : ["email"]
    }

Some required field is missing, or null.
For this method, both `username` and `email` are required.
The response body contains a lists of all missing fields: `require`.


##### Status Code 403

    HTTP/1.1 403 Forbidden
    
`username` is not valid. A valid `username`:

* must not be null
* cannot be longer than 128 characters
* can only contain: 0-9 a-z A-Z '.' '-' '_'


##### Status Code 409

    HTTP/1.1 409 Conflict

The provided `username` has been used by someone else.

##### Status Code 406

    HTTP/1.1 406 Not Acceptable

The provided `email` is not valid.

##### Status Code 410

    HTTP/1.1 410 Gone

The provided `email` has been used by another user.

##### Status Code 422

    HTTP/1.1 422 Unprocessable Entity
    
Failed to send verification email to the provided email address.

### 2. PUT /users/{username}/password

#### Request

    PUT /users/joe/password
    
    {
        "old_password" : "p@ssC0de",
        "new_password" : "N@wPassC0de"
    }

#### Response

##### Status Code 204

    HTTP/1.1 204 No Content

Password has been changed successfully.

##### Status Code 404

    HTTP/1.1 404 Not Found

No user bound to the `username` provided.

##### Status Code 406

    HTTP/1.1 406 Not Acceptable

The old password does not match.

##### Status Code 409

    HTTP/1.1 409 Conflict

The new password is not valid. A valid password must match all of the following criteria:

* Must be at least 8 characters in length
* Must contain at least on uppercase character
* Must contain at least one number

### 3. POST /users/{username}/password_reset

#### Request

    POST /users/joe/password_reset
    
    {
    	"email" : "joe@example.com"
    }

#### Response

##### Status Code 204

    HTTP/1.1 204 No Content

An email with a link to reset password has been sent to the provided address.

##### Status Code 404

    HTTP/1.1 404 Not Found

No user bound to the `username` provided.

##### Status Code 409

    HTTP/1.1 409 Conflict

The provided email address does not match `username`

### 4. GET /users/{username}

Retrieve the record data for the specified user.

#### Request

    GET /users/joe


#### Response

##### Status Code 200

    HTTP/1.1 200 OK
    Content-Location: https://www.balloon.com/api/v1/users/joe
    Content-Type: application/json; charset=UTF-8

    {
        "_links"       : {
            "self"       : {"href": "/users/joe"},
        }
        "created"      : "2012-02-29T16:57:20-08:00",
        "updated"      : "2012-02-29T16:57:20-08:00",
        "email"        : {"joe@example.com"},
        "username"     : "joe",
    }


##### Status Code 404

    HTTP/1.1 404 Not Found

No user bound to the `username` provided.

### 5. PUT /users/{username}

#### Request

    PUT /users/joe

    {
        "email"        : "joe@example.com",
    }


##### Optional elements:
Any element can be optional. The purpose of this function is to update a specific user's info. Each info will be updated by the new value if provided.

#### Response

##### Status Code 200

    HTTP/1.1 200 OK
    Content-Location: https://www.balloon.com/api/v1/users/joe
    Content-Type: application/json; charset=UTF-8

    {See Response Body for Section 4}

##### Status Code 404

    HTTP/1.1 404 Not Found

No user bound to the `username` provided.

##### Status Code 406, 410, 422

See Section 1.

### 6. DELETE /users/{username}

#### Request

    DELETE /users/joe

#### Response

##### Status Code 204

    HTTP/1.1 204 No Content

Successfully deleted the user of provided `username`.

##### Status Code 404

    HTTP/1.1 404 Not Found

No user bound to the `username` provided.