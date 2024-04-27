
* What is the difference between `change` and `manual` while dealing with changesets in ash? 
* How to do bulk udpates? 
* Manage nested relationships ? 

Pyro + ashpyro can help make me ship ideas faster. 
What ideas make money? 
Minimalist Entrepreneur says what about it ?


How to decide A is worth doing? => is it important to you ? 


What improves life quality? 
* alertness in actions 
* awareness in thoughts


What is important to me ?
* wife  - improving life quality. 

--- 


I think there is a need for a 'showcase' repo / livebook depicting various use cases for `many-many` relationships. especially, showcasing `manage_relationship` -- that is most common use case where I get stuck frequently. 

Here are a few scenarios I have come across. Feel free to add more to the list. 

---

1. **user**  many-many **orgs** `through` **user_orgs**

**user_orgs** has keys `:user_role` + `user_id` + `org_id`

A. when deleting **user** -> delete from **user_orgs** but don't delete **orgs** 
B. when deleting **user** -> also delete **user_orgs** and **orgs** 
C. when creating **user** -> also create **org** and pass the `user_role ` in `manage_relationship`

---

2. **artist** many-many **album** `through` **artist_album** 
**album** many-many **songs** `through` **album_songs**

A. `create_song_with_artist_and_album` -> takes **song** attributes and artist_id, album_id to manage those 
 





