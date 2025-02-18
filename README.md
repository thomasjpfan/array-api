# Array API standard
<!-- ALL-CONTRIBUTORS-BADGE:START - Do not remove or modify this section -->
[![All Contributors](https://img.shields.io/badge/all_contributors-29-orange.svg?style=flat-square)](#contributors-)
<!-- ALL-CONTRIBUTORS-BADGE:END -->

This repository contains documents, tooling and other content related to the
API standard for arrays (or tensors).

These are relevant documents related to the content in this repository:

- [Rendered html docs for latest version](https://data-apis.github.io/array-api/latest)
- [Consortium announcement blog post](https://data-apis.org/blog/announcing_the_consortium/)
- [Blog post on first release of draft array API standard](https://data-apis.org/blog/array_api_standard_release/)

See [CONTRIBUTING.md](CONTRIBUTING.md) for how to go about contributing to
this array API standard.


## Building docs locally

### Quickstart

To install the local stubs and additional dependencies of the Sphinx docs, you
can use `pip install -r doc-requirements.txt`. Then just running `make` at the
root of the repository should build the whole spec website.

```sh
$ pip install -r doc-requirements.txt
$ make
$ ls _site/
2021.12/  draft/  index.html  latest/  versions.json
```

### The nitty-gritty

The spec website is comprised of multiple Sphinx docs (one for each spec version),
all of which exist in `spec/` and rely on the modules found in `src/` (most
notably `array_api_stubs`). For purposes of building the docs, these `src/`
modules do not need to be installed as they are added to the `sys.path` at
runtime.

To build specific versions of the spec, run `sphinx-build` on the respective
folder in `spec/`, e.g.

```sh
$ sphinx-build spec/2012.12/ _site/2012.12/
```

Additionally, `make draft` aliases

```sh
$ sphinx-build spec/draft/ _site/draft/
```

To build the whole website, which includes every version of the spec, you can
utilize `make spec`.


## Making a spec release

The Sphinx doc at `spec/draft/` should be where the in-development spec resides,
with `src/array_api_stubs/_draft/` containing its respective stubs. A spec
release should involve:

* Renaming `src/array_api_stubs/_draft/` to `src/array_api_stubs/_YYYY_MM`
* Renaming `spec/draft/` to `spec/YYYY.MM`
* Updating `spec/YYYY.MM/conf.py`

  ```diff
  ...
  - from array_api_stubs import _draft as stubs_mod
  + from array_api_stubs import _YYYY_MM as stubs_mod
  ...
  - release = "DRAFT"
  + release = "YYYY.MM"
  ...
  ```

* Updating `spec/_ghpages/versions.json`

  ```diff
  {
  +     "YYYY.MM": "YYYY.MM",
  ...
  ```

* Updating `Makefile`

  ```diff
  ...
  	-sphinx-build "$(SOURCEDIR)/PREVIOUS.VER" "$(BUILDDIR)/PREVIOUS.VER" $(SPHINXOPTS)
  + 	-sphinx-build "$(SOURCEDIR)/YYYY.MM" "$(BUILDDIR)/YYYY.MM" $(SPHINXOPTS)
  - 	-cp -r "$(BUILDDIR)/PREVIOUS.VER" "$(BUILDDIR)/latest"
  + 	-cp -r "$(BUILDDIR)/YYYY.MM" "$(BUILDDIR)/latest"
  ...
  ```

These changes should be committed and tagged. The next draft should then be
created. To preserve git history for both the new release and the next draft:

1. Create and checkout to a new temporary branch.

  ```sh
  $ git checkout -b tmp
  ```

2. Make an empty commit. <sup>This is required so merging the temporary branch
   (4.) is not automatic.</sup>

  ```sh
  $ git commit --allow-empty -m "Empty commit for draft at YYYY.MM "
  ```

3. Checkout back to the branch you are making a spec release in.

  ```sh
  $ git checkout YYYY.MM-release
  ```

4. Merge the temporary branch, specifying no commit and no fast-forwarding.

  ```sh
  $ git merge --no-commit --no-ff tmp
  Automatic merge went well; stopped before committing as requested
  ```

5. Checkout the `spec/draft/` files from the temporary branch.

  ```sh
  $ git checkout tmp -- spec/draft/
  ```

6. Commit your changes.

  ```sh
  $ git commit -m "Copy YYYY.MM as draft with preserved git history"
  ```

You can run `git blame` on both `spec/YYYY.MM` and `spec/draft` files to verify
we've preserved history. See this [StackOverflow question](https://stackoverflow.com/q/74365771/5193926)
for more background on the approach we use.

<!-- TODO: write a script to automate/standardise spec releases -->


## Contributors ✨

Thanks goes to these wonderful people ([emoji key](https://allcontributors.org/docs/en/emoji-key)):

<!-- ALL-CONTRIBUTORS-LIST:START - Do not remove or modify this section -->
<!-- prettier-ignore-start -->
<!-- markdownlint-disable -->
<table>
  <tr>
    <td align="center"><a href="http://saulshanabrook.github.io/"><img src="https://avatars.githubusercontent.com/u/1186124?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Saul Shanabrook</b></sub></a><br /><a href="#tool-saulshanabrook" title="Tools">🔧</a> <a href="#ideas-saulshanabrook" title="Ideas, Planning, & Feedback">🤔</a> <a href="#research-saulshanabrook" title="Research">🔬</a></td>
    <td align="center"><a href="https://github.com/stdlib-js/stdlib"><img src="https://avatars.githubusercontent.com/u/2643044?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Athan</b></sub></a><br /><a href="#content-kgryte" title="Content">🖋</a> <a href="#data-kgryte" title="Data">🔣</a> <a href="#tool-kgryte" title="Tools">🔧</a> <a href="#research-kgryte" title="Research">🔬</a></td>
    <td align="center"><a href="https://github.com/steff456"><img src="https://avatars.githubusercontent.com/u/20992645?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Stephannie Jimenez Gacha</b></sub></a><br /><a href="#data-steff456" title="Data">🔣</a> <a href="#content-steff456" title="Content">🖋</a> <a href="#research-steff456" title="Research">🔬</a></td>
    <td align="center"><a href="https://github.com/asmeurer"><img src="https://avatars.githubusercontent.com/u/71486?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Aaron Meurer</b></sub></a><br /><a href="#content-asmeurer" title="Content">🖋</a> <a href="https://github.com/data-apis/array-api/commits?author=asmeurer" title="Tests">⚠️</a> <a href="#tool-asmeurer" title="Tools">🔧</a></td>
    <td align="center"><a href="http://deathbeds.github.io/"><img src="https://avatars.githubusercontent.com/u/4236275?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Tony Fast</b></sub></a><br /><a href="#maintenance-tonyfast" title="Maintenance">🚧</a></td>
    <td align="center"><a href="https://github.com/rgommers"><img src="https://avatars.githubusercontent.com/u/98330?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ralf Gommers</b></sub></a><br /><a href="#blog-rgommers" title="Blogposts">📝</a> <a href="#business-rgommers" title="Business development">💼</a> <a href="https://github.com/data-apis/array-api/commits?author=rgommers" title="Code">💻</a> <a href="#content-rgommers" title="Content">🖋</a> <a href="https://github.com/data-apis/array-api/commits?author=rgommers" title="Documentation">📖</a> <a href="#fundingFinding-rgommers" title="Funding Finding">🔍</a> <a href="#maintenance-rgommers" title="Maintenance">🚧</a> <a href="#ideas-rgommers" title="Ideas, Planning, & Feedback">🤔</a> <a href="#projectManagement-rgommers" title="Project Management">📆</a> <a href="#talk-rgommers" title="Talks">📢</a></td>
    <td align="center"><a href="https://github.com/teoliphant"><img src="https://avatars.githubusercontent.com/u/254880?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Travis E. Oliphant</b></sub></a><br /><a href="#business-teoliphant" title="Business development">💼</a> <a href="#fundingFinding-teoliphant" title="Funding Finding">🔍</a> <a href="#ideas-teoliphant" title="Ideas, Planning, & Feedback">🤔</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://leofang.github.io/"><img src="https://avatars.githubusercontent.com/u/5534781?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Leo Fang</b></sub></a><br /><a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aleofang" title="Reviewed Pull Requests">👀</a> <a href="#ideas-leofang" title="Ideas, Planning, & Feedback">🤔</a> <a href="#content-leofang" title="Content">🖋</a></td>
    <td align="center"><a href="https://tqchen.com/"><img src="https://avatars.githubusercontent.com/u/2577440?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Tianqi Chen</b></sub></a><br /><a href="#ideas-tqchen" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Atqchen" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="http://stephanhoyer.com/"><img src="https://avatars.githubusercontent.com/u/1217238?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Stephan Hoyer</b></sub></a><br /><a href="#ideas-shoyer" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Ashoyer" title="Reviewed Pull Requests">👀</a> <a href="#question-shoyer" title="Answering Questions">💬</a></td>
    <td align="center"><a href="http://www.ic.unicamp.br/~tachard/"><img src="https://avatars.githubusercontent.com/u/5061?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alexandre Passos</b></sub></a><br /><a href="#ideas-alextp" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aalextp" title="Reviewed Pull Requests">👀</a></td>
    <td align="center"><a href="http://paigevie.ws/"><img src="https://avatars.githubusercontent.com/u/3712347?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Paige Bailey</b></sub></a><br /><a href="#fundingFinding-dynamicwebpaige" title="Funding Finding">🔍</a></td>
    <td align="center"><a href="https://github.com/apaszke"><img src="https://avatars.githubusercontent.com/u/4583066?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Adam Paszke</b></sub></a><br /><a href="#ideas-apaszke" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aapaszke" title="Reviewed Pull Requests">👀</a> <a href="#talk-apaszke" title="Talks">📢</a></td>
    <td align="center"><a href="http://amueller.github.io/"><img src="https://avatars.githubusercontent.com/u/449558?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Andreas Mueller</b></sub></a><br /><a href="#ideas-amueller" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aamueller" title="Reviewed Pull Requests">👀</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://www.linkedin.com/in/shengzha/"><img src="https://avatars.githubusercontent.com/u/2626883?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Sheng Zha</b></sub></a><br /><a href="#ideas-szha" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aszha" title="Reviewed Pull Requests">👀</a> <a href="#talk-szha" title="Talks">📢</a></td>
    <td align="center"><a href="https://github.com/kkraus"><img src="https://avatars.githubusercontent.com/u/1324560?v=4?s=100" width="100px;" alt=""/><br /><sub><b>kkraus</b></sub></a><br /><a href="#ideas-kkraus" title="Ideas, Planning, & Feedback">🤔</a> <a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Akkraus" title="Reviewed Pull Requests">👀</a> <a href="#talk-kkraus" title="Talks">📢</a></td>
    <td align="center"><a href="https://tomaugspurger.github.io/"><img src="https://avatars.githubusercontent.com/u/1312546?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Tom Augspurger</b></sub></a><br /><a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3ATomAugspurger" title="Reviewed Pull Requests">👀</a> <a href="#question-TomAugspurger" title="Answering Questions">💬</a></td>
    <td align="center"><a href="https://github.com/edloper"><img src="https://avatars.githubusercontent.com/u/5790348?v=4?s=100" width="100px;" alt=""/><br /><sub><b>edloper</b></sub></a><br /><a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aedloper" title="Reviewed Pull Requests">👀</a> <a href="#question-edloper" title="Answering Questions">💬</a></td>
    <td align="center"><a href="https://github.com/aregm"><img src="https://avatars.githubusercontent.com/u/1798344?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Areg Melik-Adamyan</b></sub></a><br /><a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aaregm" title="Reviewed Pull Requests">👀</a> <a href="#fundingFinding-aregm" title="Funding Finding">🔍</a></td>
    <td align="center"><a href="http://math.stackexchange.com/users/11069/sasha"><img src="https://avatars.githubusercontent.com/u/21087696?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Oleksandr Pavlyk</b></sub></a><br /><a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aoleksandr-pavlyk" title="Reviewed Pull Requests">👀</a> <a href="#question-oleksandr-pavlyk" title="Answering Questions">💬</a></td>
    <td align="center"><a href="https://github.com/tdimitri"><img src="https://avatars.githubusercontent.com/u/62962217?v=4?s=100" width="100px;" alt=""/><br /><sub><b>tdimitri</b></sub></a><br /><a href="#ideas-tdimitri" title="Ideas, Planning, & Feedback">🤔</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://github.com/jack-pappas"><img src="https://avatars.githubusercontent.com/u/477287?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Jack Pappas</b></sub></a><br /><a href="#ideas-jack-pappas" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://github.com/agarwalashish"><img src="https://avatars.githubusercontent.com/u/3207727?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Ashish Agarwal</b></sub></a><br /><a href="https://github.com/data-apis/array-api/pulls?q=is%3Apr+reviewed-by%3Aagarwalashish" title="Reviewed Pull Requests">👀</a> <a href="#question-agarwalashish" title="Answering Questions">💬</a></td>
    <td align="center"><a href="http://ezyang.com/"><img src="https://avatars.githubusercontent.com/u/13564?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Edward Z. Yang</b></sub></a><br /><a href="#ideas-ezyang" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://github.com/mruberry"><img src="https://avatars.githubusercontent.com/u/38511765?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Mike Ruberry</b></sub></a><br /><a href="#ideas-mruberry" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="http://ericwieser.me/"><img src="https://avatars.githubusercontent.com/u/425260?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Eric Wieser</b></sub></a><br /><a href="#ideas-eric-wieser" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://www.willingconsulting.com/"><img src="https://avatars.githubusercontent.com/u/2680980?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Carol Willing</b></sub></a><br /><a href="#ideas-willingc" title="Ideas, Planning, & Feedback">🤔</a></td>
    <td align="center"><a href="https://arogozhnikov.github.io/"><img src="https://avatars.githubusercontent.com/u/6318811?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Alex Rogozhnikov</b></sub></a><br /><a href="#ideas-arogozhnikov" title="Ideas, Planning, & Feedback">🤔</a></td>
  </tr>
  <tr>
    <td align="center"><a href="https://explosion.ai/"><img src="https://avatars.githubusercontent.com/u/8059750?v=4?s=100" width="100px;" alt=""/><br /><sub><b>Matthew Honnibal</b></sub></a><br /><a href="#ideas-honnibal" title="Ideas, Planning, & Feedback">🤔</a></td>
  </tr>
</table>

<!-- markdownlint-restore -->
<!-- prettier-ignore-end -->

<!-- ALL-CONTRIBUTORS-LIST:END -->

This project follows the [all-contributors](https://github.com/all-contributors/all-contributors) specification. Contributions of any kind welcome!
