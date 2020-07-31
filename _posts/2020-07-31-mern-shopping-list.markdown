---
layout: post
title:      "MERN-shopping-list"
date:       2020-07-31 13:48:20 +0000
permalink:  mern-shopping-list
---


![](https://repository-images.githubusercontent.com/276986179/c4e42180-d2ff-11ea-89d6-5e5ee42b720c)

## Finally the Long awaited release of my MERN-stack application MERN-Shopping-list

This blog post is going to run through a list of the features as well as get a hair technical about the source code. I hope this helps some other developers solve some problems they may be having in their code or get some ideas of what to include in their projects.

## Overview
I have been writing in my past few blog posts about a MERN stack, the components individually, together, and deployment. This is the project I have been working on while learning the topics for those posts. Incase the name is not direct enough the MERN-shopping-list is a project built fully on the mern stack. It uses a MongoDB database, Node.js as a JavaScript runtime, Express.js as a backend frame-work, and React.js as a front-end framework. I will go into more detail about each of these later but I do incorperate other technology such as JWT (Json-Web-Tokens), Bcrypt, Bootstrap / React-strap, Mongoose, Redux / React-Redux (including Thunk), Axios, and a few unlisted technologies.

## Database
Per the usual with MERN stack I used MongoDB for my DataBase. This is one of the main reasons I decided to use the MERN-stack. I love the Simplicity of the Mongoose ORM. Additionally I find the data structure of a JSON object very easy to understand and much more flexible. In this project I incorperated an embedded document. This Means rather then two seperate JSON objects with Users and Items, Items is an array inside of Users. This gives a few benifits. First of all it better represents the relationship of Items belonging to users. Second paired with authentication and some middleware, this creates another layer of encapsulation. A User's access is limited to modifying their own Items how I specify in my routes. Creating my Mongoose Schema is very simple and looks like this...

```
const ItemSchema = new Schema({
    name: {
        type: String,
        required: true
    },
    department: {
        type: String
    },
    purchased: {
        type: Boolean,
        default: false
    },
    date: {
        type: Date,
        default: Date.now
    }
})

const UserSchema = new Schema({
    name: {
        type: String,
        required: true
    },
    email: {
        type: String,
        required: true, 
        unique: true
    },
    password: {
        type: String, 
        required: true, 
    },
    register_date: {
        type: Date, 
        default: Date.now
    },
    items: [ ItemSchema ]
})
```

A few things I would like to point out about this file.

1) Note how the ItemSchema is included as an array with the key of items inside the User schema. This is one way to represent the belongs to relationship. 
2) This Schema is pretty simple so I included it all in one file. I chose this because the file was not overcomlicated and still easy enough to read. However, I could have created a seperate Items model file and imported it that way.
3) Mongoose Schemas can be written a few different ways and in my case I chose to pass an object to each of the keys. This allows me to add a few basic validations to the fields. 

MongoDB comes with some amazing tools too. There is free courses teaching their product on their website. Mongo has a very powerful shell that can be used to interact with databases. There is MongoDB compase a visual tool that encorperates database management capabilities. The tool I primarily use is MongoDB Atlas. Atlas is a online visual tool where users can view and manage a live database. Atlas can also create new Clusters and Databases.

## Back-end
MERN stack is built useing only the JavaScript language. This poses a problem. JavaScript is read by the browser so how could JavaScript be used to create a back-end or server. The answer (in my case and MERN's case) is Node.js. This is a runtime that lets a local device (or deployment service's machine) read and run JavaScript code. I based my backend around a modified MVC structure to create an api. The views would be the front-end. The File listed above where the Schema's were created are considered the models. I have a file called server.js where I use the express framework to create and manage the server. This can be looked at as my one access point from the front-end. I then use middleware to create routes and direct the server requests to the correct route. This Middleware and Routes together are the controller part of MVC. I used seperate routes for the User routes and the Auth routes. Auth handles logging in and verifying users. Users is where registration is handled as well as many other functions. I stuck to RESTful conventions in my routing names. I used nested routes as another form of encapsulation. These nested route ex. `/:user_id/items/:item_id` represents the embedded document and relationship used in the database. I implemented full CRUD actions in this RESTful convention.

#### Create
I used registration to create users and a nested route to create items. 

```
// @route POST api/users/:id/items
// @desc create a new item
// access private
router.post('/:id/items', auth, (req, res) => {
    if(req.user.id != req.params.id) {
        res.status(400).json({ msg: "Not Authorized"})
    }else{
    try{
        const newItem = { name: req.body.name, department: req.body.department || null}
        User.findOne({ _id: req.params.id })
        .then(user => {
        const i = user.items.push(newItem)
        user.save()
        res.json(user.items[i - 1])
        })
    }catch(e){res.status(400).json({ msg: "Failed to add item" })}
}})
```

