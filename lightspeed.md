# Notes on Lightspeed work for Kaiser

## 12.27.21 API --->SQL?

Looking at new features and the heavy reporting focus we're trending towards it would be extremely useful if I could query data directly with SQL. As there is no way for direct SQL access through the API my thought is that I could build a bot of sorts that crawls through the necessary API endpoints and get needed info and sticks it in a DB.

Biggest advantages would be a clean separation between API access code and any actual app code, and performance improvements.

Biggest downside is more complexity regarding on-demandedness of information - someone makes a change in Lightspeed and immediately switches to the app and wants the report refreshed - info will be stale until bot updates it again. 

Another big downside is that this could not scale well. There's a finite number of API requests I can make to Lightspeed in a given time interval. As the number of users grow the information latency grows with it - not a good pattern.

Could make it so if user's press an "update" button of some sort that the bot runs on the relevant data set and then the page refreshes. Seems kind of "Web 2.0". May be ways to combat that

Could try to detect refreshes and trigger the bot then

The alternative would be to try to find a way to speed up the API access/data wrangling. Could be some kind of caching solution. 

I think the initial step here is to examine my current Django/Plaw code to really figure out which part is taking so long and see if there are optimizations that can be made.

## 12.28 No more Plaw

In diagnosing the performance issue I'm realizing that the abstraction from API --> Wrapper --> Service --> View is too much to reason about and is resulting in unnecessary manipulation and passage of data. I think instead I should put the core parts of Plaw into the service functions for a more direct integration.

### Functions that call API

`_call_api` 

Required args are endpoint and account. Optionally takes params.

Tests

* Test that it properly converts datetimes ✔
* Test that it handles invalid tokens ✔
* Test that it returns the correct number of pages in generator ✔
* Test that it handles query operators  ✔

`_call`

Endpoint, params, and account all required. Account may be `None` and params may be empty dict. Puts the access token in the header and makes the response. Throws an error if token is invalid. Returns `json` of response.

Tests

* Test that it adds endpoint to Base URL ✔
* Test that it adds access token to header ✔
* Test that it raises on 401 and 400 ✔
* Test that it returns JSON of response ✔

`refresh_access_token`

Just requires account. Raises if refresh token is invalid. Returns new access token.

Tests

* Test that it POSTS to correct URL with proper payload ✔
* Test that it raises on 400 ✔
* Test that it returns new access token ✔

`get_tokens`

Just requires temporary code from Lightspeed. Raises an exception if Lightspeed doesn't honor the code (likely timeout related). Returns access and refresh tokens.

Tests

* Test that it POSTS to correct URL with proper payload ✔
* Test that it raises on 400 ✔
* Test that it returns tokens ✔

`get_account_info`

This is kind of a special case as it does make a direct call to API but it never needs an account ID, just client ID and secret, and tokens. Also, we need to call this once before we ever construct the account object, as this call is what will give us the account ID and name and allow us to make more API calls. 

What we'll do is construct some kind of alternative `account`-like object, like a `Record` type or whatever, to pass into `_call_api` that looks and behaves like an `account` from the perspective of `_call_api` and `_call`. 

Tests

* Test that it returns account info ✔
* Test that it returns new access_token if it must be refreshed ✔

## 01.03 Revisiting views and service functions

Now that I have Plaw's most basic functionality in I need to take a step back and look at the pipeline of data from API to template and figure out the best way to move it through with minimal manipulation. So for each page on the site I need to ask "What data from Lightspeed is being displayed to the user and in what form?" and, when that's determined, look at the API docs and see what calls the service function needs to make and in what ways it needs to manipulate data to get it into proper form. The view should do very little, if any, manipulation of data. It should just receive it and pass it on. 

#### Punch Log page

User inputs desired dates. Page displays breakdown of hours, by day and shop, and aggregate information such as total hours, total by shop, total by employee.

Needed API endpoints: EmployeeHours, which gives a list of shifts by employee and shop, Employees which gives a list of employees and IDs, and Shops, list of shops and IDs.

Data structure returned by service function: `dict` with keys `total_hours`, `totals_by_shop`, `totals_by_employee`, and `punch_log`. 

`total_hours` is just a float of total values.

`totals_by_shop` is another dict with shop keys pointed to floats.

`totals_by_employee` is another dict similar to shop

`punch_log` is a dict. Keys are dates, which point to a dict. keys are shops, which point to dict. keys are name, clock in, clock out, hours.

Tests

* Test that it makes the correct punch log for a given list of shifts/employees/shops
* Test that it doesn't shit the bed if there is one shift
* Test that it doesn't shit the bed if there are no shifts
* Test that it handles date wrangling correctly











