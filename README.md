Atlas Documentation
==========

This will walk you through the basics of Atlas and guide you through creating your first Atlas robot.

## How it works

Atlas is, at its core, a central brain for your robot. Using the API, you'll send sensor readings to Atlas, we'll process the data and send back actions to be performed based on your robot's capabilities. The actions will be high-level; for instance, if the sensors detect that you're moving quickly toward a wall, we'll tell your robot to turn 45° and avoid the wall. Then your robot can interpret the action given its onboard software (ROS, Arduino, etc.) and hardware.

## Step-by-Step Guide

**Step 1.** Create an Account

Go to **/users/sign_up** and fill out the form.

![Sign Up page](http://i45.tinypic.com/2ltitrr.jpg)

**Step 2.** Generate an App ID and Secret

Click the Account link in the header, or go to **/account**.

![Account link](http://i47.tinypic.com/azb0cm.jpg)

Click **Manage my applications**, then **New application**.

Now name your application whatever you'd like. Since we'll be developing this tutorial on the Atlas website, we'll set the Redirect URI to be **urn:ietf:wg:oauth:2.0:oob**. If you were developing your own interface for testing or visualizations, you could replace the Redirect URI with your own domain.

![New application](http://i47.tinypic.com/34ybs5g.jpg)

**Step 3.** Generate an Access Token

Follow the link to authorize the app by clicking **Authorize**, then hit the **Authorize button**. Grab the Authorization Code, and follow the guide for your language of choice to generate a token.

### Ruby

    irb -r oauth2
    callback = '…'
    app_id = '…'
    secret = '…'
    auth_code = '…'
    
    client = OAuth2::Client.new(app_id, secret, site: "http://tann.la")
    client.auth_code.authorize_url(redirect_uri: callback) # Generate authorization URL, if you haven't authorized your app, yet
    access = client.auth_code.get_token(auth_code, redirect_uri: callback)
    
    access.token # => "1cb2d5226e3ffba323asd821jo0f7e78a8"

### Python

Coming soon.

### C++

Coming soon.

Once you have your access token, go back to your **Dashboard** and **Create a New Robot**.

![New robot](http://i50.tinypic.com/2z8ng52.jpg)

## Interfacing with the RESTful API

Pass in your access token for every request, e.g.

    http://tann.la/api/robots?access_token=1cb2d5226e3ffba32372ff792c6e9192be48

### View All Robots

GET /api/robots

### View a single robot

GET /api/robots/:id

    "id" : "ak18uasjnaknca281"

### Send sensor data

POST /api/robots/:id/readings/

    "id" : "ak18uasjnaknca281",
    "reading" : {
      "sensor" : "range",
      "reading" : 50
    }

### Get a new action recommendation

GET /api/robots/:id/actions/new

    "id" : "ak18uasjnaknca281"

The recommended action is based on past sensor data.

Right now, this requires you to specify a sensor range. *This will no longer require a sensor_range to be passed in, soon.*

    "sensor_range" : "0..1023"

### Get existing capabilities

GET /api/capabilities

### Get user information

GET /api/user
