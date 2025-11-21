---
title: "NestJS REPL: Bringing Back the Rails Console Experience"
seoTitle: "Enhance Your NestJS Development: Create a Rails-Like REPL with Prisma"
seoDescription: "Discover how to bring the Rails console experience to NestJS with a custom REPL. Learn to integrate Prisma for efficient data exploration and query testing,"
datePublished: Fri Nov 21 2025 18:46:33 GMT+0000 (Coordinated Universal Time)
cuid: cmi97olwu000802ldantuc03k
slug: nestjs-repl-bringing-back-the-rails-console-experience
ogImage: https://cdn.hashnode.com/res/hashnode/image/upload/v1763750573259/c656b2cd-91af-4b66-9693-47934490b7f7.png
tags: rails, nestjs

---

If you've ever worked with Rails, you're likely familiar with the console, a tool that becomes second nature for exploring models, debugging, or modifying data quickly. Transitioning to NestJS, you might miss this handy feature, as it's not available out of the box. However, you can create your own version.

A straightforward approach is to use Prisma, as its client is self-contained and easily integrated into a REPL. A simple script can get you started:

```ts
import { PrismaClient } from '@prisma/client';

async function main() {
  const prisma = new PrismaClient();
  const repl = require('repl').start('> ');
  repl.context.prisma = prisma;
}

main();
```

Place this script in a file like `src/repl.ts` and execute it:

```plaintext
npx ts-node src/repl.ts
```

Now, you can perform similar tasks as you would in a Rails console:

```bash
> await prisma.user.findMany()
```

While it doesn't load the full Rails environment, it serves its purpose.

### And yes, Prisma Studio exists

Before diving into building a custom REPL, consider that Prisma comes with **Prisma Studio**:

```bash
npx prisma studio
```

Prisma Studio provides a clean interface for viewing and editing tables. It's excellent for browsing data but doesn't quite capture the "let me try this query right now" feel of the Rails console. This is where the REPL comes in handy.

### When the REPL is the Right Choice

* Testing a query before integrating it into a service
    

In summary, a small REPL can be sufficient for quickly testing queries without the need to launch the entire application.