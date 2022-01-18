* `npx create-react-app [app name] --template typescript`
* Create main branch / delete master
* `npm audit fix`
* `npm i --save react-router-dom styled-components graphql @apollo/client @types/styled-components @apollo/client`
* `npm i --save-dev eslint`
* Delete src/logo.svg, src/App.css
* Inside src, create folders: components, containers
* Inside src/containers, create App folder
* Move all App… files from src into src/containers/App
* Rename App.tsx to index.tsx

* In src/index.js (NOT Dashboard/index.js):
* Remove import of index.css and App. Add:
```
import { BrowserRouter } from 'react-router-dom'
import { ApolloClient, InMemoryCache, ApolloProvider } from "@apollo/client"
import Router from './Router'
import GlobalStyles from './GlobalStyles'
const client = new ApolloClient({
  uri: 'http://localhost:8080/graphql',
  cache: new InMemoryCache()
});
```
* Replace `<App>` with:
```
    <ApolloProvider client={client}>
      <BrowserRouter>
        <Router />
        <GlobalStyles />
      </BrowserRouter>
    </ApolloProvider>
```

* In src, create Router.tsx. Add:
```
import { Route, Routes } from 'react-router-dom'
import App from './containers/App'
function Router () {
  return (
    <Routes>
      <Route path={'/'} element={<App />} />
    </Routes>
  )
}
export default Router
```

* Rename src/index.css to GlobalStyles.ts. Add:
```
import { createGlobalStyle } from 'styled-components'
const GlobalStyles = createGlobalStyle
```
* Wrap existing styles in back ticks and attach to createGlobalStyle
* `export default GlobalStyles`

* Replace everything in src/containers/App/index.tsx with:
```
import React, { useState, useEffect } from 'react'
import styled from 'styled-components'
const AppWrapper = styled.div`
  text-align: center;
`
const AppHeader = styled.div`
  min-height: 100vh;
  display: flex;
  flex-direction: column;
  align-items: center;
  justify-content: center;
  font-size: calc(10px + 2vmin);
`
function App() {
  const [message, setMessage] = useState('')
  useEffect(() => {
    setMessage('Hello, world!')
  }, [])
  return (
    <AppWrapper>
      <AppHeader>
        <h1>App</h1>
        <p>Message: {message}</p>
      </AppHeader>
    </AppWrapper>
  )
}
export default App
```

* In package.json, add: `"proxy": "http://localhost:8080",`
* `npx eslint --init`
* How would you like to use ESLint?: To check syntax and find problems
* What type of modules does your project use?: JavaScript modules (import/export)
* Which framework does your project use?: React
* Does your project use TypeScript?: Yes
* Where does your code run?: Browser
* What format do you want your config file to be in?: JavaScript
* Finally, you will be prompted to install some additioanl eslint libraries. Choose Yes.

