+++
title = "Handling null properties in JSON payloads in Power Automate"
date = "2024-03-08"
author = "Jude Sudbury"
cover = "/20240308/true_or_false.webp"
description = "Handling null properties in JSON payloads in Power Automate"
+++

Following on from my previous article on querying the Companies House API in Microsoft Power Automate, in this article I'll explore an option for handling missing JSON properties in a given payload that are necessary for subsequent actions in your flow.

#### The Problem

As an overview, this is what my flow does:

1. From a source Excel spreadsheet, iterate through a list of company registration numbers that I'm interested in finding charge information for.

2. In this loop, there is an HTTP GET action that constructs a URI from the current row of the Excel spreadsheet.

3. Following this is a Parse JSON action, the contents of which are used to set up another (nested) loop which iterates through all 'charges' listed against that company

4. There is then a 3rd nested loop (a loop within a loop within a loop) that iterates through 'persons entitled', which is an array associated with each charge.

5. The data is written to a SQL database using an Insert Row action.

However, during the course of testing my flow, I noticed that some of the JSON payloads were causing the flow to fail. Why? Because some charges (very old ones) registered on Companies House have no persons entitled listed. This means the JSON property 'persons_entitled' was completely missing. Without this, the 3rd loop, which required this property to exist, would fail.

Power Automate doesn't have a failsafe mechanism for dealing with null entities, so we have to be a little creative.

#### The Solution

The solution I used was to create a condition action:

![The condition action in Power Automate](/20240308/condition.png)

Here you can see my flow. The condition action sits directly below the loop which iterates through the list of charges in the Companies House JSON payload. Its job is to check the existence of the 'persons_entitled' property. If it exists, then continue with the inner most loop (IteratePersonsEntitled), if not, then effectively do nothing (I have a compose action here that simply concatinates a string; it's merely there so *something* happens in the false branch).

The condition contains the following expression:

```javascript
contains(items('IterateCharges'), 'persons_entitled')
```

The contains function inspects items from the IterateCharges loop, looking for the existence of the persons_entitled array. If this property was missing entirely from the JSON payload (for a given charge) then that array will be absent and the function will return false.

![Condition expression configuration](/20240308/function.png)

So you can see the logic. If the persons_entitled array is present, the contains function returns true. The condition expression states that if the expression = true then proceed with the actions in the true branch, else follow the false branch.

Now my flow can continue without falling over in the event of a missing property!

#### Other Tweaks

- I had to amend the JSON schema I posted in my [previous article](https://echo3.co/posts/using-power-automate-with-the-companies-house-api/), removing several required properties, notably 'links' and 'persons_entitled' since this was also causing my flow to fail when these were not included in the JSON payload.

- Some companies queried via the API do not have any charges listed, so the API returns a 404 not found error. To handle this in Power Automate, you must configure the 'run after' setting for the action directly following the HTTP action:

![Configuring run after in Power Automate](/20240308/runafter.png)

In my flow, I have a delay action that I added to avoid the 600 queries per 5 minutes imposed by the Companies House API. Here I ticked the 'Has failed' option under Run After under the HTTP action.


☕️ If you found this post helpful, I'd appreciate it if you would [buy me a coffee](https://www.buymeacoffee.com/heyjudeuk)