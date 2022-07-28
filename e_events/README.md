# a_flow

This example uses events in the main flow. The main flow generates an event with the joke topic and after generating it it waits for an event to come back. Theer are three flows waiting. One for each topic. They get the joke and return it to the main flow. Error handling was removed to make the flow simpler to follow.

The API call would be:

http://SERVER/api/namespaces/getting-started/tree/e_events/a_flow?op=wait

Or targeting one field in the response:

http://SERVER/api/namespaces/getting-started/tree/e_events/a_flow?op=wait&input.joke=Dark&field=joke&raw-output=true&ctype=text/html


*Calls*:

```
echo '{ "joke": "Programming" }'  | direktiv-sync exec e_events/a_flow.yaml
```
