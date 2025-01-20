# Client Component Placement

To compensate for server components not being able to manage state and handle interactivity, you need to create
client components.

It's recommended to place these client components lower in your tree.

When we make a component a client component, all its children automatically become client components.