#### Read
I have one route to act as a Users show page which is really a collection of items or Items index.

```
// @route GET api/users/:id/items
// @desc gets all items (items index)
// access private
router.get('/:id/items', auth, (req, res) => {
    if(req.user.id != req.params.id) {
        res.status(400).json({ msg: "Not Authorized"})
    }else{
    try{
        User.findOne({ _id: req.params.id }) 
        .then( user => user.items)
        .then( items => res.json(items))
    }catch(e) {
        res.status(400).json({ msg: "could not retrive items"})
    }
}})
```

#### Update
I have a nested route to update a Users Items. I use patch as a convention because Im not replacing the whole user here but updating one peice.

```
// @route PATCH api/users/:user_id/items/:item_id
// @desc create a new item
// access private
router.patch('/:user_id/items/:item_id', auth, (req, res) => {
    if(req.user.id != req.params.user_id) {
        res.status(400).json({ msg: "Not Authorized"})
    }else{
    try{
        User.findOne({ _id: req.params.user_id })
        .then(user => {
        const item = user.items.id(req.params.item_id)
        item.purchased = req.body.purchased
        user.save()
        res.json(item)
        })
    }catch(e){res.status(400).json({ msg: "Failed to add item" })}
}})

```

#### Delete
Much like the update route I have a nested delete route to delete a Users Items.

```
// @route Delete api/users/:user_id/items/:item_id
// @desc delete an item
// access private
router.delete('/:user_id/items/:item_id', auth, (req, res) => {
    if(req.user.id != req.params.user_id) {
        res.status(400).json({ msg: "Not Authorized"})
    }else{
        User.findOne({ _id: req.params.user_id })
        .then(user => {
        user.items.id(req.params.item_id).remove()
        user.save()
        res.json({ success: true })
        }).catch(e => res.status(404).json({ success: false })
    )}
})
```

I use JWT in my backend to create and sign tokens. I use Bcrypt to salt and hash passwords in a secure way. 

## Front-end
MERN-stack front end is built with React and in my case I used React, Redux, Thunk, and Bootstrap. I store my tokens and auth info in LocalStorage. This mimics a stateful operation in the stateless browser. This local storage can be cleared and modified and accesses in various ways such as 

```
localStorage.setItem('token', action.payload.token)
```

I created a few React components such as a NavBar and Modals to display info and views when needed. I also used containers to hold and render (ocasionally map through the state) other components that display info. 

```
class ContentContainer extends React.Component {
    render(){
        return (
            <>
            { this.props.user ? 
                <>
                <ItemModal />
                <ShoppingList />
                </> : <>
                <h2 className='text-center'>Welcome to shopping list</h2>
                <p className='text-center'>Log in or Register to make shopping lists easier!</p>
                </>
            }
            </>
        )
    }
}

const mapStateToProps = (state) => ({
    user: state.auth.user
})

export default connect(mapStateToProps, null)(ContentContainer)
```

This content container adjusts what is rendered based on is a user is logged in. The main responsibility of this container is to not load anything unless logged in. 

*NavBar*

```
class AppNavbar extends Component {
    state = {
        isOpen: false
    }
    toggle = () => {
        this.setState({
            isOpen: !this.state.isOpen
        })
    }
    render() {
        const { isAuthenticated, user } = this.props.auth
        const authLinks = (
            <>
                <NavItem>
                    <span className="navbar-text mr-3">
                        <strong>{user ? `Welcome ${user.name}` : ""}</strong>
                    </span>
                </NavItem>
                <NavItem>
                    <Logout />
                </NavItem>
            </>
        )
        const guestLinks = (
            <>
                <NavItem>
                    <RegisterModal />
                </NavItem>
                <NavItem>
                    <LoginModal />
                </NavItem>
            </>
        )
        return <div>
            <Navbar color="dark" dark expand="sm" className="mb-5">
                <Container>
                    <NavbarBrand href="/">Shopping List</NavbarBrand>
                    <NavbarToggler onClick={this.toggle} />
                    <Collapse isOpen={this.state.isOpen} navbar>
                        <Nav className="ml-auto" navbar>
                            { isAuthenticated ? authLinks : guestLinks }
                        </Nav>
                    </Collapse>
                </Container>
            </Navbar>
        </div>
    }
}

const mapStateToProps = state => ({
    auth: state.auth
})

export default connect(mapStateToProps, null)(AppNavbar)
```

This Navbar includes ReactStrap / bootsrap components to give a bit of styling. This NavBar also features a toggler for smaller screans this is done with ReactStrap and renders different content depending on if a user is logged in.

#### Modals
The add items feature as well as login and register are Reactstrap Modals. This creates a screen friendly user experiance. 

