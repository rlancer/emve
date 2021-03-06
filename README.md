emve.me
---
> [Emve.me](https://emve.me)  is a personal project of [Robert Lancer](https://github.com/rlancer), developed for just for fun!
> It uses my [gapi-to-graphql](https://github.com/rlancer/gapi-to-graphql) library which converts Google's Data API's to GraphQL 

[Emve.me](https://emve.me) is a virtual video jukebox! Use your TV as the player and get a group of friends to queue up videos using their phone.

### Player
<img style="padding-right:8px;max-width: 650px" alt='emve client' src='https://user-images.githubusercontent.com/1339007/121413584-d2acff00-c933-11eb-8307-b14fc2b55c40.png' />

### Remote

![Remote](https://user-images.githubusercontent.com/1339007/121435957-bddd6500-c94d-11eb-97cd-0b67f7dd6073.png)

## Running locally

* Generate an OAuth client ID and secret for Google Login by following [this guide](https://developers.google.com/identity/sign-in/web/sign-in)
* Generate a Google API Key by following [this guide](https://developers.google.com/youtube/registering_an_application)

Create a `.env` file by based on the `.env.example` files under the`emve-client` / `emve-server` directories

```bash
git clone git@github.com:emve-me/emve.git
docker-compose up
```

Visit localhost:3035 to see it in action 

## Infrastructure

* Resources
    * Google Cloud Run  
    * CloudSQL
    * Google Cloud Pub/Sub

* GitHub Actions - CI / CD
  * On pushes to master
    * Builds and pushes to Google Container Registry 
    * Deploys new revision to Google Cloud Run
    
## Stack
   * Universal
       * Typescript
           * Type gen from GraphQL
   * Frontend
       * NextJS
           * SSR
       * Authentication
           * Google Login, JWT stored in Cookie
           * Redirs based on cookie, a value of SSR
           * JWT token send in header to API requests
           * [Cookie handling library](https://www.npmjs.com/package/vanilla-cookies)
       * React
       * Apollo
           * Splits API transport into websockets and HTTP requests
           * For subscriptions, did not like their suggested implementation so used lower level workaround
       * UX
           * Responsive
           * Kept flows a simple as possible
               * Streamline creating a party to one click
           * Empty states
           * Remote, dead simple, default to showing your parties status or a very familiar search bar to find a song
       
   * Backend
       * [gapi-to-graphql](https://github.com/rlancer/gapi-to-graphql)
       * Postgres
           * Knex for query building
               * No ORM
               * No GraphQL / DB in one, ex: graph.cool
           * Migra* diffing for migrations (have not needed to implement)
       * Apollo Server
   * Security
       * Access to channels are inherently insecure to prioritize convenience, the following precautions have been taken to mitigate this:
           * Channel IDs are in a custom base 26 encoded alphabet (the alphabet but scrambled) to avoid users being able to guess channel IDs of an ongoing sessions
           * Channels ID start at 26^4 to minimize guessability


## TODO

- Optimistic UI / update cache from mutations, relying on subscription data for channel sate now
- Global style variables
- Pretty errors from gql
- Paging

## Wish list

- Live Queries, would cut down on complexity 
- Typescript friendlier database library with typegen from schema, a few systems like that out there

## Roadmap

- Talk to someone at YouTube to see if this is kosher, maybe for YTRed subscribers
- Restore lost player sessions
    - If your player closes and you reender, it should know you have a session going on and ask you to continue it
- Export a playlist of your party to YouTube / Spotifiy
- Playback parties
- Shout box
- Up voting
- Native apps
