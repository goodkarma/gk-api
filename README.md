The brand new Good Karma API
=============================

We're excited to announce v1 of the [Good Karma API](http://goodkarmaapp.com). With only a few HTTP calls, you can let your customers, users, or visitors to your site give to nonprofits of their choice. It's a great way to align your brand with the interests of your users.

If you are interested in trying it out, please contact us [here] (mailto:api@thegoodkarma.co) and we will hook you up wuth api key.

Making a request
-----------------

All URLs start with `https://goodkarmaapp.com/api/v1/`

It is a REST-style API that uses JSON for serialization and header based authentication. At this point all requests require authorizationi and all of your api activities are scoped to your organization/api key.

Authentication
---------------

In order to make an API call you must provide an API key in the authentication header. It should be in the form:

`Authentication: Bearer YOUR_API_KEY`

For example, if you wish to see the state of a current user in your system, you could make a call like this:

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" \
  https://goodkarmaapp.com/api/v1/user/username@yourcompany.com | python -mjson.tool
```

and would receive something like this back

```json
{
    "user": {
        "balance": 5, 
        "contributed": 181, 
        "user_id": "username@yourcompany.com", 
        "wallet": [
            ... , ...
        ]
    } 
}
```

We are currently in beta so if you are interesting in trying this out, feel free to get in touch [here] (mailto:api@thegoodkarma.co) and we will provide you with a key to use for the time being.

Campaigns
---------

>As an api user, we allow you to create campaigns to help raise money for a specific nonprofit as well as set a fundraising goal for your users to reach.

You can create a campaign with a `POST` request to `https://goodkarmaapp.com/api/v1/campaign`

```shell
curl --data '{"goal": 1000, "name": "Our first campaign!","description": "We really care about animals","nonprofit_id": 1 }' \
  --header "Authentication: Bearer YOUR_API_KEY" \
  https://goodkarmaapp.com/api/v1/campaign | python -mjson.tool
```

```json
{
    "campaign": {
        "active": true, 
        "description": "We really care about animals", 
        "goal": 1000, 
        "id": 20, 
        "name": "Our first campaign!", 
        "nonprofit": {
            "id": 1, 
            "name": "Sean Casey Animal Rescue", 
            "tagline": "Coming to the aid of unfortunate animals of all kinds", 
            "url": "https://goodkarmaapp.com/np/seancasey"
        }, 
        "percent_complete": 0.0, 
        "raised": 0
    }
}
```
You can view all your campaigns with a `GET` request to `https://goodkarmaapp.com/api/v1/campaign`

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" https://goodkarmaapp.com/api/v1/campaign | python -mjson.tool
```

```json
{
    "campaigns": [
        {
            "active": true, 
            "description": "We really care about animals", 
            "goal": 1000, 
            "id": 1, 
            "name": "Our first campaign!", 
            "nonprofit": {
                "id": 1, 
                "name": "Sean Casey Animal Rescue", 
                "tagline": "Coming to the aid of unfortunate animals of all kinds", 
                "url": "https://goodkarmaapp.com/np/seancasey"
            }, 
            "percent_complete": 17.1, 
            "raised": 171
        }, 
	....
}
```

Users
-----

`GET` request to `https://goodkarmaapp.com/api/v1/user/:username` will return info on the username

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" \
  https://goodkarmaapp.com/api/v1/user/test_user
```

Would return:

```json
{
    "user": {
        "balance": 5, 
        "contributed": 181,
        "user_id": "test_user", 
        "wallet": [
            {
                "amount": 5, 
                "expires_at": 1341289935, 
                "fallback_nonprofit": {
                    "id": 1, 
                    "name": "Sean Casey Animal Rescue", 
                    "tagline": "Coming to the aid of unfortunate animals of all kinds", 
                    "url": "https://goodkarmaapp.com/np/seancasey"
                }, 
                "issued_at": 1333513935, 
                "token": "c8tf5dha04a844b884e5ad339e97dcbc"
            }
        ]
    }
}
```

Points
------

`POST` to `https://goodkarmaapp.com/api/v1/points/reward`

```shell
curl
```

Would return 

```json
```

Contributions
-------------

`POST` to `https://goodkarmaapp.com/api/v1/contribution`

Would return

```json
```

IFrame Contributions
---------------------

>Not everyone will want to provide their own contribution UI so we also provide an API endpoint to generate an iframe that you can embed into your own site. It provides a simple interface where users can decide which of your campaigns they want to contribute towards.

You can either create an IFrame on the contribution level or on the user level. This means you can authenticate an Iframe for a single contribution (using a contribution token), or for all of the unused points a user has collected (using their user id).

A contribution level IFrame is useful for one off rewards, where as a user_id level IFrame is more useful for a user page or profile.

`POST` to `https://goodkarmaapp.com/api/v1/iframe`

*User level authentication

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" --data '{"user_id": "username"}' \
   https://goodkarmaapp.com/api/v1/iframe | python -mjson.tool
```
*Contribution level authentication

```shell
curl --header "Authentication: Bearer YOUR_API_KEY" --data '{"token": "CONTRIBUTION_TOKEN"}'
  https://goodkarmaapp.com/api/v1/iframe | python -mjson.tool
```

Will return something like this:

```json
{
    "iframe_url": "https://goodkarmaapp.com/api/v1/iframe/0d9ea5046d9d4019b58a12835d682642"
}
```