```
class ItemModal extends Component {
    state = {
        modal: false, 
        name: '',
        department: ''
    }

    toggle = () => {
        this.setState({ 
            modal: !this.state.modal
        })
    }

    onChange = (e) => {
        this.setState({[e.target.name]: e.target.value })
    }

    onSubmit = (e) => {
        e.preventDefault()
        const newItem = {
            name: this.state.name,
            department: this.state.department
        }

        this.props.addItem(newItem, this.props.user)
        this.setState({
            ...this.state,
            name: '',
            department: '' 
        })
        this.toggle()
    }

    render() {
        return( 
        <div>

            { this.props.isAuthenticated ? <Button
                color="dark"
                style={{marginBottom: '2rem'}}
                 onClick={this.toggle}>Add Item
            </Button> : <h4 className="mb-3 ml-4">Please Log in to manage items</h4> }
  
            <Modal 
                isOpen={this.state.modal}
                toggle={this.toggle}>
                <ModalHeader toggle={this.toggle}>Add To Shopping List</ModalHeader>
                <ModalBody>
                    <Form onSubmit={this.onSubmit}>
                        <FormGroup>
                            <Label for="item">Item</Label>
                            <Input 
                                type="text"
                                name="name"
                                id="item"
                                placeholder="Item Name"
                                onChange={this.onChange}
                             />
                            <Label for="department">Department (Optional)</Label>
                            <Input 
                                type="text"
                                name="department"
                                id="department"
                                placeholder="Department Name (Optional)"
                                onChange={this.onChange}
                             />
                             <Button
                                color="dark"
                                style={{marginTop: '2rem'}}
                             >Add Item</Button>
                        </FormGroup>
                    </Form>
                </ModalBody>
            </Modal>

        </div>
        )
    }
}

const mapStateToProps = (state) => ({
    item: state.item,
    isAuthenticated: state.auth.isAuthenticated,
    user: state.auth.user
})

export default connect(mapStateToProps, { addItem })(ItemModal)
```

my ShoppingList container component maps through the item state and groups these items by department. It then renders a DepartmentContainer conmponent.

```
class ShoppingList extends Component{

    componentDidMount(){
        this.props.getItems(this.props.user)
    }

    getDepartments = (items) => {
        const departments = {}
        for (const item of items){
            if (departments[item.department]){
                departments[item.department].push(item)
            }else{
                departments[item.department] = []
                departments[item.department].push(item)
            }
        }
        return departments
    }
    
    mapDepartments = (departments) => {
        return Object.keys(departments).map((k, i) => {
            return <DepartmentContainer key={k} name={k} department={departments[k]}/>
        })
    }

    render(){
        const departments = this.getDepartments(this.props.item.items)
        return (
            <>
                {this.mapDepartments(departments)}
            </>
        )
    }
}

const mapStateToProps = (state) => ({
    item: state.item,
    isAuthenticated: state.auth.isAuthenticated,
    user: state.auth.user
})



export default connect(mapStateToProps, { getItems, deleteItem })(ShoppingList)
```

The DepartmentContainer gets passes down these departments as props. Then maps through the collection of items in that department and displays them together. The Shopinglist and DepartmentContainer together display the items. and they work together to display them as sorted departments. This DepartmentContainer uses Reactstrap for styling the listgroup and giving them the functionality of buttons. This allows for the delete feature and the Purchased feature where when clicked I use some traditional css to control the text decoration and opacity. This creates a line through text that is faded to make it easy to see whats already in a cart. 

```
class DepartmentContainer extends Component{

    onDeleteClick = (id) => {
        this.props.deleteItem(id, this.props.user)
    }
    
    onPurchasedClick = (item) => {
        const updatedItem = {...item, purchased: !item.purchased}
        this.props.purchasedItem(updatedItem, this.props.user)
    }

    render(){
        return (
            <Container className='pb-4'>
                <ListGroup >
                    <ListGroupItem color="dark" >
                        {this.props.name === "null" ?  "No Department" : `${this.props.name}`}
                    </ListGroupItem>
                    
                    {this.props.department.map((item) => {
                        return (
                            <ButtonGroup key={item._id} >

                                <ListGroupItem tag="button" onClick={this.onPurchasedClick.bind(this, item)} action className={`${item.purchased ? "purchased" : ''}`}>
                                    {item.name}
                                </ListGroupItem>

                                <Button 
                                    className="remove-btn float-right "
                                    color="danger"
                                    size="sm"
                                    onClick={this.onDeleteClick.bind(this, item._id)}
                                >&times;</Button>

                            </ButtonGroup>
                        )
                    })} 
                </ListGroup>
            </Container>
        )
    }
}

const mapStateToProps = (state) => ({ user: state.auth.user })


export default connect(mapStateToProps, { deleteItem, purchasedItem })(DepartmentContainer)
```

