# General Assembly Project 3

### Table of Contents
* [Description](#description)
    - [Deployment Link](#deployment-link)
    - [Code Installation](#code-installation)
    - [Timeframe and Working Team](#timeframe-and-working-team)
    - [Technologies Used](#technologies-used)
    - [Brief](#brief)
* [Planning](#planning)
    - [Styling](#styling)
    - [Finding a dataset](#finding-a-dataset)
* [Build Process](#build-process)
    - [Seeding the database](#seeding-the-database)
    - [Handling the data](#handling-the-data)
    - [Setting up the frontend](#setting-up-the-frontend)
    - [The browse page](#the-browse-page)
    - [Styling](#styling)
    - [Stretch Goals](#stretch-goals)
* [Challenges](#challenges)
* [Wins](#wins)
* [Key Learnings](#key-learnings)
* [Bugs](#bugs)
* [Future Improvements](#future-improvements)

# Description
I completed this project in week 9 of the course. It was a full stack application with an express back end and react front end. It is a movie review website. 

### Deployment Link 
https://stickypopcorn1.netlify.app/

### Code Installation




#### For the frontend:
https://github.com/Claire-bot87/sticky-popcorn-client

#### For the backend:
https://github.com/Claire-bot87/sticky-popcorn-api

### Timeframe and Working Team
We had 1 week to complete this project. I worked in a group of three with my classmates Kamran and Mai. 


### Technologies Used
GitHub
Express
Node
React
MongoDB Compass
MongoDB Atlas
Bootstrap
Netlify


### Brief
Build a REST API using MongoDB, Express, Node
Build a front-end application using React, that uses fetch or axios to call the API
Authentication/Authorization should be implemented on both front-end and back-end
Beyond the user model you should have at least 2 more data entities, like posts and replies.
CRUD should be implemented on the front-end and back-end, that is to say that I should as a user be able to successfully make a GET, POST, PUT & DELETE request from the react app.
The project is deployed (guides will be provided)


## Planning
https://trello.com/b/qLbGLI4H/sticky-popcorn-app
![Wireframe 1](https://res.cloudinary.com/dpv0j8frj/image/upload/v1743495653/wireframe_z2hmqa.png)

## Build Process
As this was a group project, I focused on authentication, both on the front end and back end. Here i’m going to focus on the Signup process.

I had a model called User

Then in my user Controller:

```.js
router.post('/signup', async (req, res, next) => {
    try {
        const user = await User.create(req.body)
        const token = generateToken(user)
        return res.status(201).json({
            message: 'User created successfully',
            token
        })
    } catch (error) {
        next(error)
    }
})
```

So it uses Model.create, a method provided by Mongoose, which takes the request as an argument. And it creates a document in our MongoDB collection(table). It also asigns that object to a variable , ‘user’.


It then passes that ‘user’ as an argument to my generateToken function

The generateToken returns a token which contains within it the username and user id as well as a token secret.

It uses a package called json web token  to generate this token and return it, where it is saved to the variable ‘token’

This is then returned to the client, alongside a message, as the res.data. So there are two keys on the data key : Token and message. 
This is saved to a variable called data within the handlesubmit function, in the client’s signup component.


It then returns a message and the token. 

 #### From the front end

I built out a Signup component in my React app.

It used useState hook to set the state for the form data.

The component returns a form .

As a user types intot the form the form data updates

```.js
 const handleChange = (e) => {
    console.dir(e.target)
    setErrors({ ...errors, [e.target.name]: '' })
    setFormData({ ...formData, [e.target.name]: e.target.value })
  }
```

When the form is submitted
The formData is passed to the signup service, which I wrote in a separate services file and folder.

The service takes the formData as an argument and sends an Axios post request to the server


The signup service function then returns res.data to the handlesubmit function where it was initially called.
That res.data is assigned to a variable called ‘data’

That is then used to set the token. It does this by passing the token key in the data object into the setToken function as an argument. It saves this token to local storage. 

```.js
export const setToken = (token) => {
  localStorage.setItem(tokenName, token)
}
```

Then we use getUserFromToken to get the user form the token 
As we know the username and id are within the token, 
Then we use what is returned from getUserFromToken to setUser, and in doing tso, we set the userContext for the whole application.

This is handled in the userContext.js file:
```.js
const UserContext = createContext(null)

// The "Provider" is wrapped around any component that should have access to the above context
function UserProvider({ children }){
  // The user state works as any other state and will form the value of our user context
  const [user, setUser] = useState(getUserFromToken())

  // Below, we provide a value key to the context provider, which gives any component access to the user and setUser values
  return (
    <UserContext.Provider value={{ user, setUser }}>
      { children }
    </UserContext.Provider>
  )
}
```



### Stretch Goals


## Challenges


## Wins


## Key Learnings


## Bugs


## Future Improvements

