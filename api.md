### Tickets, users, organizations
- Create a ticket.


```curl https://whalelava.zendesk.com/api/v2/tickets.json -d '{"ticket": {"subject": "1430My printer is on fire!", "comment": { "body": "999The smoke is very colorful." }}}' -H "Content-Type: application/json" -v -u christian.richeson@gmail.com -X POST```


- Create a ticket and create a requester for that ticket in the same command. (needs to be tested)


```curl https://whalelava.zendesk.com/api/v2/tickets.json -d '{"ticket": {"requester": {"name": "The Customer", "email": "thecustomer@domain.com"}, "submitter_id": 410989, "subject": "My printer is on fire!", "comment": { "body": "The smoke is very colorful." }}}' -H "Content-Type: application/json" -v -u christian.richeson@gmail.com -X POST```


- Create an organization and then an end-user who has that org as their main org, then a ticket with that user as the requester.
 

```curl https://whalelava.zendesk.com/api/v2/tickets.json -d '{"ticket": {"requester": {"name": "The Customer", "email": "thecustomer@domain.com"}, "submitter_id": 410989, "subject": "My printer is on fire!", "comment": { "body": "The smoke is very colorful." }}}' -H "Content-Type: application/json" -v -u christian.richeson@gmail.com -X POST```

- Assign a ticket to an agent and then solve the ticket out, with a comment, in one request.
 

```curl https://whalelava.zendesk.com/api/v2/tickets/{112}.json -H "Content-Type: application/json" -d '{"ticket": {"status": "solved", "assignee_id": "1337162288", "comment": {"public": true, "body": "Thanks, this is now solved!"}}}' -v -u christian.richeson@gmail.com -X PUT```

- Create a user with multiple organizations. What organization will be set on a ticket who has that user as a requester?


```curl https://whalelava.zendesk.com/api/v2/organization_memberships/create_many.json -H "Content-Type: application/json" -d '{"organization_memberships": [{"user_id": 1337162288, "organization_id": 756772457}, {"user_id": 1337162288, "organization_id": 599469078, "default": true}, {"user_id": 1337162288, "organization_id": 598353198, "default": true}]}' -v -u christian.richeson@gmail.com -X POST```

- Add a tag to a ticket, then a second tag. Make sure that both tags are now present on the ticket.


```curl https://whalelava.zendesk.com/api/v2/tickets/7/tags.json -X PUT -d '{ "tags": ["customer","test"] }' -H "Content-Type: application/json" -v -u christian.richeson@gmail.com```











###Account settings/buisnes rules

- Create a trigger. Update a ticket in a way that the trigger will apply, then find the audit ID of the trigger's action.


```curl -u christian.richeson@gmail.com https://whalelava.zendesk.com/api/v2/triggers.json -H "Content-Type: application/json" -X POST -d '{"trigger": {"title": "test trigger", "all": [{ "field": "status", "operator": "is", "value": "open" }, { "field": "priority", "operator": "is", "value": "high" }], "actions": [{ "field": "assignee_id", "value": "1337162288" }]}}'```

```curl https://whalelava.zendesk.com/api/v2/triggers/62446448.json -v -u christian.richeson@gmail.com```


-Create a macro. Apply that macro to a ticket and save the result.


```curl -u christian.richeson@gmail.com https://whalelava.zendesk.com/api/v2/macros.json -H "Content-Type: application/json" -X POST -d '{"macro":{"title": "72 hr no response close v2", "actions": [{"field": "status", "value": "solved"}, {"field": "comment_value", "value": "Hello, Our agent has tried to contact you about this support request and has not heard back from you in the last 72 hours.  Due to this we will be closing the ticket.  Please feel free to contact us again if you need further assistance with this issue. Thanks. "}]}}'```
 


-List all of the views in your account, then get the results of one of them.

```curl https://whalelava.zendesk.com/api/v2/views.json -v -u christian.richeson@gmail.com```

```curl https://whalelava.zendesk.com/api/v2/views/61718668/execute.json -v -u christian.richeson@gmail.com``

-Create a view.

```curl -u christian.richeson@gmail.com https://whalelava.zendesk.com/api/v2/views.json -H "Content-Type: application/json" -X POST -d '{"view": {"title": "high priority and open", "all": [{ "field": "status", "operator": "is", "value": "open" }, { "field": "priority", "operator": "is", "value": "high" }]}}'```

-Preview the results of a view that you specify without actually creating it.

```curl https://whalelava.zendesk.com/api/v2/views/preview.json -v -u christian.richeson@gmail.com -X POST -H "Content-Type: application/json" -d '{"view": {"all": [{"operator": "is", "value": "pending", "field": "status"}], "output": {"columns": ["subject"]}}}'```

