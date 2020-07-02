

# News Feed Design
## Pull
   * slow when read, but can use cache to speedup (cache the most recent 100 tweets maybe)
## Push
   * slow when write, but can be done async
    * fanout might need a few hours for a celebrity
## The right approach
    * Try to solve the problem with the minimum code change
        * add more machines in peak hours
    * Evaluate the longterm trend to decide wether to change

## Other approaches
    * Push for ordinary user
    * Pull for celebrity user
    * Preiodically to update wether a user is celebrity


# Design User System
## Scenario
    * register, login, search, user info update
## Service
    * AuthService (responsible for login and register)
    * UserService (Query and store of user info)
    * FriendshipService
## Property
    * user system: read a lot, write a little, so must use cache to speed up
    
## Memcached
## Authentication
## SQL vs NoSQL
## Friendship

## How to scale
    * sharding
    * consistent hashing
    * replica





