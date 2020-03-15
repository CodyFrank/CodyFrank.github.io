---
layout: post
title:      "React Project D&D 5e character sheet"
date:       2020-03-15 19:36:08 +0000
permalink:  react_project_d_and_d_5e_character_sheet
---


This is it the Final Flatiron Project!

## Goal
This project challenges students to create a single page web application using JavaScript with the React framework. 
The list of requirements follows

- The code should be written in ES6 as much as possible
- Use the create-react-app generator to start your project.
      Follow the instructions on this repo to setup the generator: create-react-app
- Your app should have one HTML page to render your react-redux application
- There should be 2 container components
- There should be 5 stateless components
- There should be 3 routes
- The Application must make use of react-router and proper RESTful routing (should you choose to use react-router v3         please refer to the appropriate docs; docs for v4 can be found here)
- Use Redux middleware to respond to and modify state change
- Make use of async actions to send data to and receive data from a server
- Your Rails API should handle the data persistence. You should be using fetch() within your actions to GET and POST              data from your API - do not use jQuery methods.
- Your client-side application should handle the display of data with minimal data manipulation
- Your application should have some minimal styling: feel free to stick to a framework (like react-bootstrap), but if you -        want to write (additional) CSS yourself, go for it!


## My Project
For my project I made a character sheet for dungeons and dragons that allows users to update their characters stats without having to erase and re write each stat every time something changes.

###The code should be written in ES6 as much as possible
This requirment is looking to use a version of Javascrip that includes classes as well as const and let keywords.
to complete this I use const and let as often as possible and avoid using the var (older versions) keyword. This sounds like it makes coding harder but in reality const and let throw errors and make debugging much easier.

```
let newArray = []
```

```
const attacks = character.attacks
```

For the classes I made all of my react components classes that extend React.Component. 
ex. 
```
export default class Home extends React.Component{
    render(){
        return (
            <div>
                <h1>Dungeons and Dragons virtual character sheets</h1>
                <p>A place where you can keep track of your characters without having to erase and rewrite EVERYTHING!</p>
                <Link to={`/characters`}>Click Here to See Characters</Link>
            </div>
        )
    }
}
```

### Use the create-react-app generator to start your project.
This is self explanitory. I made a repository in github. after forking and cloning I rand the create react app command 
In my case 
```
npx create-react-app character-sheet-frontend
```

### Your app should have one HTML page to render your react-redux application
This is set up from the Create React App command listed above

### There should be 2 container components
In my project I made 2 Container components CharactersContainer and CharacterContainer. These components are tasked with handling the logic. I connected these containers to the redux store to have access to state
```
const mapStateToProps = ({ characters }) => ({
    characters
})

export default connect(mapStateToProps, { fetchCharacters })(CharactersContainer)
```
and I ask them to convert the data into a form the presentational components(more on this later) can use. Then I pass this data down via props the child or presentaional components.

```
<CharacterCard key={c.id} character={c} />
```

### There should be 5 stateless components
My project has more then 5 stateless components but these are what I used as my presentational components or components that are responsible for rendering the html with the given props from the container components / parent components.

The best example of this id the CharacterCard component. It is given the props of character from the CharactersContainer. CharacterCard takes this prop and displays the html.

```
export default class CharacterCard extends React.Component{


    render(){
        return(
                <div className={"characterCard"}>
                    <h1>{`${this.props.character.name}`}</h1>
                    <h3> Level: {`${this.props.character.level}`} Class: {`${this.props.character.character_class}`}</h3>
                </div>
        )
    }
}
```

### There should be 3 routes *and* The Application must make use of react-router and proper RESTful routing
In my project I used react-router-dom to create the RESTful routes / (home), /characters (characters index), and /characters/:id (character show page).
I have them rendered inside a switch so only one of them will display at a time. and I have various links for ease of navigation.

```
    <Router>
      <div className="App">

        <Switch>

          <Route exact path="/characters/:id">
            <CharacterContainer />
          </Route>
          

          <Route path="/characters">
            <CharactersContainer/>
          </Route>

          <Route path="/">
            <Home/>
          </Route>

        
        </Switch>
        
      </div>
    </Router>
```

