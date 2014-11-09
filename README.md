## wombat

wombat is a todolist following the GTD philosophy. It's core is an http-interface and the main user interface is through a command-line tool.

It doesn't exist yet, but the plan is to make a Go client and a Go server over the coming month. 

### goal lifecycle

name | description
--- | ---
INBOX | A goal that needs to be decided on
WAITING | A goal that is waiting for a thing to happen
SOMEDAY | A goal that is sleeping and just needs a regular review
SCHEDULED | A goal that is scheduled to be done at a specific time

`wombat-cli` is the command line tool. 

### basic commands

command | description
--- | ---
`wombat today` | Show an overview of goals for today.  
`wombat goals` | List all goals
`wombat new goal [description] --shortcut [s]` | Create a new goal
`wombat [shortcut] note [note]` | Add a note to a goal
`wombat schedule [shortcut] [date]` | Schedule a goal to a specific moment
`wombat someday [shortcut]` | Move the goal to the SOMEDAY queue
`wombat waiting [shortcut] [waiting for]` | Set a goal to be waiting
`wombat inbox [shortcut]` | Move the goal to the INBOX queue

### example usage

    >> wombat today
    Today is November 9th, 2014. You have 5 goals in your inbox and 2 goals scheduled for today.

    ## Inbox

    | 08-11-2014 | smess1  | Send a message  |
    | 06-06-2014 | rev2    | Review wombat API |
    | 05-03-2014 | domADFD | Renew DNS settings for gijskoot.nl |

    ## Scheduled today

    | clean1  | Clean up kitchen |
    | post2   | Send a postcard to parents |

    >> wombat done rev2
    You have completed goal rev2. Well done!
        'Review wombat API'

    The following tasks are still scheduled for today:

    | post2   | Send a postcard to parents |

    >> wombat waiting
    There are 4 goals waiting for something else to happen

    | GOAL     | DESCRIPTION               | WAITING FOR
    | buybike1 | Buy a new race bike       | Get november salary
    | deapi    | Complete wombat api draft | UTH feedback


## API

### authentication

All users get a personal bearer token they can use to authenticate requests, over `https`. That may be changed later, but right now the idea is to run your own wombat server and invite friends on it. 

### models

user

    {
        "id" : user_id,
        "screenname": string,
        "created_at": date iso8601
    }

goal

    {
        "id": goal_id,
        "shortcut": string,
        "goal": string,
        "created_at": date iso8601,
        "user_id": user_id,
        "notes": [string],
        "lifecycle": "INBOX" | "SOMEDAY" | "WAITING" | "SCHEDULED",
        "done": boolean,
        "deleted": boolean
    }

### routes

    GET | POST /me

    GET | POST /goals

    GET | DELETE /wombat/api/goals/[goal_id]
