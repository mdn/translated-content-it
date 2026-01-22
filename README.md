# Contributing to the Italian content of MDN Web Docs

:tada: First of all, thank you for taking the time to contribute to [MDN Web Docs](https://developer.mozilla.org)! :tada:

The following is a set of guidelines for contributing to the [Italian content of MDN Web Docs](https://github.com/mdn/translated-content-it), which is hosted within the [MDN Organization](https://github.com/mdn) on GitHub.

> [!IMPORTANT]
> Please understand that **the Italian content is fully _machine_-translated from the English content**.
>
> This means that _manual_ updates will be overwritten by a newer machine-translation as soon as the corresponding English content changes.
>
> The translation process is fully automated: For every changed page, we pre-process the English content, pass it along with [this system prompt][prompt] to an [OpenAI model](https://platform.openai.com/docs/models), and post-process the resulting Italian content before committing and pushing it to this repository.

## Contributing

You can contribute to the Italian content in the following ways:

1. **Report issues** you see.
2. **Review issues** reported by other users.
3. **Contribute** to the [translation **glossary**][glossary].
4. **Review** the the [translation **prompt**][prompt].

### 1. Reporting issues

If you see any issues with the Italian content, please report them:

- Create a GitHub issue using the "_Report a problem with this content_" links at the bottom of any Italian MDN page.
- Mention the issue [on Discord][discord] in the `#italian` channel.

Please prioritize reporting the following types of issues:

- Systematic and recurring errors affecting multiple MDN pages.
- Major errors that significantly hinder accuracy or comprehension.

### 2. Reviewing issues

If you see any [open issues here](https://github.com/mdn/translated-content-it/issues), please review them:

- Add emoji reactions to signal that you agree (👍), disagree (👎), or that you're looking into it (👀).
- Add comments to provide additional context, examples, or solutions.
- Bring up the issue [on Discord][discord] in the `#italian` channel.

### 3. Contributing to the glossary

You can contribute to the [Italian translation glossary][glossary]:

- Suggest a new glossary entry by commenting in the file.
- Review the translations in the glossary, and provide feedback.
- Create a [new "Glossary" issue](https://github.com/mdn/translated-content-it/issues/new?template=bug.yml&title=Glossary+-+<SUMMARIZE+THE+PROBLEM>).

> [!NOTE]
> The translation glossary is a **work in progress**.
>
> It will be used to improve translations by passing matching glossary entries along with the system prompt.

### 4. Reviewing the prompt

If you have experience with prompt engineering, please review [the system prompt][prompt], and provide feedback:

- Start a discussion [on Discord][discord] in the `#italian` channel.
- Create a [new "Prompt feedback" issue](https://github.com/mdn/translated-content-it/issues/new?template=bug.yml&title=Prompt+feedback+-+<SUMMARIZE+THE+PROBLEM>).
- Open a PR with your suggested changes to the prompt.

## Code of Conduct

Everyone participating in this project is expected to follow our [Code of Conduct](CODE_OF_CONDUCT.md).

## License

When contributing to the content you agree to license your contributions according to [our license](LICENSE.md).

## Contribute to MDN Web Docs

You can contribute to MDN Web Docs and be a part of our community through content contributions, engineering, or translation work.
The MDN Web Docs project welcomes contributions from everyone who shares our goals and wants to contribute constructively and respectfully within our community.

By participating in and contributing to our projects and discussions, you acknowledge that you have read and agree to our [Code of Conduct](CODE_OF_CONDUCT.md).

## Get in touch

You can communicate with the MDN Web Docs team and community using the [communication channels][main communication].

Additionally, you can communicate with a specific localization team using their own available [communication channels][localization communication].

[discord]: https://mdn.dev/discord
[glossary]: ./files/it/GLOSSARY.yml
[prompt]: ./files/it/PROMPT.md
[main communication]: https://developer.mozilla.org/docs/MDN/Community/Communication_channels
[localization communication]: https://developer.mozilla.org/docs/MDN/Community/Contributing/Translated_content
