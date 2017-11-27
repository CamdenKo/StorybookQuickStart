# The Quickest Start Guide for Storybook
## Getting started
In your CLI globally install `@storybook/cli`

`npm i -g @storybook/cli`

This gives you access to the `getstorybook` command which will become relevant soon.

## Storybook in your project
 * Navigate to your project in your CLI
 * run `getstorybook`
 * Follow the prompts in the CLI
 * There should now be a new folder in the project's root directory `stories`
    * This is where everything will go inside of
    * Go ahead and delete everything inside of there
    * This is the file where we load in all of our files
  * Copy and paste this snippet into your file (Note: this is for a project with both react-redux and styled-components, feel free to remove those as needed)
  * ```javascript
    import React from 'react'
    import { configure, addDecorator } from '@storybook/react'
    import { Provider } from 'react-redux'
    import { Router } from 'react-router'
    import { ThemeProvider } from 'styled-components'

    import store from '../client/store'
    import history from '../client/history'
    import theme from '../client/theme'

    const req = require.context('../client/components', true, /\.stories\.js$/)

    const loadStories = () => req.keys().forEach(file => req(file))

    addDecorator(story => (
      <ThemeProvider theme={theme}>
        <Provider store={store}><Router history={history}>{story()}</Router></Provider>
      </ThemeProvider>
    ))
    configure(loadStories, module)
    ```
    * Let's walk through what's going on in the above code snippet
    * `req` is a variable which will go through and grab all the files that meet a certain criteria, in this case we are looking at the `../client/components` directory and grabbing every file that ends with `stories.js`
    * `addDecorator` is a function given to us from Storybook. A `Decorator` is a React Component that will wrap around every single Storybook component that we will load. In this case we want every single component to have access to the `ThemeProvider` from `styled-components`, the `Provider` from `react-redux` and the `Router` from `react-router`
