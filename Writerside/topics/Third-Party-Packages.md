# Third Party Packages

Third-party components in the React ecosystem are gradually adapting, beginning to add the "use client" directive
to components that rely on client-only features, marking a clear distinction in their execution environment.

However, many components from npm packages which traditionally leverage client-side features haven't integrated this
directive, this means that while these components will function correctly in client components, they may encounter
issues or might not work at all within server components.

To address this, you can wrap third-party components that rely on client-only features in your own client components.