# Context Providers

Context providers are typically rendered near the root of an application to share global application state and logic.

Since React context is not supported in Server Components, attempting to create a context at the root of your
application will result in an error.

To address this, you can create a context and render it's provider inside a separate client component.