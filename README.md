The following url, when POSTed to, gives back a JSON respone of all comsci modules.
https://opentimetable.dcu.ie/broker/api/categoryTypes/241e4d36-60e0-49f8-b27e-99416745d98d/categories/events/filter

The above request requires the following payload:

```json
{
  "ViewOptions": {
    "Weeks": [
      {
        "WeekNumber": 23,
        "WeekLabel": "23",
        "FirstDayInWeek": "2023-02-13T00:00:00.000Z"
      }
    ]
  },
  "CategoryIdentities": ["721c8cf9-18dc-3e8e-f11e-83900b84896d"]
}
```

The following authorisation header is also required, which uses [Basic Authentication](https://developer.mozilla.org/en-US/docs/Web/HTTP/Authentication#basic_authentication_scheme), which is clearly very easy to break.

```json
{
  "Authorization": "basic <code>"
}
```

The `scripts/fetch-auth-key` bash script will fetch the basic auth key, which is hard coded into opentimetable's
main JavaScript bundle.

The JSON response from the above URL when the prerequisites are met is structured as follows:

```json
[{
    {
        "CategoryTypeIdentity": UUID v4 of category type
        "CategoryTypeName": Programmes of Study, Location etc,
        "CategoryEvents": [
            "EventIdentity": UUID v4 for module,
            "HostKey": Not sure, e.g. 2223#SPLUSB944E9,
            "Description": 'Lecture'/'Tutorial',
            "StartDateTime": timestamp of class start,
            "EndDateTime": timestamp of class end,
            "Owner": UUID v4, not sure what for, maybe UUID of module's corresponding school,
            "Location": Lecture theatre/lab class is in,
            "ExtraProperties": [
                {
                    "Name": "Module Name",
                    "DisplayName": "Module Name",
                    "Value": Name of module,
                    "Rank": number, not sure what for
                },
                {
                    "Name": "Staff Member",
                    "DisplayName": "Staff Member",
                    "Value": Name of lecturer,
                    "Rank": number, not sure what for
                },
                {
                    "Name": "Activity.TeachingWeekPattern_PatternAsArray",
                    "DisplayName": "Weeks",
                    "Value": Range of weeks, e.g. "19-24, 26-31",
                    "Rank": number, not sure what for
                }
            ]
            "Name": Cryptic name of module,
            "Identity": UUID v4 which represents the module
        ]
    }
    "Name": Course code, e.g. COMSCI1,
    "Identity": Course's UUID v4
}]
```
