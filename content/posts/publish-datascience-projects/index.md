{
  "title": "Publish jupyter notebooks as blog posts",
  "author": {
    "name": "David Meszaros"
  },
  "date": "2024-10-02",
  "type": [
    "post",
    "posts"
  ]
}
## Goal

My goal was to publish jupyter notebooks to a [hugo blog](https://davidmeszaros.blog) as easy as possible. A jupyter notebook just needs to be created and pushed to github, where a github action will publish the notebook to my blog. All of these should be done with minimal effort.

## Motivation

I recently began sharing my work through a blog. While I currently follow a "one GitHub repo per project" approach, I've been considering the benefits of a monorepo strategy for my hobby projects.  With everything in one place, the deployment process would be simpler, additionally it could be easier to reuse code across projects, and I could just set up one development environment too.

Despite of this, it’s not the right approach for me 😀. I prefer to keep projects separate. I enjoy giving each data science project its own GitHub repository because it makes the code easier to share, and I don’t have to worry about repository size. For me, it makes more sense to keep each project in its own logical unit, even if this is subjective—it’s just the way I prefer to work.

- I might not want to share every project
- I might want specific automation for individual ones.
- Plus, a monorepo could quickly grow in size, making it harder to manage. 

For these reasons, I’ve decided to adopt the following approach:
- One repository for all my blog posts
- One repository for each data science project
- A template repository for new data science projects

As mentioned before, my goal is to publish my hobby projects with minimal effort. The first time I published my work, I had to create a notebook with all the code and explanations just to generate the final markdown file. Afterward, I had to copy the markdown from the project’s repository to the blog repository, and I had to deploy it manually. This process is tedious and time-consuming.

## The workflow

I created a *custom github action*, which basically can be used to publish the jupyter notebooks to the blog. The advantage of this approach, that I can just create a template repository, which can be used for any future data science projects, where the required github actions are already set up. Furthermore it allows me to make changes only in the custom github action, without the need to change the action in each project repository. After setting up the data science projects from the template repository, I can just focus on creating the individual project notebooks. When I am ready to publish, I just need to push the notebook to it's repository, and the workflow will take care of the rest.

The following diagram shows the workflow of the process:

![workflow](resources/publish-workflow.drawio.png)

## The custom github action

As described the first step was to create a custom github action. The action needs 4 parameters:

1. `blog-gh-token`: The github token for the blog repository. This is needed to be able to push the changes to the blog repository. The token in my case is a fine-grained personal access token, which only has the permissions to push to the blog repository.
2. `notebook-path`: The path to the notebook, which we want to publish.
3. `target-repo`: The target repository. This is the repository where the hugo blog is living.
4. `target-branch`: The target branch, of the blog repository. 

After providing the required parameters, the action will execute the following steps:

##### Setting up python with `nbconvert`
`nbconvert` is used to convert the notebook to a markdown file. In hugo the content is stored in markdown format, so this is a requirement.


##### Convert the notebook to a markdown file
This step is using a python script, which is located in the template repository. The python script is basically using the `nbconvert` library to convert the notebook to a markdown file. I added some custom preprocessing steps and configurations, to bring the notebook to the desired output format as markdown file.

Currently, this is just one python script, taking care of the conversion. The steps of the conversion:
- reading the notebook with `nbformat`
- creating and adding a custom *front-matter* for the notebook; this is needed for hugo to be able to detect the title, date, and other metadata from the notebook.
- removing empty cells and unnecessary whitespace
- handle custom resources, like images referenced in the notebook with `![alt text](image.png)`. These images are not converted by `nbconvert`, so I need to take care of them.
- putting all resources (etc.: images) to the resources folder, and reference them in the markdown file.
- writing the final markdown file

With these steps the notebook is converted with all desired files. With this approach the created files can be easily added to the target blog repository.

##### Copy the notebook to the target repository
This is done by cloning the target repository in the github action and copying the converted notebook and its resources (like images) to the target branch. After this the changes will be pushed to the target branch authenticated by the provided github token.

## Blog repositoy

The blog repository is a normal hugo repository. I am using [netlify](https://netlify.com) to host the blog. Netlify provides some nice features to deploy the blog with CI/CD. However I decided to deploy my blog "manually" (I mean with manually that I am not using the netlify's build service) because I want to have control over the full deployment process. I am using the [netlify cli](https://cli.netlify.com/) to deploy the blog. 

## Final thoughts

This approach is not perfect, but it works for me. I can live with the current solution, and it is a good starting point. In any future project, I can just create a notebook with all the code and explanations, and the workflow will take care of the rest. 

### Links

- [The custom github action](https://github.com/meszdav/notebook-hugo-action/blob/main/action.yml)
- [The blog repository](https://github.com/meszdav/blog)
