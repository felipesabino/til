# Append commands as notification

When executing long running commands you can avoid just staring at the terminal for command to be completed by adding a notification for when the command is finished.

This can be easily accomplished by appending another command to be executed in sequence using the native [`say`](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/say.1.html).

To append commands you can use `&&`, `||` or even `;`, depending on the desired behavior relative to the exit status of the first command. Confusing, right? An example will explain it better:

```
$ false || echo "Oops, fail"
Oops fail

$ true || echo "Will not be printed"
$  

$ true && echo "Things went well"
Things went well

$ false ; echo "This will always run"
This will always run
```

(source: <https://stackoverflow.com/a/4510738/429521>)

## Example

Executing test suit on your node application and waiting for the computer to say _"done"_ when tests are... done

```
$ npm run test ; say "done"
```

## Visual notification

If you want/need a visual notification instead, you can mix AppleScript's `display notification` with the CLI command to execute it [`osascript`](https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/osascript.1.html)

To just show a notification, you could do something like
```
$ osascript -e 'display notification "Lorem ipsum dolor sit amet" with title "Title"'
```

Then mixing both approaches, you could do

```
npm run test ; osascript -e 'display notification "are done!" with title "Your tests..."'
```

![notification example](./assets/append-command-notification.example.png)

## Node.js extra

An example apporach to make the workflow simpler for a node.js projects could be creating a task _notify_ on the `package.json` `scripts` sectoin with either sound or notification implementation and then just execute

```
& npm run test ; npm run notify
```

`package.json` example:

```json
{
  ...
  "scripts" : {
    ...
    "notify": "osascript -e 'display notification \"are done!\" with title \"Your tests...\"'",
    ...
  }
}
```

## References

- Apple manual on `say` - <https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/say.1.html>
- Apple manual on `osascript` - <https://developer.apple.com/legacy/library/documentation/Darwin/Reference/ManPages/man1/osascript.1.html>
- SO - ["How can I trigger a Notification Center notification from an AppleScript or shell script?"](https://apple.stackexchange.com/a/115373/6091)
- SO - ["What is the purpose of “&&” in a shell command?"](https://stackoverflow.com/a/4510738/429521)