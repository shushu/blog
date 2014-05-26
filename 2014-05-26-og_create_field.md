I started to work in Gizra more than 6 months ago. While I did have lots of Drupal experience, I didn't have a lot of Organic Groups experience, especially not in Drupal 7 version.
As a developer, I am more interested in the way things are built and less in the way they look. I mean, I know my way around front-end and user interface, but good looking UI won't make me exceited like well developed piece of code.
This is why I enjoy working with OG - since it is well-structured, lots of things works flawlessly, and easy to enhance.

In this post I want to focus on one particular task, and how it was easily done once you know OG API.

Our client decided they want to have a description for each taxonomy term, and to show the description when showing the taxonomy term page. This is easy, considering by default there is a description field. Then they decided they don't want to show the field in all pages, and they want it to be configurable.

The solution seems simple - adding a new boolean field for each taxonomy term, which will define whether the description will be seen or not.
The problem is that since we are working in OG (specifically in OpenSchoalr project), we can't and should not add the field manually, and considering each of the authorized site users can create their own vocabulary, we need to add the field automatically when the vocabulary is being created.
