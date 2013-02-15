Atlas Documentation
==========

This will walk you through the basics of Atlas and guide you through creating your first Atlas robot.

## How it works

Atlas is, at its core, a central brain for your robot. Using the API, you'll send sensor readings to Atlas, we'll process the data and send back actions to be performed based on your robot's capabilities. The actions will be high-level; for instance, if the sensors detect that you're moving quickly toward a wall, we'll tell your robot to turn 45° and avoid the wall. Then your robot can interpret the action given its onboard software (ROS, Arduino, etc.) and hardware.

## Step-by-Step Guide

**Step 1.** Create an Account

Go to [**/users/sign_up**](http://tann.la/users/sign_up) and fill out the form.

![Sign Up page](http://i45.tinypic.com/2ltitrr.jpg)

**Step 2.** Generate an App ID and Secret

After signing in, click the Account link in the header, or go to [**/account**](http://tann.la/account).

![Account link](http://i47.tinypic.com/azb0cm.jpg)

Click **Manage my applications**, then **New application**.

Now name your application whatever you'd like. Since we'll be developing this tutorial on the Atlas website, we'll set the Redirect URI to be **urn:ietf:wg:oauth:2.0:oob**. If you were developing your own interface for testing or visualizations, you could replace the Redirect URI with your own domain.

![New application](http://i47.tinypic.com/34ybs5g.jpg)

**Step 3.** Generate an Access Token

### Ruby

    irb -r oauth2
    redirect_uri = '…'
    app_id = '…'
    secret = '…'
    auth_code = '…' # from authorizing the app
    
    client = OAuth2::Client.new(app_id, secret, site: "http://tann.la")
    client.auth_code.authorize_url(redirect_uri: redirect_uri) # Generate authorization URL, if you haven't authorized your app, yet
    access = client.auth_code.get_token(auth_code, redirect_uri: redirect_uri)
    
    access.token # => "1cb2d5226e3ffba323asd821jo0f7e78a8"

### Python

Coming soon.

### C++

Coming soon.

Once you have your access token, go back to your [**Dashboard**](http://tann.la/dashboard) and [**Create a New Robot**](http://tann.la/robots/new).

![New robot](http://i50.tinypic.com/2z8ng52.jpg)

## Interfacing with the RESTful API

Pass in your access token for every request, e.g.

    http://tann.la/api/robots?access_token=1cb2d5226e3ffba32372ff792c6e9192be48

### View All Robots

    GET /api/robots

### View a single robot

    GET /api/robots/:id

:id is the robot's id, which you can find under Robot Details on its page.

### Get sensor readings

    GET /api/robots/:id/readings/
    
Useful for doing custom visualizations with your robot's data.

### Send sensor readings

    POST /api/robots/:id/readings/

Sample Data:

    "id" : "ak18uasjnaknca281",
    "reading" : {
      "sensor" : "range",
      "reading" : 50
    }
    
### Get past actions

    GET /api/robots/:id/actions
    
Useful for building a custom robot training interface.

### Train an action

    PUT /api/robots/:robot_id/actions/:id

### Get a new action recommendation

    GET /api/robots/:id/actions/new

The recommended action is based on past sensor data for the robot.

### Get existing capabilities

    GET /api/capabilities

### Get user information

    GET /api/user
