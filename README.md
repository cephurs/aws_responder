# AWS Respond

# Description

The idea of this is to have a module (plugin) based AWS response tool.

# Using

### Help

```
python .\aws_respond.py -h

usage: aws_respond.py [-h] [--module MODULE] [--listmodules] [--moduledetails]
                      [--values VALUES [VALUES ...]]

AWS Incident Response Kit (AIRK).

optional arguments:
  -h, --help            show this help message and exit
  --module MODULE       Specify the module (action) you want to run.
  --listmodules         Lists all of the available modules (actions).
  --moduledetails       Lists descriptions of the available modules.
  --values VALUES [VALUES ...]
```

### List Modules

```
python .\aws_respond.py --listmodules

userlogins
```

### Get Module Details

```
python .\aws_respond.py --moduledetails

USERLOGINS
    Module:      userlogins
    Author:      Patrick Olsen
    Version:     0.1
    Date:        2018-09-13
    Description: Lists the user logins. Includes username, userid, created date and password last used date.
```

### Running a module:

This is an example of running the userlogins module. The modules can be anything you create and then just pass them as --module <module_name>

Each module should have a .module description file as well. This gives information about the module.

This userlogins was just a POC. I will develop more as time goes on. Feel free to add any you find interesting.

```
python .\aws_respond.py --module userlogins

[
    {
        "UserName": "dfir",
        "UserId": "AID<snip>HKW",
        "CreateDate": "2018-08-13T20:47:17+00:00",
        "PasswordLastUsed": null
    },
    {
        "UserName": "polsen",
        "UserId": "AID<snip>KWUY",
        "CreateDate": "2017-10-12T18:38:32+00:00",
        "PasswordLastUsed": "2018-09-17T11:23:22+00:00"
    }
]
```

In the example with disabeling a user's access key you can pass their access key as a value. It will return Success or Failed.

```
python .\aws_respond.py --module disableaccesskey --values AK<snip>SA

Success
```

It's also possible to disable the access keys for multiple users using the disableallaccesskeys module.

```
python .\aws_respond.py --module disableallaccesskeys --values dfirtest polsen

[
    {
        "UserName": "dfirtest",
        "AccessKeyId": "AK<snip>DQ",
        "Result": "Successful"
    },
    {
        "UserName": "dfirtest",
        "AccessKeyId": "AK<snip>7A",
        "Result": "Successful"
    },
    {
        "UserName": "polsen",
        "AccessKeyId": "AK<snip>CA",
        "Result": "Successful"
    },
    {
        "UserName": "polsen",
        "AccessKeyId": "AK<snip>DQ",
        "Result": "Successful"
    }
]
```