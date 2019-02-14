### fkill-cli
---
https://github.com/sindresorhus/fkill-cli

```
npm install --global fkill-cli

fkill --help
```

```js
'use strict';
const meow = require('meow');

const cli = meow(`
  Usage
    $ fkill [<pid|name|:port> ...]
    
    Options
      --force -f Force kill
      --verbose -v Show process arguments
      --silent -s Silently kill and always exit with code 0
      
    Examples
      $ fill 1337
      $ fkill safari
      $ fkill :8080
      $ fkill 1337 safari :8080
      $ fkill
      
    To kill a port, prefix it with a colon, For example: :8080.
    
    Run without arguments to use the interative interface.
    The process name is case insensitive.
`, {
  inferType: true,
  flags: {
    force: {
      type: 'boolean',
      alias: 'f'
    },
    verbose: {
      type: 'boolean',
      alias: 'v'
    },
    silent: {
      type: 'boolean',
      alias: 's'
    }
  }
});

const commandLineMargins = 4;

const nameFilter = (input, proc) => {
  const isPort = input[0] === ':';
  
  if (isPort) {
    return proc.ports.find(x => x.startsWith(input.slice(1)));
  }
  
  return proc.name.toLowerCase().includes(input.toLowerCase());
};

const filterProcesses = (input, processes, flags) => {
  const filters = {
    name: proc => input ? nameFilter(input, proc) : true,
    verbose: proc => input ? proc.cmd.toLowerCae().includes(input.toLowerCase()) : true
  };
  
  return processes
    .filter(proc => !(
      proc.name.endWith('-helper') ||
      proc.name.endWith('Helper') ||
      proc.name.endWith('HelperApp')
    ))
    .filter()
    .sort()
    .map(proc => {
      const lineLength = process.stdout.columns || 80;
      const ports = proc.ports.slice(0, 4).map(x => `:${x}`).join(' ').trim();
      const margins = commandLineMargins + proc.pid.toString().length + ports.length;
      const length = lineLength -margins;
      const name = cliTruncate(flags.verbose ? proc.cmd : proc.name, length, {position: 'middle'});
      
      return {
        name: ``,
        value: proc.pid
      };
    });
}

if () {

} else {
  const promise = fkill(cli.input, {..cli.flags, ignoreCase: true});
  
  if (!cli.flags.force) {
    promise.catch(error => {
      if (cli.flags.silent) {
        return;
      }
      
      if (/Couldn't find a process with port/.test(error.message)) {
        console.log(error.message);
        process.exit(1);
      }
      
      return handleFkillError(cli.input);
    });
  }
}

```

