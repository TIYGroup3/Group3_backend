# flashbang api

Welcome to the flashbang API docs, hope you find what you are looking for here!

**Methods**

* [User Methods](#user-methods)
	* [Registration](#user-registration)
	* [Login](#user-login)

* [Deck Methods](#deck-methods)
	* [Create Deck](#deck-create)
	* [List Decks](#deck-list)
	* [Edit Deck](#deck-edit)
	* [Delete Deck](#deck-delete)

* [Card Methods](#card-methods)
	* [Create Card](#card-create)	 

##<a name="user-methods"></a>User Methods

###<a name="user-registration"></a>Registration

Register a new user and receive back the new user's access key and user id.

**URL** /users/register

**Method** POST

**Request**
	

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| username  | String | *(Required)*  unique username |
| fullname      | String      |  *(Required)*   User's first and last name |
| email | String      | *(Required)*   User's email (must follow format text@text.text) |
| password | String | *(Requred)* User's password


**Response**

If successful, you will receive:

	Status Code: 201 - Created
	
```json
	{ "user": 
			{ "user_id": 1,
			  "access_key": "biglongaccesskeyhere"
			}
	}
			
```

If unsuccessful, you will receive:

	Status Code: 422 - Unprocessable Entity
	
```json
	{"errors":[
				"Email has already been taken",
				"Username has already been taken"
				]
	}
```

###<a name="user-login"></a>Login

Users can get their access_key by sending a username/password.

**URL** /users/login

**Method** POST

**Request**

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| username| String | *(Required)* Existing user's username | 
| password | String | *(Required)* User's password | 

**Response**

If successful, you will receive:

	Status Code: 202 - Accepted
	
```json
	{ "user": 
			{ "user_id": 1,
			  "access_key": "biglongaccesskeyhere",
			  "username": "myusername"
			}
	}
			
```

If unsuccessful, you will receive:

	Status Code: 401 - Unauthorized
	
```json
	{ "errors": [ 
				"User or password incorrect. So sorry you aren't getting in!"
				] 
	}
```

##<a name="deck-methods></a>Deck Methods

###<a name="deck-create"></a>Create Deck

Authenticated users can create a new deck. 

Note: Users cannot have duplicate titles for decks they own.  However, two users can have decks with the same title.

**URL** /decks

**Method** POST

**Request**

*Required* 

***HEADERS*** : Access-Key = string

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| title| String | *(Required)* Deck title | 


**Response**

If successful, you will receive:

	Status Code: 201 - Created
	
```json
	{ "deck": 
			{ "user_id": 1,
			  "id": 3,
			  "owner": "terric",
			  "title": "my awesome deck"
			}
	}
			
```

If unsuccessful, you will receive:

	Status Code: 422 - Unprocessable Entity
	
```json
	{ "errors": [ 
				"Title has already been taken"
				] 
	}
```

###<a name="deck-list"></a>List Decks

Authenticated users can list ALL decks available or only decks that they have created.

**URL** /decks

**Method** GET

**Request**

*Required* 

***HEADERS*** : Access-Key = string

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| owner| String | *(Required)* 'all' for all decks or 'mine' for decks created by the authenticated user | 

**Response**

If successful, you will receive:

	Status Code: 200 - OK
	
```json
		{"decks":	[
						{ 	
						"id":1,
						"title":"test title",
						"owner":"mans"	
						},
						{	
						"id":2,
						"title":"cats",
						"owner":"terri"	
						}
					]
		}	
```

If unsuccessful, you will receive:

	Status Code: 404 - Not Found
	
```json
	{ "errors": [ 
				"User 'cats' not found"
				] 
```

###<a name="deck-edit"></a>Edit Deck

Authenticated users can edit a deck that they have created.

**URL** /decks/:id

	:id = existing Deck ID

**Method** PATCH

**Request**

*Required* 

***HEADERS*** : Access-Key = string

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| id| Integer | *(Required)* Deck ID that you wish to edit.  Must be a deck created by the Authenticated User | 
| title | String | *(Required)* New deck title.  Cannot be blank. |

**Response**

If successful, you will receive:

	Status Code: 200 - OK
	
```json
		{"success":	"Deck updated successfully"
		}	
```

If unsuccessful, you will receive:

	Status Code: 401 - Not Authorized
	
```json
	{ "errors": [ 
				"That deck doesn't belong to you!"
				] 
```

	Status Code: 422 - Unprocessable Entity
	
```json
	{ "errors": [ 
				"Title cannot be blank!"
				] 
```

###<a name="deck-delete"></a>Delete Deck

Authenticated users can delete a deck that they have created.  All cards created for that deck will also be deleted.

**URL** /decks/:id

	:id = existing Deck ID

**Method** DELETE

**Request**

*Required* 

***HEADERS*** : Access-Key = string

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| id| Integer | *(Required)* Deck ID that you wish to edit.  Must be a deck created by the Authenticated User | 

**Response**

If successful, you will receive:

	Status Code: 200 - OK
	
```json
		{"success":	"Deck deleted successfully"
		}	
```

If unsuccessful, you will receive:

	Status Code: 401 - Not Authorized
	
```json
	{ "errors": [ 
				"That deck doesn't belong to you!"
				] 
```

##<a name="card-methods"></a>Card Methods

###<a name="card-create"></a>Create

Users can create new cards within the deck.  
No card can have the same front or back.

**URL** /decks/card

**Method** post "decks/:id/cards", to:"cards#create"
**Request**

*Required* 

***HEADERS*** : Access-Key = string

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| deck_id| String | *(Required)* Deck Id | 


**Response**

If successful, you will receive:

```json
	{"card":
			{"id":6,
			"front":"Flashy",
			"back":"Cardy","
			deck_id":1}}
	}
```	
Status Code: 201 - Created	
	
I

If unsuccessful, you will receive:

	Status Code: 422 - Unprocessable Entity
	
```json
	{ "errors": [ 
				"Front or backside has already
				 been entered"
				] 
	}
```

##<a name="guess-methods></a>User Guess Methods

###<a name="guess-create"></a>Create User Guess

Authenticated users can add a guess for a specific card within a deck. 

Note: Users can guess multiple times on a card.  No limit to how many times a user can play with a deck or card.

**URL** /cards/:id/guess

	:id = existing Card ID

**Method** POST

**Request**

*Required* 

***HEADERS*** : Access-Key = string

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| Duration| Integer | *(Required)* Number of seconds that the user took to guess the answer. |
| Correct | Boolean | *(Required)* If the user got the question right or not.  Only accepted values are true and false. |


**Response**

If successful, you will receive:

	Status Code: 201 - Created
	
```json
	{ "success": "Guess saved."	}
			
```

If unsuccessful, you will receive:

	Status Code: 422 - Unprocessable Entity
	
```json
	{ "errors": [ 
				"Error message"
				] 
	}
```

###<a name="guess-create"></a>Get User Guesses

Authenticated users can get guess statistics for a specific card within a deck. 


**URL** /cards/:id/guess

	:id = existing Card ID

**Method** GET

**Request**

*Required* 

***HEADERS*** : Access-Key = string

| Parameter        | Type           | Description  |
| ------------- |:-------------:|:----- |
| owner| String | *(Required)* 'all' for all guesses for the specified card or 'mine' for guesses created by the authenticated user | 


**Response**

If successful, you will receive:

	Status Code: 200 - OK
	
```json
	{ "stats": { "guess_count": 10,
					"avg_duration": 4.5,
					"correct_count": 5,
					"incorrect_count": 5,
					"fastest_duration": 2,
					"fastest_user": "cookies" }
	}
			
```

If unsuccessful, you will receive:

	Status Code: 422 - Unprocessable Entity
	
```json
	{ "errors": [ 
				"Please specify if you want to see 'all' guesses or just 'mine' for this card!"
				] 
	}
```