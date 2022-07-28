+++
ShowToc = true
date = 2022-07-27T18:30:00Z
description = "tRPC is a typescript library, so to say, that makes it easy to create type-safe APIs without schema or any sort of code generation."
tags = ["nextjs", "prisma", "tRPC"]
title = "The type-safe guide to tRPC"
[cover]
alt = "trpcBlogCover"
image = "/uploads/trpcblog.png"

+++
Check out the original article \[here\]().

This isn't the best guide to use tRPC, probably there are better ways to do this, like \[create-t3-app\]([https://create.t3.gg/](https://create.t3.gg/ "https://create.t3.gg/")), the best I could find.

Most of what is here is from the \[tRPC's documentation\]([https://trpc.io/docs](https://trpc.io/docs "https://trpc.io/docs")), you can refer them, super helpful and easy to read.

\**What is tRPC?**

tRPC is a typescript library, so to say, that makes it easy to create type-safe APIs without schema or any sort of code generation.

\**Where to use?**

Create the *typed server* and then import its type and use it with an adaptor in the client side.

\**How does it implement type-safety?**

tRPC encourages using the \[zod\]([https://www.npmjs.com/package/zod](https://www.npmjs.com/package/zod "https://www.npmjs.com/package/zod")), a library for type validation of input and output arguments.

\**Is tRPC only limited to React?**

tRPC's core API is built to work with any client, but right now it supports **React** and can be used with **React Meta Frameworks** like **NextJS** or **SolidJS**, since it uses **React Query** under the hood to talk to the server and maintaining type-safety across the data-pipeline or data-flow.

For now, it has first-party adaptors for **React**, **NextJS**, **Express**, **Fastify**, **SolidJS**, and some community packages like for \[**tRPC for SveleteKit**\]([https://github.com/icflorescu/trpc-sveltekit](https://github.com/icflorescu/trpc-sveltekit "https://github.com/icflorescu/trpc-sveltekit"))

\**What are its features?**

\- Lightweight, a tiny bundle size for such a powerful library.

\- Type-safe to the max!

\- Support subscriptions with **websockets** library.

\- Request batching

	- Request can be made simultaneously and then are batched into one.

\- Strong User base and helpful Community

\## tRPC x NextJS

Recommended file structure:

\`\`\`tree

.

├── prisma # <-- if prisma is added

│   └── \[..\]

├── src

│   ├── pages

│   │   ├── _app.tsx # <-- add \`withTRPC()\`-HOC here

│   │   ├── api

│   │   │   └── trpc

│   │   │       └── \[trpc\].ts # <-- tRPC HTTP handler

│   │   └── \[..\]

│   ├── server # <-- can be named backend or anything else

│   │   ├── routers

│   │   │   ├── app.ts   # <-- main app router

│   │   │   ├── post.ts  # <-- sub routers

│   │   │   └── \[..\]

│   │   ├── context.ts      # <-- create app context

│   │   └── createRouter.ts # <-- router helper

│   └── utils

│       └── trpc.ts  # <-- your typesafe tRPC hooks

└── \[..\]

\`\`\`

\### Components

\**Router**

This is the router where the actual business logic will reside, create a \`backend\` folder inside the \`src\` directory and put all this stuff there.

If using prisma otherwise this is optional,

\`src/server/utils/prisma.ts\`

\`\`\`ts

import { PrismaClient } from "@prisma/client";

declare global {

	var prisma: PrismaClient | undefined;

};

export const prisma = global.prisma || new PrismaClient({

	log: \["query"\]

});

if (process.env.NODE_ENV != 'production') global.prisma = prisma;

\`\`\`

\`src/server/router/context.ts\`

\`\`\`ts

import * as trpc from "@trpc/server";

import * as trpcNext from "@trpc/server/adapters/next";

import { prisma } from "@/server/utils/prisma"; // this is optional

export const createContext = async (

	options?: trpcNext.CreateNextContextOptions

) => {

	const req = options?.req;

	const res = options?.res;

	

	return {

		req,

		res,

		prisma, // this is optional

	};

};

type Context = trpc.inferAsyncReturnType<typeof createContext>;

export const createRouter = () => trpc.router<Context>();

\`\`\`

> **Using Contexts**

> 

> We can create router without using the above context by just using \`trpc.router()\` that will work just fine. But if you are using some external API like in the above case we are using prisma, it's better to use pass in repeatedly used instances to context to avoid having multiple ones for every query we use that in, that may *affect our performance and can also be vulnerable*. 

\`src/server/router/index.ts\`

\`\`\`ts

import {createRouter} from "./contex";

import {exampleRouter} from "./example.router";

export const appRouter = createRouter()

	.merge("example.", exampleRouter)

	.query("posts.count", {

		async resolve({ctx}) {

			return await ctx.prisma.patient.count();

		}

	});

export type AppRouter = typeof appRouter;

\`\`\`

\**API handler** aka NextJS adaptor:

> The exact filename is necessary to make this work!

\`src/pages/api/trpc/\[trpc\].ts\`

\`\`\`ts

import * as trpcNext from "@trpc/server/adapters/next";

import { appRouter, AppRouter } from "@/backend/router";

import { inferProcedureOutput } from "@trpc/server";

import { createContext } from "@/backend/router/context";

// export API handler

export default trpcNext.createNextApiHandler({

  router: appRouter,

  createContext: createContext,

});

\`\`\`

\**Hooks**

These are the React hooks necessary to maintain the type-safety, this will give you React Query like hooks to fetch the API.

\`src/utils/trpc.ts\`

\`\`\`ts

import { createReactQueryHooks } from "@trpc/react";

import type { AppRouter } from "@/backend/router";

import { inferProcedureOutput } from "@trpc/server";

export const trpc = createReactQueryHooks<AppRouter>();

export type TQuery = keyof AppRouter\["_def"\]\["queries"\];

// helper type to infer query output

export type InferQueryOutput<TRouteKey extends TQuery> = inferProcedureOutput<

  AppRouter\["_def"\]\["queries"\]\[TRouteKey\]

>;

\`\`\`

\**Example query in React Component**

Now that tRPC is set up, this is how we use it inside react components.

\`src/pages/index.tsx\`

\`\`\`tsx

// we use the instance we created that has our router type definitions

import { trpc } from "@/utils/trpc";

export default SomePage() {

	const { isLoading, data:postsCount } = trpc.useQuery(\["posts.count"\]);

	return <div>...</div>

}

\`\`\`

\### SSG Helpers

SSG Helpers are helper functions that can be used to pre-fetch queries on the server upon request to reduce loading time.

They are to be used when working with SSR and SSG or ISR.

How to use it with \`getServideSideProps\` function of NextJS pages.

\`\`\`ts

// /pages/posts/\[id\].tsx

export function getServerSideProps(

	context: GetServerSidePropsContext<{id: string}>

) {

	const { id } = context.params;

	const ssg = createSSGHelpers({

		router: appRouter,

		ctx: await createContext(), // { } if no context in your router

		transformer: superjson

	});

	ssg.fetchQuery("posts.get", {id});

	return {

		props: {

			trpcState: ssg.dehydrate(),

			id

		}

	}

}

export default function PostPage(props: InferGetServerSidePropsType<typeof getServerSideProps>) {

	const {id} = props;

	// this query will be fetched instantly because of the cached

	// response of the query we fetching on server

	const {isLoading, data} = trpc.useQuery(\["posts.get"\], {id})

	return ...

}

\`\`\`

\**References**

\- Check out \[this\]([https://www.youtube.com/watch?v=I5tWWYBdlJo](https://www.youtube.com/watch?v=I5tWWYBdlJo "https://www.youtube.com/watch?v=I5tWWYBdlJo")) amazing talk by \[Theo\]([https://twitter.com/t3dotgg](https://twitter.com/t3dotgg "https://twitter.com/t3dotgg")) on tRPC vs GraphQL and their risks.

\- Check out Theo on YouTube or any other social media platform, he has a lot of content about tRPC

\- Follow \[Alex\]([https://twitter.com/alexdotjs](https://twitter.com/alexdotjs "https://twitter.com/alexdotjs")) aka Katt, the creator of tRPC.