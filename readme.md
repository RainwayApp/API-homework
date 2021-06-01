# Take-home assignment: API

For this assignment you will be creating a simple REST API around a database backend using php and mysql. You may use any framework you like for the API, but it must work with a standard mysql/mariadb database, ideally user configurable.

Here at Rainway we love video games. As such, you will be creating a system to manage users' video game libraries.

## Data Model

The data model can be described as follows:

A *game* has a *title*, *description*, and *age rating* (ESRB guidelines, E for Everyone through M for Mature).

A *user* has a *username* and an arbitrary number of *games* owned. They also have a *play time* associated with each game, denoting how long they've spent playing the game.

To get started, here is some sample data:

### Games
* title: Mirror's Edge
  * description: In a city where information is heavily monitored, couriers called Runners transport sensitive data. In this seemingly utopian paradise, a crime has been committed, & you are being hunted. You are a Runner called Faith and this innovative first-person action-adventure is your story.
  * age rating: T
* title: Deus Ex: Game of the Year Edition
  * description: The year is 2052 and the world is a dangerous and chaotic place. Terrorists operate openly - killing thousands; drugs, disease and pollution kill even more. The world's economies are close to collapse and the gap between the insanely wealthy and the desperately poor grows ever wider.
  * age rating: M
* title: Titanfall 2
  * description: Respawn Entertainment gives you the most advanced titan technology in its new, single player campaign & multiplayer experience. Combine & conquer with new titans & pilots, deadlier weapons, & customization and progression systems that help you and your titan flow as one unstoppable killing force.
  * age rating: M
* title: FINAL FANTASY XIV Online
  * description: Take part in an epic and ever-changing FINAL FANTASY as you adventure and explore with friends from around the world.
  * age rating: T

### Users
* username: xXx_sephiroth1997_xXx
  * FINAL FANTASY XIV Online, 400 hours play time
  * Mirror's Edge, 20 hours play time
  * Titanfall 2, 10 hours play time
* username: Gregor
  * Titanfall 2, 175 hours play time
  * Deus Ex: Game of the Year Edition, 230 hours play time


Please include a prepared SQL query to load the data into your database, however you should choose to architect it.

## API

The API wrapper surrounding your database should accept and return standard JSON and include the following routes:

### GET games/[id]
Returns the info for a particular game by ID.
```JSON
{
    "id": "...",
    "title": "Titanfall 2",
    "description": "Respawn Entertainment gives you the most advanced titan technology in its new, single player campaign & multiplayer experience. Combine & conquer with new titans & pilots, deadlier weapons, & customization and progression systems that help you and your titan flow as one unstoppable killing force.",
    "ageRating": "M"
}
```

### GET games
Returns info for all games in an array.

### POST games
Adds a new game. Body will contain game info. Returns the id: `{"id": "..."}`.

### PUT games/[id]
Updates or replaces a game by ID.
Request body will be JSON, same format as the previous.

### DELETE games/[id]
Deletes a game. Should also remove the game from all users' libraries.

### GET users/[id]
Gets a user's info by ID.
```JSON
{
    "id": "...",
    "username": "Gregor"
}
```

### POST users
Adds a user. Body will contain user info (just username). Returns the id: `{"id": "..."}`.
```JSON
{"username": "AwesomeUser2222"}
```

### PUT users/[id]
Updates a user. Same body as previous.

### GET users/[id]/games
Gets a user's game library.
```JSON
    {
        "games": [
            {
                "game": {...},
                "playTime": (number of minutes played)
            }
            ...
        ]
    }
```

### POST users/[id]/games
Adds a game to a user's library.
```JSON
{"id": "..."}
```

### DELETE users/[id]/games/[gameid]
Removes a game from a user's library.

When you have completed the assignment, upload it to a repository under your personal github account and link it to us in an email to work-at [at] rainway.com. Include all instructions for setting up an instance.
