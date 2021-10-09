# Suhas Siriprole Instagram Backend API Appointy

**Name:** Suhas Siriprole <br/>
**Reg.No:** 18BEC0327 <br/>
**Email:** suhas.siriprole@gmail.com <br/>
**Github-ID:** github.com/notsuhas <br/>

```
Firstly, Thank you Appointy for this Task. It has been a great learning experience for me.
As a FrontEnd Dev, This was something out of my comfort zone.
I never knew I could develop a basic API in a matter of 30 hours.
Although I haven't achieved all the constraints, I did my best learning GoLang and MongoDB
in the time I had. I had a great time working on this project. Once again, Thank you!

Also Please do read complete README.md. Comments have been written where-ever
necessary in the main.go file for clarity.

```

## Task Checklist

- Create an User

  - Should be a POST request ✅
  - Use JSON request body ✅
  - URL should be ‘/users' ✅

- Get a user using id

  - Should be a GET request ✅
  - Id should be in the url parameter ✅
  - URL should be ‘/users/< id here >’ ✅

- Create a Post

  - Should be a POST request ✅
  - Use JSON request body ✅
  - URL should be ‘/posts' ✅

- Get a post using id

  - Should be a GET request ✅
  - Id should be in the url parameter ✅
  - URL should be ‘/posts/< id here >’ ✅

- List all posts of a user
  - Should be a GET request ✅
  - URL should be ‘/posts/users/< Id here >' ✅
- Passwords should be securely stored such they can't be reverse engineered ✅
- Add pagination to the list endpoint ✅
- Make the server thread safe ❌
- Add unit tests ❌
- Quality of Code
  - Reusability `"Sadly, I failed trying to do this. But I did my best making Pure Functions."`
  - Consistency in naming variables, methods, functions, types `"I hope so, my OCD wouldn't forgive me."`
  - Idiomatic i.e. in Go’s style `"I guess?"`

## Libraries Used

No 3rd Party Libraries Used.

    "context"
    "crypto/md5"
    "encoding/hex"
    "encoding/json"
    "fmt"
    "log"
    "math"
    "net/http"
    "net/mail"
    "strconv"
    "strings"
    "time"

    "go.mongodb.org/mongo-driver/bson"
    "go.mongodb.org/mongo-driver/bson/primitive"
    "go.mongodb.org/mongo-driver/mongo"
    "go.mongodb.org/mongo-driver/mongo/options"

## User Attributes

    ID
    Name
    Email
    Password

## Posts Attributes

    ID
    UserID      // Required for Each Post, So they can be Indexed
    Caption
    Image URL
    PostedTimestamp

# API Guide

    mongoDB Port: localhost:27017
    HTTP Port: localhost:9000

Start MongoDB Server on Port : 27017 <br/>
`cd` into the Project Folder and do `go run main.go` in the Terminal <br/>
The Console/Terminal should print: <br/>

```
Started the Application...!
Connected to MongoDB!
```

If that doesn't happen, Please recheck if MongoDB is running. <br/>
Finally use Postman make GET or POST requests using the Port : 9000. <br/>

`The Local DB Name is "Suhas_Siriprole_InstagramDB"`

**Just a Word of Caution:** _There is an unexpected behaviour where doing GET or POST the first time, <br/> responds blank, If that happens, Please perform action again until you get response. <br/> I found no solution, or I might have did it wrong. But the Endpoints should work._

### 1. Creating a User (POST)

CreateUserEndpoint: `http://localhost:9000/users`

Request Body [**All Required**]:

```
{
  "Name": "Theodoric Elbourn",
  "Email": "telbourn0@google.es",
  "Password": "0u8KyH"
}
```

Output to Console:

```
ObjectID("6161be7062c19cb04123a938")
```

Response Body:

```
{
    "InsertedID": "6161be7062c19cb04123a938"
    // This ID is used to Find User and also Link User to Post!
}
```

### 2. Get User using ID (GET)

CreateUserEndpoint: `http://localhost:9000/users/6161be7062c19cb04123a938`

Request Body [**None**]:

```

```

Response Body:

```
{
    "_id": "6161be7062c19cb04123a938",
    "name": "Theodoric Elbourn",
    "email": "telbourn0@google.es",
    "password": "4f97198d296de276f3bbed9f5bc91a59"  // Hashed Password
}
```

### 3. Create a Post (POST)

CreatePostEndpoint: `http://localhost:9000/posts`

