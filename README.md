# Redis Contentful [![npm](https://img.shields.io/npm/v/redis-contentful.svg)](https://www.npmjs.com/package/redis-contentful) [![license](https://img.shields.io/github/license/shreyas-a/redis-contentful.svg)](https://github.com/shreyas-a/redis-contentful/blob/master/LICENSE)

A tiny library to map Contentful ☁️ space into Redis ⚡️

## Why should I care?

Say your marketing team loves to update content without you being involved? Great! That is why you should use Contentful CMS. But wait, there is a catch. Contentful API sometimes takes `~800ms`. And this is really a hit when you're having fancy SSR implemented to boost the performance of your Node JS app. As the creator of Gmail, Paul Buchheit had said "Every interaction should be faster than `100ms`. Why? Because `100ms` is the threshold where interactions feel instantaneous".

This is where redis will help you crunch the rendering speed. It's really, really fast! A few `ms` is all it needs to get your data. By few, I mean less than `100ms` 🚀

`redis-contentful` maps your Contentful space's content types and their published records into your Redis server. It also maintains the schema of your Contentful space. All content types are stored in Redis as hashes. Under a particular hash, the record is stored as a key value pair with id being the record's key.

You can `sync` the data manually by calling the sync method on `redis-contentful` instance manually or you can add a webhook in Contentful by exposing an endpoint on your server which will internally call `sync`. In this way, you always have the latest content from Contentful right in your Redis.
So let's get started!

## Installation

```sh
npm install redis-contentful --save
```

## Usage

Create an instance of `redis-contentful` by passing contentful space ID & access token.

```js
import RedisContentful from 'redis-contentful';

const client = new RedisContentful({
  redis: {
    database: 0, // Optional param, default - 0
    host: '', // Optional param, default - 127.0.0.1
    port: '', // Optional param, default - 6379
  },
  contentful: {
    space: '<Space ID>',
    accessToken: '<Access Token>',
    locale: 'en-GB', // Optional param, default - en-US
  },
});
```

## API

### sync

Syncs the latest space content from Contentful and dumps it in your Redis server 🎉

```js
await client.sync();
```

Send an optional boolean param to `sync` if you want to reset your redis cache ♻️

```js
await client.sync(true);
```

### get

Gets all data directly from Redis 🚀

```js
const response = await client.get();
console.log(response);
```

Pass specific content type. It's an optional parameter.

```js
await client.get('about');
```

Pass a field name inside the content type to narrow down your filter.
This is again an optional param.

```js
await client.get('about', 'title');
```

You'll get an object with your content type ID's as keys and their values as array of content objects.
If you specify a specific key, only that key will be returned in the final object.

```js
{
    "<a-key>": [{}, {}, {}],
    "<yet-another-key>": [{}, {}]
}
```

## Redis Store

> In Redis, the keys (entries) will be prefixed with their respective content type 🤓

## License

MIT ❤
