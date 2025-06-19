![github-banner-helm-helmwww](https://user-images.githubusercontent.com/686194/68531441-f4ad4e00-02c6-11ea-982b-74d7c3ff0071.png)


* hosted | [here](https://helm.sh/)
* [docs](content/en)

## Development

* TODO: 
* Helm.sh is a simple [Hugo](https://gohugo.io/) static site, built with a custom theme. To run the website locally, you'll need to first [install](https://gohugo.io/getting-started) Hugo extended edition and any dependencies.

```
brew install hugo
yarn install
```

You can then compile and run the site locally:

```
hugo serve
```

## Deployment [![Netlify Status](https://api.netlify.com/api/v1/badges/8ffabb30-f2f4-45cc-b0fa-1b4adda00b5e/deploy-status)](https://app.netlify.com/sites/helm-merge/deploys)

Changes are automatically deployed to [Netlify](https://app.netlify.com/sites/helm-merge/deploys) when merged to `main`. Build logs can be found [here](https://app.netlify.com/sites/helm-merge/deploys).


---

## Contributing

* [contributing](https://github.com/helm/helm/blob/main/CONTRIBUTING.md#sign-your-work)

### How to edit the Helm docs?

* | 
  * Helm v3+,
    * [here](/content/en/docs/)
  * Helm v3-,
    * [here](https://github.com/helm/helm/tree/dev-v2/docs)

### Updating the [Helm CLI Reference Docs](content/en/docs/helm)

* == Helm CLI Commands / 
  * exported -- from -- [helm project](https://github.com/helm/helm/blob/a6b2c9e2126753f6f94df231e89b2153c2862764/cmd/helm/root.go#L169)
  * rendered here
* steps to upgrade
  * `helm plugin uninstall`
  * | `content/en/docs/helm/`
    * `HOME='~' helm docs --type markdown --generate-headers`
      * 👀generate & replace the markdown docs files👀

### How to Write a Blog Post
* TODO:
Blog posts are created via pull requests. The following steps are used to add them:

1) Add a new file to the `content/en/blog/` directory whose name is the published date and the title. The files must be markdown formatted. See the existing titles for examples of the format
2) Add the header meta-data to the file using this format (note the permalink structure). Recommended but optional fields are `authorname` which should be name(s); these are displayed verbatim. `authorlink` is the link used by `authorname`.

```yaml
---
title: "A Fancy Title"
slug: "fancy-title"
authorname: "Captain Awesome"
authorlink: "https://example.com"
date: "yyyy-mm-dd"
---
```

3) Add the content below the `---` as Markdown. The title does not need to be included in this section
4) Any images should be placed in the `/content/en/blog/images/` directory. Images should be losslessly compressed to reduce their size. Tools, such as [ImageOptim](https://imageoptim.com/), can be used.
5) To summarize the content on the blog index page, insert a `<!--more-->` break in your markdown. This will truncate the content with a _Read More_ link.

Blog PRs require approval from the core Helm [maintainers](https://github.com/helm/helm/blob/main/OWNERS) before merge.

