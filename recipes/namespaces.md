# Namespaces

When using layout areas Atomic Layout generates React components for each declared area:

```jsx
import React from 'react'
import { Composition } from 'atomic-layout'

export const Header = () => (
  <Composition areas="logo menu">
    {({ Logo, Menu }) => ( // <-- generted areas components
      <>
        <Logo>{...}</Logo>
        <Menu>{...}</Menu>
      </>
    )}
  </Composition>
)
```

To make the rendering part shorter, we recommend using [Object Spread syntax](https://developer.mozilla.org/en-US/docs/Web/JavaScript/Reference/Operators/Spread_syntax) for areas object given as an argument to the render function.

However, this way area components reserve their namespaces within the render function's scope. This may result into conflicts when you have custom React components which name matches the area name:

```jsx
const Logo = () => <img src="logo.png" />

export const Header = () => (
  <Composition areas="logo menu">
    {({ Logo, Menu }) => (
      <>
        <Logo> // this is an area component
          <Logo /> // this is also an area component
        </Logo>
        <Menu>{...}</Menu>
      </>
    )}
  </Composition>
)
```

> Referencing custom Logo component becomes problematic and requires to rename things around.

To prevent from namespace collisions we recommend not to spread the areas Object in cases when areas names match the names of your custom React components:

```jsx
import React from 'react'
import { Composition } from 'atomic-layout'

const Logo = () => (
  return <img src="logo.png" />
)

export const Header = () => (
  <Composition areas="logo menu">
    {(Areas) => (
      <>
        <Areas.Logo> // scoped area component
          <Logo /> // your custom component
        </Areas.Logo>
        <Areas.Menu>{...}</Areas.Menu>
      </>
    )}
  </Composition>
)
```

> This way generated areas are scoped under a single "Areas" namespace.

