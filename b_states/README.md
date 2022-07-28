The following workflows will provide an example and it will be expanded with each step. Not all steps would be necessary in real life and is for learning purposes only. The idea is to provide a simple example but learn basic concepts of Direktiv. 

## a_base.yaml

The base workflow only logs the incoming data to the console. Every flow instance starts with the incoming data. If the incoming data is JSON this will the state data the workflow can work with. If the content is not JSON it will be converted to base64, e.g. if the input is "Hello World" the starting state data would be:

```json
{
  "input": "SGVsbG8gV29ybGQK"
}
```

*Calls*:

```
echo '{ "hello": "world" }' | direktiv-sync exec b_states/a_base.yaml
```

## b_validation

We wil use the [Joke API](https://sv443.net/jokeapi/v2/) in this example and want the service to get the joke topic when it starts. For this we have added a validation state at the beginnig of the flow. The validation state is a JSON-Schema state to protect the flow from rogue data. In this exmaple we are setting `additionalProperties` to false and `joke` as required. With that step we can be sure that there is no "wrong" data coming in. The following would work:

```sh
echo '{ "joke": "Programming" }' | direktiv-sync exec b_states/b_validation.yaml

# not working 
echo '{ "hello": "Programming" }' | direktiv-sync exec b_states/b_validation.yaml
```

This workflow has a transition as well. If the validation step was successful it transitions to a `start` state. The start state can securly use `jq(.joke)` now because the validation step makes sure that this value is set. 

## c_transform

Direktiv uses jq or javascript to work with state data variables. It can be used to transform data, make switching decisions or loggin. In this example we use the `transform` attribute for states. Every state can change the data via this field. Data can be added, deleted or changed. For jq transformations Direktiv's playground in the UI can be helpful to test the jq commands.

**Examples:**

Adding data: 
```yaml
transform: jq(. + { "data": "hello" })
```

Deleting data: 
```yaml
```yaml
transform: jq(. + { "data": "hello" })
```

Javascript works best for more complex scenarios. The state data comes as `data` variable into the function and can be modified. Whatever the function returns is the new state data.

**Examples:**

```yaml
transform: |
    js(
      data["hello"] = "world"
      return data
    )
```


*Calls*:

```
echo '{ "joke": "Programming" }'  | direktiv-sync exec b_states/c_transform.yaml
```



