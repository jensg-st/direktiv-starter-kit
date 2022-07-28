## a_flow and a_subflow

This examples moves the action call in a subflow to split up functionality. For a subflow a function definition has to exist as well. Direktiv does not make a difference between subflows or actual functions. 

The subflow has been changed slightly and the subflow thows an error now if the joke is inappropriate. The parent flow is handling it with a retry first:

```yaml
retries:
    max_attempts: 1
    delay: PT5S
    codes: ["inappropriate"]
```

If the joke is inappropriate it throws an error, the parent retries one more time and returns with no joke if it fails again. 


```yaml
- id: get-joke     
  type: action
  action:
...
  catch:
  - error: "*"
    transition: no-joke
```

