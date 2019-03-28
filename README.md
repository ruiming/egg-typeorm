# egg-typeorm

[TypeORM](https://typeorm.io/#/) plugin for Egg.js.

[![NPM version][npm-image]][npm-url]
[![npm download][download-image]][download-url]

[npm-image]: https://img.shields.io/npm/v/@forsigner/egg-typeorm.svg?style=flat-square
[npm-url]: https://npmjs.org/package/@forsigner/egg-typeorm
[download-image]: https://img.shields.io/npm/dm/@forsigner/egg-typeorm.svg?style=flat-square
[download-url]: https://npmjs.org/package/@forsigner/egg-typeorm

<!--
Description here.
-->

## Install

```bash
$ yarn add @forsigner/egg-typeorm mysql
```

## Usage

### Plugin

```ts
// {app_root}/config/plugin.ts
const plugin: EggPlugin = {
  typeorm: {
    enable: true,
    package: '@forsigner/egg-typeorm',
  },
}
```

### Configuration

```ts
// {app_root}/config/config.default.ts
config.typeorm = {
  client: {
    type: 'mysql',
    host: 'localhost',
    port: 3306,
    username: 'test',
    password: 'test',
    database: 'test',
    synchronize: true,
    logging: false,
    entities: ['app/entity/**/*.ts'],
    migrations: ['app/migration/**/*.ts'],
    subscribers: ['app/subscriber/**/*.ts'],
  }
}
```

### Create entity files

```bash
├── controller
│   └── home.ts
├── entity
    ├── Post.ts
    └── User.ts
```

### Entity file

```ts
// app/entity/User.ts

import { Entity, PrimaryGeneratedColumn, Column } from 'typeorm'

@Entity()
class User {
  @PrimaryGeneratedColumn()
  id: number

  @Column()
  name: string
}

export default User
```

### Use with FindOptions

```ts
// in controller
export default class UserController extends Controller {
  public async index() {
    const { ctx } = this
    ctx.body = await ctx.repo.User.find()
  }
}
```

### Use with QueryBuilder

```ts
// in controller
export default class UserController extends Controller {
  public async index() {
    const { ctx } = this
    const firstUser = await ctx.repo.User.createQueryBuilder('user')
      .where('user.id = :id', { id: 1 })
      .getOne()
    ctx.body = firstUser
  }
}
```

## Example

[example](https://github.com/forsigner/egg-typeorm/tree/master/example)

## Questions & Suggestions

Please open an issue [here](https://github.com/forsigner/egg-typeorm/issues).

## License

[MIT](LICENSE)