note everything is wrapped in a Router (renamed from BrowserRouter) this allows page naviagtion. Then the Switch holds the Routes as explained above.

### Use Redux middleware to respond to and modify state change
Redux is used to hold the state of the application in one spot that can then be accesses by any components that need it. when setting redux up you need to create a store then wrap the whole app in a Provider. this looks like 

```
const store = createStore(rootReducer, composeWithDevTools(applyMiddleware(thunk)))

ReactDOM.render(<Provider store={store}><App /></Provider>, document.getElementById('root'));
```

Please note the composeWithDevTools(applyMiddleware(thunk) allows for useage of dev tools and thunk middleware. I will explain Thunk Later.
When creating a redux store it must have a reducer. In my case I have a root reducer that uses the combine reducer method provided by redux. 
```
const rootReducer = combineReducers({
    characters: charactersReducer
})
```

This was not needed in my current project because I only have one other reducer (characters) so It is not really combining other reducers but If I needed more or added to my project later i could add them in here like so.
```
const rootReducer = combineReducers({
    characters: charactersReducer,
		spells: spellsReducer, 
		attacks: attacksReducer,
		equipment: equipmentReducer
})
```
This would create one store and when actions are dispatched it would be able to handle them in the correct reducer. The reducers look like switch statements. They are given an action with an actiontype and a payload. The Reducer uses the action type to determind what it needs to do with the payload and the payload is the data that will change in the state. 

Action : `{ type: ADD_CHARACTER, payload: c}`

reducer: 

```
 case ADD_CHARACTER:
            return [action.payload]
```
This would be sent to the reducer for interpretation
```
{ type: DELETE_ATTACK, payload: attack}
```
The components would connect to this created store 
```
connect(mapStateToProps, mapDispatchToProps)(CharacterContainer)
```
and would have a written map state to props that give the components access to the stores state via props
```
const mapStateToProps = (state) => ({
    characters: state.characters
})
```
and would have a map dispatch to props that says what actioncreators (functions that dispatch an action to the store for the reducer to handle) this component has access too
```
const mapDispatchToProps = (dispatch) => {
    return { 
        fetchCharacter: (id) => dispatch(fetchCharacter(id)),
        deleteCharacter: (id) => dispatch(deleteCharacter(id)),
        deleteSpell: (id) => dispatch(deleteSpell(id)),
        deleteAttack: (id) => dispatch(deleteAttack(id)),
        deleteEquipment: (id) => dispatch(deleteEquipment(id)),
        updateCharacter: (character) => dispatch(updateCharacter(character)),
        updateSpell: (spell) => dispatch(updateSpell(spell)),
        updateAttack: (attack) => dispatch(updateAttack(attack)),
        updateEquipment: (equipment) => dispatch(updateEquipment(equipment)),
     }
}
```

The flow would look similar to this.
a function in the component is call that will trigger some type of state change. this function will call an action dispatched to the component via map dispatch to props. The action creator will take in this data (somtimes call for a fetch ect.) and call the dispatch function with an argument of an action. once dispatched this action will get interpreted by the reducer in the redux store (that we create) and the stores state will update accordingly. Then the connected component can see the new state via map state to props function.

### Make use of async actions to send data to and receive data from a server
This is where that think called thunk I mentioned earlier comes in. Thunk lets us return a function in our action creators rather then an object(actions are just objects) this means we can fetch data and return a function while waiting on the promise to resolve. once resovled that function can be dispatched to the reducer.

```
export const fetchCharacters = () => dispatch => {
    fetch('http://localhost:3000/api/v1/characters')
    .then(resp => resp.json())
    .then(characters => characters.map( c => dispatch({ type: ADD_CHARACTERS, payload: c})))
}
```
This is a actioncreator with Thunk middleware.
Note Thunk also allows you to return more then one function so you could set your state to loading in once funtion then once the promise is resolved update the state accordingly.

### Your Rails API should handle the data persistence. You should be using fetch() within your actions to GET and POST data from your API - do not use jQuery methods.

The fetch request gets sent to my rails backend and the controllers calls a method to store the data in a database then return the new object back to the frontend.



						




