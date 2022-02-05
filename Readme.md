# Troubleshooting
1. Place your configs into folder `fluentd-config`
1. Add prints to debug the current event at specific points with: 
    ```
    <filter in.**>
        @type stdout
        @id debug1
    </filter>
    ```
1. Add prints of customized messages ([see here](https://github.com/toyama0919/fluent-plugin-formatter_sprintf)) at specific points by :
    ```
    <filter in.**>
        @type stdout
        @id debug1-with-msg
        <format>
            @type sprintf
            sprintf_format "Debugging the tag:${tag}\n"
        </format>
    </filter>
    ```
1. Optional: Enable running with verbose
1. Start by running `sudo docker-compose up --build`

# Sending Logs to FluentD
## FluentCat
Install FluentCat by [installing fluentD](https://docs.fluentd.org/installation)

or

use FluentCat by execing into the running fluentd container with
`sudo docker-compose exec fluentd bash`

Send log:
```
echo '{"event":"message"}' | fluent-cat fromfluentcat
```

Send unparsed FluentBit log:
```
echo '{"log":"{\"event\":\"message\"}"}' | fluent-cat fromfluentcat
```

## FluentBit
Append to file (watched by fluentbit):
```
echo '{"event":"message"}' >> ./fluentbit/input-fluentbit.json
```

## Examples
### No Problem 
```
echo '{"@timestamp":"2019-02-04T23:09:52.884742296+0100","event":"message"}' >> ./fluentbit/input-fluentbit.json
```

`sudo docker-compose exec fluentd bash`
```
echo '{"log":"{\"@timestamp\":\"2019-02-04T23:09:52.884742296+0100\",\"event\":\"message\", \"foo\":\"bar\"}"}' | fluent-cat fromfluentcat
```

### Invalid date or strptime format
```
echo '{"@timestamp":"2019-02-04T23:09:52.8847422960100","event":"message"}' >> ./fluentbit/input-fluentbit.json
```

`sudo docker-compose exec fluentd bash`
```
echo '{"log":"{\"@timestamp\":\"2019-02-04T23:09:52.8847422960100\",\"event\":\"message\", \"foo\":\"bar\"}"}' | fluent-cat fromfluentcat
```

# Other Resources
* [Testing FluentD Regular Expressions](https://fluentular.herokuapp.com/) 

* FluentD Config validation without start use `fluentd --dry-run`
* [fluent-plugin-debug](https://github.com/sonots/fluent-plugin-debug) (Unmaintained) Fluentd plugin to investigate incoming messages in a short-hand.

# Debug FluentD with VisualStudio Code
* Follow instructions [here](https://github.com/fluent/fluentd#development)
* Open in VisualStudioCode
* Install [ruby Extension](https://github.com/rubyide/vscode-ruby)
* Add debug config by creating file `.vscode/launch.json`
    ```
    {
        "configurations": [
            {
                "name": "Debug Local File",
                "type": "Ruby",
                "request": "launch",
                "cwd": "${workspaceRoot}",
                "program": "${workspaceRoot}/lib/fluent/command/fluentd.rb",
                "args": [
                    "-c", "myFluent.conf"
                    //, "-vv"
                ],
                "env": { 
                    "RUBYLIB":"lib"
                }
            }
        ]
    }
    ```
* Know limitation: Stopping VSCode Debugging does not stop fluentd process. Run `kill $(ps aux | grep -v grep | grep fluentd.rb | awk '{print $2}')` to stop it