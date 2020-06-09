# Customer Service Support Technician Tech Test
Test for CS Support candidates

## Requirements

Allowing for setting up the project, this tech test should take around 2 hours to complete.

1. Find the cause of issues in the application:
* There are three bugs to find
* You are not required to *fix* the bugs, your aim is to __identify__ what is causing them

2. Document your process:
  * What did you notice/find?
  * What are your recommendations?

## Getting Started
These instructions will get the application up and running on your local machine for bug finding purposes.

* Install Ruby [Ways of Installing Ruby](https://www.ruby-lang.org/en/downloads)
* Install Yarn [Ways of Installing Yarn](https://yarnpkg.com/lang/en/docs/install)
* Clone this git repository `git clone git@github.com:smartpension/smart-cs-support-test.git`
* cd into the locally cloned repo `smart-cs-support-test`
* Run `bin/setup` 
* Start up your web-server (see the output of `bin/setup`)
 * If you get an error with: `getaddrinfo: nodename nor servname provided, or not known (SocketError)` 
   * Try running your server with`./bin/rails server -b 0.0.0.0`
     
* Navigate to: http://localhost:3000

Remember to document __how__ you identified the bugs and attach your findings to your email back to us, have fun!!


## What I did

- installed ruby / yarn
- yarn is used here so similar to npm: before starting up the app i ran `yarn install` to get all JS dependencies this project uses.
- added .gitignore to remove node_modules from commit
- had issues with a `nio4r` gem which wasn't present in the Gemfile so I added that 
- issue with `nio4r` persisted, this was resolved by reinstalling ruby
- uncommented gems `redis` and `bcrypt` to be installed. I don't believe Redis was necessary as I wasn't working with the production build.
- run bin\setup

- saw this while setup was running:

      yarn cache v1.22.4
      success Cleared cache.
      Done in 47.47s.
      yarn install v1.22.4
      [1/4] Resolving packages...
      success Already up-to-date.
      Done in 1.10s.  

^ i'm assuming me running 'yarn install' myself wasn't necessary in this case (just a habit from group work)

- migrations succeeded so that is good

- <b>page loaded without issues so I proceeded to use the UI and see if I ran into any errors with any of the processess.</b>

- `Create New Company`- form filled out and on submission I was redirected to Mickeys Plaice dataset?
- Checked companies list (http://localhost:3000/companies) and my newly added company was present
- No issues deleting the company either
- All "Show" buttons redirect to "Mickeys Plaice" employees?

      `Employee Load (0.2ms)  SELECT "employees".* FROM "employees" WHERE "employees"."company_id" = ?  [["company_id", 1]]`

- it appears that instead of taking the id parameter from the url it query's employees from company with id = 1 which appears to be Mickeys Plaice.

- Similar issue when trying to edit employees at Mickeys Plaice:

      Employee Load (0.3ms)  SELECT "employees".* FROM "employees" WHERE "employees"."id" = ? LIMIT ?  [["id", 4], ["LIMIT", 1]]

- no matter which entry is selected the edit button calls queries for employee with id = 4 (which is Daffy Duck)
### CORRECTION

- having tried this with other routes (/companies/x) it appears that the query uses the id value (x) which I believe to be the company id as the employee id which is incorrect.

### FURTHER CORRECTION

- on http://localhost:3000/companies/2/employees/5/edit : </br>
when clicking "back to employees list" it returns to the url : </br>
  http://localhost:3000/companies/2/employees </br></br>
 the query for this url call is as follows:
      
      Employee Load (0.3ms)  SELECT "employees".* FROM "employees" WHERE "employees"."company_id" = ?  [["company_id", 2]]
    this url returns the correct company for that with company_id = 2


### Other findings
- Home button (logo) redirects back to localhost:3000, no issues there

- http://localhost:3000/companies/1/employees </br> this url has action "Edit Employee" and "Destroy Employee" whereas</br> http://localhost:3000/companies/5 </br>has 
action(s) "Edit". Singular / Plural column names in the incorrect places? 
Also why does one have more actions than the other?

### Keeping it DRY

- I noticed your error code .html files have the same styling written 3 times. To write cleaner code I would write one .css file that they are all linked to.
