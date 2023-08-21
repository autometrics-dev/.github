[![GitHub_headerImage](https://user-images.githubusercontent.com/3262610/221191257-ee75ed39-9c24-4480-8522-6ac47eb97532.png)](https://autometrics.dev)

[Autometrics](https://autometrics.dev) is an observability micro-framework built for developers. It makes it easy to instrument any function with the most useful metrics: request rate, error rate, and latency. Autometrics uses instrumented function names to generate Prometheus queries so you don’t need to hand-write complicated PromQL.

To make it easy for you to spot and debug issues in production, Autometrics inserts links to live charts directly into each function’s doc comments and provides dashboards that work out of the box. It also enables you to create powerful alerts based on Service-Level Objectives (SLOs) directly in your source code. Lastly, Autometrics writes queries that correlate your software’s version info with anomalies in the metrics to help you quickly identify commits that introduced bugs or latency.

See the [docs](https://docs.autometrics.dev/) for more details about using Autometrics and the ideas behind it.

[![Discord Shield](https://discordapp.com/api/guilds/950489382626951178/widget.png?style=shield)](https://discord.gg/kHtwcH8As9)

https://github.com/autometrics-dev/.github/assets/3262610/1015d153-01a9-48ab-bd81-fbc5f97e4e48

##  Implementations

Autometrics is available in the following programming languages:

- **Rust** - [Repo](https://github.com/autometrics-dev/autometrics-rs) | [Quickstart](https://docs.autometrics.dev/rust/quickstart) | [API Docs](https://docs.rs/autometrics/latest/autometrics/)
- **TypeScript** - [Repo](https://github.com/autometrics-dev/autometrics-ts) | [Quickstart](https://docs.autometrics.dev/typescript/quickstart) | [API Docs](https://github.com/autometrics-dev/autometrics-ts/tree/main/packages/lib/reference)
- **Go** - [Repo](https://github.com/autometrics-dev/autometrics-go) | [Quickstart](https://docs.autometrics.dev/go/quickstart) | [API Docs](https://pkg.go.dev/github.com/autometrics-dev/autometrics-go/cmd/autometrics)
- **Python** - [Repo](https://github.com/autometrics-dev/autometrics-py) | [Quickstart](https://docs.autometrics.dev/python/quickstart) | [API Docs](https://pypi.org/project/autometrics/)
- **C#/.NET** (community-maintained) - [Repo](https://github.com/autometrics-dev/autometrics-cs)
- **Java** (community- maintained) - [Repo](https://github.com/jamsiedaly/autometricsj) | [IntelliJ Plugin](https://plugins.jetbrains.com/plugin/22408-autometrics)

Want to help implement autometrics in another language? Take a look at the [Autometrics Specification](https://github.com/autometrics-dev/autometrics-shared/blob/main/SPEC.md) and get in touch! We'd love to work with you on it.

##  Get Involved!

We would love to have you! If you have ideas, suggestions, questions, or would like to pitch in, please come join us!

You can get invovled in a variety of ways:
- Join the conversation on [Discord](https://discord.gg/kHtwcH8As9)
- See the ongoing discussions and new ideas in the [Github Discussions](https://github.com/orgs/autometrics-dev/discussions) (this is a good place to bring up new feature ideas)
- Find the overall project roadmap [here](https://github.com/orgs/autometrics-dev/projects/1)
- And take a look at the open issues on the repos of each of the [language implementations](#2-implementations)