**Quick Note**: _You might want to spam the POST by changing something either in the Caption <br/> or ImageURL, This might be helpful for checking the API Pagination for a Given UserID!_

Request Body [**All Required**]:

```
{
  "UserID": "6161be7062c19cb04123a938",     // This is the UserID of the Person Posting!
  "Caption": "Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Donec pharetra, magna vestibulum aliquet ultrices, erat tortor sollicitudin mi, sit amet lobortis sapien sapien non mi. Integer ac neque.",
  "ImageURL": "http://dummyimage.com/162x100.png/ff4444/ffffff"
}
```

Output to Console:

```
ObjectID("6161c09962c19cb04123a939")sad
```

Response Body:

```
{
    "InsertedID": "6161c09962c19cb04123a939"
    // This ID is used to Find the Post, it is linked to the given UserID!
}
```

### 4. Get a Post using ID (GET)

CreatePostEndpoint: `http://localhost:9000/posts/6161c09962c19cb04123a939`

Request Body [**None**]:

```

```

Response Body:

```
{
    "_id": "6161c09962c19cb04123a939",
    "userid": "6161be7062c19cb04123a938",
    "caption": "Vestibulum ante ipsum primis in faucibus orci luctus et ultrices posuere cubilia Curae; Donec pharetra, magna vestibulum aliquet ultrices, erat tortor sollicitudin mi, sit amet lobortis sapien sapien non mi. Integer ac neque.",
    "imageurl": "http://dummyimage.com/162x100.png/ff4444/ffffff",
    "postedtimestamp": "Sat Oct  9"
}
```

### 5. Get all Posts of User using ID (GET)

CreatePostEndpoint: `http://localhost:9000/posts/users/6161be7062c19cb04123a938?page=2`

    perPage = 2         // Default Small Value for Demonstration Purpose

Query Params

| Key  | Value |             Notes             |
| ---- | :---: | :---------------------------: |
| page | {Int} | If Not Provided, Default is 1 |

If there Exists no Posts for the given UserID the response will just return
`"page": 1` as Response Body

Request Body [**None**]:

```

```

Response Body:

```
{
    "posts": [
        {
            "_id": "6161c46962c19cb04123a93b",
            "userid": "6161be7062c19cb04123a938",
            "caption": "2",
            "imageurl": "http://dummyimage.com/162x100.png/ff4444/ffffff",
            "postedtimestamp": "Sat Oct  9"
        },
        {
            "_id": "6161c46d62c19cb04123a93c",
            "userid": "6161be7062c19cb04123a938",
            "caption": "3",
            "imageurl": "http://dummyimage.com/162x100.png/ff4444/ffffff",
            "postedtimestamp": "Sat Oct  9"
        }
    ],
    "total": 8,
    "page": 2,
    "lastpage": 4
}
```

## Images

"This is 1_NewUser." <br/>
![This is 1_NewUser.](/images/1_NewUser.png "This is 1_NewUser.")<br/>

"This is 1_Duplicate_Email." <br/>
![This is 1_Duplicate_Email.](/images/1_Duplicate_Email.png "This is 1_Duplicate_Email.")<br/>

"This is 2_GetUser." <br/>
![This is 2_GetUser.](/images/2_GetUser.png "This is 2_GetUser.")<br/>

"This is 3_NewPost." <br/>
![This is 3_NewPost.](/images/3_NewPost.png "This is 3_NewPost.")<br/>

"This is 4_GetPostByID." <br/>
![This is 4_GetPostByID.](/images/4_GetPostByID.png "This is 4_GetPostByID.")<br/>

"This is 5_API_PaginationWithoutParam." <br/>
![This is 5_API_PaginationWithoutParam.](/images/5_API_PaginationWithoutParam.png "This is 5_API_PaginationWithoutParam.")<br/>

"This is 5_API_PaginationWithParam." <br/>
![This is 5_API_PaginationWithParam.](/images/5_API_PaginationWithParam.png "This is 5_API_PaginationWithParam.")<br/>

"This is MongoDB_Users_Collection." <br/>
![This is MongoDB_Users_Collection.](/images/MongoDB_Users_Collection.png "This is MongoDB_Users_Collection.")<br/>

"This is MongoDB_Posts_Collection." <br/>
![This is MongoDB_Posts_Collection.](/images/MongoDB_Posts_Collection.png "This is MongoDB_Posts_Collection.")<br/>

## The End
