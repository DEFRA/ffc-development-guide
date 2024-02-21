# Availability Alert Setup Guide

Availability alerts send web request to an application at regular intervals from various different Azure region around the world. It will alert a set Action Group if an application isn't responding, or if it doesn't respond within a given timescale.

The simplest test is a URL ping. A URL ping relys on a public accessible URL.

## How to add an availability alert

The availability alerts are setup through the Azure Portal on the subscription where alerting is required.

1. Select an Application Insight resource
2. Click on the "Availibility" option under "Investigate" within the navigation pane.
3. Within the "Create Test" pane

    3.1. Within "Test name" add a name for the test

    3.2. Select "URL ping test" for the "Test type"

    3.3. Within URL add the URL for the web page you would like to test

    3.4. Select Parse dependent requests. This will include any dependencies (images, scripts, style files etc...) and test will fail if any these resources cannot be found. The response time will also include the loading of these dependecies.

    3.5. Select "Enable retries for availability test failures". This will ensure if a test fails, it is retried after a short interval.

    3.6. Select a "Test Frequency"

    3.7. Use the default 5 location(s) for the "Test locations". These are the Azure regions where the ping tests will send web requests from.

    3.8. Add the required "Success criteria"

    3.9. Click "Create"

4. After a few minutes, click "Refreh" to see your test results

5. To setup "Action Groups" for your alerts. Click on the elipseat the side of the test, to open the menu

6. Click on "Open Rules (Alerts) page

7. Within the "Rules Management" screen, Click on the Rule for the Availability Alert

8. Under "Actions" Click on "Manage action groups"

9. Within the "Select an action group..." pane, Click "Create action group"

    9.1. Under "Basics" and "Instance Details" add an "Action name" and a "Display name"

    9.2. Click "Next:Notifications > "

    9.3. Under "Notifications" Select "Email/SMS message/Push/Voice". Within the pane, select "Email" and enter an email address. Click "Ok".

    9.4. Click "Review + create"

    9.5. Click "Create"

## Reference 

https://docs.microsoft.com/en-us/azure/azure-monitor/app/monitor-web-app-availability


