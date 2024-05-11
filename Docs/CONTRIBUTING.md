# Contribute to the RTEdbg project

Thank you for considering contributing. We welcome all types of contributions, including documentation clarifications, code improvements, additional demo code, porting code to new CPU cores, tools for transferring recorded data to the host computer, and new feature suggestions.

See the current **[TO-DO  list](Docs/TODO.md)**.

## How can I help?
If you find a bug or lack of functionality, search for existing issues before submitting a new one. Follow the guidelines in this document.

### Reporting bugs

A good report helps maintainers and the community understand the problem and reproduce the behavior. It should:
- Use a clear title to identify the problem.
- Describe the steps that reproduce the problem.
- Include the toolchain and version you are using.
- Be specific. Do not mix a bug report with a feature request.
- If you think RTEmsg is decoding binary data incorrectly, include the version number in the report. It's in the header of each Main.log file.
- When reporting RTEmsg errors, keep in mind that descriptions may change in future releases, but error numbers should stay the same. Always include the number when referencing.

**Suspected Bug in RTEmsg Data Decoding Application:** 
Check your format definitions and test the data logging functions with known data. If you can't find the cause of suspicious data decoding, use the -debug command line option. Follow the contribution guidelines when reporting issues.

**Note:** Don't post large files. Upload them to cloud storage and provide the download link.

### Suggesting Enhancements

Before submitting an enhancement, make sure your idea fits the scope and provide as much detail and context as possible. 
- Use a clear and descriptive problem title to identify the proposal.
- Provide a step-by-step description of the proposed enhancement, including use cases.
- Provide example(s) to demonstrate the need for the enhancement.
- Describe the current behavior, and explain what behavior you expect to see instead, and why.
- Explain why this enhancement would be useful to most users.

### Pull Requests

Follow the guidelines for [Creating a pull request](https://docs.github.com/en/pull-requests/collaborating-with-pull-requests/proposing-changes-to-your-work-with-pull-requests/creating-a-pull-request) and try to:
- Maintain the quality of the project.
- Add features or fix problems that are important to a wide community of users.
- After submitting your pull request, make sure it passes all the status checks.

## Coding Style

The C code should adhere to [Barr Group Embedded C Coding Standard](https://barrgroup.com/embedded-systems/books/embedded-c-coding-standard).

## Code of Conduct

This project and everyone who takes part in it must follow the [opensource Code of Conduct](https://opensource.com/code-of-conduct). By taking part, you agree to this.

## License

By contributing, you agree that your contributions will be licensed under the MIT License. Feel free to contact the maintainers if that's a concern.