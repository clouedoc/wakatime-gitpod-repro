# Wakatime + GitPod bug reproduction

## Error message

![Capture d’écran 2022-06-17 à 21 52 46](https://user-images.githubusercontent.com/13921610/174392732-8a8ae223-0a9e-44b7-98a5-0ae957cb91ed.png)

## Reproduction

CLI version: `v1.49.0`
Extension version: `v18.1.5`

1. Create a gitpod.io account
2. Add the WAKATIME_API_KEY variable here: https://gitpod.io/variables (with value from https://wakatime.com/api-key)
3. Open this repository using GitPod
4. Edit a file to trigger WakaTime reporting
5. You will see the error message
6. Execute `> WakaTime: Api Key` and set your API key manually
7. Edit a file. Everything will work.

## Debugging information

### `.wakatime.log`

My `.wakatime.log` says that my API key is invalid. But it is!

```txt
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: today fetch failed: invalid api key... find yours at wakatime.com/api-key. failed to load api key","now":"2022-06-17T19:51:28Z","version":"v1.49.0"}
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: sending heartbeat(s) failed: invalid api key... find yours at wakatime.com/api-key. failed to load command parameters: failed to load api params: failed to load api key","now":"2022-06-17T19:51:44Z","version":"v1.49.0"}
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: sending heartbeat(s) failed: invalid api key... find yours at wakatime.com/api-key. failed to load command parameters: failed to load api params: failed to load api key","now":"2022-06-17T19:51:46Z","version":"v1.49.0"}
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: today fetch failed: invalid api key... find yours at wakatime.com/api-key. failed to load api key","now":"2022-06-17T19:53:27Z","version":"v1.49.0"}
{"caller":"cmd/run.go:111","func":"cmd.Run","level":"debug","message":"command: heartbeat","now":"2022-06-17T19:53:55Z","version":"v1.49.0"}
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: sending heartbeat(s) failed: invalid api key... find yours at wakatime.com/api-key. failed to load command parameters: failed to load api params: failed to load api key","now":"2022-06-17T19:53:55Z","version":"v1.49.0"}
{"caller":"cmd/run.go:111","func":"cmd.Run","level":"debug","message":"command: heartbeat","now":"2022-06-17T19:54:07Z","version":"v1.49.0"}
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: sending heartbeat(s) failed: invalid api key... find yours at wakatime.com/api-key. failed to load command parameters: failed to load api params: failed to load api key","now":"2022-06-17T19:54:07Z","version":"v1.49.0"}
{"caller":"cmd/run.go:111","func":"cmd.Run","level":"debug","message":"command: heartbeat","now":"2022-06-17T19:54:10Z","version":"v1.49.0"}
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: sending heartbeat(s) failed: invalid api key... find yours at wakatime.com/api-key. failed to load command parameters: failed to load api params: failed to load api key","now":"2022-06-17T19:54:10Z","version":"v1.49.0"}
{"caller":"cmd/run.go:111","func":"cmd.Run","level":"debug","message":"command: heartbeat","now":"2022-06-17T19:54:11Z","version":"v1.49.0"}
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: sending heartbeat(s) failed: invalid api key... find yours at wakatime.com/api-key. failed to load command parameters: failed to load api params: failed to load api key","now":"2022-06-17T19:54:11Z","version":"v1.49.0"}
{"caller":"cmd/run.go:99","func":"cmd.Run","level":"debug","message":"command: today","now":"2022-06-17T19:54:27Z","version":"v1.49.0"}
{"caller":"cmd/run.go:276","func":"cmd.runCmd","level":"error","message":"failed to run command: today fetch failed: invalid api key... find yours at wakatime.com/api-key. failed to load api key","now":"2022-06-17T19:54:27Z","version":"v1.49.0"}
```

### Terminal

```sh-session
gitpod /workspace/wakatime-gitpod-repro (main) $ echo $WAKATIME_API_KEY
xxxxxxxx-xxxx-xxxx-xxxx-xxxxxxxxxxxx
```

### VSCode Developer Tools

#### Undefined env variable

```sh-session
$ process.env.WAKATIME_API_KEY
> undefined
```

### Error logs

The following error lines were present:

![image](https://user-images.githubusercontent.com/13921610/174394142-582aa8fc-965e-47d1-b7ec-2ef840396280.png)
![image](https://user-images.githubusercontent.com/13921610/174394168-2040e3c3-cb8c-4494-81c7-724bef03fe33.png)
![image](https://user-images.githubusercontent.com/13921610/174394533-5d8020c6-735a-4794-95cd-fab3efc95c01.png)

After adding the API key manually, everything works.

![image](https://user-images.githubusercontent.com/13921610/174394820-5545741e-383f-439d-9a4e-c232408a4355.png)


### Executing the command manually

Executing the command manually results in a 104 exit code, even in a terminal where the environment variable is set.

![image](https://user-images.githubusercontent.com/13921610/174394253-437f291c-b80c-44a4-9fbe-cb3d94f41375.png)


### Misc

```sh-session
gitpod /workspace/wakatime-gitpod-repro (main) $ cat ~/.wakatime-internal.cfg

[internal]
cli_version = v1.49.0
cli_version_last_modified = Mon, 13 Jun 2022 15:48:47 GMT

gitpod /workspace/wakatime-gitpod-repro (main) $ cat ~/.wakatime.cfg

[settings]
debug = true
```

## References

-
