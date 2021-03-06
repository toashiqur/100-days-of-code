
# #100DaysOfCode Log - Round 2 - Dashiell Bark-Huss

## Day 100, R2
### 7/19/19

- ## Second Round Complete
  I finished my second round of #100DaysOfCode. I'm well on my way to coding every day this year. 
  
  This last round I continued learning frontend and started to dive into backend. I'm going to continue to look into backend. My goal is to be able to make a fullstack app from scratch. I'm very close to this!

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).


  ### Where I left off:
  Yesterday, I started to redo the [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) tutorial. I continued with that today.

  Not much to report on today!

- ## Thoughts and Feelings:
  I'm super sleep deprived. I still haven't caught up on sleep from my trip home and the shiva is interfering. When I'm sleep deprived learning is difficult. Even walking is difficult. Everything is more difficult.

  I have been setting an alarm for 8:30am. My alarm wakes me every day. I think it's unhealthy to wake up to an alarm. I like to wake up *before* my alarm, because that means by body got enough sleep and woke up naturally. My alarm should just be for insurance. My alarm is waking me up before my body is ready to wake up.
  
  I don't like sleeping too late so I'm using my alarm temporarily to reset my schedule. But I'm not getting to sleep early enough.

  I'm going to try to do a short sleep related challenge. Something like 7-days-of-in-bed-by-11 or something. I'm on day 12 of no caffeine (except for chocolate before 5pm).

  The hardest part about keeping a good sleep schedule is the social pressure to stay up with people. It feels almost rude to tell someone you have to leave their event early to go to sleep. 
  
  When you have a sleeping partner it's extra hard. You're forcing them to comply with your strict schedule. I feel guilty. It was hard to convince my spouse not to be on his computer in bed while I'm trying to go to sleep. I finally got him to agree because I **let him do it anyways** and he saw that after an hour and a half I still hadn't fallen asleep. So the next night he said he would go on his computer on the couch instead. 

  If I take on a 7-days-of-in-bed-by-11 challenge, it will help me confront all these little issues that are stopping me:
  - Feeling rude for leaving events early
  - Feeling weird going to sleep before my spouse is in bed
  - Feeling guilty for having a strict sleep schedule that inconveniences others
  - Trouble stopping a chill session with my spouse or sister (who we live with) to go to sleep
  - Learning to be more assertive
  - Learning to make sleep a priority
  - Learning to feel like I deserve to control my sleep schedule
  
  Shortening my sleep onset is another issue, but I think these social issues are actually the more pressing. The guilt may itself delay my sleep onset. If I can over come these issues, I can next address shortening my sleep onset.

  