The functions and state given by react are accessed by the components in the connect method from Redux. The functions from mapDispatchToProps are action creators useing Thunk for async requests to the back end. This is where the fetching happens. I used Axios to send my requests rather then the fetch() function justs for syntax. It looks better and is simpler. ActionType constants are used to create errors if there is a typo. 

```
export const GET_ITEMS = "GET_ITEMS"
export const ADD_ITEM = "ADD_ITEM"
export const DELETE_ITEM = "DELETE_ITEM"
export const CLEAR_ITEMS = "CLEAR_ITEMS"
export const ITEMS_LOADING = "ITEMS_LOADING"
export const USER_LOADING = "USER_LOADING"
export const USER_LOADED = "USER_LOADED"
export const AUTH_ERROR = "AUTH_ERROR"
export const LOGIN_SUCCESS = "LOGIN_SUCCESS"
export const LOGIN_FAIL = "LOGIN_FAIL"
export const LOGOUT_SUCCESS = "LOGOUT_SUCCESS"
export const REGISTER_SUCCESS = "REGISTER_SUCCESS"
export const REGISTER_FAIL = "REGISTER_FAIL"
export const GET_ERRORS = "GET_ERRORS"
export const CLEAR_ERRORS = "CLEAR_ERRORS"
export const UPDATE_ITEM = "UPDATE_ITEM"
```

```
export const getItems = (user) => (dispatch, getState) => {
    dispatch(setItemsLoading())
    axios.get(`/api/users/${user.id}/items`, tokenConfig(getState))
        .then( res => 
            dispatch({
                type: GET_ITEMS,
                payload: res.data
            })
        ).catch(err => dispatch(returnErrors(err.response.data, err.response.status)))
}
```

There is a tokenConfig() function that takes getstate and is defined in my auth actions file. This is responsible for generating the token info that the backend can use to interperate the authorization status and decode to determine the user. 

```
export const tokenConfig = getState => { 
        // get token from local storage
        const token = getState().auth.token


        // headers
        const config = {
            headers: {
                "content-type": "application/json"
            }
        }
    
        // if token then add to headers
        if(token) {
            config.headers['x-auth-token'] = token
        }

        return config
}
```

*in backend auth middleware*
```
function auth(req, res, next) {
    const token = req.header('x-auth-token')

    // check for token
    if(!token) return res.status(401).json({ msg: " No token, authorization denied"})
    try{
        // verify tokern
        // config.get('jwtSecret')
        const decoded = jwt.verify(token, process.env.jwtSecret)

        // add user from payload
        req.user = decoded
        next()
    } catch(e) {
        res.status(400).json({ msg: "Token is not valid"})
    }
} 
```

Finally I my store features a Reducer using the Redux built in combine reduce function. This is built from an itemsReducer, authReducer, and errorReducer. I use Pure functions in my reducers to make sure I dont modify the existing state. These reducers are switch's that are responsible for takeing the action objects from the action creators and handling the payload efficiently. They switch on the action.type and handle the action.payload. 

*item reducer*
```
  
  
const initialState = {
    items: [],
    loading: false
}

export default function(state = initialState, action) {
    switch(action.type) {
        case GET_ITEMS:
            return {
            ...state, 
            items: action.payload,
            loading: false
        }
        case UPDATE_ITEM:
            const actionIndex = state.items.findIndex(e => e._id === action.payload._id)
            return {
                ...state,
                loading: false,
                items: state.items.map((item, index) => {
                    if (index !== actionIndex){
                        return item
                    }else{
                        return {
                            ...item,
                            ...action.payload
                        }
                    }
                })
            }
        case DELETE_ITEM:
            return {
                ...state, 
                items: state.items.filter(item => item._id !== action.payload)
            }
        case ADD_ITEM:
            return {
                ...state, 
                items: [...state.items, action.payload,]
            }
        case ITEMS_LOADING: 
            return {
                ...state,
                loading: true
            }
        case CLEAR_ITEMS:
            return{
                ...state,
                items: []
            }
        default:
             return state
    }
}
```

## Conclusion
Together these components create a Heroku deployed MERN stack application called MERN-shopping-list. You can check out the app [https://quiet-caverns-42496.herokuapp.com/](https://quiet-caverns-42496.herokuapp.com/). My goal from this project was to learn new to me technologies like Express, Node, MongoDB, Mongoose, JWT, Bootstrap / React-Strap, Axios, LocalStorage, and A whole new stack. Learning new technologies is one of my favorite parts of Software Engineering and programing. I hope this inspires web developers, software engineers, problem solvers, and anyone to learn this stack or start that new project.


