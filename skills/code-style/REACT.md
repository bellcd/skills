# React

- Code that returns JSX / React elements must be defined as JSX / React components, never as plain JavaScript helper functions.
- A component that reads data takes its query functions as a required prop rather than constructing the data source itself. Production binds the real source at the page or client boundary, and a spec binds fixtures it defines inline. A component that builds its own source can only be asserted against whatever that source returns, which moves the arrange step into a separate stub file and out of the reader's view.
