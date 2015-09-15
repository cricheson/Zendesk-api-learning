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

-Create a new brand for your helpdesk. Set it as your default brand. Add a brand image for it.