-Create a SLA policy, then update it.



```curl https://whalelava.zendesk.com/api/v2/slas/policies -H "Content-Type: application/json" -d '{ "sla_policy": { "title": "Incidents", "description": "For urgent incidents, we will respond to tickets in 10 minutes", "position": 3, "filter": { "all": [{ "field": "type", "operator": "is", "value": "incident" }], "any": [] }, "policy_metrics": [{ "priority": "normal", "metric": "first_reply_time", "target": 60, "business_hours": false }, { "priority": "urgent", "metric": "first_reply_time", "target": 30, "business_hours": false }, { "priority": "low", "metric": "requester_wait_time", "target": 6000, "business_hours": true }, { "priority": "normal", "metric": "requester_wait_time", "target": 500, "business_hours": true }, { "priority": "high", "metric": "requester_wait_time", "target": 360, "business_hours": false }, { "priority": "urgent", "metric": "requester_wait_time", "target": 240, "business_hours": false }]}}' -v -u christian.richeson@gmail.com -X POST```


-Add a new required ticket fied.

```curl https://whalelava.zendesk.com/api/v2/ticket_fields.json -H "Content-Type: application/json" -X POST -d '{"ticket_field": {"type": "text", "title": "Serial Number", "active": true, "required": true, "visible_in_portal": true, "editable_in_portal": true, "required_in_portal": true, "regexp_for_validation": "\\w{7}"}}' -v -u christian.richeson@gmail.com```

-Create a new brand for your helpdesk. Set it as your default brand. Add a brand image for it.






###End-users

-As an end user, create a new request.  (issue resolved by removing custom fields from inside the comment field

```curl https://whalelava.zendesk.com/api/v2/requests.json -d '{"request": {"subject": "Help!", "comment": {"body": "My printer is on fire!"}, "custom_fields": [{"27836418": "1234567"}]}}' -v -u guy@place.com/token:1yO4CeBapTn7T8Ag1ZKbusrAwq6in2CD6nIjLsD7 -X POST -H "Content-Type: application/json"```

-As an end user, list the requests that you have open.

```curl https://whalelava.zendesk.com/api/v2/requests.json -v -u guy@place.com/token:1yO4CeBapTn7T8Ag1ZKbusrAwq6in2CD6nIjLsD7```

-As an end user, mark a ticket as solved and then add a satisfaction rating for that ticket.

```curl https://whalelava.zendesk.com/api/v2/requests/118.json -d '{"request": {"comment": {"body": "Thanks!"}, "solved": "true"}}' -v -u guy@place.com/token:1yO4CeBapTn7T8Ag1ZKbusrAwq6in2CD6nIjLsD7 -X PUT -H "Content-Type: application/json"```

```curl https://whalelava.zendesk.com/api/v2/tickets/118/satisfaction_rating.json -X POST -d '{"satisfaction_rating": {"score": "good", "comment": "Awesome support."}}' -v -u guy@place.com/token:1yO4CeBapTn7T8Ag1ZKbusrAwq6in2CD6nIjLsD7 -H "Content-Type: application/json"```



###Help Center

-Create a new article that is public. (public status is defined by section)

```curl https://whalelava.zendesk.com/api/v2/help_center/sections/201627168/articles.json -d '{"article": {"promoted": false, "position": 2, "comments_disabled": true, "locale": "en-us", "title": "How to fix f5 memory buffering issue", "body": "upgrade to a version above 11.5.1 hf 7"}}' -v -u christian.richeson@gmail.com -X POST -H "Content-Type: application/json"```

-Create a new sections that only users with specific tags can use.

```curl https://whalelava.zendesk.com/api/v2/help_center/categories/200807198/sections.json -d '{"section": {"position": 2, "translations":  [{"locale": "en-us", "title": "Explosions", "body": "This section contains articles on exploding flight instruments"}, {"locale": "fr", "title": "French Explosions", "body": "Je ne sais pas le français, mais les explosions sont assez cool"}]}}' -v -u christian.richeson@gmail.com -X POST -H "Content-Type: application/json"```

```curl https://whalelava.zendesk.com/api/v2/help_center/sections/201950978/access_policy.json -d '{"viewable_by": "signed_in_users", "manageable_by": "staff", "required_tags": ["test"]}' -v -u christian.richeson@gmail.com -X PUT -H "Content-Type: application/json"```


-Make sure that both articles have the correct permissions.

```curl https://whalelava.zendesk.com/api/v2/help_center/sections/201950978/access_policy.json -v -u christian.richeson@gmail.com```
  
-As an end user, vote on an article.


-Add a spanish translation to an article.


-As an end user with spanish set as their locale, make sure that you get the spanish version of the article.


-Upload an image to an article and embed it in the body.
