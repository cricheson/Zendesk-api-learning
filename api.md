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

