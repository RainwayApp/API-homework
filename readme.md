# Take-home assignment: API

For this assignment you will be creating a simple REST API around a database backend. You may use any language or framework you like for the API, and any database you prefer for the backend.

Here at Rainway we love video games. As such, you will be creating a system to manage users' video game libraries.

## Data Model

The data model can be described as follows:

A *game* has a *title*, *description*, a set of 1 to 5 *images* (random strings with 32-256 chars), and *age rating* (ESRB guidelines, E for Everyone through M for Mature).

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


Please include a prepared query to load the data into your database, however you should choose to architect it.

## API

The API wrapper surrounding your database should accept and return standard JSON and include the following routes:

### GET games/[id]
Returns the info for a particular game by ID.
```JSON
{
    "id": "...",
    "title": "Titanfall 2",
    "description": "Respawn Entertainment gives you the most advanced titan technology in its new, single player campaign & multiplayer experience. Combine & conquer with new titans & pilots, deadlier weapons, & customization and progression systems that help you and your titan flow as one unstoppable killing force.",
    "ageRating": "M",
    "images":["someSetOf","bannerStrings"]
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

### DELETE users/[id]
Deletes a user.  Any "PII" (their ID), should be 100% striken from the database, but for metrics purposes we want to preserve their playtimes for games.  In no event should a deleted user ever be able to be associated back to the remembered play times.

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

### POST users/[id]/games/[gameid]
Adds playTime for a game in a users library.  The submitted time should be added to the time already stored for the game for the user.
```JSON
{
 "game":{
  "id":"...",
  "playTime":(number of minutes played, round up)
  }
}
```

### DELETE users/[id]/games/[gameid]
Removes a game from a user's library.  Time played for the game should not be removed.  To be clear Adding a game, playing for some time, removing the game, and then adding back the same game should not reset the time the user played.  This is different than from when an entire user account is deleted and should be a "recoverable" event.
```JSON
{"id": "..."}
```

### GET static/text/[a file]
Allow for fetching some sort of static content.  The contents of the static file should be returned, a few simple text files is plenty.



## Additional Constraints and Considerations

Structure the database so that it can answer typical Business Analytics questions.  Marketting may want to know "What was the most popular game played last month by users who played more than 30 minutes of this specific game".  We may also want to be answer things like "How many new games were added to the system last week".

As mentioned in the route definitions... Allow for a way to decouple playtime aggregation from users PII (their id).  In the event a user deletes their account it's important the business does not "forget" about the time a game was played.  It's vitally important however that there's *nothing* tying the deleted user back to the games they owned or played after the account is deleted.

Api, Route and database performance is important. Avoid processing input data sequentially where relevant.  Avoid connecting to the database when it's not needed.  

Any routes accepting input should properly error when that input is missing or is invalid.

When you have completed the assignment, upload it to a repository under your personal github account and link it to us in an email to work-at [at] rainway.com. Include all instructions for setting up an instance.
