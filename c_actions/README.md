## a_simple.yaml

With the input of the joke flow it is time to execute the first action. This worklfow just adds another transition to an action. To use an action state a function needs to be defined at the beginning of a flow definition. In this case we want to use Direktiv's `http` function. Functions can have multiple scopes workflow level, namespace or system wide. This defines who can use those workflows. 

After defining the function it can be called in an action state. It is referenced by `function`. The input field is the most important part of the function call. The yaml data under input becomes the JSON payload for the function. In this case the URL of the joke API. 

As already mentioned jq/javascript can be used in many locations. In this case we are taking the value of `joke` and use it in the URL. In this case we are getting topics about different topics depending on the input of the flow. 


```yaml
- id: get-joke     
  type: action
  action:
    function: http 
    input: 
      url: https://v2.jokeapi.dev/joke/jq(.joke)?type=single
```


*Calls*:

```
echo '{ "joke": "Programming" }'  | direktiv-sync exec c_actions/a_simple.yaml
```



## b_modify

Running the flow returns a huge payload with headers etc. from that request. To get only parts we are interested in jq can be used again in a transform. This is a good example to use the jq playground to figure out what the transformation should look like. In this case:

```yaml
transform:
    joke: jq(.return.body.joke)
```


*Calls*:

```
echo '{ "joke": "Programming" }'  | direktiv-sync exec c_actions/b_modify.yaml
```



## c_conditional

Further inspection of the return payload from the joke API show that jokes are getting rated safe / unsafe with the `safe` attribute in the response. In this workflow the switch checks the value and decides based on the value if it censors the joke and stops or transitions with a defaultTransition to the `done` state which adds the time via a javascript transform.


*Calls*:

```
echo '{ "joke": "Programming" }'  | direktiv-sync exec c_actions/c_conditional.yaml
```

