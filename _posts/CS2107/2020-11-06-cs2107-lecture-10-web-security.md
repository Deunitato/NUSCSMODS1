---
layout: post
published: true
title: 'CS2107 - Lecture 10: Web security'
---
# Background
Overview:
1. User click on url link
2. Http request is sent to server
3. Server construct and include a html file inside its HTTP response to the browser (Possibly with set cookie headers)
4. Browser renders the html files, which describe the layout to be rendered and presented to the user and any cookies are stored in the browser

## Sub-resource of a webpage
- COntain subresoources (Images, multimedia files, css, scripts) including from external party
- Browser will contact the respective server for the resources
- Seperate http request for every singe file on a page
![CS2107-10-1.PNG]({{site.baseurl}}/img/CS2107-10-1.PNG)

## Request and Response format

## Web client and server components
- Client:
	- HTML: Webpage content
    - CCS: Webpage presentation
    - JS: Webpage behavior, making pages responsive and interactive
- Server:
	- Web server: Scripting language is typically use as well
    - Database server: Interaction between webserver and database server via sql
    
### What javascipt can do
- Write text into page
- Read and change html elements
- React to events, such as when a page has finished loading or when user clicks on an HTML element
- Validate user data
- Access cookies
- Interact with the server
# Security issues and threat models
## Complications
Browser Operation:
- Browser run with same prev as user: Browser can access user files
- Multiple server could provide the content: Access isolation among sites is requried
- Browser support rich command set and controls for content providers to render the content
- For enhance funcitionality many browser support plugins, add-ons extensiion by third party

Browser usage:
- User info and secrets
- User could update content in the server:
	- Forum social media sites where names are to be displayed
- More and more iusers sensitive data is stored in the web cloud
- For PC, the browser is becoming the main/super applicaation, in some sense the browser is the OS

![CS2107-10-2.PNG]({{site.baseurl}}/img/CS2107-10-2.PNG)

- 1A forum poster
	- Weakest
    - User of existing app
    - Does not register domain
- 1B Web attacker
	- Oqwns valid domain and server with ssl cert
    - can entice victim to visit site
    - Can not intercept/read traffic for other sites
    - most commonly assumed attacker type

# Attacks on the secure communcation channel
# Misleading web user
## URL
Componments:
1. Scheme
2. Authority
3. Path
4. Query
5. Fragment

> THe attacker will aim authrotiy and path


### Misleading delimiter
- There is no clear visual distinction between host name and path of URL
- The supposed delimiter that seperates hostname and the path can be a chara in the host name or path
- The displayed different intensities could help user spot the attack

### Address bar spoofing
- webpage can render objects or popups in an arb location
- Attack trick the user to visit bad url

# Cookies and same origin policy
## Cookies
- Set by webserver
- Send in response using header
- Consist of name value pair
- Usually use for user preference, shopping cart content or session identifier
- Stored by user browser
- Browser auto send in inscope cookies to the server in its HTTP Request


### Usage
- Session cookie: Deleted after session ends
- Persistent: at specific date or after a specific length of time
- Secure cookie: Can only be sent on HTTPS

## TOken base authentication
1. Auth: Ask username and password
2. Valid, server send token t
3. Browser keeps token t
4. For every other (Subsequent) request to server, browser will attach t to the request

- Token is use to identify session
- Also called SID (Session ID)
- Stored in cookie often
- Using cookie is better approach than attching the SID as a url encoded parameter

### Choice of cookie
- t needs to be random and long
- if t is a random chosen number, then server has to keep a table to store all tokens issued
- To avoid storing the table,
	- (Insecure) Cookie is some meaningful infomation concat with predicatble seq number
    	- Insecure because attacker can forge uit
    - (Secure) Cookie is two parts, random chosen value or meaningful infomation (Expire data) AND concat with the message authentication code (MAC) computed with server secret key
    	- relies on MAC

## Scripts and same origin policy
- Which script can access which cookies
- Can scripts in webpage A can access the cookies stored by webpage B iff both A and B have same origin
- Origin is combi of
	- Protocol
    - hostname
    - port number

Complications:
- There are exception

![CS2107-10-3.PNG]({{site.baseurl}}/img/CS2107-10-3.PNG)

# Cross site scripting (XSS) attacks
## Background
- Client can enter string s in the browser which is to sent to the server
- Server response with html that also contain s
- BUT WHAT IF, s is a script

e.g `http://www.comp.nus.edu.sg/<script>alert("hehehehe");</script>`

> Note that the attack wint work if the server performs html encoding which replace the character `<` with `&lt`

## The attack
1. Tricks user to click on url which contains the target website and malicious scriot s
2. Request sent to server
3. Server construct a response html but does not check the request carefully
4. Browser renders the html page and runs the script s

- Attacker can know the session id of the user
- Can deface the original webpage
- Steal cookies
- Example of priviledge escalation: A malious script coming from attacker has priviledge of the web server and read the latter's cookie
- The attack above the expoits the client trust of the server: The browser believes that the injected script is from the server

## Stored (Persistent) XXS
- The script s is stored in target web server
- More dangerous than reflected xss
-  The malicous script is rendered automaitcallyu
- The victim to script ratio is many to 1

## Defences
- Relies on server side
- Server filters then removes any malicious script in http request
- Filters and removes any malicious script in a user post before it is saved into forum database

Examples:
- Script filtering
- Noscript region: do not allow javascript to appear in certain region of a webpage
- This is not a fullproof method
- Attaditionally detect reflected XSS attack, some browsers employ a client side detection mechanism: XSS Auditor

# Slides
<iframe src="https://drive.google.com/file/d/15bh62ptPmJ_gomBN2xky15cALtRLxOoJ/preview" width="640" height="480"></iframe>