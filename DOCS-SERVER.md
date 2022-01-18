* `npx express-generator server --ejs`
* `npm install --save typescript graphql apollo-server-core apollo-server-express`
* `npm install --save-dev @types/express@4.17.1`
* `rm -rf bin`
* Create tsconfig.json. Add:
```
{
  "compilerOptions": {
    "module": "commonjs",
    "esModuleInterop": true,
    "target": "es6",
    "moduleResolution": "node",
    "sourceMap": true,
    "outDir": "dist"
  },
  "lib": ["es2015"]
}
```
* Create new folder, called src.
* In src, create index.ts. Add:
```
import express from 'express';
const app = express();
const port = 3000;

app.get('/', (req, res) => {
  res.send('Hello World!');
});

app.listen(port, () => {
  return console.log(`Express is listening at http://localhost:${port}`);
});
```
* `npx tsc`
* Add to package.json (replacing the existing scripts):
```
  "main": "dist/index.js",
  "scripts": {
    "start": "tsc && node dist/indexg.js",
    "lint": "eslint . --ext .ts"
  }
```
* Create .env in root, then add: `NODE_ENV=local`
* `npx eslint --init`
* How would you like to use ESLint?: To check syntax and find problems
* What type of modules does your project use?: JavaScript modules (import/export)
* Which framework does your project use?: None of these
* Does your project use TypeScript?: Yes
* Where does your code run?: Node
* What format do you want your config file to be in?: JavaScript
* Finally, you will be prompted to install some additioanl eslint libraries. Choose Yes.

* Add to index.ts (before instantiation of express()):
```
import http from 'http'
import { ApolloServerPluginDrainHttpServer } from 'apollo-server-core'
import { ApolloServer } from 'apollo-server-express'
import { MongoClient } from 'mongodb'
import indexRouter from './routes/index'
import typeDefs from './schema'
import resolvers from './resolvers'
import ExampleAPI from './datasources/example'
const dataSources = () => ({
  exampleAPI: new ExampleAPI(),
});
async function startServer(typeDefs, resolvers) {
```
* Add to index.ts (after final app.use(), replacing everything else):
```
  const httpServer = http.createServer(app);
  const apolloServer = new ApolloServer({
    typeDefs,
    resolvers,
    dataSources,
    plugins: [ApolloServerPluginDrainHttpServer({ httpServer })],
  });
  await apolloServer.start();
  apolloServer.applyMiddleware({ app });
  await new Promise(resolve => httpServer.listen({ port: 8080 }, resolve));
  console.log(`🚀 Server ready at http://localhost:8080${apolloServer.graphqlPath}`);
}
startServer(typeDefs, resolvers)
```

* Create src/schema.ts. Add:
```
import { gql } from 'apollo-server-express'
const typeDefs = gql`
  type Query {
    hello: String
  }
`
export default typeDefs
```

* Create src/resolvers.ts. Add:
```
export default {
  Query: {
    hello: () => 'Hello, world!',
  }
}
```

* Create src/datasources/example.ts. Add:
```
import { DataSource } from 'apollo-datasource'
class ExampleAPI extends DataSource {
  initialize(config) {}
  async getMessage() {
    return 'Hello, world!'
  }
}
export default ExampleAPI
```