## Day 99, R2
### 7/18/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).
  
  ### Where I left off:
  I finally got everything important working. Except for the tweet and email related buttons, all of the buttons work.

  <img src="log_imgs/ui_7-17.PNG" width = "500"/>

  I started rereading [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## Multiple Apps
  What if I want to make multiple apps? Do I just use the same linode and MySQL but use another database? What if I want the settings to be different for each database, but if they have the same MySQL settings file, is it possible for them to have different settings??

  From reading the book, I think you might have different servers for different apps in production. In development, maybe you could keep them on the same server.

  ## Reread [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087)
  I finished rereading [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) and read chapter 7 for the first time.

  ## Redoing The Tutorial
  I started to redo and reread (again) [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

- ## Thoughts and Feelings:
  Tim Ferris talked about doing multiple passes of reading a book as opposed to doing slower comprehension focused reading. I'm implementing multiple passes of reading this tutorial. It's helpful because the first time I read about a new concept, I have no context. The second time, I have a new context for the concept and I can understand more.

## Day 98, R2
### 7/17/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ### Where we left off:
  The latest update to [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087)'s finished files ***didn't work***. Over the past few days, I got the session to save to the session table. Yesterday, I got the token to the front end, now I need to save it to localStorage.

   ## How To Store Tokens In `localStorage`
  > To save the token in your browser you can simply use:
  >
  >`localStorage.setItem('token', token);`
  >
  >and later access it with:
  >
  >`localStorage.getItem('token');`

  -*[Example: JSON Web Tokens with Vanilla JavaScript](https://jonathanmh.com/example-json-web-tokens-vanilla-javascript/)*
  
  To store the token, I added `localStorage.setItem('token', token);`
  ```javascript
  // line 74, index.html
  User.login = function(payload) {
      fetch("/api/user/login", make(payload)).then(promise => promise.json()).then(json => {
          console.log(json);
          if (json.success===true){
            return fetch("/api/session/create", make(payload))
          }
      }).then(promise => promise.json()).then(json => {
          localStorage.setItem('token', json.token); // store token
      });
  }
  ```

  Then I accessed the token with `localStorage.getItem('token');`
  ```html
  <!-- line 165, index.html -->
  <input type="button" onclick="User.authenticate({token:localStorage.getItem('token')})"
  value="authenticate (check if token exists in session table)" /><br />
  ```
  ## Logout
  There is no logout button on the UI, so that's what I want to add next. We'll also have to add the fetch call to logout to the frontend API.

  ## Finished Files Missing Logout Body
  The finished files don't have the logout body.
  ```javascript
    // line 227 api.js
  function action_logout ( request, payload ) {
      return new Promise((resolve, reject) => {
          /* implement */
      }).catch(error => console.log(error));;
  }
  ```
  I'm going to try to add it.

  ## Constructing The `action_logout` Function

  I think all we need to do is delete the session from the database. We might also have to remove the token from local storage but that might not be necessary.

  Is it better to delete the session by sending in the username in the payload? Or by sending in the the token?
  
  I'm going to delete the session by sending in the token and searching the session by token, since the token is stored in localStorage. I'm wondering if this could create issues where there are multiple sessions for one user. Multiple sessions shouldn't happen, but I wonder if it could happen somehow.
  
  ```javascript
  // line 227, api.js
  function action_logout ( request, payload ) {
      return new Promise((resolve, reject) => {
          // find the session from the database by the token and delete it
          let query = `DELETE * FROM \`session\` WHERE \`token\` = '${payload.token}'`;
          console.log(query);
      }).catch(error => console.log(error));;
  }
  ```

  Next we have to add the `database.connection.query` call.

  ```javascript
  // line 227, api.js
  function action_logout ( request, payload ) {
      return new Promise((resolve, reject) => {
          // find the session from the database by the token and delete it
          let query = `DELETE * FROM \`session\` WHERE \`token\` = '${payload.token}'`;
          console.log(query);
          database.connection.query(query, (error, results) => { // query call
            if (error)
                throw(error);
            let result = results[0];
            console.log("results[0] = ", results[0]);
            console.log("result = ", result);
            resolve(`{"success": true, "message": "user logged out!"}`);
        });    
      }).catch(error => console.log(error));;
    }
  ```

  I wonder if this is a bad idea because what if somehow the user clears their local storage? I feel like I should probably delete the session by user id. 

  I changed the query, to search the `user_id`.

  ```javascript
  // line 227, api.js
  function action_logout ( request, payload ) {
    return new Promise((resolve, reject) => {
        // find the session from the database by the user and delete it
        let query = `DELETE * FROM \`session\` WHERE \`user_id\` = '${payload.username}'`;
        // ...
  ```

  I just realized, if we access the username from localStorage, we'd have the same issue. But I'm going to leave it like this for now anyways.
  ## Logout Frontend API

  ```javascript
  // line 85, index.html
  /* Logout
    payload = {
    username: "username"} */
  User.logout = function(payload) {
      fetch("/api/user/logout", make(payload)).then(promise => promise.json()).then(json => {
          console.log(json);
      });
  }
  ```

  ## Logout UI Button
  ```html
  <!-- line 179, index.html -->
  <input type="button" onclick="User.logout({username:'felix'})" value="log out" /><br />
  ```

  For an actual app, we'll have to add the username to localStorage and call User.logout dynamically, instead if with 'felix'.

  ## Error
  I got on error related to my query
  ```bash
  Error: ER_PARSE_ERROR: You have an error in your SQL syntax; check the manual that corresponds to your MySQL server version for the right syntax to use near '* FROM `session` WHERE `user_id` = 'felix'' at line 1
  ```
  ### Query
  ```sql
  DELETE * FROM `session` WHERE `user_id` = 'felix'
  ```

  I think you can't delete all: `DELETE *`

  So I changed the query to:
   ```sql
  DELETE FROM `session` WHERE `user_id` = 'felix'
  ```
  
  Now it works!

  ## Working Files
  So far I've got everything important working. Except for the tweet and email related buttons, all of the buttons work.

  <img src="log_imgs/ui_7-17.PNG" width = "500"/>

  ### 1\.Here's the link to the finished files:
  ### [Link To Work, latest commit](https://github.com/DashBarkHuss/node_book/tree/5e8a6f8a9c93dab3264bab80b176986cdbaddba0)

  ### 2\. Here's how my **mysql user**, **felix2**, was set up:

  ![](log_imgs/mysqlusers_7-17.PNG)

  ### 3\. Here's how I logged in to **Sequel Pro**:

  <img src="log_imgs/sequel_7-17.PNG" width="400"/>

  ### 4\. Here's how my myserver user table was set up:

  ![](log_imgs/myserveruser_7-17.PNG)

  The "password_..." is password_md5.

  ### 5\.Here's how my myserver session table was set up:

  ![](log_imgs/session_7-17.PNG)



  ## What Next?
  Next I'd like to do this whole tutorial again and see If I can create api.js, index.html, and index.js (mostly) from scratch.

  Maybe I can try to make a small app?

  I started to reread the book.

- ## Thoughts and Feelings:
  I learned a lot from figuring out all the parts the Greg left out in this update. I probably learned more than if I had the complete tutorial. I had to get my hands dirty in the code and see how everything really works in order to finish where Greg left off.

  I'm very happy to have completed this. One of my goals it to be able to complete a full stack web app myself. This got me much closer.


## Day 97, R2
### 7/16/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ### Update:
  I got in touch with Greg Sidelnikov ([js_tut](https://twitter.com/js_tut)). He said he's working on the update to the [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) book. The issues I ran into may be addressed in the next update.

  ### Where we left off:
  The [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) finished files ***didn't work***. Over the past few days, I got the session to save to the session table. Today, I'm going to try to save the token to the front end.
  
  ## Storing The Token

  Yesterday, I was trying to figure out where to save the token. Where would I put this? Does it go in `index.html`, `api.js`, or `index.js`?

  ## Clarify Questions
  I was trying different ideas with my code but I'm confused so I want to clarify my questions:
  Right now I'm calling `action_create_session` within `action_login`. This is a promise within a promise. 
  
  - ### How do I get the data from `action_create_session` to go to the UI if I'm not fetching it from the UI?
    - Should I instead fetch `action_create_session` from the UI?
      - Is that secure?

  ## Fetching `action_create_session`

  I moved the `action_create_session()` call from api.js, inside `action_login`
  ```javascript
  // line 218, api.js
  action_create_session(request, payload)
  ```

  To the frontend in a fetch api call:
  ```javascript
  // line 74, index.html
  User.login = function(payload) {
      fetch("/api/user/login", make(payload)).then(promise => promise.json()).then(json => {
          console.log(json);
          if (json.success===true){
            return fetch("/api/session/create", make(payload))
          }
      }).then(promise => promise.json()).then(json => {
          console.log(json);
      });
  }
  ``` 

  Fetching `"/api/session/create"` will call `action_create_session`.
  
  I'm just confused if this is secure. Could someone change `json.success` from `false` to `true` on the frontend and  create a session without a password? Idk.

  [Using a fetch inside another fetch in javascript
](https://stackoverflow.com/questions/40981040/using-a-fetch-inside-another-fetch-in-javascript)

  ## Git Diff
  I made a mistake in my files and I wasn't sure what it was so I tried to use [`git diff`](https://veerasundar.com/blog/2011/06/git-tutorial-comparing-files-with-diff/) to see the difference between my current commit and my commit before. It was really hard to read so I just looked at the difference on github.
  
## Day 96, R2
### 7/15/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).
  
  ### Where we left off:
  The [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) finished files ***didn't work***. They didn't create a session. Now I got the app to create a session but the **authenticate (check if token exists in session table)** button didn't work.


  So I changed the token sent through the UI: from a test token to the actual token. But it still didn't work because the files didn't refresh. The payload was still logging as {` token: 'token_test_12345' }`.

  ## Working
  I tried the **authenticate (check if token exists in session table)** button again. Now it works. So it finally refreshed.

  ## Where Should The Token Actually Be?

  Right now the token is stored statically on the front end. 

  ```html
  <!-- line 159, index.html -->
  <input type="button" onclick="User.authenticate({token:'bb6cd80736ece803dd33c004f7a94278'})"
  ```

  This is just for testing. We cannot store every user's token for every session on the front end.

  So where do we store it?

  As a cookie? 

  In local storage?

  ## Don't Store Tokens In Local Storage?
  >Don't store tokens in local storage
  Browser local storage (or session storage) is not a secure place to store sensitive information. Any data stored there:
  >
  > - Can be accessed through JavaScript.
  > - May be vulnerable to cross-site scripting.
  
  -*[Where to Store Tokens](https://auth0.com/docs/security/store-tokens#don-t-store-tokens-in-local-storage)*

  ## AuthToken In Browser
  In the diagram from the book, it shows that the authtoken is stored in the browser.
  ![](log_imgs/authtoken_7-15.PNG)

  But if we shouldn't use local storage, how do we store it?

  >### If a backend is present
  >
  >If your single-page app has a backend server at all, then tokens should be handled server-side using the [Authorization Code Flow](https://auth0.com/docs/flows/concepts/auth-code), [Authorization Code Flow with Proof Key for Code Exchange (PKCE)](https://auth0.com/docs/flows/concepts/auth-code-pkce), or [Hybrid Flow](https://auth0.com/docs/api-auth/grant/hybrid).

  -*[Where to Store Tokens](https://auth0.com/docs/security/store-tokens#don-t-store-tokens-in-local-storage)*

  I'm not sure which I should be using.

  ## Reviewing Sessions and Tokens
  I looked back at these diagrams that Marco([@Wridgeu](https://twitter.com/Wridgeu)) sent me to help me understand sessions and tokens.

  ## Tokens:
  ![](log_imgs/tokens_7-6.jpg)

  ## Sessions:
  ![](log_imgs/tokens2_7-6.png)

  ## *Do* Store Tokens In Local Storage?
  Now I'm reading that you ***can*** store tokens in local storage:

  >There are three ways how to store a token in a browser:
  >
  >1\. **LocalStorage** - stores data with no expiration date, no access from a backend.
  >
  >2\. **SessionStorage** - stores data until browser/tab is open, no access from a backend.
  >
  >3\. **Cookie** - stores data, expiration time can be set individually, automatically sent with subsequent requests to the server.


  -*[Where to store auth token (frontend) and how to put it in http headers of multiple endpoints?](https://stackoverflow.com/questions/51332747/where-to-store-auth-token-frontend-and-how-to-put-it-in-http-headers-of-multip)*
  
  I like idea of storing the token in a cookie where you can control the expiration time.

  But then there's another idea here: [Store Auth-Token in Cookie or Header?](https://security.stackexchange.com/questions/180357/store-auth-token-in-cookie-or-header) Here, we can also store cookies in the `header`.

  ## Options For Storing The Token
  - **Cookie**
  - **Header**
  - **SessionStorage**
  - **LocalStorage**

  ## Storing The Token In `localStorage`
  > To save the token in your browser you can simply use:
  >
  >`localStorage.setItem('token', token);`
  >
  >and later access it with:
  >
  >`localStorage.getItem('token');`

  -*[Example: JSON Web Tokens with Vanilla JavaScript](https://jonathanmh.com/example-json-web-tokens-vanilla-javascript/)*

  Where would I put this? Does it go in `index.html`, `api.js`, or `index.js`?

  I think it goes in `index.html`. I don't think the other files have excess to the frontend and the WEB API.

  ## Log Token To Terminal
  I logged the token to the terminal by adding `.then` to line 218, api.js.

  ```javascript
  action_create_session(request, payload).then(x=>console.log((JSON.parse(x)).token)); 
  ```

  Tomorrow, I'll try to get the token to the frontend.
## Day 95, R2
### 7/14/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).
  
  ### Where we left off:
  The [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) finished files ***don't work***. They don't create a session. So I've been working on that.
  
  I had an error that had to do with the format of the timestamp when the `timestamp` function was called to create a timestamp to add to the `session` table.

  I changed the parameter of the `timestamp` function from `undefined` to `true`. But I still wondered: 
  - Should I instead change the data type of `timestamp` in the `session` table to a a "linux-length timestamp"? 
  - Or should I keep my changes I made today: changing the parameter of the `timestamp` function from `underfined` to `true`?

  I also need to look at my primary key error.

  ## Linux-length Timestamp
  Data type for a unix timestamp: 

  **int(11)**
  
  [What is the data type for unix_timestamp (MySQL)?](https://stackoverflow.com/questions/4125947/what-is-the-data-type-for-unix-timestamp-mysql).

  ## New Fix For Timestamp Error
  I changed the data type of `timestamp` to `int(11)`:

  ![int_7-14.PNG](log_imgs/int_7-14.PNG)

  And I changed the `timestamp(true)` call on line 260 back to it's original call, `timestamp()`
    ```javascript
  // line 260 col 131, api.js (database.connection.query)
  timestamp() + "', '" + token + "')"
  ```

  This is another way to fix the timestamp error. This is probably the correct way because this second way changes my `session` table, which I created without the book. So my `session` table is more likely to have errors than the downloaded github files for the project.

  ## Primary Key Error
  I have this error now:
  ```bash
  ER_DUP_ENTRY: Duplicate entry 'undefined' for key 'PRIMARY'
  ```
  
  ## Records In Session
  I looked at the records in the `session` table. `user_id` is getting saved as `undefined`:

  ![](log_imgs/content_7-14.PNG)
  The timestamp [correlates](https://www.epochconverter.com/) to yesterday: 

  - Saturday, July 13, 2019 12:08:59 PM GMT-05:00 DST

  So the first time my code worked, it saved the session `user_id` as `undefined`. Even if it saved the session `user_id` correctly as `felix`, we might still get an error. I think it would just say:
  ```bash
  ER_DUP_ENTRY: Duplicate entry 'felix' for key 'PRIMARY'
  ``` 

  So we need to do two things:
  - get the code to save the correct user id
  - add conditionals to see if the session exists.

  ## `user_id` 
  First, I'm going to see where user id is saving as `undefined`.

  The trace says that the issue happens here:
  ```bash
  at Query.<anonymous> (/Users/dashiellbark-huss/Documents/100daysofcode/node-master/module/api/api.js:260:37)
  ```

  So the issue happens at the query from yesterday's issue:
  ```javascript
  // line 260, api.js
  database.connection.query("INSERT INTO session ( `user_id`, `timestamp`, `token`) VALUES( '" + payload.id + "', '" + timestamp() + "', '" + token + "')",
    (error, results) => {
        if (error) throw(error);
        resolve(`{"found" : false,
                  "token" : token,
                  "user_id": ${payload.user_id},
                  "message": "session was created"}`);
    });
  ```

  Sure enough, logging `console.log(payload.id);` right before the query, logs `undefined`. I logged logging `console.log(payload);` and got
  ```bash
  { username: 'felix', password: 'password' }
  ``` 

  There is no `id` in this payload object. Just `username`.

  Both `payload.id` and `payload.username` are used in the code. I'm going to change  `payload.id` to `payload.username`. I'll probably have to do this multiple times because it's used in other functions too.

  ## Solved
  I changed `payload.id` to `payload.username` on lines 248, 252, and 262. Now it's sending the right username to the database. It's not giving me the duplicate error either. 
  
  I'm surprisingly not creating duplicate sessions for the same user. I thought this would be our next error. But there's a conditional in `action_create_session` that stops the code from adding duplicate entries.
  
  ## Next
  I think we might have to add `if` statements for conditionally executing the `action_create_session` or `action_get_session` because that's what the diagram from the book showed:
  
  ![](log_imgs/sessiondiagram_7-12.PNG)
  
  Right now the code is working. I'm not sure the point of `action_get_session`  because there's similar code that's conditionally executed in `action_create_session`.

  So I'm going to focus on getting the **authenticate (check if token exists in session table)** button to work. Right now it's giving me an error.

  ```bash
  API.authenticate, results.length == 0 (session with token not found)
  {"success": false, "message": "token not found in session"}
  responding =  [ undefined ]
  ```

  ## Test Token
  I realized the error is happening because the UI sends a payload with a test token:

  ```javascript
  // line 156, col 56, index.html
  {token:'token_test_12345'}
  ```

  Obviously, this does not match the session token in my database, which is a long string of numbers and letters.
  
  ## Refreshing Issue
  I changed the token to the token in my session table

   ```javascript
  // line 156, col 56, index.html
  {token:'bb6cd80836ece403dd33c004f7a44271'})
  ```

  It's still not working but it looks like it's some sort of error that has to do with the code not refreshing because the payload is still logging as `{ token: 'token_test_12345' }`. I keep restarting the server, and my files are saved so I'm not sure what's going on.

  Tomorrow, I'll try to get this to refresh.
## Day 94, R2
### 7/13/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ### Where we left off:
  The [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) finished files ***don't work***. They don't call the function that creates the session and token, `action_create_session`. 
  
  Yesterday, we tried to call `action_create_session(request, payload)` inside `action` login on line 218: 
  ```javascript
  // line 218, api.js
  action_create_session(request, payload); //add session--should be conditional but just adding for now, Dash
  ```

  But got an error:
  ```bash
  Error: ER_TRUNCATED_WRONG_VALUE: Incorrect datetime value: '1562952357' for column 'timestamp' at row 1
  ```
  
  Looks like something is wrong with the timestamp.

  ## Which timestamp?
  There's a timestamp in the `session` table and in the `user` table. After some tracing, I figured out this error relates to `timestamp` in the `session` table.

  ```bash
  at Query.<anonymous> (/Users/dashiellbark-huss/Documents/100daysofcode/node-master/module/api/api.js:259:37)
  ```
  ```javascript
  // line 259, api.js
  database.connection.query("INSERT INTO session ( `user_id`, `timestamp`, `token`) VALUES( '" + payload.id + "', '" + timestamp() + "', '" + token + "')",
    (error, results) => {
        if (error) throw(error);
        resolve(`{"found" : false,
                  "token" : token,
                  "user_id": ${payload.user_id},
                  "message": "session was created"}`);
    });
  ```
  ## timestamp Fields Data Type

  I changed the data type of the timestamp in the `session` table to match the data type in the `user` table.

  Before:
  ![](log_imgs/time1_7-13.PNG)

  After:
  ![](log_imgs/time2_7-13.PNG)

  The `session`  table had no instructions in the book. But the `user` table did. So I figured, best to match `timestamp` from the table that had explicit instructions in the book. I had to come up with the `session` table myself, since it was omitted from the book. So the `session` table is more likely to be incorrect. 


  ## timestamp Called Twice

  I put console.trace() in the `timestamp` function. `timestamp` is called twice. 
  
  It works the first time. But not the second.

  ### First Call
  **The first time**, when it works, the trace comes from `create_auth_token`. `true` is the argument:
  ```javascript
  // line 241, api.js (create_auth_token)
  let token = md5( timestamp( true ) + "");
  ```

  ### Second Call
  **The second time**, when it doesn't work, the trace comes from the query on line 259.
  No argument passed to timestamp:
  ```javascript
  // line 260 col 131, api.js (database.connection.query)
  timestamp() + "', '" + token + "')"
  ```

  ## `full` Parameter And The Return Value Of Timestamp:
  Taking a closer look at `timestamp`, this is how the return value is formed:
  ```javascript
  full ? Math.floor(timestamp) : Math.floor(timestamp / 1000)
  ```

  We're getting a shorter return value for the second call because the `full` argument is undefined.

  **First call**, create_auth_token:
  ```bash
  full:true, return val: 1563035255103
  ```

  **Second call**, `database.connection.query` line 260:
  ```bash
  full:undefined, return val: 1563035255
  ```
  ## timestamp
  ```javascript
  // Generate timestamp: if full argument is false/undefined,
  // timestamp is divided by 1000 to generate linux-length timestamp
  function timestamp(full) {
      let date = new Date();
      let timestamp = date.getTime();
      return full ? Math.floor(timestamp) : Math.floor(timestamp / 1000);
  }
  ```

  Looking at the comments for the `timestamp()` function led me to change the `full` argument from `undefined` to `true` in `database.connection.query` on line 260:
  ```javascript
  database.connection.query("INSERT INTO session ( `user_id`, `timestamp`, `token`) VALUES( '" + payload.id + "', '" + timestamp(true) + "', '" + token + "')",
  ```

  Now it works, but I have a new error.

  I'm not sure why it was `undefined` before. Maybe it was correct, and I should have changed the timestamp to be a linux-length timestamp in the database instead? But I don't know what that means.

  ## New Error

  ```bash
  ER_DUP_ENTRY: Duplicate entry 'undefined' for key 'PRIMARY'
  ```

  My new error has to do with the primary key.


  ## See Records in Sequel Pro
  `cmd` + `2`, from *[Content View](https://sequelpro.com/docs/ref/core-features/content)*.

  ## Tomorrow
  Tomorrow, I want to think more about this: 
  - Should I change the data type of `timestamp` in the `session` table to a a "linux-length timestamp"? 
  - Or should I keep my changes I made today: changing the parameter of the `timestamp` function from `undefined` to `true`?

  I also need to look at my primary key error.

## Day 93, R2
### 7/12/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ### A reminder of what we're trying to do:

  The [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) finished files ***don't work***. They don't call the function that creates the session and token, `action_create_session`. 
  
  Instructions on how to call `action_create_session` were absent from the book. So we're trying to figure out where and when to call this function so we can get the code working.

  ## Chain Of Events
  Here's our chain of events we figured out yesterday:

  ![](log_imgs/diagram_7-12.PNG)

  ## Creating A Session

  Yesterday, I thought that in order to create a session, we need to add a method on the `User` class (*line 10, index.html*) that fetches `api/create/session`. This would call the function `action_create_session`.

  ```javascript
  //  line 392, api.js
  if (identify("session", "create")) // Create session
      Action.create_session( request, json( request.chunks ) )
      .then( content => respond(response, content) );
  ```
  ```javascript
  // line 233, api.js
  function action_create_session( request, payload ) {
    // Create unique authentication token
    function create_auth_token() {
      let token = md5( timestamp( true ) + "");
        return token;
    } 
    //... more code
  ```

  However, I looked at this diagram in [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087):

  ![](log_imgs/bookdiagram_7-12.PNG)

  Here, the session is created after the user logs in. So I think `action_create_session` has to be traced to the `action_login` call.

  ## Questions
  
  ### When should we call `action_create_session`?

  - Do we first call `action_get_session`? 
    ### `action_get_session` is also never called anywhere in the finished files. This looks like a mistake. When do we call it?
    - Do we call it to see if there's a session, and then if there isn't we call `action_create_session`?
      - No, I don't think so. I think the answer is in the diagram.
        ![](log_imgs/sessiondiagram_7-12.PNG)
        - 1\. We figure out if the session exists
        - 2\. **If yes**: `action_get_session()`, then we somehow get the the AuthToken (what function does "Get AuthToken" refer to? `action_authenticate_user`?). 
       
              **If no**: `action_create_session()`, which calls `create_auth_token()`.
        - In the diagram this all happens after we "**Check if encrypted passwords match**", so 
          ### Do we put an if statement in the login to see if the session exists? How do we query from the javascript to mysql to figure that out?

  ## Calling `action_create_session()`
  I called `action_create_session(request, payload)` inside `action` login on line 218: 
  ```javascript
  // line 218, api.js
  action_create_session(request, payload); //add session--should be conditional but just adding for now, Dash
  ```

  But I got an error:
  ```bash
  Error: ER_TRUNCATED_WRONG_VALUE: Incorrect datetime value: '1562952357' for column 'timestamp' at row 1
  ```

  That's we're I'll leave off.
  
  

## Day 92, R2
### 7/11/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## `catchAPIrequest(request)` Trace

  I logged the trace to see when `catchAPIrequest(request)` is called.

  ```javascript
  // line 405, api.js
  static catchAPIrequest(request) {
    console.log("catchAPIrequest");
    console.trace();
    ...
  /*  catchAPIrequest
      Trace
          at Function.catchAPIrequest (/Users/dashiellbark-huss/Documents/100daysofcode/node-master/module/api/api.js:407:15)
          at ReadFileContext.callback (/Users/dashiellbark-huss/Documents/100daysofcode/node-master/index.js:34:25)
          at FSReqCallback.readFileAfterOpen [as oncomplete] (fs.js:230:13)
  */
  ```

  Some `fs` function calls `fs.readFile()`, which calls `catchAPIrequest(request)`.
  ```javascript
  //line 31, index.js
  fs.readFile(filename, function (error, content) {
      if (error) {
          if (error.code == 'ENOENT') {
              if (API.catchAPIrequest( request.url ))
                  API.exec(request, response);
  ...
  ```

  ## `http.createServer(requestListener);`
  `fs.readFile()` gets called in the `http.createServer()` callback. This callback is called the requestListener function.
  >The HTTP Server object can listen to ports on your computer and execute a function, a requestListener, each time a request is made.


  -*[Node.js http.createServer() Method](https://www.w3schools.com/nodejs/met_http_createserver.asp)*

  ### requestListener:
  >Specifies a function to be executed every time the server gets a request. This function is called a requestListener, and handles request from the user, as well as response back to the user.
  
  -*[Node.js http.createServer() Method](https://www.w3schools.com/nodejs/met_http_createserver.asp)*

  So every time the server gets a request, these functions are executed: the requestListener, `fs.readFile()`, and then conditionally `API.catchAPIrequest( request.url )`.

  ## Chain Of Events: Login
  Now I understand the chain of events. Here is how it works with logging in for example.

  ### UI Calls User.Login
  ```html
  <!--line 161, index.html-->
  <input type="button" onclick="User.login({username:'felix',password:'password'})"
  value="log in as user felix / password (check session)" /><br />
  ```
  **The function call in the above button:** `User.login({username:'felix',password:'password'})`

  ### User.login Makes A Fetch Request
  ```javascript
  // line 70, index.html
  /* Login and create user session (if credentials match)
    payload = {
    username: "username",
    password: "password" } */
  User.login = function(payload) {
      fetch("/api/user/login", make(payload)).then(promise => promise.json()).then(json => {
          console.log(json);
      });
  }
  ```

  Fetch makes a request using the relative path `"/api/user/login"`. 
  
  *I'm not totally sure about this part but I think*: This isn't an actually path so we have to use this info to conditionally call different parts of the API. We'll look at that later.

  ### The HTTP server object hears the request
  The HTTP server object, created by http.createServer(), listens to ports on your computer. When a request is made it executes the requestListener callback:

  **Syntax**:
  ```javascript
  http.createServer(requestListener)
  ```

  ### requestListener is executed

  ```javascript
  http.createServer(function(request, response) { //the requestListener
    // skipping code here ...
    fs.readFile(filename, function (error, content) { 
    // skipping code here ... 
      if (API.catchAPIrequest( request.url ))
                      API.exec(request, response);
      // ...
  ```
  From here we get to `API.catchAPIrequest( request.url )`.

  ### `API.catchAPIrequest( request.url )` is executed

  >Static function API.catchAPIrequest will return true if the request.url matches our API pattern. We will cancel serving the file as usual in this case.
  >
  >The static API.parts property stores the parts of the API call in an array. For example localhost:3000/api/user/get becomes: ["api", "user", "get"]. Based on these values we can tell our server what to do (execute MySQL query, etc.)

  -*[Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).*

  ```javascript
  // line 405, api.js
  static catchAPIrequest(request) {
    request[0] == "/" ? request = request.substring(1, request.length) : null;
    if (request.constructor === String)
        if (request.split("/")[0] == "api") {
            API.parts = request.split("/");
            return true;
        }
    return false;
  }
  ```
  `catchAPIrequest(request)` does two things:
  - Stores the parts of the request in API.parts
  - returns true if the first part of the fetch resource is `api`

  If `catchAPIrequest(request)` returns true, `API.exec(request, response)` gets executed.

  ### `API.exec(request, response)` is executed

  ```javascript
  // line 348, api.js
  static exec( request, response ) {
    ...
  ```
  Inside `exec` we start reading the POST data chunks:

  ```Javascript
  //line 358, api.js
  // Start reading POST data chunks
  request.on('data', segment => {
      if (segment.length > 1e6) // 413 = "Request Entity Too Large"
          response.writeHead(413, {'Content-Type': 'text/plain'}).end();
      else
          request.chunks.push(segment);
  });
  ```

  Then we finish reading the POST data:
  ```javascript
  // line 366, api,js
  // Finish reading POST data chunks
  request.on('end', () => { // POST data fully received

      API.parts = request.parts;

      if (identify("user", "register")) // Register (create) user
          Action.register_user( request, json( request.chunks ) )
          .then( content => respond(response, content) );
                      
      if (identify("user", "login")) // Log in
          Action.login( request, json( request.chunks ) )
          .then( content => respond(response, content) );
      // ...
  ```
  ### `identify("user", "login")` returns true
  `request.on('end')` check to see if `identify()` returns true for a bunch of different values. Here's `identify`:

    ```javascript
  //line 310, api.js
  function identify(a, b) { 
  console.log("identify");
    return API.parts[0] == "api" && API.parts[1] == a && API.parts[2] == b;
    }
  ```

  We're logging in, so `identify()` will return true in the if-statement with the params `identify("user", "login")`.

  ```javascript
  // line 375, api,js
  if (identify("user", "login")) // Log in
      Action.login( request, json( request.chunks ) )
      .then( content => respond(response, content) );
  ```
  Then `Action.login` is executed.


  ### `Action.login( request, json( request.chunks ) )` is executed
  Action.login is action_login:
  ```javascript
  // line 333, api.js
  Action.login = action_login;
  ```

  ```javascript
  // line 196, api.js
  function action_login ( request, payload ) {
    return new Promise((resolve, reject) => {
        // First, get the user from database by payload.id
        let query = `SELECT * FROM \`user\` WHERE \`username\` = '${payload.username}'`;
        console.log(query);
        // ...
  ```
  This function makes the necessary query.

  ## Creating A Session

  Now that we know the chain of events, we can figure out hot to link up `action_create_session` to the chain.


  ```javascript
  // line 233, api.js
  function action_create_session( request, payload ) {
    // Create unique authentication token
    function create_auth_token() {
      let token = md5( timestamp( true ) + "");
        return token;
    } 
    //... more code
  ```
  ## Session Path
  From looking at the code, I can see that the path to create a session needs to be `api/create/session`



  ```javascript
  //  line 392, api.js
  if (identify("session", "create")) // Create session
      Action.create_session( request, json( request.chunks ) )
      .then( content => respond(response, content) );
  ```

  ### However, in the frontend API *(index.html, on line 10)* there is no function that makes this fetch request.

  This is where the problem is!

  Tomorrow, I will make this function.

## Day 91, R2
### 7/10/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## Is `http.IncomingMessage` An Instance Of `EventEmitter`?

  Yesterday, I wanted to know if `request` was an instance of `EventEmitter` because It had the `.on()` method.

  >The on method binds an event to a object.

  -*[In node.js “request.on” what is it this “.on”](https://stackoverflow.com/questions/12892717/in-node-js-request-on-what-is-it-this-on)*

  >All objects that emit events are instances of the `EventEmitter` class.

  -*[Events](https://nodejs.org/api/events.html#events_events)*

  `request` is the identifier for an `http.IncomingMessage` object. So we need to know if `http.IncomingMessage` is an instance of `EventEmitter`.

  ## Tracing The Prototype Chain
  I used `Object.getPrototypeOf()` and `.__proto__` to trace the prototype chain of `http.IncomingMessage`. More on that here: [Tracing Javascript’s Prototype Chain](https://alanstorm.com/tracing-javascripts-prototype-chain/)

  The first object that `http.IncomingMessage` inherits from is `Readable`:
  ```javascript
  console.log('proto', Object.getPrototypeOf(http.IncomingMessage));
  /*  proto [Function: Readable] {
      ReadableState: [Function: ReadableState],
      _fromList: [Function: fromList],
      from: [Function]
    }  */
  ```

  If you trace back the inheritance chain, you find that yes, **`http.IncomingMessage` does inherit from `EventEmitter`.**

  ```javascript
  console.log('proto', Object.getPrototypeOf(http.IncomingMessage).__proto__.__proto__);
  /*  proto [Function: EventEmitter] {
      once: [Function: once],
      EventEmitter: [Circular],
      usingDomains: false,
      defaultMaxListeners: [Getter/Setter],
      init: [Function],
      listenerCount: [Function]
    }  */
  ```

  ## `http.IncomingMessage` inherits from `EventEmitter`
  This also means `request`, which is an `http.IncomingMessage` object, is an instance of `EventEmitter`.

  So now I know that `request.on()` is the `emitter.on()` method and not some other "on" method.

  ## Clarify Questions
  I looked back at my questions from yesterday to get back to what I was trying to figure out. I now wonder:

  ## How is the UI making requests?
  Let's look at the login button:

  ```html
  <!--line 161, index.html-->
  <input type="button" onclick="User.login({username:'felix',password:'password'})"
  value="log in as user felix / password (check session)" /><br />
  ```

  **The function call in the above button:** `User.login({username:'felix',password:'password'})`

  ### User.login:
  ```javascript
  // line 70, index.html
  /* Login and create user session (if credentials match)
    payload = {
    username: "username",
    password: "password" } */
  User.login = function(payload) {
      fetch("/api/user/login", make(payload)).then(promise => promise.json()).then(json => {
          console.log(json);
      });
  }
  ```

  I'm totally sure how the URL `"/api/user/login"` works.

  I think it has something to do with this method in the API class:
  ```javascript
  // line 405, api.js
  static catchAPIrequest(request) {
    request[0] == "/" ? request = request.substring(1, request.length) : null;
    if (request.constructor === String)
        if (request.split("/")[0] == "api") {
            API.parts = request.split("/");
            return true;
        }
    return false;
  }
  ```
  When is `catchAPIrequest(request)` called?

  I logged the trace to see.

  ```javascript
  // line 405, api.js
  static catchAPIrequest(request) {
    console.log("catchAPIrequest");
    console.trace();
    ...
  /*  catchAPIrequest
      Trace
          at Function.catchAPIrequest (/Users/dashiellbark-huss/Documents/100daysofcode/node-master/module/api/api.js:407:15)
          at ReadFileContext.callback (/Users/dashiellbark-huss/Documents/100daysofcode/node-master/index.js:34:25)
          at FSReqCallback.readFileAfterOpen [as oncomplete] (fs.js:230:13)
  */
  ```

    

  

## Day 90, R2
### 7/9/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## API Instance Of Class?
  Yesterday when I logged `API`, I wonder if the result was the API call or an instance of a class?
  
  I think it's the class. Because it's called 'API' and it logs `class API{...`. What confuses me is that `API.parts`changes. So it seems like it should be different instances of the API class.

  I looked over some things. It is logging the class. Inside the class, API.parts changes on line 369 with every new request.

  ```javascript
  // api.js line:369
  API.parts = request.parts;
  ```

  ## Clarify Questions
  I need to clarify what I'm trying to figure out so I know what to do next:

  ### Where does or where *should* `action_create_session` get called?
  - `request.on('end')`*(line 367, api.js)* conditionally calls `action_create_session`*(233)* if `identify("session", "create")`*(395, 310)* returns `true`
  
  ### So when is `request.on('end')` called and `identify("session", "create")` `true`?
  - ### `identify("session", "create")` is `true`
    - `identify("session", "create")` is `true` when the `API.parts` array is `["api", "session", "create"]`
  
      ```javascript
      // line 314 api.js
      return API.parts[0] == "api" && API.parts[1] == a && API.parts[2] == b;
      ```

    - `API.parts` is the same as `request.parts`
      ```javascript
      // line 369 api.js
      API.parts = request.parts;
      ```
  - ## `request.on('end')` gets called
    - I'm pretty sure it gets called when the http request gets sent back. I'm not totally sure how that works.
    - `request` is actually a parameter of  `exec()`
      ```javascript
      // line 348, api.js
      static exec( request, response ) {
      ```
  ### Where is `exec()` called and what is the argument for `request`?

  - `exec()` is called inside the `http.createServer` callback:

  - ```javascript
    // line 32, index.js
    API.exec(request, response);
    ```
    `createServer` and the `callback` are here:
    ```javascript
    //line 1 index.js
    let http = require('http');
    // line 16,index.js
    http.createServer(function(request, response) {
    ...
    ```
    The argument for request is the http request.

  ### Still not sure: when does `request.on('end')` get called?

  ## `request.on()`

  >The on method binds an event to a object.
  >
  >It is a way to express your intent if there is something happening (data sent or error in your case) , then execute the function added as a parameter.

  -*[In node.js “request.on” what is it this “.on”](https://stackoverflow.com/questions/12892717/in-node-js-request-on-what-is-it-this-on)*

  More about `.on` here: [emitter.on(eventName, listener)](https://nodejs.org/api/events.html#events_emitter_on_eventname_listener)

  ## EventEmitter

  >Much of the Node.js core API is built around an idiomatic asynchronous event-driven architecture in which certain kinds of objects (called "emitters") emit named events that cause Function objects ("listeners") to be called.

  >All objects that emit events are instances of the EventEmitter class. These objects expose an eventEmitter.on() function that allows one or more functions to be attached to named events emitted by the object. 

  -*[Events](https://nodejs.org/api/events.html#events_events)*



  Is `request` a kind of EventEmitter? Can I log it to find out?

  ## IncomingMessage Object
  `request` is an IncomingMessage object.

  >The IncomingMessage object is passed as the first argument in the requestListener function:

  -*[Node.js IncomingMessage Object](https://www.w3schools.com/nodejs/obj_http_incomingmessage.asp)*

  Is the IncomingMessage object  a kind of EventEmitter?

  According to [this page](http://www.acuriousanimal.com/2015/08/31/how-to-read-from-a-writable-stream-httpserverresponse-in-node.html), http.IncomingMessage is a readable stream. I'm not sure if it still can't inherit from EventEmitter though.

  ## Prototype Of An Object In Node?
  In DevTools I can `console.dir()` to find out an object's prototype. But in node when I `console.dir(http.IncomingMessage);` I just get `[Function: IncomingMessage]`.

    
    

  
## Day 89, R2
### 7/8/19

- ## Twitch
  I played with streaming on twitch. It slowed down my computer so much. I explained my AR project
  
  I decided not to twitch my node project because OBS was slowing my computer down. My computer's been acting weird. I should get it taken in. I should back it up first and then go to the Mac Store this week.


- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## Tracing Where `action_create_session` Gets Called
  Yesterday, I left off trying to find where `action_create_session` get's called or if it's even getting called.

  ## The function itself in api.js on line 233:
  ```javascript
  //line 233, api.js
  function action_create_session( request, payload ) { 
    // Create unique authentication token
    function create_auth_token() {
        let token = md5( timestamp( true ) + "");
        return token;
    } //...
  ```
  ## A reference to the function in api.js on line 336:
  ```javascript
  Action.create_session = action_create_session;
  ```

  ## The reference getting called conditionally in api.js on line 392:
  ```javascript
  //line 392, api.js
  if (identify("session", "create")) // Create session 
    Action.create_session( request, json( request.chunks ) )
    .then( content => respond(response, content) );
  ```
  ## Where Does `identify()` Get Called?
  I added a log statement to the `identify()` function.
  
  ```javascript
  //line 310, api.js
  function identify(a, b) { 
  console.log("identify");
    return API.parts[0] == "api" && API.parts[1] == a && API.parts[2] == b;
    }
  ```

  I thought it *wasn't* getting logged because I was looking at the *browser console*. It was instead getting logged in the *terminal*.

  ## Git Repo
  I broke my code, but pressed 'undo' until it worked again.

  I need to version control this because if I screw it up and can't get it back, that would be bad. 

  I forked javascript teachers repo, and then added that as the remote for my local repo:
  
  [Adding a remote](https://help.github.com/en/articles/adding-a-remote)

  ## Logging
  I'm so acclimated to DevTools that I forgot the amount of debugging you could do with log statements. So I playing with logging and started to understand a lot more of the code.

  ### Comments are the logs when pressing **authenticate**:
  ```javascript
  // line 309, api.js"
  function identify(a, b) {
    console.log(`API: ${API}`); //returned the entire API class from line 345, api.js
    console.log(`a: ${a}, b:${b}`); //a: user, b:authenticate
    console.log(`identify, line 3:11, API.parts: ${API.parts}`);//identify, line 3:11, API.parts: api,user,login
      return API.parts[0] == "api" && API.parts[1] == a && API.parts[2] == b;
  }
  ```

## Day 88, R2
### 7/7/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## Token Error
  After pressing **"login as felix"** I tested the **"authenticate (check if token exists in session table)"** and I got this error:

  ```bash
  request  /api/user/authenticate
  API.exec(), parts =  [ 'api', 'user', 'authenticate' ]
  API.authenticate, results.length == 0 (session with token not found)
  {"success": false, "message": "token not found in session"}
  responding =  [ undefined ]
  ```

  In the console:
  ![](log_imgs/consoletoken_7-7.PNG)

  Looking in the database there is no token:
  ![](log_imgs/token_7-7.PNG)
  


  ## Test Token?
  I noticed that in index.html, the **"authenticate (check if token exists in session table)"** button sends an object with a test token to User.authenticate.

  ```html
  <input type="button" onclick="User.authenticate({token:'token_test_12345'})"
  value="authenticate (check if token exists in session table)" /><br />
  ```

  But how is this test token supposed to be in our session table? It was never put there.

  ## When is The Token Added?

  Reading these comments, it looks like the function `User.update()` in index.html is supposed to add the token:
  ```javascript
  /* Update user table in database
      payload = {
      id: 1,
      token: "ABC123xyz" } */
  User.update = function(payload) {
      fetch("/api/user/update", make(payload)).then(promise => promise.json()).then(json => {
          console.log(json);
      });
  }
  ```

  User update gets called in the button **"update user properties"**:

  ![](log_imgs/update_7-7.PNG)

  But in the code the payload ***doesn**'t contain `token`*:
  ```html
  <input type="button" onclick="User.update({id: 1, password_sha3: 'newpass123A'})"
  value="update user properties" /><br />
  ```

  It just sends the `id` and `password_sha3`. Where's `token`?
  ```javascript
  User.update({id: 1, password_sha3: 'newpass123A'})
  ```

  `password_sha3` isn't even a field anywhere in our database. The code used an md5 hash. So I had added `password_md5` to the table. Are we supposed to have both `password_md5` and `password_sha3`?

  Looking through the code, `password_sha3` is never used. So I think sending `password_sha3` in the payload is a mistake. The payload shouldn't have `password_sha3`. But which should it have: `password_md5` or `token`?

  ## Update User
  
  I changed `password_sha3: 'newpass123A'` to `token:'token_test_12345'`:
  ```html
  <input type="button" onclick="User.update({id: 1, token:'token_test_12345'}" value="update user properties" /><br />
  ```
  But I still get an error:
  ```
  request  /api/user/authenticate
  API.exec(), parts =  [ 'api', 'user', 'authenticate' ]
  API.authenticate, results.length == 0 (session with token not found)
  {"success": false, "message": "token not found in session"}
  responding =  [ undefined ]
  ```

  I console logged the query to see what's actually going on when we update the user:
  ```sql
  UPDATE user SET `id` = '1' WHERE `id` = '1'
  ```

  Where does it add the token? It's only updating `id` but it's just setting it to the same id?

  I changed `token:'token_test_12345'` back to `password_sha3: 'newpass123A'`.

  ## More About Update
  ```sql
  UPDATE table_name
  SET column1 = value1, column2 = value2, ...
  WHERE condition;
  ```
  -*[SQL UPDATE Statement](https://www.w3schools.com/sql/sql_update.asp)*

  ## Clarify Questions
  I'm confused, so I'm going to clarify my questions to get a better idea of where to go next.

  ### Questions:

  - ### What part of the code is supposed to be adding the token?
    - `action_update_user ( request, payload )` or `action_create_session( request, payload )`?
    - It looks like it's `action_create_session( request, payload )` but I don't see anything in the UI that calls this function. That leads me to the question:
      - ### What part of our code calls `action_create_session( request, payload )`?
  - ### Why does `User.update()` set the `id = '1'` to `id = '1'`? 
    - Possible answer: if there were more users it would change the id?

  ## Tracing Where Create Session Gets Called
  The function itself in api.js on line 233:
  ```javascript
  function action_create_session( request, payload ) {
    // Create unique authentication token
    function create_auth_token() {
        let token = md5( timestamp( true ) + "");
        return token;
    } //...
  ```
  A reference to the function in api.js on line 336:
  ```javascript
  Action.create_session = action_create_session;
  ```

  The reference getting called conditionally in api.js on line 392:
  ```javascript
  if (identify("session", "create")) // Create session
    Action.create_session( request, json( request.chunks ) )
    .then( content => respond(response, content) );
  ```
  This is where I left off.
  

- ## Thoughts And Feelings:
  I am super sleep deprived from the drive over to Chicago, but I still did ok with my studying. Now that I'm at home, I'm going to set up a second monitor. I think that will help me.

  After listing out my questions, I realized how confusions happens. It happens when I have many questions that depend on the answers of other questions. Nested questions.

  I coded in the dining room and despite constantly requesting that my family not interupt my study, they kept interrupting me. Example:
  
    >Dad: Did you hear about the dish washing machine?
    >
    >Me: Dad, I really need to concentrate. But no I didn't, you can tell me but then please let me focus.
    >
    >Dad: Oh ok. It broke and we had to get a new one.
    >
    >Me: OK, now can you let me focus. I really want to get this done.
    >
    >Dad: Ok. 
    >
    >Two seconds later(not an exaggeration)
    >
    >Dad: \*\****whispers***** *Do you need an extra phone charger? I'm not using this one.* \*\****whispers*****
    >
    >Me: Dad, please I'm trying to focus!

    And this went on and on. Basically:
    ```javascript
    while (Dashie.IsInDiningRoom){
      family.interrupt(Dashie);
    }
    ```
    So I decided to just go upstairs away from everybody. That's what I'll have to do in the future.

    I got stuck again today, but I tried to remember that getting stuck is par for the course. I shouldn't let it stress me. When I get stuck I just need pivot to a new area to look at: either closer in to the details, or further out to a broader idea. I will learn more concepts even when I'm stuck. All day I was stuck, but I still learned so much.


## Day 87, R2
### 7/6/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## How Tokens & Sessions Work:
  Marco([@Wridgeu](https://twitter.com/Wridgeu)) helped me understand tokens and sessions by sending me these graphics.

  ## Tokens:
  ![](log_imgs/tokens_7-6.jpg)

  ## Sessions:
  ![](log_imgs/tokens2_7-6.png)
  
  ## Delete A Table
  To delete a table:
  ```sql
  DROP TABLE tablename;
  ```
  -*[How to Manage MySQL Database, Table & User From Command Line:DELETE MYSQL TABLES AND DATABASES](https://www.a2hosting.com/kb/developer-corner/mysql/managing-mysql-databases-and-users-from-the-command-line#DeleteMySQL-Tables-and-Databases#DeleteMySQL-Tables-and-Databases#DeleteMySQL-Tables-and-Databases)*

  Remember to replace `tablename` with your table. Go to the correct database first with the command: `use databasename`.

  ## Making the Session Table
  I'm going to try to make the `session` table with the little information I have. So I deleted the table using the above code.

  A few days ago, I found this in api.js.
   ```javascript
  database.connection.query("INSERT INTO session ( `user_id`, `timestamp`, `token`) VALUES( '" + payload.id + "', '" + timestamp() + "', '" + token + "')",
  ```
  From this bit of code, I gather that these are the fields that need to be in the `session` table: `user_id`, `timestamp`, and `token`.

  Copying this sql command I found in [Storing Sessions in a Database](http://shiflett.org/articles/storing-sessions-in-a-database):

  ```sql
  CREATE TABLE sessions (
  id varchar(32) NOT NULL,
  access int(10) unsigned,
  data text,
  PRIMARY KEY (id)
  );
  ```

  I came up with this command to make the session table and set the primary key to `user_id`.
  ```sql
  CREATE TABLE session (
  user_id varchar(32) NOT NULL,
  PRIMARY KEY (user_id)
  );
  ```

  And I'm going to add the other fields, `timestamp` and `token`, in Sequel Pro.

  ## Token Data Type?
  How is the token made and what is the data type?

  I found this in api.js:
  ```javascript
  function create_auth_token() {
    let token = md5( timestamp( true ) + "");
    return token;
  }
  ```
  So I think I need to look at what data type an **md5** token is.

  ## md5
  A few days ago I found that the `password_md5` should be:

  >MD5 generates a 128-bit hash value. You can use CHAR(32) or BINARY(16)

  -*[What data type to use for hashed password field and what length?](https://stackoverflow.com/questions/247304/what-data-type-to-use-for-hashed-password-field-and-what-length)*

  So I'll use CHAR and length: 32 in the table for `token`.

  ## Timestamp
  For `timestamp` I use the data type TIMESTAMP and set the default:
  >To assign the current timestamp, set the column to CURRENT_TIMESTAMP or a synonym such as NOW().

  -*[5.1.8 Server System Variables](https://dev.mysql.com/doc/refman/8.0/en/server-system-variables.html)*

  ## New Session Table
  Here's the new session table:

  ![screenshot](log_imgs/timestamp_7-6.PNG)

  ## Working, Sort Of!

  With the new session table, we still have an error but it's a new error and the server doesn't shut down like it did before:

  ```bash
  request  /
  API request detecting...
  request  /style/lavacode.css
  request  /favicon.ico
  request  /style/lavacode.css
  request  /api/user/authenticate
  API.exec(), parts =  [ 'api', 'user', 'authenticate' ]
  API.authenticate, results.length == 0 (session with token not found)
  {"success": false, "message": "token not found in session"}
  responding =  [ undefined ]
  ```

  This is to be expected because we haven't logged in yet, so there is no session. Let's tackle logging in.

  ## Logging In
  We need to login to create a session.

  ![screenshot](log_imgs/login_7-6.PNG)

  In the code, I actually changed `felis` it to `felix2` because that's the login I'm using.

  Pressing **log in as user felix** gave me this in the terminal:

  ```bash
  request  /api/user/login
  API.exec(), parts =  [ 'api', 'user', 'login' ]
  SELECT * FROM `user` WHERE `username` = 'felix2'
  responding =  [
    '{"success": false, "user": null, "message": ' +
      `"user with this username(felix2) doesn't ` +
      'exist"}'
  ]
  ```
  Console:
  **"user with this username(felix2) doesn't exist"** Hmmm...

  This made me think: `felix2` is a user in `user` table in the `mysql` database not the `user` table that we made in a later chapter for the `myserver` database. I'm confused!

  ## Retrieving Information from a Table
  ```sql
  SELECT what_to_select
  FROM which_table
  WHERE conditions_to_satisfy;
  ```
  -*[3.3.4 Retrieving Information from a Table](https://dev.mysql.com/doc/refman/8.0/en/retrieving-data.html)*

  ## No Felix User
  I found out there is no user `felix` or `felix2` in the `myserver` database. 
  
  I'm not totally sure why, but I have `felix2` and the original `felix` in the `user` database on the `mysql` database, and this `felix` should be in the `user` table on `myserver`. I'm not sure if it's because something was left out in the book or if I did something wrong.
  
   Are there supposed to be two separate `felix` logins, or did I do something wrong by making two `user` tables in different databases? Not sure. I'll look back at the book tomorrow.
   
   Maybe this was why `felix@localhost` didn't work and I had to use `felix@%`. Not really sure.

  <img src="log_imgs/username_7-6.PNG" width="400"/>

  No `felix`.

  ## Add Felix User

  I added `felix` through the UI:

  <img src="log_imgs/felix_7-6.PNG" width="400"/>

  I changed the code from `felix2` to `felix` pressed **log in as user felix**. Success:

  <img src="log_imgs/success_7-6.PNG" width="500"/>

    
    

## Day 86, R2
### 7/5/19

- ## Play Around
  Driving through desolate areas. Internet sucks. Putting pause on backend to just play with frontend since I don't have the internet. I feel rusty with frontend, anyways.

  Found some pictures of my sister I took several years ago. Going to play with those.

  So hot in the van. Sweating. No internet. Could only using my Dash Docs and my log notes. Taking a break to cool off and charge computer.
  
  ## Back To Coding
  Back to coding. I got some internet. The temperature cooled a little but still hot. 32 minutes left.

  ## Passing Params To Callback
  [Javascript event handler with parameters](https://stackoverflow.com/questions/10000083/javascript-event-handler-with-parameters)

  >An arrow function expression is a syntactically compact alternative to a regular function expression, although without its own bindings to the this, arguments, super, or new.target keywords.
  >
  >```javascript
  >const event_handler = (event, arg) => console.log(event, arg);
  >el.addEventListener('click', (event) => event_handler(event, 'An argument'));
  >```

  ## Made A little Something
  Here's a little something I made with the pictures of my sister. Her name is Tallulah and she is super fun.

  ![screenshot](log_imgs/tallulah_7-5.gif)

  ```html
  <!DOCTYPE html>
  <html lang="en">

  <head>
    <meta charset="UTF-8">
    <meta name="viewport">
    <meta http-equiv="X-UA-Compatible" content="ie=edge">
    <title>Document</title>
    <style>
      html,
      body {
        box-sizing: border-box;
        margin: 0;
        padding: 0;
        height: 100%;
      }

      *,
      *:before,
      *:after {
        box-sizing: inherit;
      }

      #img {
        height: 100%;
      }

      #image {
        height: 100%;
      }
    </style>
  </head>

  <body>
    <div id="img">

    </div>
    <script>
      const imageArray = ['lula1.png', 'lula2.png'];
      const colorArray = ['pink', 'lightblue', 'lightyellow'];
      const image = document.createElement("img");
      const imageWrapper = document.querySelector("#img");

      var i = 0;
      var colorI = 0;

      function putImage() {
        const img = imageArray[i];
        i === imageArray.length - 1 ? i = 0 : i++;
        image.setAttribute('src', `images/${img}`);
        image.setAttribute('id', `image`);
        imageWrapper.innerHTML = "";
        imageWrapper.appendChild(image);
        color();
      }

      function color() {
        const color = colorArray[colorI];
        colorI === colorArray.length - 1 ? colorI = 0 : colorI++;
        document.getElementsByTagName('body')[0].style.backgroundColor = color;
      }


      putImage();
      setInterval(putImage, 1600);
      document.addEventListener('click', color);
    </script>
  </body>
  </html>
  ```

- ## Thoughts And Feelings
  Still so hot and hard to concentrate. Today, we drove all day in our van. We're trying to get home to Chicago before my grandma passes. 
  
  This is probably the end of our 3 years living in the van.
  
  After today, I'm excited to code in some air conditioning!

## Day 85, R2
### 7/4/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## Session Table
  Yesterday, I got an error when pressing the **"authenticate (check if token exists in session table)" button**.
  ```bash
  Error: ER_BAD_FIELD_ERROR: Unknown column 'token' in 'where clause'
  ```
  And of course, I just realized there is no column `token` anywhere in my database. But what is `where clause`?

  It must be something like, "find token where some condition is met."

  I found this in api.js. It looks like this is where we are taking the token and puting it in the session table. But I'm not so familiar with SQL. Are `user_id`, `timestamp`, and `token` the fields in our code?
  ```javascript
  database.connection.query("INSERT INTO session ( `user_id`, `timestamp`, `token`) VALUES( '" + payload.id + "', '" + timestamp() + "', '" + token + "')",
  ```

  I just realized, sometimes people have some file in their code that creates the database tables. At least I remember that when I was working with Ruby on Rails, Maybe if I find that file in here it will show how the database should be set up? In Ruby on Rails I think it was called the `schema` file.

  I couldn't find anything about a file like this.

  There's way too much missing in this book about how to deal with sessions for a newbie like myself. Maybe I need to watch a different tutorial.

  ## Reaching Out For Help
  This feels like a point where I need to reach out for help. I posted on the patreon page for [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087)

  ![patreon post](log_imgs/patreonq_7-4.PNG)

  I also reached out to someone who posted on twitter that they were working on the book.

  ![twitter message](log_imgs/twitterq_7-4.PNG)
    
  I contacted a few more people who I saw downloaded the book off patreon.
  
  ## Searching For Info
  I searched blogs and videos about authentication, sessions, and tokens. None are worth sharing. 
  
  A lot of the tutorials used libraries and were very specific. I couldn't find much on the general concept of authentication using sessions and tokens. In fact, a lot of resources said that these concepts are mutually exclusive: You need to use either sessions or tokens, but not both together.

  Hopefully, I'll have better luck tomorrow. My internet and computer was slow today so it was getting in my way.


## Day 84, R2
### 7/3/19
- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## MD5 Password Hash

  What's the data type for the MD5 hash?

  >MD5 generates a 128-bit hash value. You can use CHAR(32) or BINARY(16)

  -*[What data type to use for hashed password field and what length?](https://stackoverflow.com/questions/247304/what-data-type-to-use-for-hashed-password-field-and-what-length)*

  So I'm using type: CHAR and length: 32 in the table.

  ## Debugging Backend 
  How???

  ## Search All Files VSC
  
  >VS Code allows you to quickly search over all files in the currently opened folder. Press Ctrl+Shift+F and enter your search term.
  
  -*[Basic Editing in Visual Studio Code](https://code.visualstudio.com/docs/editor/codebasics)*

  `Ctrl`+`Shift`+`F`

  ## Error

  ```bash
  Error: ER_BAD_FIELD_ERROR: Unknown column 'first_name' in 'field list'
  ```
  There's no `first_name` in my table. The book didn't tell us to include it. 
  
  I searched 'first_name' in the directory for the finished files that I downloaded from github for the book.

  I found this:
  ```javascript
  let fields = "( `username`, `email_address`, `password_md5`, `first_name`, `last_name`, `avatar` )";
  ```
  So I assume we'll need all these fields, many of which are left out from the instructions in the book.

  ## Database Terms
  Knowing these terms helps me search for answers.

  [Database Terms - and their Meanings](http://www.bin-co.com/database/sql_tutorial/db_terms_meanings.php)
  
  ## Image Datatype

  ### blob:
  >MySQL has a blob data type which can used to store binary data. A blob is a collection of binary data stored as a single entity in a database management system. Blobs are typically images, audio or other multimedia blob objects.

  [How to Store an image in MySQL database](https://www.daniweb.com/programming/databases/threads/439533/how-to-store-an-image-in-mysql-database)

  ## Corrected User Table

  I had to add the fields `username`, `password_md5`, `first_name`, `last_name`, `avatar` for the **"register user" button** in the UI to work.
  
  ![table](log_imgs/table_7-3.PNG).

  ## No Session Table in Book
  The **"authenticate (check if token exists in session table)" button** doesn't work.

  ```bash
  Error: ER_NO_SUCH_TABLE: Table 'myserver.session' doesn't exist
  ```

  The book left out adding a session table. 
  
  I found this:

  [Storing Sessions in a Database](http://shiflett.org/articles/storing-sessions-in-a-database):

  ``` sql
  CREATE TABLE sessions (
    id varchar(32) NOT NULL,
    access int(10) unsigned,
    data text,
    PRIMARY KEY (id)
  );
  ```

  However, the above tutorial, [Storing Sessions in a Database](http://shiflett.org/articles/storing-sessions-in-a-database), used a `sessions` table and in the node book we are supposed to have a `session` *no-s-singular* named table. 
  
  That makes me wonder if there are any differences between what this tutorial intends to accomplish and what the node book intends to accomplish. 
  
  I believe in databases *plural names* and *singular names* allude to different uses.
  
  ## Adding Session Table
  There were issues with adding a primary key using Sequel Pro, so I just used the mysql command instead.
  
  ```sql
  mysql> use myserver;
  Reading table information for completion of table and column names
  You can turn off this feature to get a quicker startup with -A

  Database apanged
  mysql> CREATE TABLE session (id varchar(32) NOT NULL, access int(10) unsigned, data text, PRIMARY KEY (id));
  Query OK, 0 rows affected (0.01 sec)
  ```
  
  I still get an error when pressing the **"authenticate (check if token exists in session table)" button**.
  ```bash
  Error: ER_BAD_FIELD_ERROR: Unknown column 'token' in 'where clause'
  ```

  This is where I left off.
## Day 83, R2
### 7/2/19
## Halfway Point
I said yesterday was the halfway point of the year but think it's actually today (day 182.5). Happy half year!

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## Chapter 6, 7/2/19 Error

  I got through chapter 6 but I ran into an issue when I ran `node index.js` in section 6.9, page 59.

  ```bash
  Tue Jul 02 node-master Dashie$ node index.js
  Creating MySQL connection...Ok.
  Server running at http://xx.xx.xx.xxx:3000/
  Error: getaddrinfo ENOTFOUND xx.xx.xx.xxx
      at GetAddrInfoReqWrap.onlookup [as oncomplete] (dns.js:60:26)
  process.exit
  ```
  I found out what was going on.

  The book says:
  >make sure you already have a running MySQL server with user table in it and you included MySQL credentials (IP, username, password and database name) in api.js

  ***But you also need to replace line 13 in index.js.***

  ```javascript
  // replace xx.xx.xx.xxx with your own remote IP address or localhost (127.0.0.1)
  const ip = 'xx.xx.xx.xxx';
  ```

  I changed it to 127.0.0.1

  I then ran into this error when trying to add a user on the UI:
  ```bash
  Error: ER_BAD_FIELD_ERROR: Unknown column 'username' in 'field list'
  ```

  Username was left out of the image of the table on p.41. So I added username to my table. I got the same error for password_md5, which was also left off the table. I'm not sure yet what to put in the `type`, `length`, etc.. for `password_md5` on the table. That's where I left off.

## Day 82, R2
### 7/1/19

- ## Halfway
  Today is the halfway point of the year. 
  
  That means I have accomplished half my goals this year!
  
   That's amazing. I have never had such a successful half year.

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## Where To Find Help
  I was wondering where to get help. 
  
  Here's some [#100DaysOfCode](https://www.100daysofcode.com/connect/) related channels to get help.
  
  ## Yesterday's Error: Solution
  Yesterday, I couldn't log into my database from Sequel Pro.

  <img src="log_imgs/error_6-30.PNG" width="400"/>

  More about the error [here.](#error-6-30-19)

  [@TheDLFord](https://twitter.com/TheDLFord) on twitter solved my problem:

  <img src="log_imgs/help1_7-1-19.PNG" width="500"/>

  So I created a new user using `'%'` for the host: 

  ```sql
  create user 'felix2'@'%' identified by "password";
  Query OK, 0 rows affected (0.00 sec)

  grant all privileges on *.* to 'felix2'@'%';
  Query OK, 0 rows affected (0.01 sec)
  ```

  Exited mysql. Then restarted in terminal with:
  ```bash
  /etc/init.d/mysql restart
  ```
  It worked! I was able to log in through Sequel Pro!

  ## Question: How do you Secure This?
  [@TheDLFord](https://twitter.com/TheDLFord) said using `%` for the host name isn't secure. So how do I keep my project secure?

  <img src="log_imgs/help2_7-1-19.PNG" width="500"/>

  ## Production vs. Development
  So we use the user with the wildcard, `'%'`, only in development. But what does that involve?

  <img src="log_imgs/help3_7-1-19.PNG" width="500"/>

  ## Finally Moving On
  I started chapter 5 on May 29th. I spent over a month on it. I'm finally on chapter 6 of the book. Without the #100DaysOfCode challenge, I don't think I would have pushed through this chapter.


  ## Outsourcing Update
  My IT guy, Moshe spent a few hours over the last week trying to fix my network issue. He wasn't able to do it so I just bought Linode. My issue wasn't his area of expertise.
  
  Moshe was cool about it and said not to pay him.
  
  But I paid him a tip anyways for his time. 
  
  I'm appreciative that he put time in to try. He is a good person that I'd like to keep in my professional life. So ending on, *not good, but **great*** terms is very important to me.

  Even though this outsourcing experience didn't solve my issue, I got some good take aways.

  ### Take Aways:
  - **When outsourcing, look around for someone who thinks they can solve your problem.** Moshe told me upfront he didn't think my issue was in his knowledge set.
  - Outsourcing is a skill I want to get comfortable with.

  I see this experience as a learning experience worth the money I paid. In time I will get better at outsourcing.
  
  I also made a good professional connection with Moshe.



## Day 81, R2
### 6/30/19

- ## [Cloning a repository](https://help.github.com/en/articles/cloning-a-repository)
  I followed this article to clone my **100_days_of_code** repo. Now I can commit my logs from my computer instead of copying and pasting them in to my repo every day and uploading images manually.

  I went through the local repo and delete some extra files and cleaned it up. It's harder to do that directly on github.

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).

  ## Chapter 5 Notes
  ```bash
  root@localhost:~# mysql -u root -p
  ```

  I ran this but I don't remember ever setting up a password or username. But I used my password for `root@localhost` and that worked. So maybe the root for mysql is the same? Is it somehow connected to my **root@localhost** account?

  I think they are the same. Because when we make another user, we use the command:

  ```sql
  create user 'felix'@'localhost' identified by "password";
  ```
  The key thing to note is `@'localhost'`. I didn't notice or perhaps fully understand that before. Our root user in mysql would also be `root@localhost`. That's the same as the root user on my machine. I thought the root user on mysql was totally separate.

  [Download Sequel Pro](https://sequelpro.com/download)

  >If you set up MySQL
server on localhost use that as the Host. Otherwise use your web host's database
IP address (which very often is the same IP address for your web hosting server.
It makes senes, because technically MySQL is installed on "localhost" there too.)

  I just tried the same IP address I used to ssh into.

  ### Error 6-30-19

  I can't login through Sequel Pro.

  <img src="log_imgs/error_6-30.PNG" width="400"/>
  
  Wasn't sure if I shouldn't show my IP addresses so they are colored out. Pink is my remote server IP address. Green is different, but I'm not sure what it is. Maybe my computers IP address.
  
  ### I double checked to make sure:

  My bind-address is commented out:

  <img src="log_imgs/bind_6-30.PNG" width="400"/>
  
  My privileges are set:
  
  <img src="log_imgs/privileges_6-30.PNG" width="400"/>

  I tried:
  - Changing the bind-address to my servers address instead of commenting it out.
  - `FLUSH Privileges;`
  - Logging in with root
  - Restarting mysql a bunch
  
  Nothing worked so far.

## Day 80, R2
### 6/29/19

- ## AR
  I played with [AR.js](https://jeromeetienne.github.io/AR.js/) today.

  ## Custom Marker
  To make a custom marker use this page in **Firefox**: [AR.js Marker Training](https://jeromeetienne.github.io/AR.js/three.js/examples/marker-training/examples/generator.html).
  
  I used this tutorial to make my own marker:
  [How To Create your Own Marker ?](https://medium.com/arjs/how-to-create-your-own-marker-44becbec1105)
  - The code in step 4 is wrong. instead of `patternUrl`, just do `url`.

  Here, I made my own **custom marker.** *It's "**Dash**" with a heart around it*:
  
  ![screenrecording](log_imgs/armarker_6-29.gif)
  
  ## Adding Text
  I used this tutorial to add text to the scene: [Creating Augmented Reality with AR.js and A-Frame](https://aframe.io/blog/arjs/):
  
  I changed to the 3D box to text by replacing the `<a-box>`tag with:
  ```html
  <a-text value="Hello, World! It's Dash!"></a-text>
  ```
  ![screenrecording](log_imgs/artext_6-29.gif)

  
  ## Multiple Markers

  Here, I linked different markers with different images. I used this tutotial: [Creating Augmented Reality with AR.js and A-Frame](https://aframe.io/blog/arjs/):

  ![screenrecording](log_imgs/armany_6-29.gif)

  ```html
  <body style='margin : 0px; overflow: hidden;'>
    <a-scene embedded arjs='sourceType: webcam;'>

      <a-marker type='pattern' url='pattern-dannymarker_6-29.patt'>
        <a-image src="danny.png"></a-image>
      </a-marker>


      <a-marker type='pattern' url='pattern-dash.patt'>
        <a-image src="dashie.png"></a-image>
      </a-marker>


      <a-marker type='pattern' url='pattern-shlomarker_6-29.patt'>
        <a-image src="shlomo.png"></a-image>
      </a-marker>
      <a-entity camera></a-entity>

    </a-scene>
  </body>
  ```

  ## Javascript Generated HTML for Multiple Markers

  I made an AR project using photos of coders off twitter. It uses javascript to create the HTML code.

  ![screenrecording](log_imgs/coders_6-29.gif)

  [See the project here.](https://github.com/DashBarkHuss/ar_fun_with_coders)
  



## Day 79, R2
### 6/28/19

- ## Helping
  I helped two people using [Scrimba](https://scrimba.com/). Watch the videos here:
  - [Anthony](https://scrimba.com/c/c7zL6Kta)
  - [Ayu](https://scrimba.com/c/c8gLJEHD)

- ## Backend
  I signed up for [Linode](https://www.linode.com/). This ubuntu server setup  with my IT guy is just taking so long, plus I think I want to make actual hosted projects now. Linode is what Greg Sidelnikov ([js_tut](https://twitter.com/js_tut)) uses.

  I got the "nanode" plan. It's only $5 per month.

  I put mysql_server on my linode server to get back to where I left off in [Node API Endpoint Roundtrip + Complete Source Code](https://www.patreon.com/posts/node-api-source-27588087)

- ## Javascript30: Native Speech Recognition
  I started [Native Speech Recognition](https://courses.wesbos.com/account/access/5c51dab432bb6d664e015352/view/194128445).

- ## Thoughts & Feelings:
  I switched around too many times today. I was helping people while doing js30 while getting linode while asking Greg questions. It was hard to get anything done with approach. But I wanted to leave my Twitter open for people I was helping, so they could reach me. I think this is ok once in a while, but on most days I should turn off twitter and focus on one thing at a time.

## Day 78, R2
### 6/27/19

  
My IT guy still hasn't figured out my issue. Why is this so difficult? I don't know. So back to [Javascript30](https://javascript30.com/).

- ## Javascript30: Unreal Webcam Fun
  Continuing day 19 of [Javascript30](https://javascript30.com/), [Unreal Webcam Fun](https://courses.wesbos.com/account/access/5c51dab432bb6d664e015352/view/194128614).

  ### Favorite quotes:

  >This is something called base 64. And this is a uh like a text representation of the picture of of me. So this like "8bPb4fb" that means like "stunning smile" and this uh "Rvs0" that means like "sweet baby blues."


  *Wes Bos setting an alt attribute on an image of himself to **"Handsome Man"`**:*
  >Adjust accordingly if you're not handsome"

  ## Javascript AR
  This webcam Javascript30 project got me thinking about AR. I skimmed [Building AR/VR with Javascript and HTML](https://blog.halolabs.io/building-ar-vr-with-javascript-and-html-97af4434bcf6) and [Augmented Reality in 10 Lines of HTML](https://medium.com/arjs/augmented-reality-in-10-lines-of-html-4e193ea9fdbf) and watched a bit of a video in the latter article with instructor Alexandra Etienne([@AndraConnect](https://twitter.com/AndraConnect)).


  Here's the code pen of [AR.js - Augmented Reality in 10 lines of html](https://codepen.io/jeromeetienne/pen/mRqqzb)

  Here's me playing with the code pen:

  <img src= "log_imgs/ar_6-27.gif" width="400"/>

  ### Hiro Marker:
  You need this to play with the codepen:

  <img src = "https://stemkoski.github.io/AR-Examples/markers/hiro.png" width="400"/>

  Link: [Hiro Marker](https://stemkoski.github.io/AR-Examples/markers/hiro.png)

  ## Custom Canvas Video Filter
  After watching Wes Bos's [Unreal Webcam Fun](https://courses.wesbos.com/account/access/5c51dab432bb6d664e015352/view/194128614), I tried making my own filter. This one turns my hoodie periwinkle.

  ```javascript
  function filter(pixels){
    for(let i=0; i < pixels.data.length; i+=4){

      if (pixels.data[i+1] - pixels.data[i+0] > 10 && pixels.data[i+1] - pixels.data[i+2]> 10) {
        pixels.data[i] = 120;
        pixels.data[i+1] = 120;
        pixels.data[i+2] = 200;
      }
    }
    return pixels;
  }
  ```

  <img src="log_imgs/filter_6-27.gif" width="400"/>

## Day 77, R2
### 6/26/19

  
  Still waiting for Moshe The IT Guy to fix my network so I can continue with my node server project. We are supposed to meet tonight.

  In the meantime, I'm going to continue watching some videos.

- ## Crash Course
  I continued watching [Crash Course Computer Science](https://www.youtube.com/watch?v=O5nskjZ_GoI). I got up to #8.

- ## Javascript30: Unreal Webcam Fun
  I started day 19 of [Javascript30](https://javascript30.com/), [Unreal Webcam Fun](https://courses.wesbos.com/account/access/5c51dab432bb6d664e015352/view/194128614).


## Day 76, R2
### 6/25/19

- ## Backend 
  I'm waiting for Moshe The IT Guy to fix my network so I can continue with my node server project. 
  
  In the meantime, I'm going to watch some videos.

  ## Authentication
  Finished watching this video on authentication:

  [Coding Class - Intro to API Authentication Types Oauth, token HTTP Basic repeated session](https://www.youtube.com/watch?v=GjLmjI1r1cg&feature=youtu.be)


  ## OAuth 2.0 Roles

  - **Resource Owner:** Actual user
  - **Client:** The application
  - **Authorization Server:** Verifies identity and issues tokens to the application *(ex: "Log in through Facebook." Facebook would be the application server)*
  - **Resource Server:** Hosts the protected accounts/content

  ## OAuth 2.0 Terms
  - **Application/Client Registration**
  - **Client ID/Secret:** Issued by Authorization server when application (client) is registered
  - **Authorization Grant:** 4 types
    - Authorization Code
    - Implicit
    - Resource Owner Password Credentials
    - Client Credentials

  ## OAuth 2.0 Access Token
  To understand access tokens I watched: [OAuth 2.0 access tokens explained](https://www.youtube.com/watch?v=BNEoKexlmA4)

  Some of the things the instructor said made me think about how API's are separate from the application.

  ## Web Server vs Application Server vs API
  Learned the difference between application servers, web servers, and API by watching this: [Web Server vs Application Server vs API](https://www.youtube.com/watch?v=bgqtxlp7ftc)

  

  I posted a question in response to the video:
  >Question: I'm still confused where the web server comes in. In your last slide we see how the App Server connects to the API. But what about the Web server? Is it just that in cases where you have an API, you don't have a web server, you only have an app server?

- ## Computer Science
  I went back to 
  [Crash Course Computer Science](https://www.youtube.com/watch?v=O5nskjZ_GoI) and started watching from the beginning.  I watched the first 3 videos and started the 4th.

  ### Quote about early computing devices that I liked:
  >Each one of these devices made something that was previously laborious to calculate much faster, easier, and often more accurate. It lowered the barrier to entry and at the same time amplified our mental abilities.

  I learned the term "computer bug" came from an incidence in 1947, when operators of the Harvard Mark II, a large electromechanical computer, pulled a dead moth from a malfunctioning relay.
  
  ## Thanks
  Thank you to my teachers today:

  - [@theDeNap](https://twitter.com/theDeNap)
  - [@rajat1saxena](https://twitter.com/@rajat1saxena)
  - [@MissPhilbin](https://twitter.com/@MissPhilbin)
  

## Day 75, R2
### 6/24/19

- ## Network
  I had my IT guy try to set p port forwarding for me but it didn't work. He said he had to look something up.

  While he was working on my computer I watched him a little bit to learn. But then I started google other thing and watching tutorials on my phone.

  ## Gateway
  Yesterday, Moshe said I would have to reset the gateway many times if I did a bridged network. So I looked up what a gatway was today:

  [What is Gateway](https://www.youtube.com/watch?v=ai5bFPVToMU&feature=youtu.be)

  ## Authentication
  I watched half of this video about authentication. Will finish tomorrow.

  [Coding Class - Intro to API Authentication Types Oauth, token HTTP Basic repeated session](https://www.youtube.com/watch?v=GjLmjI1r1cg&feature=youtu.be)

## Day 74, R2
### 6/23/19

- ## Node Tutorial
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087).
  
  ## Network Problem
  My network went back to how it was before Moshe fixed it, where it was missing the bridged network. I tried to do what Moshe did but it didn't work.

  <img src="log_imgs/terminal_6-23.PNG" width="500" />

  I got this error:
  ```bash
  SIOCSIFNETMASK: No such device
  ```
  ## Working again
  Turns out I was just missing "enp0s3":

  <img src="log_imgs/terminal2_6-23.PNG" width="500" />

  But I didn't realize this until later so I moved on to do a kata until I could get in contact with Moshe.

- ## CodeWars: Split Strings

  I did the kata [Split Strings](https://www.codewars.com/kata/515de9ae9dcfc28eb6000001).

  I used the [JavaScript String Reference](https://www.w3schools.com/jsref/jsref_obj_string.asp) to get ideas for dealing with the string.

  My solution was longer than the other solutions. I wanted it to be more readable. That makes it longer. I saw some creative ways to solve it that I want to look at tomorrow.
  
  ```javascript
  function solution(str) {
  const arr = [];
  for (let i = 0; i < str.length; i += 2) {
    const strLengthIsOdd = str.length % 2 !== 0;
    const iterationIsLast = i === (str.length - 1);
    
    let item = str.slice(i, i + 2);    
    if (strLengthIsOdd && iterationIsLast) item += "_";
    arr.push(item);
  }
  return arr;
  }
  ```
- ## Node Tutorial /Network
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) and setting up my network.

  I started to follow the book but ran into more network issues. I can SSH into test_machine, my virtual machine ubuntu server. But I can't [ping](https://www.dummies.com/computers/pcs/how-to-use-ping-to-check-internet-availability/) from it. Meaning, I can't reach the internet:

  <img src="log_imgs/terminal3_6-23.PNG" width="500" />

  I'm waiting for Moshe to help with with this.
  
- ## CodeWars: Perimeter of squares in a rectangle

  I started the kata [Perimeter of squares in a rectangle](https://www.codewars.com/kata/perimeter-of-squares-in-a-rectangle) while I wait for my networking issue to be fixed.

  The kata said to make an auxilary function. So I looked up what that was:

  >a function you create to help you with some recurring tasks/problems.
  
  -*[What is auxiliary function in programming?](https://www.quora.com/What-is-auxiliary-function-in-programming)*


  

## Day 73, R2
### 6/22/19

- ## Code Wars
  Returning to the [Moving Zeros To The End](https://www.codewars.com/kata/52597aa56021e91c93000cb0) kata.

  Yesterday, we left off with a discrepency: my code worked on chrome, but not on codewars. 
  
  ## Engines
  [A-sin Cole](https://twitter.com/Asincole) figured out that the issue was the javascript engine.

  <img src="log_imgs/asin1_6-22.PNG" width = "500"/>
  <br>
  <img src="log_imgs/asin_6-22.PNG" width = "500"/>

  **Different browsers use different engines to interpret and execute javascript.** Chrome uses the V8 engine. I couldn't find what Firefox uses. [But it's not V8](https://www.diffen.com/difference/Firefox_vs_Google_Chrome). 

  ## Chrome VS Firefox
  ### Code:

  ```javascript
      var moveZeros = function (arr) {
      arr.sort((snd, frst) => {
        if (frst === 0) {
          return -1;
        };
      });
      return arr;
    };
    ```

  ### Engine Difference: 
  ![engines](log_imgs/engine_6-22.PNG)

  Chrome takes the *second* element first. Firefox takes the *first* element first.

  How can we code it to work in both? I think If I can fix it in Firefox, it might work on a node engine. Node is the engine that Codewars uses.
  

  ## More Differences
  There must be more differences because just switching the parameters doesn't fix the problem in Firefox.

  Second difference: if we return **positive numbers** in Firefox in the compare function, the engine will **switch the elements**. But in Chrome it switches the elements if the function returns **negative numbers**. 
  
  This makes sense, because if the engines are taking the items in opposite orders, a-b is going to give different results.

  `array = [0,1];`

  0-1 = -1

  **vs**

  1-0 = 1 

  This makes sense if you're using the compare function `(a,b)=>a-b` or `(a,b)=>b-a` to control whether the order is ascending or descending. But it makes it more difficult in our case.

  ## Can't Use `.sort`?
  I think we can't use sort for this. Since we can't be sure which parameter comes first, we can't know if we should switch the order. 
  
  And, without knowing the engine, we don't know how to tell the engine to switch the order. In Chrome a negative return value means switch but in Firefox a positive return value means switch.

  It seems you should only use sort when trying to compare values and arrange them in ascending or descending order.

  I think there are too many unknowns to solve this kata with sort.

  ## Record On Twitch

  I went on twitch to record myself explaining this but it was a big mess! So I deleted it. But here is how you record on Twitch:

   **Settings** > **Broadcast Settings** and check the box next to **Automatically save stream to file.**

  Twitch saves the videos temporarily. To save the file permanently you should [set up obs](https://www.cnet.com/how-to/twitch-streaming-from-your-pc-guide-recording-your-stream/) to save the video files.
  
  ## Codewars Solution

  To solve the [Moving Zeros To The End](https://www.codewars.com/kata/52597aa56021e91c93000cb0) kata, I used the [JavaScript Array Reference](https://www.w3schools.com/jsref/jsref_obj_array.asp) to pick a different array method. I went with filter.

  ```javascript
  var moveZeros = function (arr) {
    const nonZeros = arr.filter(x => x !== 0)
    const zeros = arr.filter(x => x === 0)
    return [...nonZeros, ...zeros];
  };
  ```

## Day 72, R2
### 6/21/19

- ## Backend

  ## Bridged Network Setup!!!
  ### Aka the magic of outsourcing
  ### Aka, I am not trying to learn IT
  I paid someone to figure out my networking problem and it was $30 *welllll* spent. Sometimes you got to outsource. They didn't want to take my money because they didn't feel knowledgable in the subject and googled the answer. But I also googled the answer too many times, and wasn't knowledgeable enough to figure it out. So I offered $30. It was worth it to me. I'd like to outsource more things.

  This was the tutorial he followed:
  [No “eth0” listed in ifconfig -a, only enp0s3 and lo](https://askubuntu.com/questions/704035/no-eth0-listed-in-ifconfig-a-only-enp0s3-and-lo)

  The answer he followed was:
  
  >If you need to set the static IP of the VM:
  >
  >1. Change the "Network Adapter" to bridged mode in Oracle's Ubuntu VM system settings.
  >
  >2. Start Ubuntu VM
  >
  >3. Type ifconfig
  >
  >4. ifconfig returns enp0s3 and lo,  therefore,
  >
  >5.Type sudo ifconfig enp0s3 192.168.0.111 netmask 255.255.255.0 and you will set the static IP of the VM to 192.168.0.111.

  My IT guy, Moshe, explained some things to me about networks and IP addresses.

  I realized I do not want to spend my time on networking. So outsourcing IT is worth my time.

  If you need IT support I'd recommend this guy: [Moshe Wolfe](https://www.facebook.com/mosesnbros).

- ## Twitch
  I wanted to play with live streaming. I set up my OBS and twitch with the help of these tutorials but I didn't follow them exactly. I set up my OBS differently:
  
  
  [Beginner's guide to setting up and streaming with OBS](https://www.windowscentral.com/beginners-guide-obs)

  [How to Stream to Twitch](https://www.tomsguide.com/us/how-to-stream-to-twitch,news-21077.html)

  My scene set up:

  <img src="log_imgs/obs_6-21.PNG" width="400"/>

  How it looks:

  <img src="log_imgs/obs2_6-21.PNG" width="400"/>

  Here's my [twitch](https://www.twitch.tv/dashbarkhuss);

  The video didn't look like it saved.

- ## Code Wars
  Returning to the [Moving Zeros To The End](https://www.codewars.com/kata/52597aa56021e91c93000cb0) kata.
  
  As suggested by [A-sin Cole](https://twitter.com/Asincole), I logged the array before my code began. This way I could see the **inputs** the codewars tests were using. 
  
  I took those inputs and tried them in my browser. **They worked fine.** 
  
  I played with console logging throughout the codewars code. It was difficult. It's easier to use DevTools, but I wasn't sure how to find the code in DevTools on codewars. But I'm pretty sure you can do it.


  ![](log_imgs/codewars_6-21.gif)

## Day 71, R2
### 6/20/19

- ## Backend, Networking etc.
  I was supposed to have a meeting about my network setup with an IT guy, but it was pushed over to tomorrow. Instead I did a codewars kata.

- ## Codewars: Moving Zeros To The End
  Getting some more javascript practice with the [Moving Zeros To The End](https://www.codewars.com/kata/52597aa56021e91c93000cb0) kata.

  I used `.sort()` for this kata. I thought I figured it out. But I'm failing the tests on codewars. I'm not failing it with any inputs I'm testing. So I'm not where to go from here. Can I find out the codewars test inputs?

  ![](log_imgs/zeros_6-20.PNG)
  
## Day 70, R2
### 6/19/19

- ## Backend, Networking etc.
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) and working with my network.

  ## New Error
  A mentor told me to check out this tutorial. 
  I followed some of this but it didn't work: [Linux Force DHCP Client (dhclient) to Renew IP Address](https://www.cyberciti.biz/faq/howto-linux-renew-dhcp-client-ip-address/)

  I tried running `sudo dhclient -v -r eth0` but I got this error: 
  ```bash
  Error getting hardware address for "eth0": No such device.
  ``` 
  ## Deleting A Virtual Machine
  I thought I should trying setting up the ubuntu server differently, so I deleted one of my ubuntu servers: [How to Delete a Virtual Machine from VirtualBox](http://osxdaily.com/2019/03/19/how-delete-virtual-machine-virtualbox/). I didn't get around to setting up a second one because it took way too long to download the ubuntu installer, and then my computer canceled the download. So I just kept playing around with the other virtual ubunter server I have, **test_machine**.

  ## Trying More Things
  The OP in this thread had a similar issue to my: [Missing eth0 Ethernet interface in Ubuntu — can't connect to router](https://superuser.com/questions/328800/missing-eth0-ethernet-interface-in-ubuntu-cant-connect-to-router).

  One answer said:

  >However, if you don't even see any ethX interfaces when you do ifconfig -a, it's a driver issue (Ubuntu isn't even seeing the interface)

  I didn't understand their suggestion. It was really complicated. 
  
  I have tried a few other things to no avail. I feel like I'm trying to do a simple task and I don't understand why it's so complicated.

  ## Returning to the Tutorial
  I'm thinking I just stopped too early in the [Learning Ubuntu Server](https://www.lynda.com/Linux-tutorials/Learning-Ubuntu-Server/718668-2.html) tutorial.

  I watched more of the tutorial but it didn't look like it was going to help.

  ## Really Asking For Help

  I decided to really go and find people who are experts in this stuff and ask for help. If I am going to be an entrepreneur one day, I can't do everything myself. I have to outsource.

  I also wanted to get more comfortable with asking questions. Sometimes that means finding new people to ask questions to.

  I asked a few people on Simbi and someone I know of who is in my friend's entrepreneur accelerator. I offered to pay him.
  
  I'm also going to look on [When Hub](https://www.whenhub.com/experts/search) if that doesn't pan out.



## Day 69, R2
### 6/18/19

- ## Backend, Networking etc.
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) and working with my network.
  
  ## Bridged Network Not Working

  In this tutorial, [How to SSH into your VirtualBox Guest](https://linuxhint.com/ssh_virtualbox_guest/), it says to change your VM's network to a **bridged network** in order to ssh into it from the host machine.

  I changed ubuntu_server to the the wifi bridged network. But I'm not seeing it in the network when I go to look at the settings for the router. 
  
  When I ran ifconfig, ubuntu_server's IP address is still 10.0.2.20 even though I switched to the bridged network. It's supposed to have an IP address that doesn't start with 10.0 when you're on a bridged network.

  I switched back to NATnetwork and ran ifconfig but the result were the same. The network names didn't change.

  ### Bridged:

  ![screenshot](log_imgs/ifconfig1_6-18.PNG)

  ### NATnetwork

  ![screenshot](log_imgs/ifconfig2_6-18.PNG)

  **enp0s3** is the ethernet interface and **lo** is the loopback interface([Linux ifconfig command](https://www.computerhope.com/unix/uifconfi.htm))

  > wlan0 is the name of the first wireless network interface on the system. Additional wireless interfaces would be named wlan1, wlan2, etc.

  -*[Linux ifconfig command](https://www.computerhope.com/unix/uifconfi.htm)*

  But there is no **wlan0**. I think there should be if the bridged network is working.

  I tried renewing the lease of the DHCP, like this said to do:
  
  [Modify an Existing Virtual Network Adapter for a Virtual Machine](https://pubs.vmware.com/workstation-11/index.jsp?topic=%2Fcom.vmware.ws.using.doc%2FGUID-C20DCA65-B9E2-44B5-9705-47DB752D9F42.html)
  
  But nothing changed.

  I tried changing the MAC address like this tutorial said:
    > The solution was to change the MAC address in guest OS and have same MAC for guest as well as host OS.
  
  -*[VirtualBox Ubuntu 14.04 - Bridge adapter not working](https://askubuntu.com/questions/722512/virtualbox-ubuntu-14-04-bridge-adapter-not-working)*

    I found the MAC address here
  [MAC OS - DISPLAYING, RELEASING AND RENEWING A DHCP LEASE](https://kb.wisc.edu/page.php?id=2258)

  But VirtualBox wouldn't let me change the address.
  
  I asked a handful of people for help but haven't heard back.
  





## Day 69, R2
### 6/18/19

- ## Backend, Networking etc.
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-api-source-27588087) working with my network.
  
  ##  Structure of the project?
  >At one point, you will want to limit your MySQL database access to localhost only. This means only your Node server can connect to the database directly.

  I'm still confused about how the end of this is going to look. 
  
  I got node on my host machine now. But I have a "remote" server on another machine (which is actually on just a virtual machine for learning purposes). 
  
  So are we going to move the node application we made earlier to the machine with the ubuntu server? I think so.

  ## Sequal Pro
  Now we can't connect to the database on the ubuntu_server from Sequal Pro. The ubuntu_server is on a NATnetwork and the host can't access that. 
  
  I knew this wasn't going to work when I set it up, but I just wanted to get through the last section.
  
  Now do we start form scratch with a different network setup? Or can I change the network on my ubuntu_server without starting form scratch?

  > NAT is useful because it protects our guest systems from the Internet at large; but in order to access the them, we need to set up port forwarding to access the guests from the host

  -*[How-to: VirtualBox Networking Part Two - NAT and Bridged](https://catlingmindswipe.blogspot.com/2012/06/how-to-virtualbox-networking-part-two.html)*

  ## Connecting The VM to the Host

  I read through and skimmed through these articiles:

  [How-to: VirtualBox Networking Part One - NAT and Bridged](https://catlingmindswipe.blogspot.com/2012/06/how-to-virtualbox-networking-part-one.html)

  [How-to: VirtualBox Networking Part Two - NAT and Bridged](https://catlingmindswipe.blogspot.com/2012/06/how-to-virtualbox-networking-part-two.html)

  [How to Forward Ports to a Virtual Machine and Use It as a Server](https://www.howtogeek.com/122641/how-to-forward-ports-to-a-virtual-machine-and-use-it-as-a-server/)

  I followed this article: [How to SSH into your VirtualBox Guest](https://linuxhint.com/ssh_virtualbox_guest/). But Once I got here:

  > Check your router settings to see if the same works for you.

  How do I check? I'm not usually on my own router. I'm sometimes on my verizon hotspot, but usually I'm on other routers at cafe's or friends houses. So can I still check this info? 
  
  I tried following [Access the Web-Based Setup Page of the VoIP Router](https://www.cisco.com/c/en/us/support/docs/voice-unified-communications/spa8000-8-port-ip-telephony-gateway/108646-setup-voip-108646.html). I found the routers IP address and entered in into browser like the webpage says, but I got, "This site can’t be reached" because the IP address refused to connect.

  ## More Info On the IP Address

  I looked up the router's IP address whihch I had found in my network preferences.

  >IP address 192.168.1.120 is registered by the Internet Assigned Numbers Authority (IANA) as a part of private network 192.168.1.0/24.

  -*[LookIP.netIP address lookup and information tool](https://www.lookip.net/ip/192.168.1.120)*

  So I put 192.168.1.0 in the address bar and that said, "Your Internet access is blocked Firewall or antivirus software may have blocked the connection."  

  ## Router IP address
  All the IP addresses I found were the wrong thing. I finally found the IP address of the router here: [Find a Router IP Address in Mac OS X](http://osxdaily.com/2011/10/05/find-router-ip-address-mac/)

  When I put this in the address bar it worked.

  ## Synogogues' Site
  I connected with my synogogue. They want me to make a new site for them.

## Day 67, R2
### 6/16/19

- ## Server
  ## Can't Login Without Sudo
  I can't login to mysql without sudo now. 

  ```bash
  mysql -u root -p
  Enter password:
  ERROR 1698 (280000): Access denied for user 'root'@'localhost'
  ```

  However `sudo mysql -u root -p' works.

  I'm trying to figure out why, and if that's bad.

  ### Questions
  What's the difference between logging in with sudo and without? I also found I can login without a password if I use sudo. How does that work? Why have a password then? Is it that only I can login without a password when I'm on my machine, but you can't login without a password with other machines?

  ## Sudo Issue Solved
  I asked Greg([@js_tut](https://twitter.com/js_tut)) and he said it's normal to have to login with sudo. 
  
  The only reason he is able to login without sudo in the tutorial, is because he granted all privileges to users. That makes your server vulnerable but it's ok for development.

  Greg also said:

  >...sudo is your root user. Many commands wont work w/o it
  >
  >because mysql was installed using root user it normally has admin access to all mysql activity (deleting database etc)
  >
  >only root can give privileges to other users so they can become root-like

  ### I'm confused about this:

  Sudo is your root user, but that's your systems root user. But you also have a root user in mysql. So is he saying the systems root user has access to delete databases? Or mysql's root user? Which root user are we talking about here?

  I realized too, that I'm logged in as dash on my ubuntu_server, not the root user. I checked to see all the users with `less /etc/passwd` and I saw root and dash. So they're not the same. Maybe if I was already logged in to root I wouldn't have to run sudo?

  I also realized, I never set a root password. So how do I login to root?

  I think I might have ssh'd into the root of ubuntu_server and added mysql on root. And now I have to sudo in because dash@localhost doesn't have permission. I don't know how that works because I can't even login to root on the ubuntu_server itself.
  
  ## Root User

  I read more about root user in [Edward Falk's answer here](https://unix.stackexchange.com/questions/291454/difference-between-sudo-user-and-root-user).

  ## Table Content
  I tried to view the table content for th e users table.
  ```mysql
  SELECT * FROM users;
  ```
  But this command showed the table data all mish-moshed.

  <img src="log_imgs/table_6-16.PNG" width="400"/>

  But I was able to see the individual columns:

   ```mysql
  SELECT users FROM users;
  ```
  
  ## How do you scroll up/down on the Linux console?
  **VM Ubuntu on a Mac:** fn + shift + up/down arrows


- ## Thoughts and Feelings
  Warning, I am sleep deprived which affects my mindset, so the following may be more negative than usually.

  I still wasn't able to get a lot of sleep last night. I'm having a difficult time putting my foot down about my sleep schedule and my priorities.

  I always assume most people want to support your goals but I'm finding that a lot of people don't care. There are a few people who are really supportive of my goal to code everyday, but in practice most people, even if they think they support you, try to derail you by encouraging you to take day off. Usually for their benefit.
  
  On the flip side, I have never been encouraged to try harder or NOT skip a day of coding(or my diet, sleep schedule, or workouts). 

  If I am doing something on my own accord, that is usually not enough for people. So I have two options. I either have to be more forceful about my priorities. Or I can hack the situation. I can put *something else* on the line. 
  
  I can **"have to"** get up at 6am to code because I am meeting with someone. Or I can **"have to"** code two hours a day, because if I don't I will have to pay $100 to my minder. If someone else is involved, then other people may respect my routine more. People have a hard time respecting pure discipline driven by goals and no outside forces.
  
  

## Day 66, R2
### 6/15/19

  I am really not feeling so hot. I'm so tired. I don't think I'm sleep deprived, it's just that my sleep schedule shifted. And I've had too much coffee these past few days. So excuse the quality of my last two posts. I'm in LA, where I have lots of friends and family. I'm having trouble keeping a good schedule with the influx of hanging out time. Pray for me.

  Being in LA is a unique situation. I'm going to have to be more disciplined when I get home and have rules about how often I can go out for my own health and sanity. For some people it's fine to go out a lot and stay up late. But either I'm sensitive, or maybe other people just don't want to put in the effort to function at 100%. Maybe most people are ok functioning at 75% or with the aid of coffee, alcohol, meds, etc. I need to put some rules in place to prevent this.

- ## Server
  Yesterday, I was wondering if the root login was different than "dash" but I think "dash" IS my root login.

   ## MySQL User
   Yesterday, when I installed mysql, it didn't ask me to add a root user.

  In the video, [Installing and Configuring MySQL](https://www.lynda.com/Linux-tutorials/Installing-configuring-MySQL/718668/779185-4.html?autoplay=true), after installing mysql_server, the instructor runs:
  ```bash
  sudo mysql_secure_installation
  ```
   to set  root user password. The instructor disallows root login remotely, but does that apply to using ssh? 

   I wonder if I can run `sudo mysql_secure_installation` again to change the settings later?

   ## More On Uncomplicated Firewall

  >Right now we can access this database server locally. So a web application or something like that running on this machine would be able to use the database. But we can access it remotely. If we did need to access it remotely we'd need to add a rule to the firewall to allow access through tcp port 3309.
  
  -*[Installing and Configuring MySQL](https://www.lynda.com/Linux-tutorials/Installing-configuring-MySQL/718668/779185-4.html?autoplay=true)*

  ```bash
  sudo ufw allow 3309/tcp
  ```

  This further explains what ufw is doing. So does ufw override "Disallow root login remotely"? 

  Even though I selected "Disallow root login remotely" I was still able to get into mysql from test_machine with an ssh connection to ubuntu_server.
  
   ![](log_imgs/disallow_6-15.PNG)

   Maybe this is because I already ran that ufw command yesterday. I also didn't have to run `systemctl mysql start` to login to mysql. I exited and ran `systemctl mysql start` again anyways just to make sure I don't run into any bumps down the road. I'm not totally sure what this command does. Does it start mysql? Can I stop mysql? Is there a way to see what state (start or stopped) mysql is in?

   ## Grep
   I played around with `grep` to try to find the mysql configuration file but it wasn't working how I expected. I was able to use `find` but in a limited way.

- ## Thoughts and Feelings
    Sleep deprived right now. I'm in LA and we know so many people here so we've been hanging out late. I don't have enough sleep. Maybe I do but my sleep schedule is messed up and I've been drinking coffee which makes you feel more tired. So I am feeling messed up. Angry. Irritable. Pissed. Hard time focusing. I really don't like this.
   

## Day 65, R2
### 6/14/19

- ## Server
  When I tried to connect to my ubuntu server from the host, it didn't do anything. I think it's because the Ubuntu server is on a NATnetwork. Which means I can't connect to it from the host. But I can connect to it from another vm: [Unable to ssh into Ubuntu VM running w/ a NAT ip address even w/ openssh-server installed](https://askubuntu.com/questions/224391/unable-to-ssh-into-ubuntu-vm-running-w-a-nat-ip-address-even-w-openssh-server)

  I could just change the network to bridged, but then I'd have to find more tutorials on that. And I might run into more problems with the tutorial down the road. I just want to get through this tutorial so I can get to the Node book. 

  ## Matching Environments
  
  There's a lot I'm doing that's unnecessary. I'm doing it for the sake of following the tutorials. It's easier to get through the tutorial when my environment matches the instructors.
  
  I don't even need this Ubuntu Server to set up a node server. But the Node book only shows how to do it through an ubuntu server. So, in order to get through the tutorial I'm setting everything up so I can follow the steps exactly. Once I do that, I'll have more experience and understanding. Then I can try it on localhost or whatever I want. 
  
  When my environment didn't match the instructor's, I ran into all these side issues: Does this step apply to me? This step doesn't work in my environment. etc. Which is doubly difficult when the subject is new and you don't understand what you're doing yet.

  So I just need to match the environment so I can get through the tutorial and learn.

  ## Installing Second VM
  I created another virtual machine, called **"test_machine"** to ssh into the first vm, **"ubuntu_server"**, like the instructor does. But I didn't know exactly what to do for the network configurations. 

  This is what the instructor did for **his ubuntu server.**

  <img src="log_imgs/instructor_6-14.PNG" width="500"/>

  I did the same thing, but what do I do for **my second ubuntu** server? I guessed that only the address had to be different. I'm not sure if I did it right.

  <img src="log_imgs/dash_6-14.PNG" width="500"/>

  I was able to ssh into the **"ubuntu server"**, but the server 

  ## Uncomplicated Firewall
  What is uncomplicated firewall?
  > Assuming mysql installation was successful, you can now use the ufw utility program (Which stands for Uncomplicated Firewall.)

  -*from [Node Server Setup](https://www.patreon.com/posts/node-api-source-27588087)*

  >You can use ufw allow and ufw disallow commands to allow incoming and outgoing trac from a program.

  -*from [Node Server Setup](https://www.patreon.com/posts/node-api-source-27588087)*


  ## No Root User, No Mysql Password
  When I set up the ubuntu server, I set up a login with my name, "dash". But in the Node book, Greg uses the root login. I'm wondering if that's fundamentally different.

  When I installed mysql server on ubuntu_server it didn't ask to set a up a username and password, so `mysql -u root -p` doesn't work. Setting up user on the ubuntu server was not in the Node tutorial. So I'm confused. 

  I guess I need to add it?

  Do I want mysql on the server(ubuntu_server) or on the computer ssh'ing into the server(test_machine)?

## Day 64, R2
### 6/13/19

- ## Spark AR
  I started looking at Spark AR, which is the tool you use to make instagram filters. Coding is involved somewhere, but not in the begginer tutorials, so I'm going to go back to this for outside of my coding time.

  [Tutorial: Quick Start Guide to Spark AR Studio](https://sparkar.facebook.com/ar-studio/learn/documentation/tutorials/quick-start-guide)

- ## Backend
  Back to backend. Last time I left off watching a tutorial on VirtualBox so I can run a virtual server. Greg([@js_tut](https://twitter.com/js_tut)) updated his Node Book yesterday, so I'm going to check out the changes to it today. [Here](https://www.patreon.com/posts/node-api-source-27588087) is the updated version.

  ## Ubuntu Server

  Ubuntu server is a Linux distribution that's intended to be run **headless**. Headless means without a display attached.

  I was watching [Learning Ubuntu Server](https://www.lynda.com/Linux-tutorials/Learning-Ubuntu-Server/718668-2.html). I really want to just get this server up! But it suggested watching all these other prerequisites without suggesting any specific course. I'm having trouble figuring out where to learn all this stuff about networks, ports, blah blah blah. 

  I started to watch this but it was hard to understand:[Network Address Translation(Nat)](https://www.lynda.com/Cisco-Switches-tutorials/Network-Address-Translation-NAT/445426/475831-4.html)

  This video really helped me understand the different networks: NAT, NATNetwork, Bridged, Host-only, and internal: [Networking with VitrualBox](https://www.lynda.com/Linux-tutorials/Networking-VirtualBox/597026/678866-4.html?srchtrk=index%3a1%0alinktypeid%3a2%0aq%3alearning+virtual+box%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2)

  <img src="log_imgs/network_6-13.PNG" width="500"/>

  I set up my ubuntu server on virtual box following [this video](https://www.lynda.com/Linux-tutorials/Installing-Ubuntu-Server/718668/779168-4.html). I powered off my machine for the day. I'm not sure what that does, but I hope I can get back to it tomorrow without issue.

## Day 63, R2
### 6/12/19

- ## Backend
  Continuing my journey into understanding backend.
  
  ## IP Addresses

  Brian Morrison II
[@bmorrisondev](https://twitter.com/bmorrisondev) answered my questions about IP addresses.

  <img src="log_imgs/ipq_6-12.PNG" width="400"/>
  <br>
  <img src="log_imgs/ip_6-12.PNG" width="400"/>

  ## Continued [The World Wide Web: Crash Course Computer Science #29](https://www.youtube.com/watch?v=AEaKrq3SpW8):
  Some Terms: 
  - URL (uniform resource locator)
  - DNS Server (Domain name search)
  - Web Server

  ### Paraphrased from [The World Wide Web: Crash Course Computer Science #29](https://www.youtube.com/watch?v=AEaKrq3SpW8):

  When you request a site, the first thing your computer does is a DNS lookup. The DNS server takes the domain name and returns the IP address of the respective computer(the web server). Then your computer opens a TCP connection to a computer that's running a special piece of software called a web server. The standard port number for web servers is port 80. The computer is now connected to the web server at the correct address, but the next step is to ask that web server for a hypertext page. To do this it uses Hypertext Transfer Protocol (HTTP).

  The first browser was released to the public in 1991. So the World Wide Web is as old as me!

  <img src="log_imgs/crashcourse_6-12.PNG" width="400"/>

  
  ## CrashCourse Computer Science Playlist
  I didn't watch all of these but here is the whole [CrashCourse computer science playlist](https://www.youtube.com/playlist?list=PL8dPuuaLjXtNlUrzyH5r6jN9ulIgZBpdo).

  ## Network Adapter
  I'm trying to understand more about network configurations and I came accross this term.

  >A network adapter is the component of a computer’s internal hardware that is used for communicating over a network with another computer. It enable a computer to connect with another computer, server or any networking device over an LAN connection. A network adapter can be used over a wired or wireless network.
  
  -*from [What is a Network Adapter](https://www.techopedia.com/definition/8546/network-adapter)*

  ## Networking For Web Developers
  I found this udacity course I might want to go back to so I'm saving it here:
  [Networking For Web Developers](https://classroom.udacity.com/courses/ud256/0)

  ## [Learning VirtualBox](https://www.lynda.com/Linux-tutorials/Learning-VirtualBox/597026-2.html)
  I've been surfing the web trying to find more information about host-only networks, NATs, bridged networks, and other networking concepts.
  
  I decided it's enough theory for now, and I need to just start doing. So I'm going to set up virtual box with the course, [Learning VirtualBox](https://www.lynda.com/Linux-tutorials/Learning-VirtualBox/597026-2.html). Then I can set up a server and continue the Node Server Setup book.

  ## Virtualization
  A hypervisor provides a protected space for a guest OS. Hypervisor provides some of the host's resources to the guests: cpu resources, memory, storage, and communications.

  ## CPU
  The tutorial said I should understand what CPU is. I started watching [The Central Processing Unit (CPU): Crash Course Computer Science #7](https://www.youtube.com/watch?v=FZGugFqdr60) but it seemed like to needed to have watched the pervious videos in the CrashCourse playlist to understand it. So I read [Explain CPU Threads Like I'm Five](https://dev.to/venkateshbella7/explain-x-like-im-five-2d20). So it seems like CPU directs processes? I read [What Is A CPU](https://www.digitaltrends.com/computing/what-is-a-cpu/).

## Day 62, R2
### 6/11/19

- ## Server Etc.
  Continuing my journey into understanding backend.

  ## IP Address
  Yesterday I was confused how the instructor got the IP address that he used for his server. I asked a peer on twitter, Jacob M-G Evans([@JacobMGEvans](https://twitter.com/JacobMGEvans)),  for help. Jacob said you can get the IP address by running `ifconfig` in the terminal if  you're on a mac.

  > If the virtual machine is local, then yes. But if your trying to get the IP of the VM it would be the same, might even be able to configure the VMs IP yourself
  >
  > No matter what the ifconfig command will give the IP, depending on what IP you need and what it is for.
  
  -*Jacob M-G Evans [@JacobMGEvans](https://twitter.com/JacobMGEvans)*
  
  But if the server already knows the IP address in the terminal, why do you have to put it in the configurations? What's the point? Doesn't the virtual machine have access to this, if you can access this through the command line? Why did the instructor put it in his configurations?

  > Whether you are using host-only, network address translation (NAT), or bridged networking, each virtual machine must be assigned an IP address. For host-only networking, the host must also be assigned an IP address.

  -*from [Assigning an IP Address to a Virtual Machine](https://stuff.mit.edu/afs/sipb/project/vmdialup/archive/i386_linux24.old/lib/vmware-console/help/gsx/networking_assignip.htm)*

  ## Networking

  I decided I need to understand networks and IP addresses more, so I started watching [Networking Foundations: Networking Basics](https://www.lynda.com/Windows-Server-tutorials/Foundations-Networking-Networking-Basics/408231-2.html)

  **Notes:**

  ## Logical and Physical Topologies

  **Physical topology:** Referes to the physical layout of the wires in a network.

  **Logical topology:** Refers to how the data moves through the networks

  Networks can have a different physical topology than logical topology.

  **This tutorial isn't so good, so I'm going to move on.**

  ## IP Addresses
  > An IP address is split up into a network code and a host id.
  
  -*from [IP Addressing and How it Works](https://www.youtube.com/watch?v=KFooN7Mu0IM)*

  <img src="log_imgs/ip_6-11.PNG" width="500"/>
  
  ## Computer Networks
  Watched this youtube video: [Computer Networks: Crash Course Computer Science #28](https://www.youtube.com/watch?v=3QhU9jd03a0)
  
  Clarified that networks are actually connected by ethernet cables or wifi. Not all networks are internet. A computer connected to a printer can be a network.
  
  ### Local Area Networks 
  Relatively small networks of close by computers. LANs can be as small as two machines or as big as a university campus with thousands of computers.

  ### The Internet
  The internet is the biggest computer network. It interconnects a bunch of smaller networks allowing communication.

  ## The Internet
  Watched the next youtube video: [The Internet: Crash Course Computer Science #29](https://www.youtube.com/watch?v=AEaKrq3SpW8))
  
  Clarified that you LAN is the first part your computer goes through to get to the internet. It's every device in your house connected to your wifi router.

  The router connects to a Wide Area Network (WAN), which is a router run by your ISP.

  There will be a regional router like one for your neighborhood. That one connects to an even bigger WAN, like one for your whole city.

  You can see the route that data takes to different places on the internet by running `traceroute somesite.com` in the command line.

  ### Internet Protocal
  Internet packets have to conform to a protocol called the Internet Protocol.

  ### UDP
  IP gets the packet to the right computer but UDP gets the packet to the right program running on that computer.

  ## TCP
  Like UDP, but for when you need all the information,for example in an email instead of streaming video. In TCP packets are given sequential numbers. The receiver has to send back an acknowledgement(ack) to the sending computer, and then the sender can send the next packet. It can send many packets at once and have many outstanding acks.

  ## The World Wide Web
  Watched the next youtube video: [The World Wide Web: Crash Course Computer Science #29](https://www.youtube.com/watch?v=AEaKrq3SpW8)

  The World Wide Web is not the same as the internet. It runs on top of the internet. The www is a huge distributed application running on millions of servers world wide. You access it using a web browser. Pages are the fundamental building blocks of the www . Hyperlinks connect different pages.




## Day 61, R2
### 6/10/19


- ## Notes On "Networking Foundations: Servers" Course
  Continuing my notes on: [Networking Foundations: Servers](https://www.lynda.com/Windows-Server-tutorials/Foundations-Servers/503999-2.html)
  


  ## Virtualization
  Before virtualization, you could only use a server for one thing. This would waste a lot of the servers capabilities.

  A virtual machine is a set of files on a hard drive that emulate an actual computer.

  The actually machine that the virtual machine is on is known as the host.

  A host must have certain specifications and the hardware to support virtualization.
  
  A virtual machine can be set up to network with the internet or just with the other virtual machines. Can it have even more network setups?

  ## Storage Devices and Technologies

  - Disk Specifications
    - RPM (faster it spins the faster you can access data)
    - Dimensions
    - Capacity
    - IOPS (input/output per second)
    - Seek time and latency (speed of actuator arm)
    - Hot Swappable
  
  - Disk Interfaces
    
    How you connect the harddrives to the server

    - Serial Attached SCSI (SAS)
    - Serial ATA (SATA)
    - SCSI
    - USB
    - Fibre Channel (common for shared storage)

  - Other Storage Technologies
    - Direct-attached Storage (DAS)
    - Network-attacked storage (NAS, storage device accessable of TCP/IP network)
    - Stoarage area network (SAN, like NAS but outside of LAN, connected by highspeed private network)
    - Just a bunch of disks (JBOD, not organized & no fault tolerance)

  - Other things not so popular
    - Tape
    - Optical
    - CompactFlash

  ## Fault Tolerance and Capacity Planning

  Things to consider when planning a storage solution:

  - Space
  - Performance
  - Fault tolerance

  Fault Tolerance: A way to ensure that in the event of a hardware failure with one of your hard drives, you not only don't lose the data, but you, in some cases, may not even lose access to the data while the failure's taking place.

  ## RAID
  RAID: redundant array of independent disks.

  - RAID 0
    <img src="log_imgs/raid0_6-10.PNG" width="400"/>
    - Disk striping
    - No fault tolerance
    - Min 2 disks
    - Increased read performance because no fault tolerance
    - 100% drive space utilization

  - RAID 1
    <img src="log_imgs/raid1_6-10.PNG" width="400"/>
    - Disk Mirroring
    - Exactly 2 disks
    - Increased space utilization
    - 50% drive space utilization
    - Provides fault tolerance

  - RAID 5
    <img src="log_imgs/raid5_6-10.PNG" width="400"/>
    - Disk striping with parity
    - Min 3 disks
    - Increased read performance but decreased write performance by a little
    - Efficient disk space utilization
    - Provides fault tolerance

  There are also hybrids of these.

  ## Capacity Planning Considerations

  - OS Growth
    - Patches
    - Service packs
    - Log files
    - Temp files
  - Data Growth
    - Customer Data
    - Archived Data 
    - Recovery growth
  
  - Mitigation Strategies
    - Disk quotas (limiting how much space a user is allowed)
    - Compression (take data and compress it, but performance decreases so should be with older not regularly accessed data)
    - Regular cleanup 
    - Routine archival (when you can't delete but you can move it and store it somewhere else)
  
  ## Physical Security

  **Multifactor Authentication:** When you have to use more than of the following
    - What you know (password)
    - What you have (smartcard)
    - Who you are (finger print)

  **Mantrap:**

  <img src="log_imgs/mantrap_6-10.PNG" width="400"/>

  ## Server Hardening

  - OS Hardening
    - Stop and/or uninstall unneeded services
    - Close unneeded ports
    - Minimize software installations
    - Keep security patches up to date
  - Application Hardening
    - Disable/uninstall unneeded...
      - services
      - roles
      - features
    - Install latest version, updates, and patches

  - Hardware Hardening
    - Disable unneeded hardware and physical ports/devices
    - BIOS password
    - Disable Wake-on-LAN if it's not a feature you're using
    - Set appropriate boot order
    - Chassis locks

  - Endpoint Security
    - Intrusion detection system (IDS)
      - Host-based
      - Network based
    - Anti-malware software
    - Vulnerability scans


  If it's not needed get it out. You want the fewest number of vulnerability points possible.

  ## Access Control

  You can set what permissions different users are allowed.

  ## Environment Control & Storage Disposal.

  - UPS (uninterrupted power supply)
    - Automated graceful shutdown
  - Power Distribution Unit

  - Safety Issues
    - Electrostatic Discharge procedures
    - Fire Suppression
    - Rack Stability
    - Floor load limitations
    - Sharp edges and pinch points
  - HVAC Issues
    - Temperature
    - Humidity
    - Air flow
    - Hot aisle/cold aisle
  - Secure Disposal
    - Soft wipe
    - Hard wipe
    - Remote wipe
    - Physical destruction (dispose pieces in different places)

  ## Basic IP Configuration
  The instructor showed where he configured the IP address on his server but he didn't explain how he got that IP address. I did a bit of googling but the results were not so great. My question is probably so off base that my results aren't going to help. So I messaged my question to someone on twitter instead.
  


## Day 60, R2
### 6/9/19

- ## Servers
  I continued learning about servers.

  ## Notes On "Networking Foundations: Servers" Course
  Continuing my notes on: [Networking Foundations: Servers](https://www.lynda.com/Windows-Server-tutorials/Foundations-Servers/503999-2.html)

  ## Installation
  The instructor installed a **server operating system** on a virtual machine. So this shows us another example that, whether a computer is a server or a pc depends on the software.

  ## Basic Configuration

  After you install a server operating system, you then have to do some basic configurations. [Video Here](https://www.lynda.com/Windows-Server-tutorials/Basic-configuration/503999/539126-4.html?autoplay=true)

  The instructor set the IP address of the VM but I didn't understand where he got the number from. But he said it was important to set this IP address up correctly to connect to the domain.

  You need to do these configurations before you give a specific "role" to your server. I don't know what "role" means yet.

  ## Server Roles
  There are different services a server can provide.

  There are a bunch of different roles:
  
  <img src="log_imgs/roles_6-9.PNG" width="400"/>

  I didn't realize that a server can do so many different things. Am I trying to make a web server?

  After the instructor sets up his server as a DNS, he says "it will go ahead and install the role and pretty much make me become a DNS name server in my environment." What exactly does "in my environment" mean? How is that set up? How far does the environment reach? Is it just his computer? A network of computers? All the computers connected to the web?

  Your server can have more than one role.
  
  "And so that's something that you need to evaluate in your environment, and understand what servers are providing what roles out on your network." Two words that I don't fully grasp: **environment** and **network**.

  Clarification: 
  > Numerous applications run in a client/server environment, this means that client computers (computers forming part of the network) contact a server, generally a very powerful computer in terms of input/output

  -*from [Client/Server Environment](https://ccm.net/contents/152-client-server-environment)*

  ## Access Methods For Server Administration

  To save space, most servers don't have the peripherals we have on personal computers. So we need to know how to access the servers. 

  - Local Hardware Administration
    - KVM switch box (key board, video, mouse)
    - Serial cable (old method)
    - Virtual administration console (if you're trying to manage virtual servers)

  What exactly is a "virtual server"? Is that a server on a virtual machine?

  >On the Internet, a virtual server is a server (computer and various server programs) at someone else's location that is shared by multiple Web site owners so that each owner can use and administer it as though they had complete control of the server.

  -*from [DEFINITION
virtual server](https://searchnetworking.techtarget.com/definition/virtual-server)*

  So a virtual server is what GoDaddy said I could upgrade to a [few days ago](https://github.com/DashBarkHuss/100-days-of-code/blob/master/r2-log.md#vps-vs-dedicated-vs-shared-hosting).

  - Network-Based Hardware Administration
    - KVM over IP (via TCP/IP, connect to a KVM switch box)
    - iLO
    - iDRAC


  I don't know why the instructor called this last section "inventory." It's just ways to access servers remotely:
  - Inventory
    - Remote Desktop Protocol(RCP)
    - Secure Shell (SSH), doesn't show you actual desktop
    - Virtual Network Computing(VNC)
    - Command Line

   

   ## Maintenance

   You can see if your server is running properly in a few ways.

   Within the operating system, you can use certain tools to help see if the system is running properly.

   In a Windows Server 2012 R2 OS, you have the Server Manager, which opens by default when you log in. On the button of the screen,you can see different server roles. If everything is green it's all good.

   ![](log_imgs/maitenance_6-9.PNG)

   Another tool that Microsoft gives you is the Task Manager. It shows the different application that are running. (like activity monitor?) You can end processes. You can look at memory, CPU, and network.
  
  You also have the Event Viewer on Microsoft. Here you can see logs of activity. You can filter the logs to see warnings and errors. It's good to check this regularly to catch problems before they affect users. What is the Event Log on Mac? 
  
  You can also find logs in another place. Logs associated with just specific server roles can be found in the managers for those servers.

  Other tools on Microsoft:
  - Resource Monitor
    - processor performance
    - disk performance
    - network performance
    - memory performance
  - Performance Monitor
    
    Can show us if we have a performance issure, then we need to determine if it's the  sorftware or hardware

  There are also third party tools.
 
  ## Asset Management And Documentation
  - Licensing
    
    When you purchase software, you purchase the right to use it. A vendor can audit you and make you show them the license.
  - Labeling
    
    Label your servers and workstations, I'm not totally sure what this means. Physically labeling them with a labeler? What?
  - Warranty
    
    Keep track of warranties on hardware, and where to claim warranties.

  - Lifecycle Management
    
    Aquiring hardware, using it, and getting rid of it.

    - Procurement

      What are the procedures we go through when we procure hardware?

    - Usage

      How will that asset be used and by who? If it's software, what kind of licenses are needed?

    - End Of Life

      When are we done with this asset?
    
    - Disposal
  
      How will we dispose the hardware? Security issues.
    
  - Inventory 
    - Make
    - Model
    - Serial number
    - Asset tag
  
  - Documentation
    - Maintain service manuals
    - Diagrams of network, architecture, data flow
    - Recovery Processes
    - Change management plans
    - Service level agreements

  What are change management plans and service level agreements?
     

## Day 59, R2
### 6/8/19


- ## Servers
  Today, I looked more into servers.

  ## Server Is Software?
  Yesterday, I found conflicting information about whether a server is the software or the hardware. In response, Marco ([@Wridgeu](https://twitter.com/Wridgeu)) said that it's software:

  <img src="log_imgs/marco1_6-8.PNG" width="500">
  <img src="log_imgs/marco2_6-8.PNG" width="500">

  ## Ubuntu Server or Regular?
  Yesterday, I wasn't sure if I should use the **server version** of Ubuntu, or the **regular version** of Ubuntu.

  I talked to Greg. He said it's all set up for you by your hosting service, so he wasn't sure. But probably the server version. So that's what I'll put on my virtual machine. 


  ## SSH Into A Virtual Machine?
  I was wondering if it's possible to SSH into a virtual machine and found this:
  [Trying to SSH to local VM Ubuntu with Putty](https://unix.stackexchange.com/questions/145997/trying-to-ssh-to-local-vm-ubuntu-with-putty)

  There were a lot of terms that I was not familiar with:
  - private network
  - host network
  - NAT(Natural Address Translation)
  - port forwarding
  - network preferences
  - port
  - daemon

  But when I look for pages about how to learn backend development, none of them mention learning this stuff. So I'm not sure what all this stuff is considered. 

  So I searched all these terms at once in google and lynda. It looks like this all has to do with "networking" which I know very little about.

  I found this tutorial on some exam called [CompTIA Server +](https://www.lynda.com/IT-Infrastructure-tutorials/Cert-Prep-CompTIA-Server-Exam-SK0-004-Basics/772338-2.html).

  I watched it a little, which clued me that I might be trying to understand [server administration](https://www.lynda.com/IT-Infrastructure-tutorials/Cert-Prep-CompTIA-Server-Exam-SK0-004-Basics/772338-2.html).

  I'm not trying to understand these in detail, I just want to understand the basics.

  ## Notes On "Networking Foundations: Servers" Course
  I found a course on lynda that covers the above concepts: [Networking Foundations: Servers](https://www.lynda.com/Windows-Server-tutorials/Foundations-Servers/503999-2.html)
  
  The rest of my log is notes on what I learned.


  ## Form factor 
  
  "In computing, the form factor is the specification of a motherboard – the dimensions, power supply type, location of mounting holes, number of ports on the back panel, etc" -[Computer Form Factor, wikipedia](https://en.wikipedia.org/wiki/Computer_form_factor).

  Basically the hardware. Towers are form factors that can be used as servers or client side computer. We've probably all seen them with desktop computer setups. So this shows, that the hardware doesn't necessarily make something a server, since towers can be used as PC's or servers.
  
  Other form factors: server blades, rack mount server.

  ## Server Components

  Aka computer components, since a server is just a computer. However, computer components in a server are made to be more robust than a PC.

  - Motherboard
    - Main circuit board
    - Provides communication for all the components
  - CPU (Processor)
    - Located on the motherboard
    - Have different socket types, needs to match motherboard
    - Have different speeds
    - Cache (high speed memory)
    - Architecture: x86, x64, ARM(not in servers)
  - RAM(random access memory)
    - temporary memory
    - ECC vs non-ECC
    - DDR2 & DDR3
    - Different pin configs, needs to match mother board
  - Expansion Slot
    - Gives ability to add components to the system (expansion cards)
  - Interface Ports
    - ports built into the motherboard or expansion cards
    - Where you connect your network, monitor, usb
    - input/output communication
  - Hard Drives
    - Magnetic drives vs Solid state drives

  Many components can be changed without having to shut down the server (hot swappable)

  ## Power & Cooling

  Power Factors:
  - Voltage: Measurement of force of electricity
  - Amperage: Amount of movement of electricity (current)
  - Wattage: Actual measurement of electrical usage (volts x amps)

  #### Factors For Determining How Much Power We Need
  - Consumption: How much power the servers are going to consume.
  - Redundancy: More power than what's needed so our servers can be dependable.
  
  Cooling Factors:
  - Airflow: In the server room & inside the server itself.
  - Thermal dissipation: ex: sucking heat away from a component
  - Baffles and shrouds: deflectors to help with air flow
  - Fans: exhaust fans- blow the hot air away from components
  - Liquid Cooling

- ## Thoughts & Feelings
  I think it will be helpful to dive deeper into servers. But I also wonder if I'm getting distracted by stuff that doesn't matter. If you have any thoughts, I'm always open to critique on my learning path(on my respective twitter posts or linkedin posts for each log.)

## Day 58, R2
### 6/7/19


- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624) and setting up my remote server.

  ## Vanilla Stack
  Yesterday, I ended the day wondering what stack we're using in the Node server book. I asked Greg([@js_tut](https://twitter.com/js_tut)):

  ![messenger screenshot](log_imgs/vanilla_6-7.PNG)
  
  ## Vanilla FTW
  I thought this was really cool. It's a *vanilla* server! I really dislike working with a billion unnecessary frameworks. Frameworks/modules/libraries have a place in coding, but when you are learning they can be a huge mess.

  Let's say you run into a problem and you're using a bunch of modules. It's going to be really hard to find problem. *Is the bug related to one of the modules? Is it your code? Is it some other technology you're using?* The more minimal your code, the easier it will be to pin point the issue.

  But so many tutorials teach tutti frutti programming (a term I made up, opposite of vanilla). This makes it so much harder to learn. It's rare to find vanilla tutorials, especially the more advanced the subject. I gravitate towards the vanilla tutorials, especially when learning a new subject.

  ![messenger screenshot](log_imgs/vanilla2_6-7.PNG)

  ## Our Stack:

  Greg didn't directly answer what our stack was, but I think it's:

  - MySQL
  - MySQL Server
  - Node
  - Linux/Ubuntu

  ## Getting A Remote Ubuntu Server

  I still want to get an Ubuntu server, so I can follow the tutorial exactly. But my GoDaddy account is Cent OS unless I upgrade it. I probably eventually will upgrade it, but is there a way to get a free Ubuntu server?

  ## Virtual Machine

  I watched this video on virtual machines:
  [What is a Virtual Machine (VM)? In 3 minutes](https://www.youtube.com/watch?v=yIVXjl4SwVo)

  **Virtual Machine:** Virtual machines allow one or more 'guest' operating systems to run inside another

  So can I make an Ubuntu virtual machine that is just a server?

  ## Servers
  I watched this video on servers: [What Is A Computer Server?](https://www.youtube.com/watch?v=TQQA8RpKxqg)

  The tutorial pointed out:
  >You can *turn your computer into a server* by installing software on it that allows it to  be a server.

  So are we setting up a server on an Ubuntu OS? Or is Ubunto an OS just for servers? So it is a server itself?

  >Ubuntu is an open source software operating system that runs from the desktop, to the cloud, to all your internet connected things

  -*from [ubuntu.com](https://www.ubuntu.com/)*

  I read this 3-page tutorial on the difference between Linux and Ubunto. It helped me understand operating systems in general: [What's Ubuntu, and how is it different from Linux?](https://computer.howstuffworks.com/ubuntu.htm)

  ### What is Ubuntu?:
  >Ubuntu isn't just for personal computers. There's also a version of Ubuntu you can use if you want to turn a computer into a network server.

  -*from [What's Ubuntu, and how is it different from Linux?](https://computer.howstuffworks.com/ubuntu1.htm)*

  So are we using the *server version* of Ubuntu? Or the regular Ubuntu? Is this *a regular Ubunto OS* that we are *putting a server on*? Or are we directly ssh-ing into the server OS version of Ubuntu?

  > This section will walk you through the standard MySQL server configuration on a Ubuntu server, for no particular reason.
  
  -*from [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624)*

  So we're putting MySQL server configuration on an Ubuntu server. I think this means we're using the server version of the Ubuntu OS. But maybe you can have the regular version of Ubuntu on a server. So maybe I'm wrong. 

  ## Server Hardware vs. Software

  I'm confused what makes something a server... is it the hardware? The software? Or both?

  If I can turn my computer into a server, as the youtube video said, then it can't be the hardware that makes a server a server. 

  But if you look at pictures of servers, they have different hardware than computers, most notable no screen or keyboard.

  <img src="log_imgs/servers_6-7.PNG" width="500">

  I found this thread on Quora: [Does server mean hardware or software or both?](https://www.quora.com/Does-server-mean-hardware-or-software-or-both)

  It looks like it can mean both but usually means software.
  > While "server" can, in some circles as many have pointed out here, refer to a high-powered computer hardware system, I would venture to guess that most of the time when the term "server" is used by an IT or software engineering professional, it is usually referring to software.
  >
  >A "server" is merely a piece of software, usually with little or no user interface, that "serves" some kind of data or functionality up to "clients".

  -*from [Does server mean hardware or software or both?](https://www.quora.com/Does-server-mean-hardware-or-software-or-both)*
  

  But then I found this conflicting information on another site:
  >While almost any computer that meets the minimum hardware requirements can run a server operating system that alone does not make a desktop computer a true server. Even if the desktop computer had similar processor speeds, memory and storage capacity compared to a server, it still isn't a replacement for a real server. The technologies behind them are engineered for different purposes.

  -*from [Is Server different from a Desktop PC?](https://www.webopedia.com/DidYouKnow/Hardware_Software/difference_between_server_and_desktop.html)*
  
  It also mentioned:
  >a server manages all network resources

  ## What Are Network Resources?

  >Shared resources, also known as network resources, refer to computer data, information, or hardware devices that can be easily accessed from a remote computer through a local area network (LAN) or enterprise intranet. 

  -*from [Shared Resources](https://www.techopedia.com/definition/24796/shared-resources)*

  ## Summary
  So I'm finishing today with a bit of confusion that I slightly clarified:

  - In the node book, does Greg use:
    -  the ***server version of Ubuntu*** 
   
       or 
       
    -  the ***regular version of Ubuntu*** on a server, and then setup up the MySQL Server as the server inside of the Ubuntu OS (like a strange server sandwich)
  - Seems like nobody truly knows when something is a server and when it isn't.
  

## Day 57, R2
### 6/6/19

- ## Coding Coach Mentor
  I reached out to a guy on coding coach but he didn't have enough time to commit to what I wanted. But he agreed to answer questions I send him at his leisure. 

   Many mentors probably sign up looking for a low commitment mentorship, like the person I reached out to. **I think there is room to create an application, or a section of Coding Coach, that is more conducive to a low commitment mentorship.**
   
   When you are trying to choose a mentor it feels like dating. You are trying to pick “the one.” That’s time consuming and higher stakes for both the mentor and mentee. But if I had multiple mentors, I could be more casual about who I pick. Likewise, a mentor may be more open to who they are available to if they had many low commitment mentees. 
   
   ### So here’s my idea for a low-commitment mentor app:

    - Instead of scheduling meetings, the mentors turn on when they are available and on what mediums (phone, DM). These settings can be different for each of their mentees. 
      - **Example:** I’m a mentor and I’m going to work on my own project for 2 hours at my computer, but I’m ok with being interrupted. 
     
        So turn on my “Available” indicator. I set that I’m available by video chat & text. Only my approved 20 mentees can see this. 
        
        However, 3 of my mentees are really annoying! I have mentee “groupings”, so I set my *video availability* to just the 17 mentees. I set only *text availability* for the annoying group, today. Or I could only be available for 5 or my favorite mentees. 
        
        If I was a mentor I’d like this set up! It’s sort of like virtual co-working, but better because you can more easily control who has access to you.
   
    ## How To Build This?
    I wonder if I can contribute to coding coach and see if I can build this? Or should I just try to make it on my own? I still need to learn backend to do this. And I probably need a mentor!

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624) and setting up my remote server.

  ## Forcing Myself To Ask Questions

  I'm usually reticent about asking questions. I don't want to bother people. I try to figure out the answer before I ask, which is often the right choice. But I take this mentality to an extreme. 
  
  I'm going to push myself to ask more than I'm comfortable with. So as soon as I had a question, I forced myself to DM that question to not just one person but two people. Which feels like overkill, and even rude to me. But it is an experiment with getting comfortable with asking.

  ## Getting Help

  I reached out to Brian Morrison II ([@bmorrisondev](https://twitter.com/bmorrisondev)), who had said I could ask him some questions. 
  ### Question:
  <img src="log_imgs/que1_6-6.PNG" width="500"/>

  ### Answer:
  <img src="log_imgs/ans1_6-6.PNG" width="500"/>

  ## What Distro Does My Go Daddy Account Use?

  I tried to find out what my distro was through the command line but none of these worked except the first one which only gives the OS, but not the distro: [How to check OS and version using a Linux command [duplicate]](https://unix.stackexchange.com/questions/88644/how-to-check-os-and-version-using-a-linux-command)

  ## Apache, Ubuntu, What???

  I contacted GoDaddy on chat and asked what distro of Linux my server uses. The agent on chat said "Apache." I figured out that they were wrong: Apache is not a distro, it's something for servers, I'm not quite sure.

  ## VPS vs Dedicated vs Shared Hosting
  I called GoDaddy later and got an agent who was more knowledgeable. He told me that the distro was Cent 0S. Since I have a shared hosting plan I can't changed that. But If I got a Virtual Private Server or a Dedicated Server I could change the OS. 

  He explained that a Dedicated Server is expensive, it's literally an entire server, sort of like owning your own computer. A Virtual Private Server is in between. I didn't ask more about it, but I imagine it's something like owning a partition of a server.

  Here's a post on the differences between [Shared vs VPS vs Dedicated vs Cloud Hosting](https://wp-rocket.me/blog/shared-vs-vps-vs-dedicated-vs-cloud-hosting/).

  ## What Exactly Is Apache?

  After the agent brought up Apache, I found out "The Apache HTTP Server Project is an effort to develop and maintain an **open-source HTTP server** for modern operating systems including UNIX and Windows," [-apache.org](https://httpd.apache.org). So it's a server.
  
  ## Apache vs. MySQL
  So Apache is a server, but then what is mysql? *"MySQL is an open-source relational database management system,"* [-Wikipedia](https://en.wikipedia.org/wiki/MySQL). Oh right, those are very different. But, in the tutorial we're using MySQL server. So is that in place of an Apache server? I think these things are all part of the "stack."

  >Many people are still working with PHP servers. I love Apache and PHP but hate to say that this setup is becoming obsolete. There are good reasons for this. Node servers are used by companies to run libraries like React, which speeds development of the UI components by a large %, but Node is better even for solo developers.

  -*from [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624)*

  So, it looks like Apache uses PHP. And we're not using PHP, we're using Node.js. So we aren't using Apache.

  ## Stack

  "A **server stack** is the collection of software that forms the operational infrastructure on a given machine," 
  -*from [server stack](https://whatis.techtarget.com/definition/server-stack)*

  ### For example, the LAMP stack consists of:
  >- **Linux:** The operating system (OS) makes up our first layer. Linux sets the foundation for the stack model. All other layers run on top of this layer.
  >- **Apache:** The second layer consists of web server software, typically Apache Web Server. This layer resides on top of the Linux layer. Web servers are responsible for translating from web browsers to their correct website.
  >- **MySQL:** Our third layer is where databases live. MySQL stores details that can be queried by scripting to construct a website. MySQL usually sits on top of the Linux layer alongside Apache/layer 2. In high end configurations, MySQL can be off loaded to a separate host server.
  >- **PHP:** Sitting on top of them all is our fourth and final layer. The scripting layer consists of PHP and/or other similar web programming languages. Websites and Web Applications run within this layer.
  
  -*from [What is a LAMP stack?](https://www.liquidweb.com/kb/what-is-a-lamp-stack/)*

  I'm wondering what our stack in the Node Server Setup book is. I messaged Greg to ask him.

## Day 56, R2
### 6/5/19

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624) and setting up my remote server.

  ## Set Up SHH GoDaddy

  I followed how to [set up SHH here](https://www.godaddy.com/help/enable-ssh-16102).

  I got it to work, but instead of root I had to use my cpanel login:

  ```bash
  ssh cpanelUsername@xxx.xxx.xx.xx //replace with your cpanel username and ip address
  ```
  ## Remote Server Questions
  Now that I'm on my remove server, am I on an entirely different OS? What is it? Do I have to install MySQL-server all over again?
- ## Coding Coach
  Since backend is really new to me, I decided to get a mentor on [Coding Coach](https://codingcoach.io/). 

  I'm reading through the [Coding Coach Mentorship Guidelines](https://docs.google.com/document/d/1zKCxmIh0Sd4aWLiQncICOGm6uf38S0kJ0xb0qErNFVA/edit). It says to ***set mentorship expectations*** from the start:

  - What do you hope to achieve from the mentorship? 
  - How much time are you willing and able to commit?
  - Should the mentorship meetings be once a week, twice a week, once a month, or ad-hoc?
  - Is this mentorship open-ended or do you need just a few sessions?
  - What will your primary method of communication be?
  - How do you want to track your progress?

  It also talks about ***re-evaluating the mentorship***. I want to make it easy for mentor to end the mentorship if they no longer have time or for whatever reason. I imagine it would be hard for a mentor to confront a mentee about ending a mentorship, so I want to make it as easy as possible for them. We can make it easier on mentors by having scheduled low pressure re-evalutation sessions where they can end the the mentorship.
  

  ## Assessing Mentorship Goals

  The [Coding Coach Mentorship Guidelines](https://docs.google.com/document/d/1zKCxmIh0Sd4aWLiQncICOGm6uf38S0kJ0xb0qErNFVA/edit) suggests that you have goals for what you want from the mentorship.

  >Goals shouldn’t be too big or too small. They should be achievable within one or two sessions.

  ## Types Of Mentorship
  You can have a *technical* or *career mentorship*. I'd like both in the future, but now I'm focusing on technical. I like the idea of asking my mentor questions. Right now I have so many questions about backend.

  >You may also choose a technical mentorship structured more as a question and answer. It’s important to come to the meeting with questions already written out so you can maximize your mentorship meetings.

  ## Be A Good Mentee

  There's a whole section on being a good mentee. I live this advice:

  >Give them specific feedback about how they’ve helped you; it not only encourages them, but it also helps them become better mentors. 

  I like this because I think feedback is precious, and I read about how showing appreciation for someone can help them avoid burnout. I don't want a burnt out mentor!
  
  >...if in doubt, you can be direct about the best times/days to communicate.

  I think this applies to anything: if in doubt ask and make it easy for the mentor to be honest.

  ## What Do I Want From My Mentor?

  - Help directing me where to go with learning backend
  - Answer my questions related to backend

  Not necessary, but beneficial:
  - Someone who can work with me at times that I code (mid day-afternoon Pacific Time). I have limited access to electricity so this would be helpful, however I can probably figure it out.

  ## How would I want meetings to go?
  Ideally:
  - Occasional planned 15-30 minute phone/video conversations 
    - Estimated time: **1 per week - 1 per month**
  - Regular **"virtual co-working"**: meaning they keep themselves open to questions (text chat/voice/or video depending on the complexity of the question) during certain agreed upon times. This would virtually replicate being in same room, working by ourselves, but I could turn to them and ask them a question when I come across something. 
    - Estimated Time: **1-3 virtual co-working sessions per week, 2 hours at a time** (but the actual questions/answers for each session would probably take up **no more than 15 minutes during that time**)

  I haven't done anything like this, so I don't know how it would go. I'd be open to change this setup, of course.

  ## "Dating Period"

  I'm wondering if we should have a period where we see if we are a good fit?

  ## Find A Mentor
  Now that I know what I want from my mentor, it's time to [find a mentor](https://mentors.codingcoach.io/) and contact them.

  This was really hard! There were so many good options. I didn't know if I should contact everyone or just one person. I decided to start with one.
  
  I decided to first contact this guy who posted videos of himself teaching on twitch. Weird criteria, but I really like when people are comfortable on camera. I think those people are genuine and trustworthy. He had a really nice website. He's from near my hometown. He's self-taught, knows javascript and node.js. I think he will be able to help with backend if he knows node.js. I also like that he posts videos of himself coding, so you know he likes helping others.

- ## Thoughts and Feelings:

  I spent a lot of time looking into coding coach instead of coding today, but I think it was an important investment in my time that will highly contribute to my future coding. It was hard to pick who to contact. What if I pick the wrong person? Did I spend too much time thinking about this or not enough?

## Day 55, R2
### 6/4/19

- ## Markdown Editor
  
  I write in the Github markdown editor every day, but it's not so great. Switching between the *Preview Changes* tab and the *Edit File* tab is a time waster. I also can't spellcheck directly on Github. I want to try a different way to write markdown.
  
  ## VSC Markdown
  I found this extension for VSC: [Markdown All in One](https://marketplace.visualstudio.com/items?itemName=yzhang.markdown-all-in-one).
  
  I got that extension but I'm not sure how necessary it is because it looks like [visual studios supports markdown out of the box](https://code.visualstudio.com/docs/languages/markdown). To see a side by side, plain text vs rendered pane, hit `cmd + K` and then `k`.

  <img src="log_imgs/sidebyside_6-4.PNG" width="700px"/>

  ## Markdown Preview Github Style: Fail
  I wanted the vsc markdown preview to render like Github. so I installed this [Markdown Preview Github Styling](https://marketplace.visualstudio.com/items?itemName=bierner.markdown-preview-github-styles0) extension but it didn't do anything and there was very little documentation.


  ## Markdown Spellcheck
  I added a spellcheck extension called [Code Spell Checker](https://marketplace.visualstudio.com/items?itemName=streetsidesoftware.code-spell-checker). It's ok, but it doesn't show suggested spellings.

- ## Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624).

  ## Questions From Yesterday

  Yesterday, I was [confused by a few things.](https://github.com/DashBarkHuss/100-days-of-code/blob/master/r2-log.md#confused-6319)

  I'm going to go through what confused me and try to understand better.

  ## **Question:** Why would you need to make a new password/username? Why not just use root? What's the difference?
    
    <img src = "log_imgs/users_6-3.PNG" width="500"/>

    Greg ([@js_tut](https://twitter.com/js_tut)) reached out to me and answered this one.

    >the other users are just users with system privileges
    >
    >root is the main user added during installation
    >
    >others can be added, but with reduced access (cant delete database for example, only access it)
    >
    >whenever there are multiple users, it usually means difference in privileges: admin vs normal users for example
    
    I thought it was something like this, but now I understand that these users can be regular app users. I didn't understand that before. I thought adding *any* user would just add an *admin*. **I hadn't really thought through what it meant to add a user.**

  ## **Questions:*** Localhost vs Remote Server
    I had questions about localhost and the remote server:

    - **Question:** If I wanted to use a remote server, how do I find my *remote server address?* 
  
    - **Question:** What is a remote server? Is that like when I pay for Go Daddy hosting? Do I get a remote server address?

    Greg ([@js_tut](https://twitter.com/js_tut)) DM'd me:

    >on localhost, you can use "localhost" or "127.0.0.1" where you would use a remote address otherwise (yes its basically a hosted address, like godaddy etc)
    >
    >bind is usually set to 127.0.0.1 on localhost, if its skipped i think it will assume localhost
    >
    >because its like connecting to the mysql server from the program thats running on the same computer......

  ## Localhost:
  I read more about what [localhost actually is here](https://whatismyipaddress.com/localhost), but I didn't finish reading.
  ## **Question:** Do I skip this step if I'm just putting MySQL server on localhost?:
  
  >To log in to your web host open Terminal on your Mac and execute the command ssh root@XXX.XX.XX.XX and replace the X’es with your actual remote server address. Enter password, and you’re in!

  What is this command?: `ssh root@XXX.XX.XX.XX`

  ## SSH Command
  What is SSH?
  >SSH or secure shell, is a very widely used protocol for connecting to systems remotely to manage or use them.

  -*from [Learning SSH, lynda.com](https://www.lynda.com/Developer-Network-Administration-tutorials/Welcome/189066/365610-4.html?srchtrk=index%3a1%0alinktypeid%3a2%0aq%3assh%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2)*

  >SSH will get you to the command prompt of a remote system.

  -*from [Learning SSH, lynda.com](https://www.lynda.com/Developer-Network-Administration-tutorials/What-you-should-know-before-starting-course/189066/365611-4.html?srchtrk=index%3a1%0alinktypeid%3a2%0aq%3assh%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2)*


  So since I'm not trying to login to a remote server, I think I don't need this step. But it would be cool to try this tutorial with a remote server. I'm wondering how to do that.

  ## GoDaddy Remote Server

  I have some websites on GoDaady so I want to try use it as my remote server. I contacted GoDaddy in their chat box:

  >**Me:** I'm following a tutorial on setting up a server with Node. It says to log in to my remote server on the command line: ssh root@XXX.XX.XX.XX- and replace the X'es with your actual remote server

  The agent was really helpful. They showed me where I could find the IP address.

  I still can't login because the agent told me I have to [set up SHH here](https://www.godaddy.com/help/enable-ssh-16102). So I will do that tomorrow.

  
  ## **Question:** Why didn't I have a configuration file? How can MySQL server work without a configuration file? If mine worked without it, why would MySQL sometimes need one and sometimes not?
  This was my last question from yesterday, but I didn't get to look into it today.



## Day 54, R2
### 6/3/19
- ## MySQL & Node
  Continuing with Greg's book, [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624), and trying to get MySQL working.
  
  ## Uninstall MySQL: Success
  I was finally able to remove MySQL. I followed these instructions Greg ([@js_tut](https://twitter.com/js_tut)) suggested: [Uninstall MySql on a Mac OS X](https://community.jaspersoft.com/wiki/uninstall-mysql-mac-os-x)

  Now `which mysql` returns nothing! Yay!

  ## apt-get?
  I thought it would be best to get apt-get and download MySQL and MySQL server that way so I could follow Greg's tutorial but in [this thread](https://unix.stackexchange.com/questions/80711/how-to-install-apt-get-or-yum-on-mac-os-x) the answers say not to use apt-get on the Mac OS.
  
  ## MySQL Server UI Installer
  I decided to skip downloading MySQL and just download the MySQL server. I think the server *also* downloads MySQL. I think that's what caused my problems with the password yesterday. I think I accidently reset the MySQL password when I downloaded MySQL server. I followed this video tutorial: [How to Install and Configure MySQL Server on macOS Mac OS X](https://www.youtube.com/watch?v=Bgzf0AH1x-Y)
  
  I used vim instead of nano because the tutorial used vim and it was confusing to use. So I had to follow this page: [Vi / Vim: Save And Quit The Editor Command](https://www.cyberciti.biz/faq/linux-unix-vim-save-and-quit-command/)
  
  ## Back To Node.JS - Server Setup Book
  On page 36 of the Node book, section ***5.0.6 Make MySQL Open For Remote Access***, we edit the config file. But I had no config file.  I had to add the configuration file myself. Used the 2nd to last step in this tutorial for adding the [config file](https://blog.joefallon.net/2013/10/install-mysql-on-mac-osx-using-homebrew/).
  
  On page 38 of the book, it says to press `control` + `O` and then enter to save the file. This didn't work for me. I have to do `control` + `X`.
  
  ## Confused 6/3/19:
  ***Chapter 5: Setting Up MySQL Server*** is confusing me so much. 
  
  I got early access to the book so it's still a work in progress, so there will probably more clarifications in the next updates.
  
  ### My questions 6/3/19:
  
  - Do I skip this step if I'm just putting MySQL server on localhost?:
    - >To log in to your web host open Terminal on your Mac and execute the command
ssh root@XXX.XX.XX.XX and replace the X’es with your actual remote server
address. Enter password, and you’re in!

  - If I wanted to use a remote server, how do I find my *remote server address?* 
  
  - What is a remote server? Is that like when I pay for Go Daddy hosting? Do I get a remote server address?

  - Why didn't I have a configuration file? How can MySQL server work without a configuration file? If mine worked without it, why would MySQL sometimes need one and sometimes not?
  
  - Why would you need to make a new password/username? Why not just use root? What's the difference?
    - >Now we have user felix ready to log in to the database via localhost address with
provided password. This is the account you can use in your JavaScript code when
we get to the Chapter 6: Adding MySQL to api.js
      >
      >...now you can log into your MySQL server using a MySQL client
such as Sequel Pro on Mac or MySQL Workbench on Windows, using the login
token pair felix/PassWord557123! we created earlier.

      ![users screenshot](log_imgs/users_6-3.PNG)
  
  - Do I skip this step if I'm using localhost for my server?:
    - >Comment the line bind-address = 127.0.0.1 by adding # to the front:
  
  
  

## Day 53, R2
### 6/2/19

- ## MySQL & Node
  I'm continuing Greg Sildenikov's [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624), trying to get mysql-server to work. It was really hard to figure out since there aren't instructions for doing it on a mac yet in his book. I didn't get it to work.
  
  ## MySQL Server
  I followed this tutorial for getting mysql server, the section called [3. Using Homebrew service to download](https://tableplus.io/blog/2018/11/how-to-download-mysql-mac.html#3-using-homebrew-service-to-download).
  
  Then I went back to  Greg Sildenikov's [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624) on page 32 and ran `mysql -u root -p`. It worked but `mysql -u root -p` worked before following the instructions for mysql-server. So I'm not sure if that was necessary to get mysql-server: did I already have it? Is it doing something different that I don't understand/can't see? I'm not sure what it did.
  
  ## 5.0.6 Make MySQL Open For Remote Access
  In this section in the book Greg says:
  
  >...you need to find the actual mysql configuration file which should reside in one of the folders specified with !includedir.
   
  What does **specified with !includedir** mean?
  
  I'm not seeing any configuration file or related folder in etc. So maybe I still don't have mysql-server? When I run `which mysql-server` it shows nothing.
  
  ## UI Download
  I tried installing mysql server another way, using the [ui installer](https://dev.mysql.com/downloads/file/?id=486026). But when I run `which mysql-server` I'm still getting nothing. And I still don't see any configuration file or mysql folder in the ect directory.
  
  ## Another Download Method
  Here the author uses homebrew to install mysql-server but it's a different way than what I've already done: https://blog.joefallon.net/2013/10/install-mysql-on-mac-osx-using-homebrew/
  
  I tried to follow this tutorial but I ran into this error.
  
  ```bash
  Sun Jun 02 ~ Dashie$ mysql_secure_installation

  Securing the MySQL server deployment.

  Enter password for user root: 
  Error: Access denied for user 'root'@'localhost' (using password: YES)
  ```
  
  And now I can't log in with `mysql -u root -p` even with the password I wrote down. I'm sooo confused.
  
  ```bash
  Sun Jun 02 ~ Dashie$ mysql -uroot -p
  Enter password: 
  ERROR 1045 (28000): Access denied for user 'root'@'localhost' (using password: YES)
  ```
  
  ## Uninstall MySQL
  So now in addition to not being able to use mysql-server, I can't get into mysql itself. I wonder if it's because I downloaded mysql-server a bunch of times and in a bunch of different ways. Maybe I accidently changed the password for mysql. I figure, I should uninstall and reinstall mysql to change the password back.
  
  I uninstalled mysql the way I did yesterday, but instead of installing it the way the post I linked to yesterday said, I followed this: https://blog.joefallon.net/2013/10/install-mysql-on-mac-osx-using-homebrew/.
  
  But then I got this error:
  ```bash
  Sun Jun 02 ~ Dashie$ mysql.server restart
  ERROR! MySQL server PID file could not be found!
  Starting MySQL
  ... ERROR! The server quit without updating PID file (/usr/local/var/mysql/dashielusssMBP2.jetpack.pid).
  ```
  
  So I uninstalled mysql with brew again. But it doesn't seem to be working because `which mysql` still shows that I have mysql:
  
  ```bash
  Sun Jun 02 ~ Dashie$ brew remove mysql
  Error: No such keg: /usr/local/Cellar/mysql
  Sun Jun 02 ~ Dashie$ which mysql
  /usr/local/mysql/bin/mysql
  ```
  
  I can't remove mysql.
  
  I am so confused and lost and frustrated today.
  
  <img src="log_imgs/uchh_6-2-19.png" width="400"/>

## Day 52, R2
### 6/1/19

- ## MySQL
  Yesterday, I ended with an error when I tried to log into MySQL.
 
  ```bash
  mysql -u root -p 
  >ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)
  ```
  I think it's because I don't remember my password. 
  
  ## MySQL Password
  I could try to guess my password, but I'm really wondering how I can find out what my password is? Shouldn't I be able to access my own password? Who am I trying to keep out? This is only on my computer?
  
  I wanted to see when I installed MySQL incase I need to delete it and reinstall it. I wanted to make sure I added it recently and I hadn't used it a while ago for something important taht I don't remember. But:
  
  >UNIX doesn't support creation date, windows does.
  
  -from *"[what's the command to find the creation date of a certain dirctory?](https://www.unix.com/shell-programming-and-scripting/157874-creation-date-directory.html)"*
  
  If Unix doesn't support creation date, then how does finder have a creation date?
  
  <img src="log_imgs/finder_6-1.PNG" width = "500">
  
  I ended up finding MySQL in my Finder by getting the path for MySQL using `which mysql` in the command line, then going to "Go" in finder and finding where mysql was. 
  
  <img src="log_imgs/go_6-1.PNG" width = "500">
  
  It showed that I installed MySQL on August 1, 2018, this past summer. So I don't think I have anything important that I could lose since then. I probably was just following a tutorial at the time or something. I don't know, had I had any databases, if uninstalling MySQL would cause any databases to be lost, but either way it won't matter: I probably don't have anything important. So I could uninstall it and reinstall it to reset the password if I have to. 
  
  ## Where Is The Password Stored?
  
  > MySQL passwords are stored in the user table of the mysql database and are encrypted using it's own algorithm.
  
  -from *[Security: Where are MySQL passwords stored?](https://serverfault.com/questions/326649/security-where-are-mysql-passwords-stored)*
 
  Since it's encrypted, I guess you can't go trying to get in there somehow and get the password.
  
  More info on password here: [MySQL Documentation: 6.2.1 Account User Names and Passwords](https://dev.mysql.com/doc/refman/8.0/en/user-names.html)
 
  ## Uninstalling and Reinstalling MySQL
  I followed [this tutorial](https://medium.com/@devontem/solved-cant-connect-to-local-mysql-server-through-socket-tmp-mysql-sock-2-f52c9c546f7) on uninstalling and reinstalling MySQL. This fixed the poster's error. But Not mine.
   
  After uninstalling and reinstalling MySQL, I suddely remembered where I saved my password, my Keychain Access! duh. Uninstalling and Reinstalling didn't give me an option to input a new password, so my old password should probably still work. But I still got the same error.
    
  ## Solution
  
  Someone posted the [same error on stackoverflow](https://stackoverflow.com/questions/15016376/cant-connect-to-local-mysql-server-through-socket-homebrew/15016441) and linked to this [other post about a different error](https://stackoverflow.com/questions/4359131/brew-install-mysql-on-macos/6378429#6378429). The solution to this other error worked for me. It's in this stackoverflow thread, the [instructions that Lorin Rivers gave.](https://stackoverflow.com/questions/4359131/brew-install-mysql-on-macos/6378429#6378429)
  
  ### Notes:
  - ### Step 2: I skipped step 2.
  - ### Step 4:
  In Step 4, change the file to the MySQL version you just downloaded.
  
  <img src="log_imgs/step4_6-1.PNG" width="500">
  
  <img src="log_imgs/mysqlversion_6-1.PNG" width="600">
  
  - ### Prompts:
  
  This is what I did for the prompts, though I didn't understand all of these fully:
  
  ```bash
  By default, a MySQL installation has an anonymous user,
  allowing anyone to log into MySQL without having to have
  a user account created for them. This is intended only for
  testing, and to make the installation go a bit smoother.
  You should remove them before moving into a production
  environment.
  Remove anonymous users? (Press y|Y for Yes, any other key for No) :
  ```
  I said yes
  
  ```bash
  Normally, root should only be allowed to connect from 'localhost'. This ensures that someone cannot guess at the root password from the network.

  Disallow root login remotely? (Press y|Y for Yes, any other key for No) : 
  ```
  I said yes
  
  ```bash
  By default, MySQL comes with a database named 'test' that anyone can access. This is also intended only for testing, and should be removed before moving into a production environment.

  Remove test database and access to it? (Press y|Y for Yes, any other key for No) :
  ```
  I said no, because I thought it might be good to see an example of a database.
  
  ```bash
  Reloading the privilege tables will ensure that all changes made so far will take effect immediately.

  Reload privilege tables now? (Press y|Y for Yes, any other key for No) : 
  ```
  I said yes, because why wouldn't I want it to take effect immedietly? What's the alternative?
  
  - ### Step 5:
  I didn't understand Step 5 from the stackoverflow post. But MySQL started working again, so I am good! I guess I didn't need anything after step 4. 
  
  And a tip from lil Dashie: go to your Keychain Access (or whatever it is on Windows) and manually add your mysql password. Although, that was wasn't my problem here. It's just a good tip.
  
  ## Where Does MySQL Start And SQL End?
  Whenever I learn something new this is always a confusion point. Where do things start and end? Where does Javascript end and the Web API start? What part of this project is Node and what part is javascript? What's the difference between the MySQL program and the MySQL server? Github vs Git? Technologies blend together and I usually can only separate them after some time with using them.
  
  I went back to the tutorial: [Learning MySQL Development](https://www.lynda.com/MySQL-tutorials/Up-Running-MySQL-Development/158373-2.html) on lynda. We're learning commands to show tables/databases/columns etc. But I don't know if they are MySQL commands or SQL commands.
  
  This page compares the two: [SQL vs MySQL: What's the Difference?](https://www.guru99.com/sql-vs-mysql.html):
  
  >SQL is a language which is used to operate your database. SQL is the basic language used for all the databases. There are minor syntax changes amongst different databases, but the basic SQL syntax remains largely the same.
  
  So my immediate thought is that it's SQL. However, looking into it [furthur online](https://www.quora.com/How-do-you-view-all-the-tables-in-SQL), it looks like some of these commands are specific to MySQL.
  
  ## Documentation
  You can find the documentation for MySQL here: [MySQL Online Manual](https://dev.mysql.com/doc/refman/8.0/en/)
  
  

## Day 51, R2
### 5/31/19

- ## MySQL
  I decided to look more into MySQL today. I followed [Learning MySQL Development](https://www.lynda.com/MySQL-tutorials/Up-Running-MySQL-Development/158373-2.html) on lynda.
  
  The internet is not great today, so we'll see how much I can do.
  
  ## SQLFiddle
  You can use SQL Fiddle to play with SQL:
  http://sqlfiddle.com/
  
  ## myPHPadmin Demo
  You can use myPHPadmin Demo to play with SQL:
  https://demo.phpmyadmin.net/master-config/server_sql.php?lang=en
  
  ## Database Normalization
  A way to organize your fields and your tables. The accepted standard of database normalization is Third Normal Form (3NF). It includes First and Second Normal Form.
  
  ### First Normal Form:
  Each record has a single value.
  
  ***Isn't a record an entire row? That is not what the instructor means...***
  
  ### Second Normal Form
  - Must be in 1NF
  - Fields not defining a row depend on fields that do. All the information in the table is dependant on what defines the row.
  
  ### Third Normal Form
  - Must be in 1NF
  - Must be in 2NF
  - All the non-defining fields are directly dependent on the defining fields.
  
  I'm confused how 3NF is different from 2NF. ***"All the non-defining fields are directly dependent on the defining fields."*** sounds exactly like, ***"Fields not defining a row depend on fields that do. All the information in the table is dependant on what defines the row."***
  
  ## 3NF vs 2NF
  I watched videos from another lynda tutorial to understand the differences.
  
  ### 2NF: 
  All fields in the primary key are required to determine the other non-key fields.
  
  -***Paraphrased** from [Learning Relational Databases](https://www.lynda.com/Access-tutorials/Second-normal-form/604214/648094-4.html)*
  
  ### 3NF:
  Follows a similar pattern to 2NF but instead of checking against the individual components of a composite primary key, we'll check each *non-key column* against every other *non-key column*. Here, we're looking for columns that are functionally dependent on another piece of information that isn't the promary key. For instance you might try storing the state, and the state abbreviation in an address table. Since state abbreviation is entirely dependent on the state or vice versa, one of those two fields should be removed from the table.
  
  -***Paraphrased** from [Learning Relational Databases](https://www.lynda.com/Access-tutorials/Third-normal-form/604214/648095-4.html)*
  
  ### Denormalization
  When you intentionally ignore some of the normalization rules. This is done usually to increase performance, but it's only used when absolutely necessary.
  
  ## DDL
  Data definition langauge: a set of SQL commands you can use to define tables and databases. It usually involves changing how the data is stored.
  
  ## DML
  Data manipulation langauge: how you manipulate data or change it.
  
  ## OLAP
  Online Analytical Processing: An application that does a lot of reports
  
  ## OLTP
  Online Transactional Processing: An application that does a lot of transactions 
  
  ## Some Terms
  **Column:** Field
  
  **Row:** Record (entire entry)
  
  **Queries:** Commands used to send statements to a table. DDL, DML, or reading and searching information from a table is  how we query tables.
  
  **Table:** a collection of records. A table stores the records that are described by the fields in their row.
  
  **Database:** a container that holds tables
  
  **Schema:** database
  
  **Changing a schema/creating a schema:** changing the definition of what is in the database.
  
  ## Can't Run MySQL
  
  When I run: 
  
  `mysql -u root -p`
  
  I get this error: `ERROR 2002 (HY000): Can't connect to local MySQL server through socket '/tmp/mysql.sock' (2)`
  
  I get it no matter what password I put in, so maybe it's just because I don't remember my password. I don't know what password this is refering too.
  
  ## MySQL Password
  I'm not sure if I just forgot my password. I was thinking, maybe if I just uninstall and reinstall mysql I can make a new password. But what if this messes up some databases I have on here? I didn't put any mysql data bases on here, but what if some other program did? Is it possible? 
  
  I want to see the date that I installed MySQL. I couldn't find a way in bash to list the date created. But I couldn't find the MySQL folder in the GUI, even after pressing `cmd` + `shift` + `.`. So I couldn't see the date created by looking in the GUI either.
  
  I'll have to investigate more tomorrow.

## Day 50, R2
### 5/30/19

- ## Node
  Continuing Greg Sildenikov's [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624).
  
  ## Setting Up MySQL Server
  This next section confuses me. It talks about installing mysql on Windows and Ubuntu, but not Mac. Luckily Greg is pretty responsive on twitter so I wrote to him to tell him what confused me.
  
  ## Correspondence With Greg
  Greg got back to me and clarified a few things. He said he will include more clarification based on my feedback and questions.
  
  ## MySQL vs MySQL Server
  I was confused about the difference between mysql and mysql server.
  
  Greg said:
  >Well, mysql is a program just like any other. It runs on your computer.
  >
  >But when you write code, you have to install mysql modules to add to the node server with require keyword... that's  separate from the actual mysql program
 
  -*Greg Sidelnikov aka [JavaScript Teacher @js_tut](https://twitter.com/js_tut), Twitter DM*
 
  I'm not sure, but I think he is referring to the difference between MySQL and the mysql node module. I don't think mysql-server is a node module, but I'm not sure.
  
  ## What is MySQL
  
  >MySQL is an Oracle-backed open source relational database management system (RDBMS) based on Structured Query Language (SQL).
  
  -*[MySQL DEFINITION ](https://searchoracle.techtarget.com/definition/MySQL)*
  
  ## What is MySQL Server
  
  >MySQL server is the MySQL database software acting as a server/service towards clients. It is a concrete installation of MySQL.
  
  -*[Olaf Doschke, Quora: What is the difference between a MySQL server and a MySQL client?](https://www.quora.com/What-is-the-difference-between-a-MySQL-server-and-a-MySQL-client)*
  
  >Mysql is a SQL implementation.
  >
  >Mysql server is the application that runs mysql database system, where all the data is stored and processed.
  
  -*[Nicolas Martin Gonzalez, Quora: What is the difference between MySql & MySql Server?](https://www.quora.com/What-is-the-difference-between-MySql-MySql-Server)*
  
  ### Download MySQL:
  Greg said there was a UI to download the MySQL Server.
  
  I found this: [Download MySQL Community Server](https://dev.mysql.com/downloads/mysql/)
  
  Also when I go to the [MySQL github](https://github.com/mysql) there's a repo for MySQL server.
  
  I'm confused: It seems like MySQL server is required if you want to use mysql. So why have them separate? When would you ever need one without the other? Why download one and not the other? Why aren't they just part of the same package?
  
  ## More On MySQL
  I decided to do some more exploring of MySQL.
  
  >MySQL Community Server. When People talk about using MySQL, this us usually what they mean.
  
  -*[MySQL: Installing and Running Ruby on Rails on Mac](https://www.lynda.com/Ruby-Rails-tutorials/MySQL/500549/533096-4.html)*
  
  So maybe MySQL and mysql server do usually go together.
  
  
  
- ## Thoughts and Feelings:
  I spent a bit of my coding time talking to Greg. Part of the time we were talking about the book and he was helping me. For some of the time we were talking about ideas tangentially related to coding. So I dont know if that's cheating! But I'll count it for today.
  
  We talked about feedback. It's important to both give and recieve honest criticism. We shouldn't think of these things negatively. They are useful. It's a mitzvah to give constructive criticism and it's a blessing to recieve it. It's the only way to get better.
  
  Don't be afraid that you'll hurt someone's feelings by giving feedback. People don't just want your flattery. People who are passionate about their work love constructive criticism. The more specific and detailed the better. Likewise, if you want to excell you need to crave feedback too. And you need to let go of your bruised ego and grow a pair of tits, bro! Or should I say, bra? In my case, I have tits, so I needed to grow balls. Now I am complete.
  
  We also talked about embracing stupidity. You should never be afraid to look stupid. Stupidity is a blessing. It's a compass that shows us where we need to go next. When you feel confused, you know that you need to clarify that confusion. If we never felt stupid we wouldn't know what we need to learn next. Understanding your stupidity is it's own kind of wisdom. And, here too, we need to get over our egos. We are all stupid. Only those who ask the stupid questions will get past their stupidity.

## Day 49, R2
### 5/29/19

- ## Node 
  Continuing Greg Sildenikov's [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624).
  
  It's cool to see how things work behind the scenes. I can see how the server loads  the favicon for example:
  
  <img src="log_imgs/favicon_5-29.PNG" width="500">
  
  ## Debugging In Node, Server
  
  The last time I looked into debugging in node I found that you can [debug in VSC](https://github.com/DashBarkHuss/100-days-of-code/blob/master/r2-log.md#debbuging-node-in-vsc). 
  
  However, I'm still having trouble. It seems ***VSC doesn't like to keep the debugger on when I'm also running the file.*** 
  
  ### So how can I debug my server?
  
  For example, on line 14 we have the variable `extension`. 
  
  <img src="log_imgs/debug_5-29.PNG" width="500">
  
  It's made up of other expressions and variables which make it long and confusing.
  
  Sometimes I like to get a clearer picture of a confusing variable by using the DevTools debugger to pause the script and select and hover over all the expressions to see the value of each expression, like this:
  
  ![debugger](log_imgs/devtools_5-29.gif)
  
  I don't see a way of doing that with node and the VSC debugger. 
  
  I cannot run the server ***and*** the debugger at the same time. So I can't pause the script after a request is made to the server and then see the values of the expression `extension`. I wonder what the solution to this is?
  
  ### Possible Solutions:
  - Maybe there's a way of debugging that I don't know about
  - Could unit testing have something to do with this?
  - console log everything
  
  For now, I will console log everything, but there's got to be a better way.
  
  ## Building An API
  >So far we learned how to serve requested files. But if you want to build real web
applications, you might want to integrate support for API endpoints.
  >
  >An endpoint is a URL that follows a special pattern. We can intercept this pattern
and upon its presence we will execute an API command, instead of serving a file
  
  -*Greg Sildenikov, [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624).*
  
  When an API end point is requested, we cancel serving the file, and instead run some server-side commands and return a result.
  
  ## Static Keyword
  In our API class from page 26 of [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624), we use the keyword, `static`:
  
  ```javascript
  static catchAPIrequest(v) {
    // function body etc...
  ```
  
  ### What is the static keyword?:
  >The static keyword defines a static method for a class. Static methods aren't called on instances of the class. Instead, they're called on the class itself.
  
  -*from [static, Mozilla Docs](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Classes/static)*
  
  ## Branching Out
  Greg uses the term *"branching out"*:
  
  >We used boolean result of API.catchAPIrequest to branch out.
   
  -*Greg Sildenikov, [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624).*
  
  ### What is branching out?:
  >branching is a way to make a program behave differently based on Boolean expressions (e.g., involving user input)
  
  -*from [Chapter 4: Boolean expressions and Branching](https://hank.feild.org/feild-guide-cpp/conditionals.html)*
  
  ## Confused About Mac:
  In next chapter, Chapter 5: Setting Up MySQL Server, the first two sections are:
  - 5.0.1 Install MySQL Server Locally on Windows
  - 5.0.2 Install MySQL Server on Ubuntu
  
  But nothing on installing it on a Mac.
  
  A few days ago, when I was confused about installing node on mac, Greg said:
  
  >(Ps: windows global installation is unique to the OS, on Linux you can call mysql from anywhere by default)
  
  Is this what he was talking about?
  
  It doesn't say anything about skipping these sections if you're on a Mac.
  
  Times up for today. I will figure it out tomorrow.
  
## Day 48, R2
### 5/28/19

- ## Node 
  Continuing Greg Sildenikov's [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624).
  
  ## Serving Files
  
  I'm now able to serve up files after following the code on page 23 of [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624).
  
  ## Review: Adding ESlint & prettier Locally
  Taking a break from the Node tutorial inorder to set up Eslint and Prettier in this project.
  
  Since this is local, all commands should be run in your project directory.
  ### ESLint
  1. added .eslintrc.json
  2. `npm init`
  3. `eslint --init` (make sure you get to the point where it asks if you'd like to install airbnb style dependancies)
  ### Prettier
  1. add `"prettier"` to `"extends"` and `"plugins"` in .eslintrc.json
  2. Configure prettier in .eslintrc.json in `"rules"`
  - ```javascript
    "rules": {
      "no-console": "off",
      "prettier/prettier": [
        "error",
        {
          "singleQuote": true
        }
      ]
    }
    ```
  3. `npm install --save-dev eslint-plugin-prettier`
  4. `npm install --save-dev eslint-config-prettier`
  5. This is already set up for my workspace but incase I change work spaces or for anybody else's benefit:
  - in User settings:
    ```javascript
    "editor.formatOnSave": true,
    "[javascript]": {
      "editor.formatOnSave": false
    },
    "eslint.autoFixOnSave": true,
    ```
  ## JSON Formatting
  I saw a cool way of formatting JSON in [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624):
  
  ### Colons/Values Aligned:
  <img src="log_imgs/colon_5-28.PNG" width="600"/>
  
  ### Instead of Properties Aligned:
  <img src="log_imgs/linestart_5-28.PNG" width="500"/>
  
  I wonder how this is done. I like how it looks.
  
  Couldn't find much online, I asked on twitter.
  
  ## ESLint: No Shadowing
  > Shadowing is the process by which a local variable shares the same name as a variable in its containing scope. For example:
  >```javascript
  >var a = 3;
  >function b() {
  >  var a = 10;
  >}
  >```
  
  
  ## Spellcheck When Writing In Markdown?
  Is there any way to enable spellcheck while I'm writing in my github markdown log? I have so many typos.
  
- ## Thoughts and Feelings:
  I got good news today related to the bad news that I got a few days ago. I'm so relieved. My focus is so much better. I'm so grateful. The issue is still going on, but the severity is not as extreme as previously thought. 
  


## Day 47, R2
### 5/27/19

- ## Node / Unix
  Today, I'm going to focus on Node and Unix again which I was last learning about on [May 23rd](https://github.com/DashBarkHuss/100-days-of-code/blob/master/r2-log.md#day-43-r2). 
  
  ## Windows Vs. Linux
  
  Last time I ran node from the terminal it worked without having to set up the  environment variables. I was surprised by this. But Greg Sidelnikov reached out to me and explained:
  
  >(Ps: windows global installation is unique to the OS, on Linux you can call mysql from anywhere by default)
  
  -*Greg Sidelnikov aka [JavaScript Teacher @js_tut](https://twitter.com/js_tut)*
  
  I don't know what mysql has to do with it, but I guess you can run node anywhere on a mac.
  
  <img src="log_imgs/terminal1_5-27.PNG" width="300"/>
  
  ## C:
  I know `C:` has something to do with paths and directories but what exactly does it mean?
  
  It's difficult to google questions that involve special symbols so I used [SymbolHound.com](http://symbolhound.com/) to search `C:`. Otherwise, google usually ignores punctuation and special symbols.
  
  >"Windows displays partition labels as uppercase letters (C:)." 
  
  -*[Stackoverflow](https://stackoverflow.com/questions/93228/is-a-hard-drive-partitions-label-upper-or-lowercase-c-or-c)*
  
  It looks like `C:\` is a way that windows represents a harddrive partition. I think you can have other letters for other partictions. Then a path would look like this: `C:\somefolder/somefile`. Since I don't use windows this was new to me. Atleast, I think it's just a windows things.
  
  ## Where Are Core Node Modules Stored?
  
  >Modules are stored in your node installation folder (C:\Program Files\nodejs
in our case) under node modules.
  >You might also find them in nodejs\npm folder:
C:\Program Files\nodejs\node modules\npm\node modules\
 
  -*Node.js – Server Setup, Greg Sidelnikov*

  ## `import`
  
  In the react tutorial from yesterday I saw the `import` keyword and was wondering what it was:
  
  > In browser-side JavaScript code, you can use import / export keywords. But in
addition to that, in node you can use require keyword to add useful modules to
your app from the main NPM repository.

  -*Node.js – Server Setup, Greg Sidelnikov*
  
  So it looks like it's basically the same as require but it's browser-side JavaScript code. So does that mean it's part of javascript or part of the Web API?
  
  It's also nice to know that ***`require()` is just pulling from the NPM repository***. I think that means the folders where the core modules are stored (as mensioned above). So, this clarifies how `require()` works and why you don't need to include the path for the core modules.
  
  ## Importance of a Node Server Continuously
  
  Here Greg is comparing the process of running a simple javscript file in node with running a node server continuously:
  
  >After executing index.js the node process quits, and
control is given back to cmd. This won’t help us run our application server. We
need to build something that runs continuously so we can serve a file every time
someone will access our server from their browser.

  -*Node.js – Server Setup, Greg Sidelnikov*
  
  So, this makes me question what we are actually about to do?:
  
  ***Can we actually deploy a site to the web if we make our own server? Or do we still need heroku and netify etc to do that? What actually is the server?*** I thought when we make a server it still only runs on our own computer and other people cannot access it. That's what I thought I had done in the past when running live servers on my computer to test certain code that couldn't be tested without a server. But here Greg Says, "every time someone will access our server." 
  
  So there's definetly a lot I'm realizing I don't understand. Let's see if I can figure it out in the upcoming sections.
  
  ## Node Server!
  So cool I got a simple node server running!
  
  ```javascript
  let http = require('http');

  const ip = `127.0.0.1`;
  const port = 3000;

  http.createServer(function(request, response){
    console.log('request: ', request.url);
  }).listen(port, ip);

  console.log(`Running at http://` + ip + `:` + port + `/`);
  ```
  
  ## Serving Requested Files
  So far we only are able to send requests. You can see the requests coming in to the terminal 
  
  <img src="log_imgs/requests_5-27.PNG" width="400">
  
  But the code doesn't serve up any files.
  
  I followed the code for serving up files which can be found on page 23 of the May 14th update to Greg Sildenikov's [Node.js – Server Setup](https://www.patreon.com/posts/node-js-book-26866624). It's not free but it's only a dollar a month to support his patreon and you get all his books. If you don't have an extra dollar, you probably should read a book on getting your finances in order (I'm currently reading I Will Teach You To Be Rich). But besides that if you ask 5 strangers for 25 cents they will probably oblidge and you can then support the patreon and buy yourself a candy.
  
  ## MIME?
  What is MIME? In our code we set up a mime variable:
  
  ```javascript
  // Define acceptable file extensions
  let mime = {".html":"text/html"};
  ````
  
  >A multipurpose internet mail extension, or MIME type, is an internet standard that describes the contents of internet files based on their natures and formats.
  
  -*[What Is a File Extension and MIME Type?](https://www.lifewire.com/file-extensions-and-mime-types-3469109)*
  
  

## Day 46, R2
### 5/26/19

- ## React
  Continuing [React tutorial on lynda.com](https://www.lynda.com/React-js-tutorials/Learning-React-js/800214-2.html) with Eve Porcello [@eveporcello](https://twitter.com/eveporcello).
  
  ### Question:
  Where does `props` go when we make a class? I'm guessing in the argument to `render()`?

  ## Conditionally Render Components     
  
  ```jsx
  const Dashboard = ({name})=>(<h1>Welcome {name}!</h1>);

  const App = ({loggedIn}) => (
    loggedIn 
    ? <Dashboard name = "Dash" /> 
    : <h1>You are not logged in</h1>
  );

  ReactDOM.render(
    <App  loggedIn = {false}/>, 
    document.getElementById("root")
  );
  ```
  ![condition](log_imgs/condition_5-26.gif)
  
  ## Render Components From A List
  
    > ...each child in an array or iterator should have a unique key property. So whenever we're rendering some sort of a list, with dynamic values,  we're going to need to give each a key, so that React can re-render things appropriately according to the rendering rules.  
    
  -[Eve Porcello](https://www.lynda.com/React-js-tutorials/Render-components-from-list/800214/2202388-4.html?autoplay=true)

  
  ```jsx
  const shoppingList = [
    "Avocado", "Tahini", "Organic Grass Fed Beef", "Spinach"
  ];

  const App = ({list}) => {
    return (
    <ul>
      {list.map((item, i)=><li key={i}>{item}</li>)}
    </ul>
    )
  };

  ReactDOM.render(
    <App  list = {shoppingList}/>, 
    document.getElementById("root")
  );
  ```
  
  <img src="log_imgs/list_5-26.PNG" width="400"/>
  
  ## Render Object List
  
  ```jsx
  const ingredients = [
    {id: 1, name: "Avocado", grams: 60},
    {id: 2, name: "Tahini", grams: 20},
    {id: 3, name: "Certified USDA Organic Grass Fed Beef", grams: 130},
    {id: 4, name: "Spinach", grams: 120},      
  ];

  const App = ({list}) => {
    return (<ul>
      {list.map((item)=><li key={item.id}>{item.grams}g {item.name}</li>)}
    </ul>
    )
  };

  ReactDOM.render(
    <App  list = {ingredients}/>, 
    document.getElementById("root")
  );
  ```
  
  <img src="log_imgs/objects_5-26.PNG" width="400"/>
  
  ## Create React App
  I already had create-react-app installed. I used it to make a new project.
  
  In the directory that you want to make your project in, run the command:
  
  `create reacte app <projectname>`
  
  ## Run A create-react-app Project In The Browser
  
  First make sure you are in your new project directory. Then run `npm i` to install dependencies. 
  
  Then run `npm start` to start up the project in the browser. This will run your project on local host 3000.
  
  ### Question:
  I see all these import statements at the top again. I'm not sure what they are and the instructor didn't talk about them.
  
  ```javascript
  import React from 'react';
  import ReactDOM from 'react-dom';
  import './index.css';
  import App from './App';
  import * as serviceWorker from './serviceWorker';
  ```
  
  It looks like `require()`.
  
  ## Build For Production
  
  To put your create-react-app project on the web, you'll want to optimize it for production. You can do that really simply by running:
  
  `npm run <nameforbuild>`
  
  To run the build in the browser:
  
  `serve -s <nameforbuild>` 
  
  But this actually didn't work for me, it ran a different folder with the same name. So I have to include the entire path not just the name for build. Then I also ran `sudo npm install serve` (by accident) and `sudo npm install serve -g` and now I can just put the probect name without the path but I'm not sure why. I'm not sure where I have serve installed before. But now I have it installed in too many places.
  
  
  
## Day 45, R2
### 5/25/19

- ## React
  Continuing [React tutorial on lynda.com](https://www.lynda.com/React-js-tutorials/Learning-React-js/800214-2.html) with Eve Porcello [@eveporcello](https://twitter.com/eveporcello).
  
  ## Composing Components
  
  Composing components means putting components together to make a larger interface.
  
  ```javascript
  const Person = ({name, age})=>{
    return (
      <h1> {name}, you are {age}</h1>
    );
  };

  const Friends = () => {
    return (
      <div>
      <Person name = "Shlomo" age = "33"/>
      <Person name = "Mom" age = "old"/>
      </div>
    );
  };

  ReactDOM.render(
    <Friends />, 
    document.getElementById("root")
  );
  ```
  
  <img src = "log_imgs/friend_5-25.PNG" />
  
  ## Class Components
  
  You can also create a component using a class. Here's the Friends component from above as a class.
  
  ```javascript
  class Friends extends React.Component  {
    render () {
      return (
        <div>
        <Person name = "Shlomo" age = "33"/>
        <Person name = "Mom" age = "old"/>
        </div>
      );
    }
  };
  ```
  
  You always need the `render()` method inside the component class.
  
  ## State
  When a component's state changes the render function will be called again to rerender the state change.
  
  ```javascript
  class App extends React.Component  {
    state = {loggedIn: true}
    render () {
      return (
        <div>
        <div>The User Is {
        this.state.loggedIn 
        ? "logged in"
        : "not logged in"}.
        </div>
        </div>
      );
    }
  };

  ReactDOM.render(
    <App />, 
    document.getElementById("root")
  );
  ```
  <img src = "log_imgs/state_5-25.PNG" />
  
  There is more to this that we will find out later.
  
  > So this is a simple reflection of the state value as UI but we're going to use the state with events in order to change the state of our entire application.
  
  -[Eve Porcello](https://www.lynda.com/React-js-tutorials/Understanding-React-state/800214/2201438-4.html)
  
  ## Binding Event Functions to UI Elements
  Here we have our class from above with the state value defined, but we also add functions, `logIn` and `logOut`. Theses functions change the state. We attach them to "Log in" and "Log Out" UI buttons.
  
  ```javascript
  class App extends React.Component  {
    state = {
      loggedIn: false
    };
    logIn = () => this.setState({loggedIn: true});
    logOut = () => this.setState({loggedIn: false});
    render () {
      return (
        <div>
          <button onClick = {this.logIn}>Log In</button>
          <button onClick = {this.logOut}>Log Out</button>
          <div>The User Is {
            this.state.loggedIn 
            ? "logged in"
            : "not logged in"}.
          </div>
        </div>
      );
    }
  };
  ```
  This is called binding an event function to a UI element.
  
  ![screenrecording](log_imgs/stateevent_5-25.PNG.gif)
  
## Day 44, R2
### 5/24/19

- ## React
  I didn't have interenet on this day so I watched a tutorial that I previously downlo.aded on React. You can find it [here on lynda.com](https://www.lynda.com/React-js-tutorials/Learning-React-js/800214-2.html)
  
  A lot of the text here in this log is straight from the tutorial so, I credit a lot of the difinitions here to Eve Porcello [@eveporcello](https://twitter.com/eveporcello)
  
  ## What is react?
  React is a javascript library that’s used for building user interfaces. React aims to make developing large scale single page applications easier.

  ## Functional Javascript:
  A paradigm that emphasizes function, composition, over object orientation.

  ## Install React Developer Tools
  I  already have these installed but this will help you.

  ## Using React Offline
  Since I don’t have internet right now, I can’t link to the react library like the instructor does. How can I use react without the internet: is it a js library file: react.js? is it a node module: require(react)? I skipped to the last project file to see if the instructor uses react without the internet. There I found this: 

  ```javasript
  import React from 'react'
  import ReactDOM from 'react-dom'
  ```

  What is `import`? Is it like `require`? Is it part of javascript or node? Or maybe it’s part of the Web Api?
  
  ## Using A Relative Path Offline
  I found a `react.development.js` file on my computer that was downloaded with the exercise files for this tutorial. Great! 
  
  ## Node Module?
  This file is in a node_modules folder so I’m inferring that react is a module you can require. However, I tried requiring ’react’ and ‘reactjs’ and neither worked: `The module could not be found.`

  Maybe react is not a core module, and I was requiring it without a path which you can only do with core modules.
  
  ## Using the Files
 
  I found the two files that correspond to the links the teacher used, added them to my directory, and added links for them:
  
  ```html
  <script src="react.development.js"></script>
  <script src="react-dom.development.js"></script>
  ```

  I don’t know the difference between the two files.

  I got an error in the `react.development.js` and `react-do.development.js` files:
  `process is not defined`

  I got internet for a bit and got ***more recently versions of the react files. It works now!***

  ## JSX
  JSX, Javascript as XML, is a language extension that allows you to write tags directly in the javascript. For JSX to work you need to link to the babel library in the head and change the script `type` attribute value:

  ```javascript
  <script type="text/babel">
    ReactDOM.render(
      React.createElement("div", {
        "style": {
          "color": "hotpink"
        }
      }, <h1>hoo</h1>), //jsx
      document.getElementById("root")

    );
  </script>
  ```
  
  <img src="log_imgs/jsx_5-24.PNG" width = "400" />

  
  ## You can use variables in JSX:
  ```javascript
  const name = "Dash";

  ReactDOM.render(
    <h1 className="greet"> Hi {name} </h1>, //class is a reserved word, use className
    document.getElementById("root")
  );
  ```
  <img src="log_imgs/jsxvar_5-24.PNG" width = "400" />


  ## Components
  The way that a react application represents a UI element is with a component. A component let’s you put together a user interface with independent reusable pieces. 

  Here we create and add a `Dash` component:
  ```javascript
  const Dash = () => { // create a function component
    return(
    <div>
    <h2>Dash</h2>
    <ul>
      <li>28</li>
      <li>Smart</li>
      <li>Pretty</li>
    </ul>
    </div>
  )
  };

  ReactDOM.render(
    <Dash />, // use the component
    document.getElementById("root")
  );
  ```
  <img src="log_imgs/props_5-24.PNG" width = "400" />


  ## Props
  Props is an object in react that contains properties about the component. With props we can display dynamic data within a component.

  ```javascript
  const Dash = (props) => {
    console.log(props)
    return(
    <div>
    <h2>Dash</h2>
    <ul>
      <li>28</li>
      <li>Cute</li>
      <li>My {props.relation}</li>
    </ul>
    </div>
  )
  };

  ReactDOM.render(
    <Dash relation = "favorite newbie developer"/>,
    document.getElementById("root")
  );
  ```
  
  <img src="log_imgs/component_5-24.PNG" width = "400" />



## Day 43, R2
### 5/23/19

- ## Node
  I finished the tutorial I was learning on node. Today, I'd like to set up a server on node. I was following ***Node.js Server Set Up*** by Greg Sidelnikov aka [JavaScript Teacher @js_tut](https://twitter.com/js_tut) but the latest update doesn't have all the mac instructions. So I'm going to look at other resources too.
  
  ## Unix
  There's a lot of unix commands involved in node. It's been a while since I looked at unix in depth so I may have to refresh my memory.
  
  ## `chown` 
  In [this tutorial on setting up a node server for mac](https://rickchristianson.wordpress.com/2013/11/15/how-to-setup-a-node-js-server-on-mac-os-x-in-less-than-10-minutes/) it uses the `chown` command:
  
  >The chown command is most commonly used by Unix/Linux system administrators who need to fix a permissions problem with a file or directory, or many files and many directories.
  
  *-from [The Linux `chown` command](https://alvinalexander.com/linux-unix/linux-chown-command-chgrp-files-directories)*
  
  ### Stupid Questions:
  Why would I need to change permissions on my computer for any file? Why don't I just have permission to do everything? Why would I disallow myself to ever have permission? What is my username and how do I see who owns what? Is it possible for a file to not be owned by any username?
  
  ## Environment Variables
  
  Skimmed this: [An Introduction to Environment Variables and How to Use Them](https://medium.com/chingu/an-introduction-to-environment-variables-and-how-to-use-them-f602f66d15fa)
  
  In Greg's tutorial for setting up a node server, it says that after we install node, we won't be able to run node from anywhere because we still need to add the path to our environment variables. However, mine is working from anywhere and I don't see the environment variables for the node path when I run `env` in the command line. What's happening?
  
  ## Path Variable
  In [this tutorial on setting up a node server for mac](https://rickchristianson.wordpress.com/2013/11/15/how-to-setup-a-node-js-server-on-mac-os-x-in-less-than-10-minutes/) it says to add these two variables through the command line:
  
  ```bash
  $ export NODE_PATH="/usr/local/lib/node"
  $ export PATH="/usr/local/share/npm/bin:$PATH"
  ```
  
  However, I already have a `PATH` variable that's different: 
  ```bash
  PATH=/Users/dashiellbark-huss/.rbenv/shims:/usr/local/bin:/usr/local/sbin/:/usr/local/mysql/bin:/usr/local/bin:/usr/bin:/bin:/usr/sbin:/sbin
  ```
  
  `PATH` seems like such a broad variable name. Why would we use it for something specific like just for npm? Why not name it `NPM_PATH`? 
  
  Am I supposed to write over my last `PATH` variable? What will that affect? The next time I follow a tutorial, will they tell me to write over `PATH` again for some other `PATH`? Won't that screw things up?
  
  ## PATH Is A List Of Paths
  
  >The $PATH variable is specified as a list of directory names separated by colon (:) characters. 
  
  *-from [Mac OS X: Set / Change $PATH Variable](https://www.cyberciti.biz/faq/appleosx-bash-unix-change-set-path-environment-variable/)*
  
  ## Current Session?
  
  I this stackoverflow thread, it mentions adding `PATH` to the *current session* vs *permanently*:
  
  >"...that would be for the current session, if you want to change permanently add it to any .bashrc, bash.bashrc, /etc/profile - whatever fits your system and user needs." 
  *-from [Removing a directory from PATH](https://unix.stackexchange.com/questions/108873/removing-a-directory-from-path)*
  
  I found out this means the current shell session. That clarfies things.
  
  ## Understanding PATH
  This article, [How to add directory to system path in Linux](https://www.computerhope.com/issues/ch001647.htm), answered a lot of my questions:
  
  ### Appending to PATH
  In the tutorial where it said to set the `PATH`, we're not overwriting `PATH`
  
   ```bash
  $ export PATH="/usr/local/share/npm/bin:$PATH"
  ```
  
  At the end of the variable value, we add the `$PATH` variable itself. So we're **appending** the npm path ***to our $PATH***.
  
  ### Current Session
  
  >The methods we've used so far only sets the environment variable for your current shell session; when you logout or close the terminal window, your changes will be forgotten.
  
  *-from [How to add directory to system path in Linux](https://www.computerhope.com/issues/ch001647.htm)*
  
  ## Unix: A Multiuser Environment
  
  Earlier I wondered why I would need to give permissions for files on my own computer:
  
  > Unix is fundamentally a multi-user environment.
  
  -[Kevin Skogland](https://www.lynda.com/Mac-OS-tutorials/Logging-using-command-prompt/78546/83613-4.html?srchtrk=index%3a1%0alinktypeid%3a2%0aq%3abash+profile+unix%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2)
  
  >Other Unix systems don't automatically log you in. You'd put in your username and your password for it to know who you are. But your Mac already knows who you are. Now if you are using your Mac as a single-user environment, that may come as a bit of surprise to you because you may not even think about the fact that you are logged in as you. But if you go to Apple menu and down to System Preferences and into Accounts, you can see that we can manage different user accounts...OS 9 which was a single-user system. But once we moved OS X with this Unix underneath, Unix is fundamentally a multi-user environment.
  
  -[Kevin Skogland](https://www.lynda.com/Mac-OS-tutorials/Logging-using-command-prompt/78546/83613-4.html?srchtrk=index%3a1%0alinktypeid%3a2%0aq%3abash+profile+unix%0apage%3a1%0as%3arelevance%0asa%3atrue%0aproducttypeid%3a2)
  
  ## Summary
  Today I had a lot of questions about Unix. Some I found answers to, but some I still need to understand:
  
  - What is the bash profile, the place where variables are stored for all sessions? What else is it used for? How can I see it?
  - Why didn't I have to add the path environment variables for my node to work?
  
  I think I'll focus on unix tomorrow too. Since this is a core part of using node, I think it will really help me. It's been a while since I've really looked into Unix. 
  
  I'm familiar with a lot of the commands but not so familiar with the structure: Like what does unix do with `$PATH`, the bash profile, and where are usernames and passowrds stored?
  
- ## Thoughts and Feelings:
  I'm going to embrace my **"stupid questions"**. Some questions feel stupid because they can clue others to the fact that you really don't understand a subject. That's good. 
  
  Most times people don't know what **you** don't understand. Asking stupid questions helps them understand what information you're missing and explain it to you better. 
  
  ### *Be **specific** and **abundant** in your stupid questions.* Really get down to what is confusing you.
  
  Ask others, ask yourself, and ask the internet/google.
  
  It's good to ask questions when you think you might even know the answer are unsure. ***With your questions, always act stupider than you are.**
  
  Another benefit to **writing out and defining your confusion** is that once you understand something, when you go back and read what you were confused by, you feel like you learned a lot. You feel like, *"Who is this confused dumbo from the past?"* Not me! Not anymore!

## Day 42, R2
### 5/22/19

- ## Node
  Continuing my practice and notes for [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html). I finished this tutorial yesterday but I still have to practice some of the concepts.
  
  ## Child Processes
  >Node.js comes with a child process module, which allows you to execute external processes in your environment. In other words, your Node.js app can run and communicate with other applications in your environment.
  [Child processes Node.js Documentation](https://nodejs.org/api/child_process.html)
  
  *-[Alex Banks](https://www.lynda.com/Node-js-tutorials/Readable-file-streams/5016729/2811746-4.html?autoplay=true)*

  
  ## `child_process.exec()`
  
  With the `exec()` function you can run terminal commands in node.js. `exec.()` handles ***synchronous*** processes.
  
  Here, we open Facebook:
  
  ```javascript
  const cp = require("child_process");

  cp.exec("open http://facebook.com");
  ```
  ![open](log_imgs/open_5-22.gif)
  
  ## Handle Data
  You can handle the data that a terminal command returns:
  
  ```javascript
  const cp = require("child_process");

  cp.exec("ls", (err, data, stderr) => {
    if(err){ 
      console.log(stderr);
      throw err;
    }
    console.log(data);
  });
  ```
  ![ls](log_imgs/ls_5-22.gif)
  
  Here we console log the list command's results to the console. The third argument `stderr` is the error that the child process gives, aka the terminal in this program\*, while `err` is the error that Node.js gives.
  
  \*I'm not sure if it can be other processes or just the terminal.
  
  ## `child_process.spawn()`
  You can also run commands in the terminal using spawn. `.spawn()` handles ***asyncronous*** processes.
  
  `.spawn()` returns a ChildProcess Object, as does `.exec()`.
  
  ```javascript
  const cp = require('child_process');

  const questionApp = cp.spawn('node', ['questions.js']);

  questionApp.stdin.write("dash \n");
  questionApp.stdin.write("a van \n");
  questionApp.stdin.write("eating \n");

  questionApp.stdout.on('data', data => {
    console.log(`from the question app: ${data}`);
  });

  questionApp.on('close', () => {
    console.log('questionApp process exited');
  });


  ```
  
  ![spawn](log_imgs/spawn_5-22.gif)
  
  ## Answers In The Terminal?
  
  With the last program, if I take out:
  
  ```javascript
  questionApp.stdin.write("dash \n");
  questionApp.stdin.write("a van \n");
  questionApp.stdin.write("eating \n");
  ```
  It doesn't let me input answers through the terminal. I'm not sure why. I guess it's only putting output from the questions app to the terminal and not listening for input.
  
- ## Node.js Server Set Up
  
  I started reading ***Node.js Server Set Up*** by Greg Sidelnikov aka [JavaScript Teacher @js_tut](https://twitter.com/js_tut).
  
  ## Notes:
  ## 3 Thinks to Understand About Node Server:
  - The difference between Global and Local installations
  - Configuring `package.json`manifest file
  - Understanding the `node_modules` directory
  
  ## Mac?
  It was interesting to read through so far but, it looks like a lot of the instructions are only for Windows. So I'm not sure if this will be helpful to me. Maybe in the next update the Mac instructions will be added.

## Day 41, R2
### 5/21/19

- ## Github
  I forgot to `git add .` which is why I was having trouble yesterday. I havn't used git in a while so I guess I was a bit rusty. I also needed to enter my username and password which I dudn't used to have to do. Maybe it's because I'm using a different terminal application or because I changed my username.

- ## Node
  Continuing my practice and notes for [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  I finished practicing the functions I learned in the last chapter concerning file system functions.
  
  You can see those practoce files here: [File system Node.js Practice](https://github.com/DashBarkHuss/node_fs_practice/commit/63f16ad919bfe81e09291c5dfb02af8c57cbd477)
  
  ## Stream Interface
  >The stream interface provides us with the technique to read and write data. We can use it to read and write to data files, to communicate with the internet, to communicate with other processed.
  
  -[Alex Banks](https://www.lynda.com/Node-js-tutorials/Readable-file-streams/5016729/2811746-4.html?autoplay=true)
  
  >A stream is an abstract interface for working with streaming data in Node.js. The stream module provides a base API that makes it easy to build objects that implement the stream interface.
  >
  >There are many stream objects provided by Node.js. For instance, a request to an HTTP server and process.stdout are both stream instances.
  >
  >Streams can be readable, writable, or both. All streams are instances of EventEmitter.
  >
  >The stream module can be accessed using:
  >
  >`const stream = require('stream');`
  >
  >While it is important to understand how streams work, the stream module itself is most useful for developers that are creating new types of stream instances. Developers who are primarily consuming stream objects will rarely need to use the stream module directly.
  
  -from [Stream Node.js Documentation](https://nodejs.org/api/stream.html#stream_types_of_streams)
  
  This idea is new to me. I think I'll understand it more when I play with it
  
  Here's some stream functions from the tutorial:
  - `fs.createReadStream()`
  - `fs.createWriteStream()`
  - `fs.createWriteStream().write()`
  - `fs.createReadStream().pipe()`
  - `fs.createReadStream().on()`
  - `fs.createReadStream().once()`
  
  
  
  ## Appending To a File With fs.createWriteStream()
  
  If you use `fs.createWriteStream()` it writes over the file you write to everytime you run it. I wanted to append to the file. On this stackoverflow page it showed how to append: [How to create appending writeStream in Node.js](https://stackoverflow.com/questions/3928926/how-to-create-appending-writestream-in-node-js)
  
  You just add the options parameter, which is the second parameter and is an object, and put in `'flags':'a'`.
  
  ```javascript
  const fs = require('fs');

  const writeStream = fs.createWriteStream(`./assets/words.txt`, {'flags':'a'} , `UTF-8`);
  const readStream = fs.createReadStream(`./assets/words.txt`, `UTF-8`);

  readStream.on("data", data => {
    writeStream.write(data);
  });
  ```

  This program reads whatever is in the file `words.text` and then write that text *back* into the file. 
  
  So if `"hi"` is in the file, after running this program, `"hihihihihi"` is the new text in the file. I'm not sure why it writes `"hi"` 4 more times instead of once.
  
- ## Thoughts & Feelings:
  I coded through driving into Utah. We saw some really red mountainss.
  
  They were ***more red*** in real life:
  
  ![utah](log_imgs/utah-5-21.gif)
  
## Day 40, R2
### 5/20/19

- ## Node
  Continuing my practice and notes for [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  ## File System Functions We Learned:
  - fs.readdir
  - fs.readfile
  - fs.writefile
  - fs.mkdir
  - fs.appendfile
  - fs.rename
  - fs.unlink
  - fs.rmdir
  
  I want to make a small project using these functions.
  
  I used this page for help with Node.js fs(file system) functions:
  [Node.js Documentation fs](https://nodejs.org/api/fs.html)
  
  ## Project
  
  I made a file that creates letters to all my friends based on a `friends.json` file. Here's the function that creates the letter:
  
  ```javascript
  const makeNoteFile= (friend) => {
    const noteTo = (friend) => {
      return ( 
      `Dear ${friend.name},
        I hope this finds you well. I was thinking about how special you are. 
      You are my one and only true ${friend.relation}. A gem at a young ${friend.age}.

      I am thinking of you,
      Dashie`
      )
    }

    fs.writeFile(`${friend.name}.txt`, noteTo(friend),  err => {
      if (err) throw err;
      console.log('file created');
    });
  };
  ```
  
  I also made a bunch of little files that deal with the other functions. I'm having trouble pushing the project to github. My internet is slow today as I travel. So I'm going to leave this log short today.

## Day 39, R2
### 5/19/19

- ## Helping
  Last time I was helping someone on twitter with [this problem](https://github.com/DashBarkHuss/100-days-of-code/blob/master/r2-log.md#day-38-r2). We ended with this:
 
  <img src="log_imgs/scroll_5-19.gif" width="400"/>
 
  The divs look like they have uniform gradient but if you scroll the gradient stays fixed to the viewport.
 
  [JavaScript Teacher @js_tut](https://twitter.com/js_tut) recommended this:
 
  >Make one parent. Create an SVG with a hole and just the white corners. Apply it to 3 gradientless children ;)
 
  This could work however it becomes more difficult when the page is responsive or if there are other elements on the page that might interfere. If the divs are changing perspectives, then we have to change the SVG too. I'm not sure how to do that without warping the perspective of the smooth corner radius of the SVG. You could probably do it with javascript. 
 
  I'm pretty sure there's other ways to do this in javascript too. Like maybe you can change where the background starts with a specific pixel measurement and use javascript to start all the backgrounds in the appropriate spot so that they all line up.
 
  Since I haven't heard back from the OP, I'm gonna leave this problem and go back to node.
 
- ## Node
  Continuing my practice and notes for [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  ## Debbuging Node in VSC
  Yesterday [Philippe Vaillancourt @snowfrogdev](https://twitter.com/snowfrogdev) told me that there's a way to debug built into VSC. Press `fn` + `F5`. Then just debug directly in VSC. 

  ![screenrecording](log_imgs/debug_5-19.gif)
  
  You can do what I did in the video or:
  1. `fn` + `F5`
  2. Select Environment: **"Node.js"**
  3. Click the debugging view or `shift` + `cmd` + `D`
  4. Click the Debug Console Button
   - <img src="log_imgs/debugbutton_5-19.PNG" width = "300" />
  
  [Here's a video](https://www.youtube.com/watch?v=2oFKNL7vYV8) tutorial of someone using the debugger.
  
  ## Ideas From Today's Lynda Videos
  
  This stuff is starting to feel very backendy to me which is fun and exciting!
  
  ### We learned about the Node module `fs` and these functions:
  - fs.readdir
  - fs.readfile
  - fs.writefile
  - fs.mkdir
  - fs.appendfile
  - fs.rename
  - fs.unlink
  - fs.rmdir
  
  These functions all deal with deleting, creating, or changing files and directories.
  
  I zoomed through this section of the tutorial, simply watching without coding along. Tomorrow, I'll take what I learned from these concepts and make a small project, reviewing any video I need to review. I think this might be a more effective way of learning for me.
  
- ## Thoughts and Feelings:
  I have been learning a lot about how I learn. That's a great side effect of 100DaysOfCode. I like to learn slowly. I feel if I learn a new concept slowly in the beginning, I'm able to learn faster later when I get to the more difficult parts of the subject. Actually, you could say I like to change up the pace depending on my goal.
  
  I'm learning Node.js pretty slowly and deliberately. It's more enjoyable this way and will stick better. I even went off my tutorial to learn how to set up debugging in node. I think debugging is so important. It makes understanding the code a billion times easier. So I took the time to learn how to set debugging up, which slows down my progress in my tutorial but speeds up my progress in the long run.

  I'm going to start posting my log on linked in. I want to start building up my linked in. I don't really know how to use linked in well. Maybe I can take a tutorial.

## Day 38, R2
### 5/18/19

- ## Helping
  Someone asked:
  >I have some children divs (`position: absolute`) nested inside a parent div. The parent div has a gradient background.
  >Question: is it possible to make the children divs inherit the same gradient background from the parent?
  
  I found that this worked:
  ```css
  #parent>* {
    background-image: inherit;
  }
  ```
  
  I thought maybe the OP left out some information because this was simple, and I was right. They also want the gradient to be uniform between the parent and the child.
  
  <img src="log_imgs/css_5-18.PNG" width="400"/>
  
  I looked around for some ideas but nothing I came up with panned out so I posted it on twitter.
  
  ### But then I did figure it out! Sort Of
  
  If you add a `background-size` to the parent you can then have the children inherit that size along with `background-attachment: fixed;` which the OP showed me.
  
  ```css
  #parent {
    background-image: linear-gradient(blue, yellow);
    height: 400px;
    width: 200px;
    background-size: 600px 600px; //size
  }

  #parent>* {
    background-attachment: fixed;
    background-image: inherit;
    background-size: inherit;
  }
  ```
  
  <img src="log_imgs/solution_5-18.PNG" width="400"/>
  
  But the OP got back to me and apparently this solution is limited:
  
  >Very clever! But because `background-attachment: fixed` is relative to the viewport and not to the parent, children will only align with the gradient at the top position. Scroll down and the colour starts changing.
But I think we're getting somewhere with your trick! Thank you!
  
- ## Node
  Continuing my practice and notes for [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  ## Debugging Node
  I love using DevTools for debugging. But since node runs on the server and not on the browser, what do people do to debug it? How to I get that same functionality I get from DevTools for node?
  
  I found this article: [Debug Node.js in browser with real Chrome Developer Tools](https://hackernoon.com/debug-node-js-with-chrome-devtools-aca7cf83af6b). But it didn't work. I ran the command:
  
  >Sat May 18 start Dashie$ node --inspect --debug-brk ask.js
  
  The terminal responded:
  >Debugger listening on ws://127.0.0.1:9229/4046c63c-1c1c-4580-9e23-1506a47c8feb
  >For help, see: https://nodejs.org/en/docs/inspector
  
  But when I put the link in the browser, it didn't work: 
  >The webpage at ws://127.0.0.1:9229/4046c63c-1c1c-4580-9e23-1506a47c8feb might be temporarily down or it may have moved permanently to a new web address
  
  I went to the link that the terminal provided https://nodejs.org/en/docs/inspector. It said to go here chrome://inspect. When I went there, I saw my `ask.js` app. So it worked! 
  
  I got a warning:
  
  > DeprecationWarning: `node --inspect --debug-brk` is deprecated. Please use `node --inspect-brk` instead.
  
  ## How to Debug Node, Sort Of
  ### Steps:
  1. run `node --inspect-brk <yourfile>.js`
  2. Go to chrome://inspect
  3. Click 'inspect' by your app
  ![screenshot](log_imgs/inspect_5-18.PNG)
  
  ## Using DevTools on Node
  
  When it came to actualy using the DevTools it was really confusing. As soon as I got a bug my sources disappeared. So this is still confusing me.
  
  ## Practicing EventEmitter
  
  I used EventEmitter successfully on my node module after a while of running into an error. The reason? I forgot to return the emitter instance in my function! 
  
  Using the EventEmmiter helped me understand what might be going on in the background when you use regular events.

## Day 37, R2
### 5/17/19

- ## Node JS Notes / Practice
  I'm reviewing what I learned so far in the lynda.com tutorial:
  [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  I'm going to redo some of the projects he does in the tutorial.
  
  ## Alex Bank's Process Tips
  
  I liked how Alex walked through his process:
  
  >...we want to have a variable that can hold our questions:
  
  ```javascript
  const questions = [
  
  ] 
  ```
  >..within this array we want to beable to add any number of questions, and have those questions asked to the user through the terminal.
  
  ### Example questions:
   ```javascript
  const questions = [
    "What is your name?",
    "What is your favorite snack?",
    "Do you like cilantro?"
  ] 
  ```
  
  >**So sometimes when I have a coding challenge, I try to think about what I do and represent it as a function call.** So, for instance I know that we need to collect the answers from the user in the terminal. I'm not necessarily sure how to do that, but I can imagine that there would be a function called `collectAnswers`. 
  
  ```javascript
  collectAnswers()
  ```
  >And, without knowing how the `collectAnswers` function will work, what I do know is I want to send it an array of questions,
  
  ```javascript
  collectAnswers(questions)
  ```
  
  >...and I want the `collectAnswers` function to ask every question in the array, in the order that they're presented, of the user. And, when I get a list of answers back, I can handle that with a callback function. So I know that the second argument of my collectAnswers function should probably be a callback function,
  
    ```javascript
  collectAnswers(questions, callback)
  ```
  
  >...and that callback function should pass a list of answers back. So, I send a list of questions in as an array, and I expect a list of answers back. 
  
  ```javascript
  collectAnswers(questions, answers => {
  
    //do something with the answers, exit the process
  
  })
  ```
  
  >**So this is what I want to do, and at this point I might not necessarily be sure how to do it, but I know that I need a function called `collectAnswers`, so I'm going ot go ahead and create that function.** And the first argument that the `collectAnswers` function is going to take, is an array of questions, so I'll go ahead and call them questions. And, the second argument is a callback function, to be invoked when we're finished, so I'm going to call this done, meaning I want to invoke this function, once the user has answered all of the questions.
  
    ```javascript
  const collectAnswers = (questions, done) = {
  
  }
  ```
  
  ## Making An Ask Questions Module
  
  ### Module:
  
  ```javascript
  const readline= require("readline");

  const rl = readline.createInterface({
    input: process.stdin,
    output: process.stdout
  });

  const collectAnswers = (questions, done = f=>f) => {
    let answers=[];

    const questionAnswered = (answer)=>{
      answers.push(answer);
      if (answers.length == questions.length){
        done(answers);
      } else if (answers.length < questions.length){
        rl.question(questions[answers.length], questionAnswered);
      }

    }

    rl.question(questions[0], questionAnswered);

  };

  module.exports = {
    collectAnswers
  }
  ```
  
  ## Requiring The Module
  
  ```javascript
  const ask = require("./askQuestions");

  const questions = [
    "How are you?",
    "Do you like mac or pc?",
    "Do you like organic bacon?"
  ]

  ask.collectAnswers(questions, answers=>{
    console.log("thank you! Answers:", ...answers);
    process.exit()
  });

  ```
  ![module](log_imgs/askQuestions_5-17.gif)


## Day 36, R2
### 5/16/19

- ## Node JS Notes
  Continued notes on node.js lynda.com tutorial:
  [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  ## Core Modules
  
  Core modules are modules that come with node.js so you don't need to install them. But you do need to import them to use them. To do this you use `require()`
  
  ## Destructure Module Functions
  If you only want one function from a module you can use destructuring:
  
  ### Regular:
  ```javascript
  const path = require("path");
  const util = require("util");

  util.log(path.basename(__filename));
  ```
  
  ### Destructured:
  ```javascript
  const path = require("path");
  const { log } = require("util"); //destructured

  log(path.basename(__filename));
  ```
  
  ## Dot Slash Folder (./folder)
  `./` means:
  > ./ is the the folder that the working file is in
  
  -*from [What does “./” (dot slash) refer to in terms of an HTML file path location?](https://stackoverflow.com/questions/7591240/what-does-dot-slash-refer-to-in-terms-of-an-html-file-path-location)*
  
  So it's no different from `../`
  
  ## Custom Modules
  
  When you make your own module, you must include the path in order to require it:
  ```javascript
  const name = require("./myModule");
  ```
  You don't need the path if the module ships with node or is installed via npm.
  
  In the module file, you need to `export` a value:
  ```javascript
  module.exports = "alex";
  ```
  
  ## More Complex Custom Module Example:
  
  ### Count Module
  ```javascript
  let count = 0;
  const inc = () => ++count;
  const dec = () => --count;

  const getCount = () => count;

  module.exports = {
    inc,
    dec, 
    getCount
  }
  ```
  
  ## Importing the Count Module
  
  ```javascript
  const { inc, dec, getCount } = require("./myModule");
  inc();
  inc();
  inc();
  dec()

  console.log(getCount()); //2
  ```
  
  
  ## Property Value Shorthand
  
  I've seen this before but it confused me today:
  
  ![Shorthand](log_imgs/shorthand_5-16.PNG)
  
  An object with properties that aren't in the `key: value` format. This is called the [property value shorthand]( https://javascript.info/object#property-value-shorthand).
  
  ## Method Shorthand
  There's also a [method shorthand](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Object_initializer#Method_definitions):
  
  >In ECMAScript 2015, a shorthand notation is available, so that the keyword "function" is no longer necessary.
  >
  >```javascript
  >// Shorthand method names (ES2015)
  >var o = {
  >  property(parameters) {},
  >  *generator() {}
  >};
  >```
  
  ## Event Emitter
  A node.js module that gives us a mechanism for emitting custom events and wiring up listeners and handlers for those events:
  
  ### Construct a new instance of the event emitter that that we can use to raise custom events:
  ```javascript
  const events = require("events");

  const emitter = new events.EventEmitter();
  ```
  
  Use the `emit()` function to raise custom events:
  ```javascript
  process.stdin.on("data", data => {
    const input = data.toString().trim();
    if (input === "exit") {
      emitter.emit("customEvent", "Goodbye!", "process"); //customEvent raised when we hit this point
      process.exit();
    }
    emitter.emit("customEvent", input, "terminal"); //customEvent raised when we hit this point
  });
  ```
  Wiring up listeners and handlers with the Events Emitter's `.on()` function:
  ```javascript
  emitter.on("customEvent", (message, user) => { //handle custom event
    console.log(`${user}: ${message}`);
  });
  ```
  This says, when a custom event occurs handle it with this callback function.
  
  Event Emitter is asyncronous. The events are raised when they happen.
  
- ## Thoughts and Feelings:
  Learning these new concepts is more draining than building something. I needed to take more breaks. Tomorrow, I'm going to do some more hands on coding with what I learned from today.
  

## Day 35, R2
### 5/15/19

- ## Node JS Notes
  Continued notes on node.js lynda.com tutorial:
  [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  ## Debugging?
  So far we have only run these node files in the terminal. But how do I debug them?
  
  I guess I don't know yet because I don't know how to put our files into the browser. I think you have to set up a node server or something so node an compile the code.
  
  ## `setTimeout` and `setInterval`
  
  I started to recreate the instructer's project but I wanted to make it more interesting. This led to my playing with `setTimeout` and `setInterval`. 
  
  I'm having trouble getting a loop to delay using `setTimeout` or `setInterval`.
  
  I redid my code so I can do it in the browser and use the debugger.
  
  The answer on the thread [How do I add a delay in a JavaScript loop?](https://stackoverflow.com/questions/3583724/how-do-i-add-a-delay-in-a-javascript-loop) looks like it uses recursion:
  
  ```javascript
  
  var i = 1;                     //  set your counter to 1

  function myLoop () {           //  create a loop function
     setTimeout(function () {    //  call a 3s setTimeout when the loop is called
        alert('hello');          //  your code here
        i++;                     //  increment the counter
        if (i < 10) {            //  if the counter < 10, call the loop function
           myLoop();             //  ..  again which will trigger another 
        }                        //  ..  setTimeout()
     }, 3000)
  }

  myLoop();                      //  start the loop
  
  ```
  
  There was debate about whether the recursion was a problem in the comments.
  
  ## Practice: Node.js New Years
  
  I remade what the the time that the teacher made, with my own pizazz: I made a NYE countdown timer.
  
  ![countdown](log_imgs/count_5-15.gif)
  
  ### Code:
  
  ```javascript
  const waitTime = 6000;
  const intervalTime = 1000;
  let seconds = waitTime /1000;

  function countDown(){
    process.stdout.clearLine();
    process.stdout.cursorTo(0);
    process.stdout.write(`${seconds -1}!!!!`);
    seconds -= 1;

  }

  function sing() {
    clearInterval(interval);
    process.stdout.clearLine();

    var i = 0;                     
    let numOfWords;
    const song = 
    `Happy New Year!!!!!!
    Should old acquaintance be forgot, 
    and never brought to mind?
    Should old acquaintance be forgot,
    and old lang syne?`;

    const lines = song.split("\n");

    function myLoop () {  

      const callback = function () {    
        console.log(`${lines[i]}
      `);          
        i++;                     
        if (i < lines.length) {            
           myLoop();             
        }                        
     };
        const numOfWords = (lines[i].match(/ /g) || []).length + 1;
        setTimeout(callback, 170 * numOfWords) 
     }

    myLoop(); 
  }

  const interval = setInterval(countDown, intervalTime);

  setTimeout(sing, waitTime)

  ```


## Day 34, R2
### 5/14/19


- ## Helping
  I looked at [Swishyfishie's @Swishyfishie2](https://twitter.com/Swishyfishie2) code. He has a counter here. You can add 1 or 2 and then submit to save to `sessionStorage`. 
  
  <img src="log_imgs/swishy_5-14.PNG" width=300>
  
  It was adding more numbers than expected after you press submit. 
  
  I made a `log(func)` function to call from each function that Swishy made to see where the problem was. The `log(func)` function console logs the function name and the `count` variable's value:

  ```javascript
  function log(func) {
    console.log(`func:${func}, count:${count}`);
  };
  ```
  You call `log(func)` from each function, passing in a name for your function such as "+1" or "save".

  What I saw was that when the submit button was pressed, `count` increased by one. Sure enough Swishy was adding 1 to `count`, probably left over from copy and pasting from his first function.
 
  ```javascript
  btnSave.onclick = function () {
      saved.innerHTML = localStorage.getItem('county', count += 1); //oops adding 1 here
      log("save");
  }
  ```
  
  Seems to work after deleting `count += 1` from that line. 
 
- ## Node JS Notes
  Continued notes on node.js lynda.com tutorial:
  [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  [How, in general, does Node.js handle 10,000 concurrent requests?](https://stackoverflow.com/questions/34855352/how-in-general-does-node-js-handle-10-000-concurrent-requests)
  
  ## Global Object
  
  [Mozilla Docs: Global Object](https://developer.mozilla.org/en-US/docs/Glossary/Global_object)
  
  >The global object's interface depends on the execution context in which the script is running. For example:
  >
  >- In a web browser, any code which the script doesn't specifically start up as a background task has a Window as its global object. This is the vast majority of JavaScript code on the Web.
  >
  >- Code running in a Worker has a WorkerGlobalScope object as its global object.
  >
  >- Scripts running under Node.js have an object called global as their global object.

  [Node.js v12.2.0 Documentation: Global Objects](https://nodejs.org/api/globals.html)
  
  ## Modules
  
  Every node.js file we create is refered to as a module.
  
  ## `require()`
  
  `require()` is used to load external modules. Modules are other javascript files containing code. We can either load modules that have shipped with our installation of Node.js, modules we install with npm, or other modules that we create. `require()` is available to us on the global object.
  
  # `process` object
  The `process` object contains information about the current process as well as tools to allow us to interact with the process.
  
  ![process](log_imgs/process_5-14.PNG)
  
  ## Practice What I Learned
  
  I made my own version of a little questions program that the instructor made. I did different questions.
  
  ```javascript
  const questions = [
    "Who do you love?",
    "Who are you?",
    "Are you horny? Answer y or n"
  ];

  const ask = (i = 0 ) => {
    process.stdout.write(`\n${questions[i]} \n\n`);
  }

  ask()

  const answers = [];

  process.stdin.on("data", (data)=>{
    answers.push(data.toString().trim());

    if (answers.length < questions.length) {
      ask(answers.length);
    } else {
      process.exit();
    }
  });

  process.on("exit", ()=>{
    const [lover, name, horny] = answers;

    const doThisTo = horny === "y" ? "make love to": "kiss";
    console.log(`
    ${name.charAt(0).toUpperCase() + name.slice(1)}, you should ${doThisTo} ${lover.charAt(0).toUpperCase() + lover.slice(1)}!
    `);
  });
  ```
  
  ![questions](log_imgs/questions_5-14.gif)
  
- ## Thoughts and Feelings:
  If I don't do some sort of hands on coding (not just coding along with the tutorial), I get super bored by tutorials. So I need to always try to do something at least slightly creative along with the tutorial. Today, that meant recreating the questions program with my own questions.

 
## Day 33, R2
### 5/13/19

- ## NPM Notes
  I continued this npm tutorial:
  [Learning npm the Node Package Manager Tutorial](https://www.lynda.com/Node-js-tutorials/Learning-npm-Node-Package-Manager-2018/761956-2.html)
  
  ## Notes:
  
  `npm outdated`: To see if any packages are outdated locally, `-g` to get global
  
  `npm update <package>` or `npm install <package>` to update package.
  
  `npm uninstall <package>`: uninstall a dependency
  
  ## "devDependencies" vs "dependencies"
  >The difference between these two, is that devDependencies are modules which are only required during development, while dependencies are modules which are also required at runtime.
  
  -*from [NPMmmm #1: Dev Dependencies, Dependencies](https://medium.com/@dylanavery720/npmmmm-1-dev-dependencies-dependencies-8931c2583b0c)*

  
  ## Semantic Versioning
  ![version](log_imgs/version_5-13.PNG)
  
  ### Controlling Version:
  
  ### ^1.x.x
  
  The ^ caret symbol means:
  
  Update all releases that have a major release of 1.
  
  ```javascript
   "devDependencies": {
    "babel-preset-env": "^1.7.0",
  }
  ````
  
  ### ~1.7.x
  
  The ~ tilde symbol means:
  
  Update all releases that have a major release of 1 and a minor release of 7.
  
  ```javascript
   "devDependencies": {
    "babel-preset-env": "~1.7.0",
  }
  ````
  
  ## `package-lock.json` vs `package.json`
  Confused about the difference between these two.
  
  It seems like `package-lock.json` controls things more, specifies things more. But then what's the point of having both?
  
  [Everything You Wanted To Know About package-lock.json But Were Too Afraid To Ask](https://medium.com/coinmonks/everything-you-wanted-to-know-about-package-lock-json-b81911aa8ab8)
  
  >package-lock.json: records the exact version of each installed package which allows you to re-install them. Future installs will be able to build an identical dependency tree.
  >
  >package.json: records the minimum version you app needs. If you update the versions of a particular package, the change is not going to be reflected here.
  
  -*from [What is the difference between package.json and package-lock.json](https://delegatecall.com/questions/what-is-the-difference-between-packagejson-and-packagelockjson-2bd0e4e1-a65f-4d78-b8d8-b51905a37adb)*
  
  ## npm Cache
  
  npm keeps a cache of your install modules so that it doesn't have to get them everytime. Clearing your cache should always be a step in your troubleshooting when working with modules and not understanding what's happening.
  
  `npm-cache-verify`: Verifies you cache. (What does that mean?)
  `npm-cache-clean --force`: clean the cache
  
  ## npm audit
  
  Hard to understand this video without having context for it.
  
  `npm audit` is a command that will check the dependencies of your project and make sure they are safe to use.
  
  [Run an npm audit Lynda Video](https://www.lynda.com/Node-js-tutorials/Run-npm-audit/761956/800277-4.html?autoplay=true)
  
  ## Scripting in package.json
  Scripting in a package.json file gives us a way to do a simple command, to repeat, or execute complex commands.
  
  [npm-scripts docs](https://docs.npmjs.com/misc/scripts)
  
  ![scripting](log_imgs/npmscript_5-13.PNG)
  
  **Run:**`npm <preset script name>` or `npm run <your own script name>`
  
  ## npm npx
  
  npx lets you temporarily install a package just to run one of the commands from that package. This way you avoid package pulution.
  
  [npm npx docs](https://www.npmjs.com/package/npx)
  
- ## Node JS Notes
  Notes on node.js lynda.com tutorial:
  [Node.js Essential Training 2019 Lynda Tutorial](https://www.lynda.com/Node-js-tutorials/Node-js-Essential-Training-Part-I-2019-REVISION/5016729-2.html)
  
  ## Single Threaded non-blocking event driven IO
  ### Single threaded
  Node is single threaded. This confuses me because what if an application has millions of users? How can they all be on the same thread without slowing things down?
  
  ### I/O
  I don't know what I/O means for and I had trouble finding out. 
  
  >Node.js is an open source project designed to let you write JavaScript programs that talk to networks, file systems or other I/O (Input/Output) sources
  >...
  >What is an "I/O based program"? Here are some common I/O sources:
  >
  >Databases (e.g. MySQL, PostgreSQL, MongoDB, Redis, CouchDB)
  >APIs (e.g. Twitter, Facebook, Apple Push Notifications)
  >HTTP/WebSocket connections (from users of a web app)
  >Files (image resizer, video editor, internet radio)
  
  -*from [introduction to node: Understanding node](https://gist.github.com/maxogden/4011336#understanding-node)*
  
- ## Helping
  
  I started to look at some code that [Swishyfishie @Swishyfishie2](https://twitter.com/Swishyfishie2) was stuck on but I didn't get enough time to really check it out. Will look at it tomorrow!
  
## Day 32, R2
### 5/12/19

- ## Testing eslint-plugin-html
  I got the plugin to work again a different way. I just copied and pasted the `node_modules` folder from the other projects into a new project and copied the `.eslintrc.json` file.
  
  ## User Settings vs `.eslintr.json`
  But if I remove this from the user settings:
  
  ```javascript
    "eslint.options": {
    "plugins": [
      "html"
    ]
  },
  ```
  
  Eslint stops linting HTML on the last two project folders: `testing_eslint_project` and `playing with javascript`. But it still works on `perfect_fit_meal`.
  
  Now I see why. The `.eslintrc.json` in `perfect_fit_meal` had: 
  
  ```javascript
  "plugins": [
    "html"
  ]
  ```
  
  But the other two didn't have those settings in their `.eslintrc.json` file.
  
  ## Global Vs Local `eslint_plugin_html`
  
  I can also see that `perfect_fit_meal` has `eslint-plugin-html` in the `node_modules` folder. The other two projects don't. They must be getting the plugin globally. 
  
  The other two also don't have `eslint-plugin-html` listed in the `"dependenies"` property in the `package.json` file like `perfect_fit_meal` does. I think I got it there when I ran `npm install --save-dev eslint-plugin-html`.
  
- ## Prettier
  
  I think I understand a bit more about these plugins now. I'm going to try to install prettier again.
  
  ## Wes Bos Prettier + ESLint Video
  
  I followed this Wes Bos [How to Setup VS Code + Prettier + ESLint](https://www.youtube.com/watch?v=YIvjKId9m2c) video again. I still get ***"Failed to load plugin prettier: Cannot find module 'eslint-plugin-prettier'"***
  
  ## No Dependency
  Now that I understand a little more I can see that I do not have that module in my `node_modules` folder; globally or locally. It's also not in the `package.json` file's `"dependencies"`. 
  
  But why doesn't Wes Bos tell you how to get the dependency?
  
  ## Adding The Dependency Through the Terminal
  
  I'm guessing I need to add it like how I got the eslint dependency, `npm install --save-dev eslint-plugin-html`
  
  So I ran:
  
  `npm install --save-dev eslint-plugin-prettier`
  
  Now I see `"eslint-plugin-prettier": "^3.1.0"` in my `"dependencies"` in my `"package.json"` file.
  
  ## No Dependency In node_modules
  
  Even though I see `"eslint-plugin-prettier": "^3.1.0"` in `"package.json"`, I don't see it in my `node_modules` folder. Even though it says it added the plugin:
  
  ![screenshot](log_imgs/prettier_5-12.PNG)
  
  ## Another Missing Dependency
  I restarted VSC and got this error: 
  
  ***[Error - 13:55:17] Cannot find module 'eslint-config-prettier' Referenced from: /Users/dashiellbark-huss/Documents/100daysofcode/perfect_fit_meal/.eslintrc.json***
  
  So I also ran:
  
  `npm install --save-dev eslint-config-prettier`
  
  Now I see `eslint-plugin-prettier` but no `eslint-config-prettier` in the node_modules. But I do see both in the `"dependencies"` in the `package.json`.
  
  ## Restart VSC to See the Dependencies
  
  I restarted VSC and now I see the `eslint-config-prettier`! ***So maybe after you add a dependency through the terminal you have to refresh VSC.***
  
  ## Prettier Configs
  
  I added this back to the `.eslintrc.json` file which Wes Bos said to add:
  
  ```javascript
    "prettier/prettier": [
    "error",
    {
      "singleQuote": true
    }
  ]
  ```
  
  But I got this error:
  
  ***Error: ESLint configuration in /Users/dashiellbark-huss/Documents/100daysofcode/perfect_fit_meal/.eslintrc.json is invalid:***
  ***Unexpected top-level property "prettier/prettier".***
  
  In these [docs for eslint-config-prettier](https://github.com/prettier/eslint-config-prettier) they have ***the `"prettier/prettier"` property inside the `"rules"` property. I moved it there and that error went away.***
  
  ```javascript
  "rules": {
    "no-console": "off",
    "prettier/prettier": [
      "error",
      {
        "singleQuote": true
      }
    ]
  }
  ```
  
  ## Still Missing Dependencies
  
  But I have new errors:
  
  ***[Error - 14:13:42] ESLint stack trace:***
  ***[Error - 14:13:42] Error: Cannot find module 'prettier'***
  
  So I ran:
  
  `npm install --save-dev prettier`
  
  and restarted VSC.
  
  No errors!
  
  Now let's see if it worked.
  
  ## Working (sort of)!
  
  It fixing on save on .js files on. But not .html. It is registering prettier on those files:
  
  ![screenshot](log_imgs/prettiernotif_5-12.PNG)
  
  But when I save it doesn't change the quotes.
  
  ## Prettier Doesn't Work on Script Tags
  
  Looks like it's not possible to fix on auto save in the HTML files:
  
  >It's a planned feature but not supported just yet.
  
  ***-from [How to format code in script tags?](https://github.com/prettier/prettier/issues/2648)***
  
  ## Summary
  
  Looks like I got it working. My problem was that I never actually installed the dependancies. I don't know why that wasn't part of the Wes Bos tutorial. Now I understand a little more about how the dependancies work, where they are, and how to add them. I'm glad I took the time to figure this out because I understand these packages better. That will come in handy when I learn node which I'm going to do soon!

## Day 31, R2
### 5/11/19

- ## Getting ESLint to Work on HTML AGAIN

  I'm trying to figure out these npm packages. I can't get ESLint to lint the HTML in my `playing_with_javascript` directory.
  
  I tried adding configurations to the user settings instead of the `.eslintrc.json` file according to this tutorial: [Eslint setup in Visual Studio Code](https://youtu.be/o2H8kvuwMKE?t=344)
  
  I deleted all my files in `playing_with_javascript` related to node or eslint. I followed this video to install eslint: [Eslint setup in Visual Studio Code](https://youtu.be/o2H8kvuwMKE?t=344). I made sure my terminal was in my project directory.
  
  I got this notification in my terminal: ***"We recommend using this local copy instead of your globally-installed copy."*** How do I control which one my project uses? Not sure, but I can see that it's loading locally in the ESLint tab.
  
  ## ESLint HTML Plugin
  
  In my project directory I ran `npm install --save-dev eslint-plugin-html`, following: [How can I get ESLint to lint HTML files in VSCode?](https://stackoverflow.com/questions/54138689/how-can-i-get-eslint-to-lint-html-files-in-vscode/54138880#54138880)
  
  I added this to the `.eslintrc.json` file:
  ```javascript
  "plugins": [
    "html"
  ]
  ```
  
  I also ran `eslint --ext .html,.js playing_with_javascript` while in the `100daysofcode` directory:
  
  > Note: by default, when executing the eslint command on a directory, only .js files will be linted. You will have to specify extra extensions with the --ext option. Example: eslint --ext .html,.js src will lint both .html and .js files in the src directory. See ESLint documentation.
  
  -*from [BenoitZugmeyer/eslint-plugin-html](https://github.com/BenoitZugmeyer/eslint-plugin-html#usage)*
  
  ## It Works Now But...
  
  Ok I just realized it actually works. ESlint is linting HTMl files in `playing_with_javascript`. But I hadn't realized it was working for a while. I'm not sure for how long. So I'm not sure what I did that was necessary and what was unneccessary.
  
  ## Undoing Adding ESLint
  
  I'm going try to undo everything I did when trying to add ESLint and the html plugin. Then do it again so I can see when it actually starts linting the HTML.
  
  ## Uninstalling ESLint
  Following this: [Uninstalling packages and dependencies](https://docs.npmjs.com/uninstalling-packages-and-dependencies). 
  
  In my project directory, I ran `npm uninstall eslint` and it didn't look like it did anything. 
  
  So I also ran `npm uninstall --save eslint`. That didn't look like it did anything either because my `node_modules` folder, `.eslintrc.json` file, and package.json file were all still there. 
  
  But when I restarted VSC it wasn't loading ESlint anymore. I looked it my `package.json` file and I could see eslint wasn't there, just some of the plugins that go with eslint were there:
  
  ```javascript
  "devDependencies": {
    "eslint-config-airbnb-base": "^13.1.0",
    "eslint-plugin-html": "^5.0.3",
    "eslint-plugin-import": "^2.17.2"
  }
  ```
  
  And I no longer saw the `eslint` folder in `node_modules`. So I guess the command **did** remove ESLint, it just didn't get rid of npm. Maybe that seems like obvious behavior to most people, but I'm still trying to understand this all.
  
  ## Uninstalling NPM
  
  I couldm't find any information about removing npm locally through the terminal, except for this:
  
  > Local installs are completely contained within a project’s node_modules folder. Delete that folder, and everything is gone (unless a package’s install script is particularly ill-behaved).
  
  -*from [npm-removal Cleaning the Slate](https://docs.npmjs.com/misc/removing-npm.html)*
  
  It doesn't mention the `package.json` file or other files that were installed with npm. I'm just going to delete them.
  
  ## `--ext`
  
  The only thing I still have to undo is this command: `eslint --ext .html,.js playing_with_javascript`. But I'm not sure what it did? How to undo it? I could maybe set it back to just `.js`, like this: `eslint --ext .js playing_with_javascript` But I'm not sure if that would undo the command to lint html extensions. I really don't know what this command is changing? Is there a configuration somewhere?
  
  >My questions are:

  >I couldn't find the configuration option for doing this from .eslintrc.
  >
  >...
  >
  
  One of the answers:
  >You can’t define this in .eslintrc files because file traversal happens before the configuration is read
  [Configuration option for '--ext' switch #3469](https://github.com/eslint/eslint/issues/3469)
  
  So it's not in the  `.eslintrc.json` file. 
  
  I think if I just pass in .js, it will set it to only use .js extensions again because I found this:
  
  >Use only .js2 extension
  >eslint . --ext .js2
  
  -*from [Command Line Interface](https://eslint.org/docs/user-guide/command-line-interface#--ext)*
  
  I ran `eslint --ext .js playing_with_javascript`. I got an error:
  
  ![screenshot](log_imgs/error_5-11.PNG)
  
  This is confusing. If you can't define the extensions in the `.eslintrc` files, then why would it matter that there is no `.eslintrc` file? But maybe, even though the extenson configurations aren't stored there, you for some reason still need that file.
  
  ## Starting over
  
  I think everything is reset to how it was to begin with in my `playing_with_javascript` directory. So I started again.
  
  ## It worked!
  
  This is what I did:
  
  I ran these commands on the project directory and followed the prompts for each:
    
  `npm init`
  `eslint --init`
  
  I restarted VSC and ESLint now ***lints the HTML*** on the `playing_with_javascript` directory even though I did nothing else. Probably because I put this in the user settings for my workspace earlier:
 
  ```javascript
  "eslint.validate": [
    "javascript",
    "javascriptreact",
    "html"
  ],
  "eslint.options": {
    "plugins": [
      "html"
    ]
  },
  ```
  
  Normally the plugins can be put in the `.eslintrc.json` file. But when it's in the user settings in `"eslint.options"` it applies to the workspace.
  
  I'm not sure if `eslint --ext .html,.js playing_with_javascript` did anything earlier. 
  
  ## Summary
  
  So I did a lot of stuff I didn't need to do but I learned a lot in the process.
  
## Day 30, R2
### 5/10/19

- ## Helping
  Today I got to help someone who posted about a css issue on Twitter. I have yet to hear back if it was helpful. Hopefully it was.
  
  They were having trouble getting these inputs to vertically align:
  
  ![img](log_imgs/help_5-10.PNG)
  
  ### 1. To start, I added a border to all the divs to see better what was going on:
  
  ```css
  * {
  border: 1px solid red;
  }
  ```
  
  ### 3. I got rid of these extra divs:
  
  ![img](log_imgs/start_5-10.PNG)
  
  ![img](log_imgs/divs_5-10.PNG)
  
  ### That gave me these centered but misaligned inputs:
  
  ![img](log_imgs/centered_5-10.PNG)
  
  ### 4. I added a class to the labels so I could make their widths uniform to align everything:
  
  ![img](log_imgs/labelclass_5-10.PNG)
  
  ### 5. Added `text-align: right` to line up the colons.
  
  ![img](log_imgs/done_5-10.PNG)
  
- ## Prettier
  Now I need to figure out how to get prettier to work. I'm going to do some investigating to see how these extensions work.
  
  ## ESLint-plugin-html
  
  I have the plugin `eslint-plugin-html` locally and globally. I wasn't sure which I was using with my `perfect_fit_meal` project. To find out, I messed with the file name of my local plugin.
  
  <img src="log_imgs/name_5-10.PNG" width="300">
  
  When I restarted VSC it gave me this error:
  
  ![img](log_imgs/terminal_5-10.PNG)
  
  So now I know that my `esLint-plugin-html` is coming from the local directory.
  
  ## What about other project directories?
  
  I wonder how ESLint will work in other project directories? Where will it load from?
  
  I opened the directory for a project called `playing_with_javascript`, a project that didn't have ESLint installed locally to see what would happen. One thing to note is both these directories, `playing_with_javascript` and `perfect_fit_meal`, are in the same workspace. So I believe they share the same user settings.
  
  ## No Local ESLint? Load Globally
  
  This directory, `playing_with_javascript`, loaded ESLint from my global modules.
  
  ![img](log_imgs/noconfig_5-10.PNG)
  
  As you can see from the underlined error, it looks like your `.eslintrc.json` should in to your project directory even if there's no local ESLint.
  
  ## Adding Configs
  
  I copied the `.eslintrc.json` file from my `perfect_fit_meal` directory and pasted it in to `playing_with_javascript`. Now we can see it's using the config file to get the airbnb style guide, but it can't find the airbnb module.
  
  ![img](log_imgs/noairbrb_5-10.PNG)
  
  ## Adding `node_modules`
  
  To get the air bnb module into the project, I copied the `node_modules` directory from the `perfect_fit_meal` directory. I pasted it into the `playing_with_javascript` folder.
  
  Now `playing_with_javascript` loads the ESLint locally from the local `node_modules` folder instead of the global one. And it's gettting the airbnb module from there. We can tell that it's getting the airbnb module from there because that error went away after adding `node-modules`.
  
  ![img](log_imgs/local_5-10.PNG)
  
  But our html files aren't getting linted.
  
  ## Why Aren't HTML Files Getting Linted?
  
  I messed with the `eslint-plugin-html` directory name again to see if that would cause issues. It did. So I know the html plugin is being accessed. 
  
  ![img](log_imgs/failed_5-10.PNG)
  
  I changed the folder name back. The error went away. 
  
  But why isn't ESLint linting the html files in `playing_with_javascript`?
  
  This is where I left off. Confused still, but a little less so. So I'm stoked!
  
  
## Day 29, R2
### 5/9/19

- ## Recipe Calculator
  ## To Do
  - Set Up dotenv
  - lint my code
  - Look into Prettier vsc extension because [Jacob M-G Evans  ⚛ @JacobMGEvans](https://twitter.com/JacobMGEvans) said it goes well with ESLint (but what about Beautify?)
  
  ## Prettier
  I started to set up Prettier as a ESLint plugin following Wes Bos's tutorial.
  
  [How to Setup VS Code + Prettier + ESLint](https://www.youtube.com/watch?v=YIvjKId9m2c)
  
  ## Errors!
  Now my ESlint is underlining the wrong things. For example, it underlines `this.readyState` but when I hover over `this.readyState`, ESLint tells me what's wrong with a *different* part of my code.  I deleted everything I added to my settings and configurations and it works again. 
  
  It's probably not working because *Prettier* failed to load:
  
  Terminal:
  
  ```
  Failed to load plugin prettier: Cannot find module 'eslint-plugin-prettier'
  Happened while validating /Users/dashiellbark-huss/Documents/100daysofcode/perfect_fit_meal/index.html
  This can happen for a couple of reasons:
  1. The plugin name is spelled incorrectly in an ESLint configuration file (e.g. .eslintrc).
  2. If ESLint is installed globally, then make sure 'eslint-plugin-prettier' is installed globally as well.
  3. If ESLint is installed locally, then 'eslint-plugin-prettier' isn't installed correctly
  ```
  
  So these extra *Prettier* configurations are maybe throwing off my ESlint configs since Prettier isn't actually installing. I don't know but I want to get *Prettier* to work.
  
  ## Is My ESLint Installed Globally?
  
  Since these errors had to do with whether ESLint was installed locally or global, I had to figure that out.
  
  [How to tell if npm package was installed globally or locally](https://stackoverflow.com/questions/26104276/how-to-tell-if-npm-package-was-installed-globally-or-locally):  
  - `npm list -g`
  - `npm list -g --depth=0`
  
  ### My Global Packages:
  
  <img src="log_imgs/terminal_5-9.PNG" width="400"/>
  
  Looks like **it's installed globally.** 
  
  But, it's **also installed locally** since I can see the *ESlint* files in my project directory. That's probably causing issues.
  
  ## ESLint Button
  
  I figured out how to get that little "ESLint" button to show up in the corner:
  
  Put `"eslint.alwaysShowStatus": true` in your user settings in vsc.
  
  ![eslint_button](log_imgs/eslint_5-9.PNG)
  
  When I click on the ESLint status button button, a tab pops up that shows where my project is getting *ESLint* from:
  
  >[Info  - 14:55:45] ESLint server is running.
  >
  >[Info  - 14:55:45] ESLint library loaded from:
  >
  >/Users/dashiellbark-huss/Documents/100daysofcode/perfect_fit_meal/node_modules/eslint/lib/api.js
  
  So it looks like it's getting it locally. But I'm not sure. Maybe the global installation is interfering?
  
  Also I'm curious where it's getting the ESLint html plugin from, because I saw it both locally and globally. 
  
  I think there's problems with ESLint being local and global. 
  
  ## What To Do?
  
  ### Do I need to install the ESLint prettier plugin both locally *and* globally?
  
  or
  
  ### Do I need to uninstall either my global or local ESLint package so that I only have one package? And then get prettier to work for that?
  
  I'm nervous that if I delete either package, I'll break my `eslint_html_plugin` which took me a while to get to work.

  ## `.eslintrc.json`
  
  I wanted to see if I could find a `.eslintrc.json` file in the global installation of EsLint. To find the global directory:
  
  [Where does npm install packages](https://stackoverflow.com/questions/5926672/where-does-npm-install-packages):
  
  - `npm list -g`
  
  <img src="log_imgs/path_5-9.PNG" width="400"/>
  
  To see this file from the GUI (Finder) I followed this: [how to browse /usr/local directory in finder?](https://discussions.apple.com/thread/5418731)
  
  I didn't see `.eslintrc.json` file in the global eslint directory. So I'm not sure how to add Prettier globally and then how do you configure it? Or do you install it through the command line globally and then configure it locally? Do you just manually add the `.eslintrc.json` file to the global package? So confused.
  
  ## Should ESLint be Global or Local?
  
  > We generally recommend installing ESLint and dependencies locally. It makes for a more reliable experience and it's better for collaboration as well.

  *[Kevin Partington platinumazure](https://github.com/eslint/eslint/issues/10533)*

  >If you want to include ESLint as part of your project’s build system, we recommend installing it locally. 
  >...
  >
  >If you want to make ESLint available to tools that run across all of your projects, we recommend installing ESLint globally.
  
  [Getting Started with ESLint](https://eslint.org/docs/user-guide/getting-started)
  
  ## Uninstalling NPM Packages:
  
  I wanted to see how to uninstall ESlint. I found this:
  
  [Uninstalling packages and dependencies](https://docs.npmjs.com/uninstalling-packages-and-dependencies):
  
  - Unscoped packages: `npm uninstall <package_name>`
  - Scoped packages: `npm uninstall <@scope/package_name>`
  
  What does "scoped" mean? Here's the documentation for [npm-scope Scoped packages](https://docs.npmjs.com/misc/scope), but this is over my head. Is my ESLint scoped? I think not:
  
  > The scope folder (@myorg) is simply the name of the scope preceded by an @ symbol, and can contain any number of scoped packages.
  *[npm-scope Scoped packages](https://docs.npmjs.com/misc/scope)*
  
  ESlint isn't preceded by an **@** symbol *so I think it's **not** a scoped package.*
  
- ## Thoughts & Feelings:
  These packages keep running me into trouble. I don't really understand how they work. I'm not sure how to get more practice with them or what resources to use to understand them. I was watching an npm tutorial but I feel like it skipped a lot of information. Maybe I need to finish that tutorial. But then how to I get hands on practice/experience with these packages?

## Day 28, R2
### 5/8/19

- ## Recipe Calculator
  ## Custom Type Ahead
  
  I got the custom type-ahead to select the suggestion when it is either clicked or `enter` is pressed. 
  
  I changed around the event listeners:
  
  ```javascript
  foodInput.addEventListener('keyup', displaySavedFoods);
  form.addEventListener('keydown', formHandler);
  suggestions.addEventListener('mouseover', changeFocus);
  suggestions.addEventListener('click', searchSuggestedFood);
  ```
  Now, `changeFocus` is called on the `suggestions` list element, before it was called on `form`, which `suggestions` is a child of. I think it reads better.

  I also had to change the callback for `form.addEventListener('keydown',...` from `traverseFocus` to `formHandler`. This is because `form.addEventListener('keydown',...` now has two possible callbacks: `traverseFocus` or `searchSuggestedFood` depending on what key the user pressed.
  
  I also cleaned up irrelevent css and but my css in a separate file. 
  
  Thanks to [Philippe Vaillancourt
@snowfrogdev](https://twitter.com/snowfrogdev) and [Giuseppe 🇮🇹
@montyDev_](https://twitter.com/montyDev_) for the ideas to clean up the CSS and move the event listener to a different element.
  
  ## Added Documentation for Type-ahead Search
 
  I'm calling this a finished project! 
  
  It's just one part of my larger project, but I the search feature it might help others with their projects so I put it out with documentation.
  
  You can go to [Type-ahead Search](https://github.com/DashBarkHuss/type_ahead_search) to see the complete project, along with the documentation.
  
  ![screenrecording](log_imgs/ux_5-8.gif)

**Link To Work:** [Type-ahead Search](https://github.com/DashBarkHuss/type_ahead_search)

- ## Thoughts & Feelings:
  Lately I haven't had to take much breaks. For the most part, I've sped through my two hours of coding with no breaks. Maybe coding is getting more habitual, and my brain needs less breaks because coding is less effortful for me now. Maybe it's because I raised my calories and my brain has excess fuel? I hope it's not that, because that means when I do a cut, my brain will need more breaks.

## Day 27, R2
### 5/7/19

- ## Recipe Calculator
  ## Custom Type Ahead
  
  Today I created two event listeners that change the focus to different suggestions in the type-ahead feature.
  
  ```javascript
  form.addEventListener('keydown', traverseFocus);
  ```
  `traverseFocus()` traverses through the suggestions when you press the up or down arrow.
  
  ```javascript
  form.addEventListener('mouseover', changeFocus);
  ```
  
  `changeFocus()` changes the focus to whatever suggestion you mouseover.
  
  <img src="log_imgs/ui_5-7.gif" width="400">
  
  ## Focus
  
  I used the [`focus()` method](https://developer.mozilla.org/en-US/docs/Web/API/HTMLElement/focus) to set the focus.
  
  I used the [.activeElement property](https://developer.mozilla.org/en-US/docs/Web/API/DocumentOrShadowRoot/activeElement) to find the last element that had focus in order to traverse the suggestions.
  
  ## To Do
  
  I still have to add functionality for when you select a suggestion.
  

**Link To Work:** [Type Ahead Search, not finished](https://github.com/DashBarkHuss/type_ahead_search/tree/5699e5a8a6580426cea92c5d07808d8bdd87a5b5)
  

## Day 26, R2
### 5/6/19

  
- ## Recipe Calculator
  ## Custom Type Ahead Conditionals
  I got my conditionals working. In "link to work."
  
  ## Functionality
  I started working on the functionality of the 'type ahead' feature. The up and down arrows should traverse the options and enter should select it. Mouse should work too. I'm not posting it cause it's a mess still. 
  
  This is going to be a short post today because I'm in a hurry to get to my host's house.
  
**Link To Work:** [Type Ahead Search, not finished](https://github.com/DashBarkHuss/type_ahead_search/tree/a74f0cfdf79e954a85ba00ee050fe57f4606ffd4)


## Day 25, R2
### 5/5/19

- ## Recipe Calculator
  Today we're moving the type ahead output from the console to the UI. This type ahead feature suggests foods to search form a list of saved foods in a json file.
  
  ## Custom Type Ahead UX
  
  I wasn't sure about the UX for the type ahead feature. Should choosing a suggested entry *search* that entry? Or should it *select* that entry and add it to the recipe? I decided to copy what the [USDA database](https://ndb.nal.usda.gov/ndb/search/list) does.
  
  ![usda](log_imgs/usda_5-5.gif)
  
  If you select the suggestion it searches the suggestion. It doesn't choose that entry, which on the USDA database would mean taking you directly to that entries nutrient page.
  
  
  ## Conditionals Not Right
  
  I need to redo my conditional statements so that the `#suggestions` div doesn't display anything when there's no input in the search field. This doesn't work quite right:
  
  ```javascript
  async function displaySavedFoods(){
  const foods = await fetch("saved_food_data.json").then(x=>x.json()).then(x=>x.foods);
  const searchTerm = foodInput.value;

  const filtered = filter(foods, searchTerm);
  const filteredChanged = filtered.length != lastFiltered.length;
  if (filteredChanged){
       const div = document.querySelector("#suggestions");
       div.innerHTML="";
       div.style.width = `${foodInput.offsetWidth}px`;
       if(searchTerm==""){
         lastFiltered="";
         return;
       } else {
         filtered.forEach(x=>div.innerHTML+=`<li>${x.name}</li>`);
         lastFiltered = filtered;
       }
     }
  }
  ```
  <img src="log_imgs/ui_5-5.gif" width="400">
  
  As you can see, even though I have no input in the text field at the end of this gif, the `#suggestions` div still displays.

**Link To Work:** [Type Ahead Search, not finished](https://github.com/DashBarkHuss/type_ahead_search/tree/644753d33806dd4994883e68d4177c2182a9be67)


## Day 24, R2
### 5/4/19

- ## Recipe Calculator
   May the 4th and day 24th be with you!
   
  ## Devtools Console + Sources
  
  To open the **console** and **sources** at the same time:
  
  Press `esc` while on the **Sources Panel**
  
  ![sources and console](log_imgs/console_sources_5-4.gif)
  
  ## Custom Type Ahead
  
  I got my custom type-ahead to pick the correct saved foods. But right now it just logs the results to the console. 
 
  ```javascript
  const form = document.querySelector("form[name=food_search]");
  foodInput = form.querySelector("#food_to_search"); //food search input
  let lastFiltered=[];
  
  async function displaySavedFoods(){
  const foods = await fetch("saved_food_data.json").then(x=>x.json()).then(x=>x.foods);
  const searchTerm = foodInput.value;
  const filtered = filter(foods, searchTerm);
  const filteredChanged = filtered.length != lastFiltered.length;
  if (filteredChanged){
    console.log("%cFiltered:", "color:red;font-size:20px;");
    filtered.forEach(x=>console.log(x.name));
    lastFiltered = filtered;
  }
  }

  function filter(foods, searchTerm){
    let filtered = foods;
    const terms = searchTerm.split(" ").map(x=>x.toLowerCase());
    for(i=0; i<terms.length; i++){
      filtered = filtered.filter(food=>food.name.toLowerCase().includes(terms[i]));
    }
    return filtered;
  }
  
  foodInput.addEventListener('keyup', displaySavedFoods);
  ```
  
  ![type-ahead](log_imgs/type_ahead_5-4.gif)
  
  I tried to use regex for this but had trouble. I think regex would improve the performance of the algorithm.

## Day 23, R2
### 5/3/19

- ## Async, Ajax, Promises
   Was confused why `getData()` was returning a promise when 'name' is not a promise. I did the same lines of code that are in my `getData()` function, in the console to compare what `name` returns in the console with what `getData()` returns.

  ![async](log_imgs/async-5-3.PNG)
  
  Oddly, you can only use `await` inside a function with the `async` keyword. So, I'm not sure how it's working in my console, but it is.
  
  ## Async Always Returns a Promise
  
  This is why `getData()` returns a promise:

  >The word “async” before a function means one simple thing: a function always returns a promise. Even If a function actually returns a non-promise value, prepending the function definition with the “async” keyword directs JavaScript to automatically wrap that value in a resolved promise.
  
  *[Async/await](https://javascript.info/async-await)*
  
  ## Loading in Parallel vs in Sequence
  
  In the [Fun Fun Function Async/Await video](https://youtu.be/568g8hxJJp4?t=991) the instructor says that in his async/await example the images are being loaded in *sequence* and in the promise example they are being loaded in *parallel*.
  
  I read a little more about that here: [JavaScript Async/Await: Serial, Parallel and Complex Flow](https://techbrij.com/javascript-async-await-parallel-sequence)
  
  Still not quite sure what that means, but I have more of a broad idea now.
  
- ## Recipe Calculator
  ## Custom Type Ahead
  
  I went back to my type-ahead problem for my recipe calculator. This is the one chrome does automatically. But I want my custom type-ahead to pull from a list of saved ingredients.
  
  ### Chrome's Type-Ahead
  <img src="log_imgs/type_ahead_5-3.PNG" width="500">
  
  I ended up using async/await for this. 
  
  I also realized that async/await can help with debugging because it's easier to debug with sequencial code. But I'm not sure if that really makes sense. 
  
  I didn't finish. I should start commiting my work again, but not today.  

## Day 22, R2
### 5/2/19

- ## Async, Ajax, Promises
  I read this article that I started yesterday:
  
  [Async, Callback & Promise](https://medium.com/front-end-weekly/ajax-async-callback-promise-e98f8074ebd7)
  
  It also talked about CORS and JSONP which I'm still new to.
  
  ## Async Call Values
  
  Yesterday I learned:
  
  > you cannot return from an asynchronous call inside a synchronous method.
  
  *[How To Return Value from an Asyncronous Callback Function](https://stackoverflow.com/questions/6847697/how-to-return-value-from-an-asynchronous-callback-function)*
  
  So today I redid my AJAX call. It pushes the data to an array instead of tryin to return the data.
  
  ![array](log_imgs/array_5-2.PNG)
  
  ## Redo Above With Promise:
  
  I redid the above with a promise.
  
  ![promise](log_imgs/promise_5-2.PNG)
  
  ## Async Await
  I went back to [this video by Fun Fun Function about async/await](https://www.youtube.com/watch?v=568g8hxJJp4) to learn about async/await now that I've played with promises more.
  
  I like this definition the instructor gives about async/await:
  
  ![async/await](log_imgs/async_await_5-2.PNG)
  
  I played with async/await a bit.

## Day 21, R2
### 5/1/19

- ## Promises
  
  I played with this function that returns a promise of an XMLHttpRequest.
  
  ```javasript
  const makeRequestPromise = (url) => {
    const request = new XMLHttpRequest();

    return new Promise((resolve, reject) => {
      request.onreadystatechange = () => {
        if (request.readyState !== 4) return;
        if (request.status >= 200 && request.status < 300) {
          resolve(request);
        } else {
          reject(new Error({
            status: request.status,
            statusText: request.statusText,
          }));
        }
      };
      request.open('GET', url, true);

      request.send();
    });
  };
  ```
  
  It helped me see how promises work and `.then()`
  
  ![promises](log_imgs/promise_5-1.PNG)
  
  ## Return Value Asyncronous Callback
  
  ![return](log_imgs/return_5-1.PNG)
  
  > you cannot return from an asynchronous call inside a synchronous method.
  
  *[How To Return Value from an Asyncronous Callback Function](https://stackoverflow.com/questions/6847697/how-to-return-value-from-an-asynchronous-callback-function)*
  
  ## Promises vs AJAX & Callbacks
  
  Still trying to sort these out. Here's some resources I'm looking at.
  
  >You are confused about promises and Ajax calls. They are kind of like apples and knives. You can cut an apple with knife and the knife is a tool that can be applied to an apple, but the two are very different things.
  >
  >Promises are a tool for managing asynchronous operations. They keep track of when asynchronous operations complete and what their results are and let you coordinate that completion and those results (including error conditions) with other code or other asynchronous operations. They aren't actually asynchronous operations in themselves. An Ajax call is a specific asynchronous operation that can be used with with a traditional callback interface or wrapped in a promise interface.

  *[What's the Difference Between Promise and Ajax](https://stackoverflow.com/questions/39751395/whats-the-difference-between-promise-and-ajax)*
  
  [Async, Callback & Promise](https://medium.com/front-end-weekly/ajax-async-callback-promise-e98f8074ebd7)
  
  

## Day 20, R2
### 4/30/19

- ## Fetch API

  ### Warning, I was confused during this word vomit:

  I was examing the the Fetch API and I wanted to know what `.json()` does. I couldn't find it in the Promise directory (`console.dir(Promise)`). I found out [.json()](https://developer.mozilla.org/en-US/docs/Web/API/Body/json) is a [method on the Body mixin](https://developer.mozilla.org/en-US/docs/Web/API/Body) of the Fetch API.

  ## Mixins
  >As defined in Wikipedia, a mixin is a class that contains methods for use by other classes without having to be the parent class of those other classes.
  
  *[Mixins](https://javascript.info/mixins)*
    
  ```javascript
  // mixin
  let sayHiMixin = {
    sayHi() {
      alert(`Hello ${this.name}`);
    },
    sayBye() {
      alert(`Bye ${this.name}`);
    }
  };

  // usage:
  class User {
    constructor(name) {
      this.name = name;
    }
  }

  // copy the methods
  Object.assign(User.prototype, sayHiMixin);

  // now User can say hi
  new User("Dude").sayHi(); // Hello Dude!
  ```
  
  In the example above, we see the mixin methods in the `Users` prototype.
  
  <img src="log_imgs/user_4-30.PNG" width="300">
  
  But I don't see that in the Promise directory.
  
  >The Body mixin of the Fetch API represents the body of the response/request...
  >
  >Body is implemented by both Request and Response
  
  *[Body, Mozilla Docs](https://developer.mozilla.org/en-US/docs/Web/API/Body)*
  
  ### Unconfused:
  
  **Oh duh**, it's on the ***Fetch API**, not a method on **Promise!*** I was confusing Promise with the Fetch API.
  
  [Fetch API](https://developer.mozilla.org/en-US/docs/Web/API/Fetch_API):
  
  > Fetch provides a generic definition of Request and Response objects (and other things involved with network requests).
  
  [Request](https://developer.mozilla.org/en-US/docs/Web/API/Request)
  
  [Response](https://developer.mozilla.org/en-US/docs/Web/API/Response)
  
  The [Body methods](https://developer.mozilla.org/en-US/docs/Web/API/Body#Methods) **are** in the Response directory for example. Same with Request.
  
  <img src="log_imgs/response_4-30.PNG" width="400">
  
  
  I'm less confused but still kind of confused with how all of these relate.
  
  >The fetch() method takes one mandatory argument, the path to the resource you want to fetch. It returns a Promise that resolves to the Response to that request, whether it is successful or not...
  > 
  >Once a Response is retrieved, there are a number of methods available to define what the body content is and how it should be handled (see Body).
  
  A little more clear.
  
  ## Promises
  
  >Essentially, a promise is a returned object to which you attach callbacks, instead of passing callbacks into a function.
  
  *[Using Promises](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Guide/Using_promises)*
  
  Theres an example on that page of a Promise with attached callbacks vs passing callbacks into a function.
  
  To understand the difference between Promises and regular XHR I looked at this [page on how to combine XHR and promises](https://gomakethings.com/promise-based-xhr/). I followed along and made this Promise based XHR. Then I made a regular XHR. Tomorrow, I'll play around with them.
  
- ## Thoughts and Feelings:
  Sometimes writing my thoughts out in my log can help me understand what I'm thinking and why I'm confused. 
  
  I need to remember that when I feel like I don't want to code because something is hard, that feeling will soon pass. And come again. And pass again.
  
## Day 19, R2
### 4/29/19

- ## Helping
  I helped someone today on twitter. They sent their code and I showed them what each line was doing and where they had a typo:
  
  **Typo:**
  <hr>
  <img src="log_imgs/typo_4-29.PNG" width="300">
  
  **Step Through:**
  <hr>
  <img src="log_imgs/help_4-29.PNG" width="600">
  

- ## Toggle Eslint
  I couldn't find a built-in way to do this but I found this [Setting Toggle extension by Ho-Wan](https://marketplace.visualstudio.com/items?itemName=Ho-Wan.setting-toggle).
  
  I set my user settings to toggle the `eslint.enable` setting.
  
  <img src="log_imgs/settings_4-29.PNG" width="500">
  
  Then I can press this button to turn Eslint on and off.
  
  <img src="log_imgs/toggle_4-29.PNG" width="500">
  
- ## What Happen To My Log
  
  I looked at my old log and the markdown is not rendering:
  
  https://github.com/DashBarkHuss/100-days-of-code/blob/master/r1-log.md
  
  <img src="log_imgs/log-29.PNG" width="500">
  
  It looks like I'm in edit mode but I'm not. What's up?
  
  Ok I don't know what happened, it's working again.
  
- ## Async/Await & Promises

  I ran into this [exact problem on stackoverflow](https://stackoverflow.com/questions/54082327/why-does-logging-the-result-of-fetch-break-it-body-stream-is-locked) with `fetch()`. I get this error when I try to `console.log` the promise: `Uncaught (in promise) TypeError: Failed to execute 'json' on 'Response': body stream is locked`
  
  The reason is because:
  
  >.json() (and .body(), .text()) may only be called once.
  >
  >The HTTP request is modeled as a stream, and you can't really read from a stream twice.

- ## Thoughts and Feelings:
  Coding was hard today because I was hungry and freezing in the super cold cafe.

## Day 18, R2
### 4/28/19

- ## NPM
  ## Lynda: Learning npm the Node Package Manager
  ### Notes:
  **--save-dev** is used to save the package for development purpose. 
  
  **Where global packages are stored on Mac** /user/local/lin/node_modules or /user/local/lin/node
  
  **-g or -glabal**: add to `npm install` command to install globally
  
  **How to resolve permissions errors**: [Resolving EACCES permissions errors when installing packages globally](https://docs.npmjs.com/resolving-eacces-permissions-errors-when-installing-packages-globally)

- ## ESLint
  ## ESLint on HTML!
  I decided to go back and try to get ESLint to work on HTML files. 
  
  I found this [stackoverflow thread](https://stackoverflow.com/questions/54138689/how-can-i-get-eslint-to-lint-html-files-in-vscode) on ESlint and HTML files.
  
  One of the answers says:
  >I have this in <projectfolder>/.vscode/settings.json
  >
  >{
  >  "eslint.validate": [ "javascript", "html" ]
  >}
  
  `.vscode` is in the directory for the workspace folder, *not* the project folder. That confused me! I found these settings in that file:
  
  ```javascript
  "eslint.validate": [
    "javascript",
    "javascriptreact"
  ]
  ```
  That file is **read-only** so I added this to the **user settings** panel.
  ```javascript
   "eslint.validate": [
        "javascript",
        "javascriptreact",
        "html"
      ]
  ```
  <img src="/log_imgs/settings_4-28.PNG" width="500" />
  
  ### Success! 
  
  <img src="/log_imgs/eslint_4-28.PNG" width="500" />
  
  Glorious red lines!
  
  ## Format in VSC
  
  <img src="/log_imgs/format_4-28.PNG" width="500" />
  
  ## AirBnb Style Guide
  
  I setup my EsLint to use the [AirBnb style guide](https://github.com/airbnb/javascript).
  
  ### Format Tabs
  AirBnb format uses 2 spaces for tabs so I changed my user settings so that tab is 2 spaces not 4. 
  
  [How to customize the tab-to-space conversion factor when using Visual Studio Code?](https://stackoverflow.com/questions/29972396/how-to-customize-the-tab-to-space-conversion-factor-when-using-visual-studio-cod)
  
  ### Functions
  
  I'm noticing that the airbnb style guide has some rules about naming functions. The style guide explains why [here](https://github.com/airbnb/javascript#functions) and [here it talks about arrow functions](https://github.com/airbnb/javascript#arrow-functions).
  
  ## Linting Is FUN!
  Maybe not fun, but this is really helpful! I'm always wondering "How should I best organize my code?" and "How can I make my code more readable?" Linting is like having a teacher over your shoulder telling you what to do and not to do. And then I can look up why in the [AirBnb style guide](https://github.com/airbnb/javascript). This is great! I need to thank that guy from the meetup in Austin!

  ## ESLint Toggle?
  If I want to temporarily toggle off the markings from ESlint; is there a way to do this **quickly**?
  
  I've seen a little ESLint button on the status bar of VSC in other people's setups. But it's not showing up on mine. I wonder if you can toggle ESLint on and off with this.
  
  ### My Status Bar
  <hr>
  <img src="/log_imgs/mineVSC_4-28.PNG" width="500" />
  
  ### Others' Status Bars
  <hr>
  <img src="/log_imgs/othersVSC_4-28.PNG" width="500" />
  
  Couldn't find anything about this in all my searches.
  
## Day 17, R2
### 4/27/19

I decided to look into npm today because I didn't know what I was doing yesterday trying to get ESLint to work. I'll probably be using npm a lot more. Getting more comfortable with npm will help prepare me for learning node.

- ## NPM
  Looked through some of this: [A Beginner’s Guide to npm — the Node Package Manager](https://www.sitepoint.com/beginners-guide-node-package-manager/)

  Watched some of this short a lynda video on package management: [Package Management Basics](https://www.lynda.com/Linux-tutorials/Package-management-Basics/618702/772939-4.html)
  
  Started watching the [Learning npm the Node Package Manager](https://www.lynda.com/Node-js-tutorials/Learning-npm-Node-Package-Manager-2018/761956-2.html) course on lynda.
   
  ## Lynda: Learning npm the Node Package Manager
  ### Notes:

  **package.json:** map for building your project and setting your dependencies.

  **npm init:** base command to initialize a new package.json file.

  **`Cmd` + `Shift` + `.` (dot):** toggle hide/show hidden files

  **locally installed:** When a package is installed locally it's installed on your projects directory.
  
  **globally installed:** When a package is installed globally it will be installed in your system available to all projects.
  
  ## Command Prompt Undo?
  
  I came accross this dilemma and posted it on [stackoverflow](https://stackoverflow.com/questions/55883264/is-it-possible-to-go-back-and-change-the-last-input-you-entered-into-a-command-p)
  
  <img src="log_imgs/npm_4-27.PNG"  width="600"/>

  ## JavaScript Modules: ES6 Import and Export
  The instructor in the lynda tutorial used `import`. I've seen this before but I've never used it.
  
  Here's the [`import` docs page](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Statements/import).
  
  Here's a video on `import` and `export` that I didn't yet finish: [JavaScript Modules: ES6 Import and Export](https://www.youtube.com/watch?v=_3oSWwapPKQ).


## Day 16, R2
### 4/26/19
- ## Recipe Calculator
  ## Async/Await & Promises
  
  I played around with fetch and promises.
  
  I reviewed this video on promises: [Promises - Part 8 of Functional Programming in JavaScript](https://www.youtube.com/watch?v=2d7s3spWAzo)
  
  Then I started to watch this video on async/await: [async / await in JavaScript - What, Why and How - Fun Fun Function](https://www.youtube.com/watch?v=568g8hxJJp4)
  
  ## VSC Extensions
  In the async await video the instructor uses a cool inline evaluation tool called [Quokka](https://quokkajs.com/). I wanted to use it too but after downloading it following the [Quokka documentation](https://quokkajs.com/docs/) and the [Manage Extensions in Visual Studio Code](https://code.visualstudio.com/docs/editor/extension-gallery) page, I realized that to do what the teacher is doing I need the pro version. So forget quokka.
  
  ## ESLint
  
  I also added the ESLint extension and the Beautify extension.
  
  To set up ESLint I followed this: [Eslint setup in Visual Studio Code](https://www.youtube.com/watch?v=o2H8kvuwMKE). The process was a little different for me, so the video might be a little out of date. 
  
  ## ESLint on HTML Files
  I realized ESLint doesn't work on javascript sections on HTML files. It only works on .js files. However, you can add the [eslint-plugin-html](https://github.com/BenoitZugmeyer/eslint-plugin-html) to get ESLint to work on html files.
  
  ## Not Working on HTML Files
  I'm having trouble getting this to work. I'm not sure what this means, in the [docs](https://github.com/BenoitZugmeyer/eslint-plugin-html)
  
  > Note: by default, when executing the eslint command on a directory, only .js files will be linted. You will have to specify extra extensions with the --ext option. Example: eslint --ext .html,.js src will lint both .html and .js files in the src directory.
  
  I'm not sure what the `src` file should be in `eslint --ext .html,.js src`. I tried a path that goes out of my currect directory and back in like this:
  
  `perfect_fit_meal Dashie$ eslint --ext .html,.js ../perfect_fit_meal`
  
  But that didn't work. Now sure what to put there.
  
  I tried going out of the directory and then using the full path:
  
  `Fri Apr 26 ~ Dashie$ eslint --ext .html,.js /Users/dashiellbark-huss/Documents/100daysofcode/perfect_fit_meal`
  
  That didn't work. I tried it again after installing the extension glolablly and that still didn't work.
  
## Cows Showed Up While I Was Coding
  
![cows](log_imgs/cows_4-26.jpg)

## Day 15, R2
### 4/25/19
- ## Recipe Calculator
  
  ## #100DaysOfMath
  In addition to the coding, I added 30 minutes of math study to complement #100daysofcode. I need some math skills for this app. I need to understand a math subject called *optimization*. 
  
  I talked to a twitter peer, Ross Mason, who said he under estimated the amount of math he needs for AI. Since I want to learn AI, I figured I'd start now brushing up on my math.
  
  So I'm doing a mini, not-so-serious, #100DaysOfMath. By not-so-serious I mean I probably won't post about it and I might skip days. It depends on what seems to be a priority on that day. I don't want to take on too much extra study, and then fall behind on what's really important.
  
  ## Scrollbar Size
  
  I figured out how to change the scroll bar size, add bottom `margin` to the `-webkit-scrollbar-track`. I added a top margin too because I thought it looked better. 
  
  ```css
  #search_results_wrapper::-webkit-scrollbar-track {
      margin: 4% 0 var(--add-button-height);
  }
  ```
  
  <img src="log_imgs/scroll_4-25.gif"  width="300"/>
  
  ## Promises & Fetch
  
  I worked on changing my `XMLHttpRequest`s to `fetch()` calls so I can work with promises. I also want to have a pedictive type ahead feature in my search bar like Wes Bos does in [Javascript30](https://courses.wesbos.com/account/access/5c51dab432bb6d664e015352/view/194130156). The problem is I don't think I can do one request for all food names and then just filter throught them, like Wes does. The NDB limits the number of items in a single request. Maybe there is a work around.

## Day 14, R2
### 4/24/19

- ## Recipe Calculator
  ## Overriding User Agent Stylesheet Issue

  <img src="log_imgs/override_4-24.PNG"  width="600"/>
  
  Both of these pages show a workaround, but I didn't find an explanation for why this happens:
  - https://css-tricks.com/snippets/css/change-autocomplete-styles-webkit-browsers/
  - https://mariusschulz.com/blog/how-to-remove-webkits-banana-yellow-autofill-background
  
  ### Workaround:
  
  <img src="log_imgs/overridegood_4-24.PNG"  width="600"/>
  
  Now it's working in my app!
  
  <img src="log_imgs/uiselected_4-24.PNG"  width="300"/>
  
  ## Scroll Bar
  
  I ended trying to add a scrollbar to the box that contains food search results. This way the user knows they can scroll.
  
  Depending on the browser window size, the user may not realize they can scroll:
  - Obvious Scroll:
     - <img src="log_imgs/scrollclear_4-24.PNG"  width="300"/>
  
  - Unclear Scroll:
    - <img src="log_imgs/scrollunclear_4-24.PNG"  width="300"/>
  
  I found [this tutorial](https://scotch.io/tutorials/customize-the-browsers-scrollbar-with-css) which helped me add a scroll bar. 
  
  ### Scrollbar Size
  Now I want to change the size of the scrollbar and scrollbar thumb. I think it might have something to do with `webkit-scrollbar-thumb`.
  
  This might help: https://stackoverflow.com/questions/26493446/change-size-of-scrollbar-thumb-with-css.
  
## Day 13, R2
### 4/23/19

- ## Recipe Calculator
  ## Wrapper Vs Container:
  
  
  >In programming languages the word container is generally used for structures that can contain more than one element.
  >
  >A wrapper instead is something that wraps around a single object to provide more functionalities and interfaces to it.
  
  *[CSS Language Speak: Container vs Wrapper?](https://stackoverflow.com/questions/4059163/css-language-speak-container-vs-wrapper)*
  
  ## Overriding User Agent Stylesheet Issue
  
  I ended with this issue. I'm having trouble over riding the `user agent stylesheet`.
  
  ![screenshot](log_imgs/override_4-23.PNG)
  
  ## Today's End Result
  
  <img src="log_imgs/ui_4-23.gif"  width="300"/>

## Day 12, R2
### 4/22/19

- ## Recipe Calculator
  ## Relative Position & Border Radius
  
  If you have a relative div inside an unpositioned div with rounded corners, you have to add positioning to the container div and z-index.
  
  ### Bad
  <img src="log_imgs/cornersbad_4-22.PNG"  width="300"/>
  
  ```css
  #container{
      border: 5px solid green;
      border-radius: 20px;
      height: 200px;
      overflow: scroll;
  }

  .rectangle{
      height: 60px;;
      background: rgb(31, 255, 31);
      border: 3px solid rgb(255, 255, 255);
      position: relative;
  }
  ```
  
  ### Good
  <img src="log_imgs/cornersgood_4-22.PNG"  width="300"/>
  
  ```css
  #container{
      border: 5px solid green;
      border-radius: 20px;
      height: 200px;
      overflow: scroll;
      position: relative; //________________added
      z-index: 0; //________________________added
  }

  .rectangle{
      height: 60px;;
      background: rgb(31, 255, 31);
      border: 3px solid rgb(255, 255, 255);
      position: relative;
  }
  ```
  ## Absolute Positioned Element in Front of Scroll
  
  This was hard to figure out. Finally got it.
  
  <img src="log_imgs/scroll_4-22.PNG"  width="300"/>
  
  ```css
    #big_container{
      border: 5px solid green;
      border-radius: 20px;
      height: 200px;
      overflow: hidden;
      position: relative;
      z-index: 0;

  }
  #container{
      overflow: scroll;
      position: relative;
      height: 100%;
  }

  .rectangle{
      height: 60px;;
      background: rgb(31, 255, 31);
      border: 3px solid rgb(255, 255, 255);
      position: relative;
  }
  .red{
      background: rgb(255, 31, 199);
      border: none;
      height: 40px;
      position: absolute;
      width: 100%;
      bottom: 0;
  }
  ```
  
  ```html
  <div id="big_container">
      <div id="container">
          <div class="rectangle"></div>
          <div class="rectangle"></div>
          <div class="rectangle"></div>
          <div class="rectangle"></div>
          <div class="rectangle"></div>
      </div>
      <div class="rectangle red"></div>
  </div>
  ```
  ### Push The Last Item Up
  I realized that when you scroll to the bottom the pink element is still covering part of the last item, so I added this css to push the last item up:
  
  ```css
  #container>.rectangle:last-child{
      margin-bottom: 40px;
  }
  ```
  
  ## Form Attribute
  In HTML5, you can use the form attribute to have a form submit button element outside of the respective form element:
  https://stackoverflow.com/questions/6644128/html-input-field-outside-of-form
  
  ## Coded While Driving Through a Dust Storm! 
  <img src="log_imgs/dust_4-22.gif"  width="600"/>

## Day 11, R2
### 4/21/19

- ## Recipe Calculator
  ## Shadow Color
  
  Originally I had a dark green color that matched the hue of the main background color but was a darker green.
  
  ```css
  --shadow: 0px 010px 8px rgb(7, 161, 136);
  --main-hue: rgb(5, 219, 184);
  ```
  
  This colored shadow caused issues. I couldn't use this shadow on other background colors. You don't naturally see dark green shadows on a white background, for example. Thar color of a shadow should change if the background is a gradient of two different colors.
  
  Colored shadows are limited. This green shadow works well on the green side of the gradient, but not the blue.
  
  <img src="log_imgs/greenshadow_4-21.PNG"  width="300"/>
  
  Black shadows with low opacity work on all colors.
  
  <img src="log_imgs/greyshadow_4-21.PNG"  width="300"/>  
  
  So I changed the shadow to black with a very low opacity.
  
  ```css
  --shadow: 0px 010px 8px rgba(0, 0, 0, 0.26);
  ```
  
  Now this shadow will work on any color.
  
  ## White Gap
  
  If you zoom in very close, you can see a white gap between a container that has a border, and the content inside that has a background color.
  
  ```html
  <style>
      #container{
          border: 5px solid green;
          border-radius: 
      }
      #content{
          background: green;
      }
  </style>
  <div id="container">
    <div id="content">
        some content
    </div>
  </div>
  ```
  <img src="log_imgs/whitegap_4-21.PNG"  width="300"/>
  
  You can't see it when zoomed out.
  
  <img src="log_imgs/nogap_4-21.PNG"  width="300"/>
  
  However you can see it in the corners when the corners of the divs are rounded.
  
  <img src="log_imgs/round_4-21.PNG"  width="300"/>
  
  ```html
  <style>
        #container{
            border: 5px solid green;
            border-radius: 20px;
        }
        #content{
            background: green;
            border-radius: 15px; //takes into account 5px border
        }
  </style>
  </head>
  <body>
  <div id="container">
    <div id="content">
        some content
    </div>
  </div>
  ```
  
  ### My solution: 
  Instead of giving the content div a border radius, give the container div an `overflow` of `hidden` or `scroll`. 
  
  ## To Do
  I left off with an issue where the content div is not contained in the container div even with the container is set to `overflow: scroll`. This only happens when the user hovers over the content div which changes the background color.
  
- ## Thoughts and Feelings:
  My log is taking up a lot of my time. I'm not sure if anyone reads it, and I'm not sure how much of it is for me. I think I will cut down on logging. I need to only post the things that will help me, like links that will be helpful later or explanations of concepts I have trouble with and need to remember. 
  
  It might be hard to see when a concept should be logged or not. I probably need a lot less words and explanation than I post now, if I'm going to just log to help myself.
  
  I think it's especially important that I cut down on logging while traveling. My internet & power is limited. I want to learn as much as I can without worrying as much about my log. My log should be a tool to help me, not something that holds me back and takes up my time. Right now it can take longer than an hour just for my log.
  
  I took a break from logging while at Big Bend because there wasn't enough reception out there. I found I got a lot done.



## Day 10, R2
### 4/20/19

  I didn't have internet on this day until the end of the day so my log is sparse.
  
- ## Recipe Calculator
  I worked on css. 
  
  Weird space only appears at difference levels of zoomed in.
  
  <img src="log_imgs/space_4-20.PNG"  width="300"/>
  
  Video of my custom radio inputs:
  
  <img src="log_imgs/ui_4-20.gif"  width="300"/>


## Day 9, R2
### 4/19/19

  I didn't have internet on this day. I'm writing this on 4/20/19.
  
- ## Recipe Calculator
  I worked on css. 
  
  I learned that the `body` in HTML doesn't start until the first element starts. So if the first element is pushed down it will push the body down from the top of the window too. 
  
  Used `::before` and `:last-of-type`.
  
  <img src="log_imgs/ui_4-19.PNG"  width="300"/>

## Day 8, R2
### 4/18/19

  I didn't have internet on this day. I'm writing this on 4/20/19.
  
- ## Recipe Calculator
  I worked on css. 
  <img src="log_imgs/ui_4-18.PNG"  width="300"/>

## Day 7, R2
### 4/17/19
  
  I didn't have internet on this day. I'm writing this on 4/20/19.
  
 - ## Recipe Calculator
   I worked on css. 
   
   I worked with css variables.
   
  ```javascript
  :root { //declare variables
    --accent-color: rgb(0, 196, 163);
    --text-color:rgb(2, 90, 75);
    --text-highlight: rgb(227, 255, 247);
    --border-radius: 20px;
    --shadow: 0px 010px 8px rgb(7, 161, 136);
    --white-shadow: 0px 010px 8px rgba(115, 177, 161, 0.473);
    --main-hue: rgb(5, 219, 184);
    --second-hue: rgb(5, 219, 155);
    --button-font-size: 1.1rem;
  }
  
  form[name=search_results] input:hover{  //use variables
      background: var(--text-highlight);
  }

  ```
  <img src="log_imgs/ui_4-17.PNG"  width="300"/>

## Day 6, R2
### 4/16/19

  ## Coding With No Internet
  I'm going to Big Bend National Park for a few days. There's a big chance we won't get service out there. So I won't be logging or posting starting in two days or tomorrow. Not sure when our service will cut out. 
  
  To prepare for offline coding I'm downloading the documentation I normally use and w3schools.
  
  ## Offline Documentation
  
  ### JS, CSS, HTML, SVG
  I'm downloading documentation for Javascript, HTML, CSS, and SVG through [Dash](https://kapeli.com/dash). You can download the documentation directly [here](https://kapeli.com/mdn_offline]) but Dash helps with searching through the documentation.
  
  ### W3Schools
  I'm also [downloading W3Schools](http://www.mediafire.com/file/wyhv2wik6ttwfz8/www.w3schools.com.rar). This w3schools is from 2016, so there's a probably a good amount out of date. This one [on micrsoft](https://www.microsoft.com/en-us/p/w3schools-offline-version/9nblggh5792k?activetab=pivot%3Aoverviewtab) might be more recent, but I can't access it because of a login issue but you might be able to. I tried a bunch of other links and they were all a bust.
  
  If you need to search W3schools while offline, search while in the *www.w3schools.com* folder on your computer while "search www.w3schools.com" is selected and only search for html documents.
  
  <img src="log_imgs/search_4-16.PNG"  width="750"/>
  
  
- ## Recipe Calculator
 
  Our internet is already going in and out as we're drving through Texas. I've already used my offline resources successfully. 
   
  I'm working on the design for the recipe calculator.
  
  I ended with this design in the browser using DevTools. I wonder if there's a way to easily see all the changes I made to the css in DevTools that I added to "element.style" for a bunch of different elemets. I wasn't able to find them and save these changes. But it will be easy to recreate.
   
  <img src="log_imgs/ui_4-16.PNG"  width="300"/>
  
  ## Saving CSS From DevTools
  
  I didn't yet try this but this looks helpful:
  
  - [Save your CSS changes right in the Google Chrome inspector](https://rafaltomal.com/tips/save-css-chrome-inspector/)

## Day 5, R2
### 4/15/19

- ## Recipe Calculator
  
  ## EMathHelp Might Not Help
  
  I thought [eMathHelp](eMathHelp](https://www.emathhelp.net/calculators/algebra-2/system-of-linear-equations-calculator/)) was gonna help me figure this out, but it only works when the number of variables is the same as the number of equations. 
  
  The number of equations corresponds to the number of nutrients I'm tracking(fat, carbs, protein, etc) and the number of variables corresponds to the number of ingredients(brocolli, beef, whatever you like). Obviously, we're not going to limit the number of ingredients just because we only want to track 3 nutrients.
  
  The fact that the site can't calculate this makes me wonder if it's really complicated to solve what I'm trying to solve. Otherwise, they'd probably program it to work. I'm guessing we can't follow the steps thay take.
  
  ## Back To Asking For Help
  
  Well, I hit a wall but I've learned a bit more. Time to update my posts and get some help.
  
  ### Updated Simbi & Quora Post
  
  I rewrote my question on [simbi](https://simbi.com/dashiell-bark-huss/math-help).
   
  I also posted it on [quora](https://www.quora.com/unanswered/How-can-I-find-the-closest-possible-positive-values-to-solve-a-system-of-equations-for-many-variables-and-equations). Details in comments.
  
 
  ### Email
  I also emailed my mother in law's friend who is a math professor who I already was in contact with:
  
  >Hey Joanne,
  >
  >Here's my updated question on Simbi with the new knowledge I've acquired: https://simbi.com/dashiell-bark-huss/math-help.
  >
  >To summarize: How can I find the closest possible positive values to solve a system of equations for many variables and equations?
  >
  >Context, I've had very little math experience in the last 10 years. I'm trying to write a program that uses this math. 
  >
  >Any thoughts are appreciated.
  >
  >-Dash 
  
  I was trying to be succinct but polite. Hopefully these emails and questions will help you by giving you an example of how you might ask a question which is why I'm including them in my coding blog.
  
  ## Where Now?
  
  I'm already getting some answers back and this looks majorly complicated. Maybe I should have clarified that I haven't done any math in ten years. I'm considering partnering with a math person for this project. What do you do when you need help? 
  
  - When should you outsource?
    - pay someone? 
    - get free help?
    - partner with someone?
    
  - When should you learn the subject? 
  
  If it's hard, it's worth doing, right? Whatever means it takes.
  
  I'm gonna take a break and work on css or something fun and cute. 
  
  ## CSS
  I played around with css for the last half hour of my two hour session. I looked at code pen for ideas.
  
  ## Search Bar
  I changed my search and input so it's cuter.
  
  ### Before:
  <img src="log_imgs/search_4-15.PNG"  width="200"/>
  
  ### After:
  <img src="log_imgs/searchcss_4-15.PNG"  width="200"/>
  
  ## Get Rid Of The Space Between Text Input and Submit
  
  Here's some [tricks for getting rid of the space](https://css-tricks.com/fighting-the-space-between-inline-block-elements/) between inline block elements. I guess inputs have an inline-block display by default, but I'm not sure how to check what they default to. For mine, I just got rid of the space in my html even though I don't like the look in the html.
  
  ## Dev Tools CSS Tricks
  
  Sometimes I forget how to write css selectors. If you inspect an element you can see that selector here:
  
  <img src="log_imgs/selector_4-15.PNG"  width="300"/>
  
  And you can add css in the browser on that selector if you add the selector on devtools here on the styles panel:
  
  <img src="log_imgs/addselector_4-15.PNG"  width="300"/>
  
  You can also add a class if you click the .cls button.
 
  

## Day 4, R2
### 4/14/19

- ## Recipe Calculator

  I have been thinking about this math problem nonstop. I seriously dreamt about it all night!
  
  ### Here's the progress I made yesterday:
  
  Yesterday I called my friend [Shaily Hakimian](https://twitter.com/hakimian45) who is a math tutor. Shaily said we should brainstorm on bitpaper.io a whiteboard site. [Jerami](https://twitter.com/CodeAndLonely) has been helping too and joined the whiteboard.
  
  <img src="log_imgs/bitpaper_4-14.PNG"  width="500"/>
  
  Talking it out and trying to explain the problem to Shaily and Jerami helped me ***abstract my problem into an equation.***

  I then posted the problem on [Simbi](https://simbi.com/dashiell-bark-huss/math-help)
  
  > Hey I’m looking for help with a math problem. I have no clue what kind of math this problem uses. I don’t want the answer alone, I want to know how it’s solved or any information on how it might be solved. Ultimately I’m trying to figure out the general algorithm to solve issues like these.
  >
  >Problem:
  >
  >10≈1x+7y 
  >
  >6 ≈ 2x+1y 
  >
  >15≈5x+2y
  >
  >How can we solve for x and y so that the values on the right equal as close as possible to the values on the left. These equations all live in the same world, x is the same in all of them and so is y.
  >
  >Ex: 
  >If X=3 and y =1 the difference between our left side and and right side is an absolute value of 3.
  >
  >10≈10_________+0 
  >
  >6 ≈ 7 __________+1 
  >
  >15≈17 _________+2 
  >
  >—————————— 
  >
  >Total difference: 3
  >
  >But are there values for x and y that would give a smaller total difference? How can we solve this algorithmically?
  >
  >I’d also like to be able to extend this problem to more variables. ex:
  >
  >10≈1x+7y +6z 
  >
  >6 ≈ 2x+1y +12z 
  >
  >15≈5x+2y +3z
  >
  >A concrete example:
  >
  >If I have apples, chicken, and butter and I want to eat 20g of fat, 22g of protein, and 10g of carbs, how much of each food should I eat to get as close as possible to my desired macronutrients?
  >
  >Per 100g 
  >
  >Apple: fat: 0g protein: 1g carbs: 16g 
  >
  >Chicken: fat: 4g protein: 20g carbs: 0g 
  >
  >Butter: fat: 100g protein: 0 carbs: 0g
  >
  >Fat 20 ≈ 0a + 4c + 100b 
  >
  >Protein 22 ≈ 1a + 20c + 0b 
  >
  >Carbs 10 ≈ 16a + 0c + 0b
  >
  >a= Amount of apple (1=100grams of apple) 
  >
  >c= Amount of chicken 
  >
  >b= Amount of butter
    
  ## System of Equations
  
  I sent my simbi post to my friend Sean. **That's when I got a break through!**

  Sean said he knew what this problem was: *a system of equations*.
  
  I finally had *something* to google. This helped me tremendously. 
  
  ## Solving System of Equations With Matrices
  
  Sean said watch this video to learn how to solve the problem: [Matrices to solve a system of equations](https://www.youtube.com/watch?v=AUqeb9Z3y3k&fbclid=IwAR2SaAZPtdZZmbSFi8QWkIne989Z_-kPWa8H4tG0un9tDDkoG9Z18Wdb5NI)
  
  So I solved it on paper and it worked. 
  
  <img src="log_imgs/mathpaper_4-14.PNG"  width="400"/>

  Getting closer!
  
  ## What About For More Variables(Ingredients) and More Equations(Nutrients)?
  
  It took me a while, but I finally found a calculator that can solve systems of equations with more than 2 variables and 2 equations. [eMathHelp](https://www.emathhelp.net/calculators/algebra-2/system-of-linear-equations-calculator/)! 
  
  EMathHelp also shows the ***steps you need to take.*** Knowing the steps will be important because we'll need to **translate those steps to code.**
  
  I tested a bunch of sites with these 3 equations since I know the answer here should be **1** for every variable. **b=1, a=1, g=1**
 
  - 25b+3a+0g=28 
  - 0b+10a+0g=10 
  - 15b+1a+100g=116 
  
  All amounts should equal 1. A lot of sites couldn't figure it out. I don't know why. But [eMathHelp](https://www.emathhelp.net/calculators/algebra-2/system-of-linear-equations-calculator/) worked.

  ## Testing With EMathHelp
  
  Let's say we want these foods:
  
  *Grams per 100 gram measurements:*
  - Apple : { protein: 3, carbs: 10, fat: 1 }
  - Beef: { protein: 25, carbs: 0, fat: 15 } 
  - Ghee: { protein: 1, carbs: 2g, fat: 100 }
  
  *(nutrient measurements fudged)*
  
  And we want these total nutrients:
  
  Protein: 20, Carbs: 7, Fat: 20
  
  ### That gives us these eqautions:
  
  - 3a+25b+1g=20 *(total protein)*

  - 10a+b+2g=7 *(total carbs)*

  - 1a+15b+100g=20 *(total Fat)

  We need to solve for ***a, b, g***

  [Here's the solution using emathhelp with steps](https://www.emathhelp.net/calculators/algebra-2/system-of-linear-equations-calculator/?s=3a%2B25b%2B1g%3D20%2C++10a%2Bb%2B2g%3D7%2C++1a%2B15b%2B100g%3D20&v=&method=j&steps=on)
  
  Answer: a=7151171≈**0.610589239965841**, b=8471171≈**0.72331340734415**,  g=1001171≈**0.085397096498719**
  - 61g of apple
  - 72g of beef 
  - 8.5 grams of ghee
  

  ## What Could Go Wrong?
  
  Next I need to do this all in code. But I wonder:
  
  - What if there are no exact answers? 
    - How can we find the closest answer?
  - What if the answers are negative? 
    - How can we find the POSITIVE closest answer? You can't have a negative food, unless maybe you vomit it up, but that's a different app.
  - What if the answer is too small? Probably round??
  
- ## Thoughts and Feelings:

  I spent so much time on the math of this problem and hardly any coding. I guess that's part of coding when you get into making useful stuff, you'll need to learn other things. This would be a good time to have a math consultant. Is that a thing? I must be because what else do math people do?
 
  I went to a crowded coffee shop today. It forced me to sit with strangers. The strangers happened to be developers! What a cool coincidence! They were fun to talk to. Their names are Michael and Dan. 
 
  Here's a cool [project Dan made](https://helveticascenario.dev/mandlebrot). Click-drag to zoom in, right click to zoom out.
 
  They're working on a productivity app together. They talked to me about the pros and con of living in Austin vs. the bay area. Basically, you can get ahead in the bay area, but it's a bit a soul sucking and people are unaware of how the world works because they're stuck in tech world where smart-toilets are a good idea. Like L.A. but for tech.



## Day 3, R2
### 4/13/19

I went to a meetup today that had a lot of coders. This guy Joe was talking me up the wazoo so I didn't get a lot done. His favorite saying was "Too make a short story long". But he was cool and gave me some good advice. 
  
Joe told me to reexamine how deeply I want to go into a subject as opposed to using a framework. I have been doing a lot of vanilla JavaScript. Maybe it's time to try react soon?
  
For more on this conundrum Joe recommended listening to this podcast:
 - [Episode #205: Beginners and Experts Panel](https://talkpython.fm/episodes/show/205/beginners-and-experts-panel)
 
His general advice for picking a tech podcast to listen too was to find some old dude who's been around the block but isn't yet bitter. I think that's good advice.

He also said to start working with a linter because it makes it easy for others to read your code. Uniformity.
  
  
- ## Recipe Calculator

  I basically am thinking through the problem from yesterday both on code and on paper. I think I'm closer. But I am so far from an answer. I'm gonna leave my log short today because I've just done a ton of brain storming and working through this problem. So not much concrete to post about. And because I was delayed from all the meetup shmoozing. See you tomorrow.
  

## Day 2, R2
### 4/12/19

- ## Recipe Calculator

  ## How To Find The Right Combination of Ingredients?
  
  I'm trying to figure out how to find how much of each ingredient I need given a certain amount of nutrient-totals the user wants to reach. 
  
  I thought the answer would be complicated and I would need to find some complex algorithm. But my very helpful twitter peer [Khawar Jatoi](https://twitter.com/khawar_jatoi) said that an algorithm isn't necesary. He said, think about iterations and things you already know. I decided to give it a shot. Heads up, I didn't get the answer yet and I'm probably doing a bunch of things worng but this is my exploration into the problem. 
  
  ## Experimenting On Paper
  I started by trying to figure out the answer on paper using my brain. This way I would get an idea of what the code needs to do.
  
  Using my example from yesterday: 
  
  ```javascript
   {
      object1: { blue: 3, red: 20, yellow: 0 }
      object2: { blue: 0, red: 2,  yellow: 12 }
      object3: { blue: 20 red: 4,  yellow: 6 }

   }

   // How much of each object to total as close as possible to: blue: 70 red: 40, yellow: 15?
   ```
 
  I went through each object: How much of object1 would I need to total to blue:70? Blue for object 1 is 3 so 70/3 is 23.33. What would that do to the rest of our numbers: Multiply them by 23.33. Well, red would be too high, yellow would sill be zero.
 
  I thought through this for a while. It started to clarify some things to me. I decided to make a program to help me play around with this idea. 
 
    
  ## Experiementing In Code
 
  I took this problem to code, inorder to play around with it faster.
 
  ```javascript
   var array=[

      {fat:1, protein:2, carbs:4},
      {fat:4, protein:2, carbs:1},
      {fat:1, protein:4, carbs:2}

    ]
    
    const a = array[0];
    const b = array[1];
    const c = array[2];
    
    //__________________some made up desied totals the user might want
    var tFat = 15;
    var tProtein = 10;
    var tCarbs = 7;

  ```
  I made this array with objects representing foods with different amounts of nutrients. Then I made functions to help figure out what I was doing on paper, but faster in code. I'm not going to post the entire code because I left off with a bug and and some messiness. I will post it tomorrow. 
  
  You can see here how I could use my functions to tell me what all of our total nutrients would be if we used only one ingredient to reach an exact amount of our desired fat, or carbs, or protein:
  
   <img src="log_imgs/ndb_4-12.PNG"  width="350"/>
  
  Q is the [quotient](http://www.amathsdictionaryforkids.com/qr/d/division.html) (How much we need of the ingredient)
  
  F, P, C are fat protein and carbs.
  
  The parenthesis show how much under or over the macro totals would be from our desired totals.
  
  This helped me see how, if we were going to only use one ingredient (an over simplified answer to my problem), the best answer would probably be the one where the absolute values of the numbers in the parenthesis add up to the lowest amount.
  
  Still a lot to do.
  
  ## CSS In Console
  
  I used this article to try to get [CSS in my logs](https://hackernoon.com/styling-logs-in-browser-console-2ec0807dc91a). I didn't finish.

  ## Can you Spot the problem?
  
  I spent a while on this issue. Whenever I called `overOrUnder`, I got `undefined`. I spent way too long on this problem.

  Can you spot the issue?
  ```javascript
  const over = 'background-color: red';
  const under = 'background-color: yellow';
  const exact = 'background-color: green';
  
  const overOrUnder= (x)=>{
    if(x=0)return exact;
    if(x>0)return over;
    if(x<0)return under;
  }
  ```
  
  ## Answer
  
  I used the equals assignment operator `=` instead of the equals relational operator `==`.

## Day 1, R2
### 4/11/19

## Starting Round 2
Round 2 let's go!

- ## Recipe Calculator
  
  ## `.find()`
  
  I used `find()` to get the object in an array that matches a nutrient id. ex: 
  
  ```javascript
  nutrientRecipeArray[0].nutrients.find(x=>x.nutrient_id=="208")
  ```
  
  This is where I look for array methods and where I found `find()`: [JavaScript Array Reference](https://www.w3schools.com/jsref/jsref_obj_array.asp)

  ## Added A bunch of Functions
  
  I added a bunch of functions:
  - `nutrientValue(food, nutrient)`
    - returns a nutrient value for a specified food object. 
    - ex: `nutrientValue(nutrientRecipeArray[0], "fiber")`
  - `calories(food)`
    - short cut for `nutrientValue(food, "calories")`
  - `netCarbs(food)`
    - finds the net carbs from `nutrientValue(food, "carbs")` & `nutrientValue(food, "fiber")`
  - `fat(food)`
    - short cut for `nutrientValue(food, "fat")`
  - `protein(food)`
    - short cut for `nutrientValue(food, "protein")`
  - `nutrientRatio(food, nutrient, perNutrient)`
    - gets the ratio of a two nutrients for a specified food object. 
    - ex: `nutrientRation(nutrientRecipeArray[0], "protein", "calories")`
  - `sortDescending(array, nutrient, perNutrient)`
    - Sorts the food objects of an array by nutrient ratio. 
    - ex: `sortDescending(nutrientRecipeArray, "netCarbs", "calories")`
    
 ## Thinking Through Functions
 
 I really thought through how to seperate the functions so that they could be reused by other function. I think I did a good job. A lot of my functions use other functions that I made. This makes it easier to change add other related functions later and cleaner to see.
 
 ## Do I need An Algorithm?
 
 I feel like this next part is complex. But I feel like there might be an algorithm to solve it. I haven't really learned about algorithms.
 
 I'm trying to get the amounts needed of a list of ingredients that would total to specified nutrients or close to specified nutrients.
 
 I'm not sure what to search to even begin.
 
 ### Using another example: 
 If I have a collection of objects, each with different values of colors:
 
 ```javascript
 {
    object1: { blue: 3, red: 20, yellow: 0 }
    object2: { blue: 0, red: 2,  yellow: 12 }
    object3: { blue: 20 red: 4,  yellow: 6 }
    
 }
 
 // How much of each object to total as close as possible to: blue: 70 red: 40, yellow: 15?
 ```
 
 How much do we need of each object inorder to total as close as possible to specific amounts?  **ex totals: blue: 70 red: 40, yellow: 15**
 
- ## Thoughts and Feelings:
  Can't believe I'm on round two! I got a lot done because I saved most of my log until the end, instead of logging while coding.
 
 
 
    
 
