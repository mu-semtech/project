# mu-semtech documentation standards
- Our documentation follows the amazing documentation system used at Divio. [You can view the 30min talk and/or documentation on their website](https://documentation.divio.com/), and/or use the [checklist below](#checklist) to see if the documentation you've written fits.
- The docs...
    - ... about semantic microservices/mu-semtech as a whole should be located in this repository ([mu-semtech/project](https://github.com/mu-semtech/project)).
    - ... concerning specific microservices should be located inside their respective repositories.


## Checklist
This checklist is an attempt at boiling down the divio documentation system on how to write better documentation into something unobtrusive enough for people familiar with it to use as a checklist, but instructive enough for people unfamiliar with it to somewhat do the thing.

### Tutorials?
For each tutorial: write as if you're *teaching a child how to cook*.

Checklist:

- [ ] Holds the readers hand from start to finish
- [ ] Gives a sense of achievement
- [ ] Works for everyone, with minimal explanation
- [ ] Also teaches what ***you*** take for granted

### How-To guides?
For each how-to: write down how to achieve **1** specific thing as if it were a *cooking recipe*.

Checklist:

- [ ] Only 1 practical goal per how-to
- [ ] Minimal explanation
- [ ] Flexible: works for different but similar uses 


### A reference?
For each reference: write as if you are writing an *encyclopedia* article.

Checklist:

- [ ] Structured like the codebase
- [ ] Doesn't explain common tasks ([how-to guides](#how-to-guides))
- [ ] Doesn't explain basic concepts ([tutorials](#tutorials))
- [ ] Fully describes the machinery

### Explanations: discussions/background material?
For each background material: write as if you're writing about the *history and context* of a subject.

Checklist:

- [ ] Explains design decisions
- [ ] Considers alternatives
- [ ] Helps the reader make sense of things


## Releasing a new version
Once a new version of a semantic.works repository is made it should...
- Be tagged using `git tag`
- Have a DockerHub image with the same tag (NOTE: this happens automatically if your repository has a `.woodpecker/.tag.yml` configuration)

*This will ensure that the documentations in app-mu-info get updated. See [project/discussions/how-tos/documentation-structure](../discussions/documentation-structure.md) for more info*



