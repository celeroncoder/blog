+++
ShowToc = true
date = 2022-09-06T18:30:00Z
description = "Brief intro to winston transports and how to create custom ones."
tags = ["webdev", "winston", "logging", "custom"]
title = "Winston Transports??"
[cover]
alt = "winston transport blog post cover"
image = "/blog/uploads/winston-transport-cover.png"
+++

### Brief Introduction to what is `winston`?

> Winston is a node package that provides a simple and universal logging library. It supports **multiple transports**, such as file, console, and syslog. Winston is easy to use and can be **customized** to meet your specific needs.

Did you see the words `multiple transports` and `customized` in the above definition of `winston` (that I asked Google Bard to write)? Yeah, `winston` is probable the most sophisticated and customizable logger out there.

### What are transports?

So we know that `winston` supports multiple transports, but what is a transport anyway?

> In `winston`, transport is essentially a storage device for your logs. Each instance of a `winston` logger can have multiple transports configured at different levels.

So transport is basically like where you want to store your logs.

There are some most basic ones like:

- **The Console**
  ```tsx
  import winston from "winston";

  export const logger = winston.createLogger({
    transports: [new winston.transports.Console()],
  });
  ```
- **Files**
  ```tsx
  ...
  new winston.transports.File({ filename: "logs/all.log" })
  ...
  ```

Console and file are like the most basic ones, Console basically prints your logs to the console and file one just stores your log in the defined file. You can go crazy with the options and do a lot more stuff, like storing logs of only a specific `level`, let’s say `error` or something.

The default ones are great, and they make for the most basic use cases, but if you’re going into production and logs are just more than something you’ll use to debug and get HTTP status codes.

There are a lot of platforms that allows you to store your logs and do crazy things with them (basically alerts and analytics). Some of them are [Logtail](https://betterstack.com/logtail), and an open source one is [Highstorm](https://highstorm.app).

To upload your logs to them, you’ll need custom transport. `winston` give you the power to create custom transports pretty easily with the `winston-transport` package, and here’s how you do it…

### Custom Transports

So basically custom transports are to upload your logs to some sort of storage or maybe do something else with it, like creating alerts and stuff.

Here’s how you create basic custom transport for `winston`:

```tsx
// my-transport.ts

import Transport, { TransportStreamOptions } 'winston-transport';
import type { LogEntry } from "winston";

export class MyTransport extends Transport {
  constructor(
    opts: TransportStreamOptions
  ) {
    super(opts);

		/*
			* Consume any custom options here. e.h:
				* Connection information for databases
				* Authentication information for APIs
		*/
  }

	// this functions run when something is logged so here's where you can add you custom logic to do stuff when something is logged.
  log(info: LogEntry, callback: any) {
		// make sure you installed `@types/node` or this will give a typerror
    // this is the basic default behavior don't forget to add this.
		setImmediate(() => {
      this.emit("logged", info);
    });

    const { level, message, ...meta } = info;

    // here you can add your custom logic, e.g. ingest data into database etc.

		// don't forget this one
		callback();
  }
}
```

Now you can add your custom transport very easily as:

```tsx
// logger.ts

import {MyTransport} from "./my-transport.ts"

const options = {
	// custom transport options
}

const myTransport = new MyTransport(options)

export const logger = winston.createLogger({
	...
	transports: [
		...
		mytransport,
		...
	]
});
```

Pretty easy to write, yup, thanks to `winston` being the best logger out there with this level of customization. With the above example, you can create any custom transport you want with whatever fancy logic you want.

Check out these to dive deeper:

[npm: winston-transport](https://www.npmjs.com/package/winston-transport)

[npm: winston](https://www.npmjs.com/package/winston)

Here are some packages that provide transports to do various things (mostly upload to their service):

[npm: @logtail/winston](https://www.npmjs.com/package/@logtail/winston)

[npm: winston-transport-sentry-node](https://www.npmjs.com/package/winston-transport-sentry-node)

[npm: winston-kafka-transport](https://www.npmjs.com/package/winston-kafka-transport)

[npm: @shelf/winston-datadog-logs-transport](https://www.npmjs.com/package/@shelf/winston-datadog-logs-transport)
