# General Assembly Project 3

### Table of Contents
* [Description](#description)
    - [Deployment Link](#deployment-link)
    - [Technologies Used](#technologies-used)
    - [Brief](#brief)
* [Planning](#planning)
* [Build Process](#build-process)
* [Challenges](#challenges)
* [Wins](#wins)
* [Key Learnings](#key-learnings)
* [Bugs](#bugs)
* [Future Improvements](#future-improvements)

# Description
I completed this project in week 9 of the course. It was a Full Stack application with an Express backend and ReactJS frontend. It is a movie review website. This was a team project. I worked with classmates Raj and Mai. We shared responsibilities for planning, development and deployment.

### Collaborators
[Kamran Raja](https://github.com/Kam-Gemini)\
[Mai-Yee Crossley](https://github.com/maiyeecrossley)

### Deployment Link 
https://stickypopcorn1.netlify.app/

### Technologies Used
- GitHub
- Express
- Node
- ReactJS
- MongoDB Compass
- MongoDB Atlas
- Bootstrap
- Netlify


### Brief
- Build a REST API using MongoDB, Express and Node.
- Build a frontend application using ReactJS, that uses fetch or axios to call the API.
- Authentication/Authorization should be implemented on both frontend and backend
- Beyond the user model you should have at least 2 more data entities.
- CRUD should be implemented on the frontend and backend.



## Planning
[Trello Board](https://trello.com/b/qLbGLI4H/sticky-popcorn-app)\
[Entity Relationship Diagram](https://dbdiagram.io/d/StickyPopcorn-67a5d3a3263d6cf9a05e3be6)\
[Router Table](https://docs.google.com/spreadsheets/d/1dBa-vcvxqSvyo68SDoe2hoy5MUcR_3tIvn12t-FFFl8/edit?usp=sharing)\
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

The generateToken returns a token which contains within it the username and user ID as well as a token secret.

It uses a package called JSON Web Token  to generate this token and return it, where it is saved to the variable ‘token’.

This is then returned to the client, alongside a message, as the res.data. So there are two keys on the data key : Token and message. 
This is saved to a variable called data within the handleSubmit function, in the client’s Signup component.


It then returns a message and the token. 

 #### From the frontend

I built out a Signup component in our React app.

It used the useState hook to set the state for the form data.

The component returns a form .

As a user types into the form, the form data updates:

```.js
 const handleChange = (e) => {
    console.dir(e.target)
    setErrors({ ...errors, [e.target.name]: '' })
    setFormData({ ...formData, [e.target.name]: e.target.value })
  }
```

When the form is submitted, the formData is passed to the Signup service, which I wrote in a separate services file and folder.

The service takes the formData as an argument and sends an Axios post request to the server


The signup service function then returns res.data to the handlesubmit function where it was initially called.
That res.data is assigned to a variable called ‘data’.

That is then used to set the token. It does this by passing the token key in the data object into the setToken function as an argument. It saves this token to local storage. 

```.js
export const setToken = (token) => {
  localStorage.setItem(tokenName, token)
}
```

Then we use getUserFromToken to get the user from the token, as we know the username and ID are within the token.
Then we use what is returned from getUserFromToken to setUser, and in doing so, we set the userContext for the whole application.

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


## Challenges
The key challenge in this project was working as a team on a single code base, maintaining clear lines of communication and remembering to git pull the latest code.



## Wins
- Great team communication and collaboration.
- Additional features like 'Watchlist' and 'Favourites'.

## Key Learnings

This was my first React App so I was able to better understand state management and hooks like useState and useEffect.

I was also able to better understand authorisation and authentication. It was good to be able to complete the whole process, on the backend and frontend. I had previously had some difficulties understand authentication tokens. So this gave me an opportunity to deepen my understanding.

## Bugs
I need to add in error messages to the sign-up and sign-in pages.

## Future Improvements
At the moment we have a star-rating button on the SingleMovie page. But the ratings are not currently saved and we do not get have a 'top-rated movies' filter. This is an improvement I would like to make.
