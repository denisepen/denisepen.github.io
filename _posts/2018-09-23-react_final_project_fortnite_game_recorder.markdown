---
layout: post
title:      "React Final Project: Fortnite Game Recorder"
date:       2018-09-23 23:26:13 +0000
permalink:  react_final_project_fortnite_game_recorder
---


For my React final project I created a Fortnite game recorder.  My sons played Fortnite all summer and they were constantly arguing about who killed the most enemies in each game and who killed the most enemies overall, how many times they were in first place, and so on and so on...  I got sick of hearing the bickering so I decided to build an application they could use to track their progress.  

In the app the user can create a new game by filling out a form . The form has input fields for the date, the game mode, the player's total number of enemy eliminations, the player's final standing, and any comments the user wants to record about that particular game.  Once the form is submitted this input creates the individual games state:

```
this.state = {
      date: '',
      mode: '',
      max_kills: '',
      final_place: '',
      comments: ''
    }
		
 handleSubmit = event => {
  event.preventDefault();
  const game = this.state
  this.props.addGame(game)

  this.setState({
    gameId: '',
    date: '',
    mode: '',
    max_kills: '',
    final_place: '',
    comments: ''
  })
```

In the handleSubmit function I set the state to the variable 'game' and then passed this game variable (the state) to the 'addGame' function.  The 'addGame' function is an action creator that creates and sends the data to the rails api and also sends the created game to the reducer and adds it to the overall application state.

```
addGame(game) {

      const request = {
        method: 'POST',
        body: JSON.stringify(game),
        headers: {
          'Content-Type': 'application/json',
          'Authorization': 'Bearer ' + localStorage.getItem("jwtToken")
        }
      };
      
      return (dispatch) => {
        console.log("Dispatched request", request);
        return fetch('/games', request )
                 .then(response => response.json())
                 .then(newGame => dispatch({ type: 'ADD_GAME', newGame}))
      }
    }
```

This is the reducer used to send the newly created game to the store to be used by the rest of the application:

```
const initState = {games: [], users:[], user: {}};
export default function manageGame(state = initState, action) {

switch (action.type) {
case 'ADD_GAME':
      console.log("Add Action:", action);
      return { ...state, games: state.games.concat(action.newGame)};
			
		default:
      return state;
  }	
}
```

There were other such actions and reducers used to delete games from the store and also to add users and the current user to the store.  

With the data saved in the store I could fetch and display all of the game data that a user created as well as present the data in chart form so the user could view their progress over time.  My process for creating the charts was documented [here](http://deniseonaquest.com/react_building_chart_components).

One additional feature of this project was user authentication.  Since this is not something we covered in the online curriculum it took me a while to figure out how to do it.  Eventually I learned the process of authenticating a user in a Rails/React project. I needed to create a user model and User controller in the Rails Api.  

```
class User < ApplicationRecord
  has_secure_password
  has_many :games
	
end	


class UsersController < ApplicationController

  def index
    @users = User.all
    render json: @users
  end

  def create
    user = User.new(user_params)
      if user.save
        payload = { user_id: user.id)
        token = encode_token(payload)
        render json: { user: user, jwt: token }

      else
        render json: {errors: {message: "This user failed to save"}}
      end
    end


    def show
      @user = User.find_by(id: params[:id])
        render json: @user
    end


    private

    def user_params
        params.permit(:name, :gamer_tag, :email, :password)
     end


end
```

I put all of the necessary authentication code in the Application Controller so the other controllers would have access to it. What this code does is encode and decode the JWT (JSON web token) given to the front-end (our React front-end). In order to access a user's data from the Api, React must submit this code with every request. Once this request has been authenticated the user's games can be shown (See the games controller index and create actions). 

```
class ApplicationController < ActionController::API
  before_action :authorized, except: [:welcome]
  

  def encode_token(payload)
    token = JWT.encode(payload, {THE_SECRET})
  end

  def auth_Header
    header = request.headers["Authorization"]
  end

  def decoded_token
    if auth_Header
      token = auth_Header.split(" ")[1]
      begin
        decoded_token = JWT.decode(token, {THE_SECRET}, true, {algorithm: "HS256"})
      rescue JWT::DecodeError
        [{}]
      end
    end
  end


  def current_user
    if decoded_token
       if user_id = decoded_token[0]["user_id"]
       @user = User.find(user_id)
     end
    end
  end

  def logged_in?
    !!current_user
  end

  def authorized
    redirect_to '/welcome' unless logged_in?
    # redirect_to '/welcome' unless current_user
  end

  def welcome
    render json: {message: "Please Log In"}
  end
end



class GamesController < ApplicationController
  before_action :current_user

def create
  @game = Game.new(game_params)
  @game.user_id = @user.id
  if @game.save
    render json: @game
  else
    render json: {errors: {message: "This game failed to save"}}
  end
end

  def index
    if @user
      games = @user.games
    else
     @games = Game.all
  end
    render json: games
  end

  def show
    @game = Game.find_by(id: params[:id])
      render json: @game
  end

  def destroy
    Game.find(params[:id]).destroy
  end


  private

  def game_params
      params.require(:game).permit(:mode, :max_kills, :final_place, :comments, :date, :user_id)
   end

end
```

On the front end this JWT is stored in the browser's local storage when a user signs up or signs in.  As you can see from the action creator used to add the user to the store, the JWT is created and saved in the browser at signup:

```
export function addUser(user) {
 
      const request = {
        method: 'POST',
        body: JSON.stringify(user),
        headers: {
          'Content-Type': 'application/json',
        }
      };
      return (dispatch) => {
        return fetch('/users', request )
                 .then(response => response.json())
                .then(user => {localStorage.setItem("jwtToken", user.jwt)})
                 .then(newUser => dispatch({ type: 'ADD_USER', newUser}))
      }
    }
```

If you go into your browser's developer tools. Look in Application, Local Storage and you will see the saved token.  This token is sent with every request to the Api in the request header.  The Api takes this token, decodes it, and if valid will display the requested data. 

![img](https://i.imgur.com/aUai9km.png)



Below is the fetch action that sends the code to the Api to request the user's games.

```
export function fetchGames() {

  const request = {
    method: 'GET',
    headers: {
      'Content-Type': 'application/json',
      'Authorization': 'Bearer ' + localStorage.getItem("jwtToken")
    }
  };
  return function (dispatch){
     return fetch('/games', request)
           .then(response => response.json())
            .then(games => {
              dispatch({ type: 'FETCH_GAMES', games})
          })
    }
  }
```

I was also to use this token in the React components to enhance functionality. For example, If the user was signed in I wanted to set it up so they would no longer be able to access the login or signup pages .  I simply had to check if there was a token saved in the local storage.  If not they could view the signup and signin pages. If a token was saved they would be redirected to the welcome page whenever they clicked on the signin link. 

```
render () {
    if (localStorage.getItem('jwtToken')) {
    
      return <Redirect to='/' />
    } else {
      return ( 
        <div>
		 <Form Component />
		</div>
	}
```

This project really pushed me to understand React and Redux and to also finetune my understanding of Rails.  Using the Rails framework strictly as an Api was new to me and there was definitely a learning curve to overcome.  Despite that I'm excited to create new projects with my newfound understanding of React, Redux, and Rails.  


