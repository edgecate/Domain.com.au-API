# Domain.com.au-API
> Python code to access Domain.com.au APIs

[![IMAGE ALT TEXT](http://img.youtube.com/vi/_OJBOy00IJ0/0.jpg)](http://www.youtube.com/watch?v=_OJBOy00IJ0 "How To Access Domain.com.au APIs")

This article is going to take a look at how to use Python and the Domain Developer APIs to get property data in Australia: https://developer.domain.com.au/docs/introduction

The Domain Developer APIs allow you to make 500 API calls per day to obtain a lot of detailed information about Agents, Listings, Properties, and Locations.

To kick things off, sign up for an account which is as easy as providing your email address and a unique password.
An email will be sent to you, with a verification link, so click on that.

Once you’re logged in:
- Go to 'Create A New Project', which determines how you’ll interact and what information you’ll download with the Domain Developer API.
- Head to the Project’s Page, click on Create Project, and give it a Project Name, and Description. I’ll call my project, Sold Prices, and a description of what the project is about.
- Click on Packages – there’s an option between Agents & Listings and Properties & Location which offer different resources and information.
- Check the Plans & Packages > Package Features menu in the main screen to check which one is right for you.
- For this example, since I want the Sold Prices, I’ll be using the Properties & Location Package. Scroll down  to Properties > Read and click on the GET link. Note down the Scope, and GET URL.
- So we’ll go back to our Project Details page, and select Properties & Location, and click Save.
- The next step is to Create A Project Key which will determine how we authenticate with the Domain API. So let’s go back to our Project page, and click on Credentials, and click Create OAuth Client.
- Select Client Credentials as the Grant Type, and provide a Client URL which I believe will be used like an App Name – I used my website URL. The other 2 fields are optional, here, so we can leave these alone.
- Click on Save Changes, then under OAuth Client Secrets, we’ll include a secret description. I’ve just typed Edgecate secret in here, and click Add.
- The last piece of information we need is the token authentication URL, which we can get from going back to the main page, and clicking on Authorisation > Client Credentials Grant.
- Copy the Python module below, and replace the input variables (client ID, secret, scope, and authentication URL) with your own details.

```python
import requests
import json

client_id = 'YOUR_CLIENT_ID'
client_secret = 'YOUR_CLIENT_SECRET'
scopes = ['api_properties_read']
auth_url = 'https://auth.domain.com.au/v1/connect/token'
url_endpoint = 'https://api.domain.com.au/v1/properties/'
property_id = 'RF-8884-AK'


def get_property_info():
    response = requests.post(auth_url, data = {
                        'client_id':client_id,
                        'client_secret':client_secret,
                        'grant_type':'client_credentials',
                        'scope':scopes,
                        'Content-Type':'text/json'
                        })
    json_res = response.json()
    access_token=json_res['access_token']
    print(access_token)
    auth = {'Authorization':'Bearer ' + access_token}
    url = url_endpoint + property_id
    res1 = requests.get(url, headers=auth)    
    r = res1.json()
    print(r)


get_property_info()
```
