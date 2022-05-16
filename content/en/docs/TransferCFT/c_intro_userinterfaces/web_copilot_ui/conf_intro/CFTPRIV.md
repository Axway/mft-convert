---

    title: CFTPRIV
    linkTitle: Privileges - CFTPRIV
    weight: 260

---
Use this procedure to view a list of privileges and descriptions and perform related tasks in the user interface. See also <a href="../../../../internal_a_m_start_here/fm_access_management" class="MCXref xref">Access Management using Flow Manager</a>

### Using CFTPRIV

Privileges give users authorization to access and perform actions in the user interface. Examples of actions include CREATE, DELETE, VIEW, EDIT (use \* to assign all actions).


| Field | Type | Comment |
| --- | --- | --- |
| id | String32 | Privilege identifier |
| comment | String80 | Comment |
| resource | String32 | Resource on which this privilege applies |
| actions | List of String32 | Actions authorized on the resource (1 to 16 actions) |
| condition | String256 | Condition to check for authorizing (<a href="#Specifyi">see below</a>) |


Example of CFTPRIV in a configuration file:

```
CFTPRIV      ID          = 'MYPRIV1',
            COMMENT     = 'My comment',
             RESOURCE    = 'TRANSFER',
             ACTIONS     = ( 'CREATE' , 'DELETE', 'VIEW', 'EDIT', 'CANCEL', 'RESUME',
                            'PAUSE', 'EXECUTE', 'SUBMIT', 'END' ),
             CONDITION   = '',
             ORIGIN      = 'CFTUTIL',
             MODE        = 'REPLACE'
```
<span id="Specifyi"></span>

### Specifying conditions

Conditions allow you to assign finer control on resources and actions by specifying a logical condition that must be true to authorize the action.

****Examples****

In these examples <span class="code">`PART `</span>and <span class="code">`ID `</span>are properties of the resource being checked. As you can see, you can use parenthesis and logical operators <span class="code">`&&`</span> (AND) and <span class="code">`||`</span> (OR).

```
PART=="PARIS" && ID=="IDFDEF"
(PART=="PARIS" || PART==”NEWYORK”) && ID~="IDF\*"
```

Comparison operators include:

- == : equals
- != : not equal
- ~= : matches (use \* and ? for jokers)
- /= : not matching (use \* and ? for jokers)
- &lt; : inferior to
- &gt; : superior to
- &lt;= : inferior or equal to
- &gt;= :superior or equal to