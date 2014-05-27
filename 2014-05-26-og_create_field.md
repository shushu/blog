I started to work in Gizra more than 6 months ago. While I did have lots of Drupal experience, I didn't have a lot of <a href="https://drupal.org/project/og" target=_blank>Organic groups</a> experience, especially not in Drupal 7 version.
As a developer, I am more interested in the way things are built and less in the way they look. I mean, I know my way around front-end and user interface, but a good looking UI won't make me as exceited as a well developed piece of code.
This is why I enjoy working with OG - since it is well-structured, lots of things works flawlessly, and easy to enhance.

In this post I want to focus on one particular task, and how it was easily done once you know OG API.

Our client decided they want to have a description for each taxonomy term, and to show the description when showing the taxonomy term page. This is easy, considering by default there is a description field. Then they decided they don't want to show the field in all pages, and they want it to be configurable.

The solution seems simple - adding a new boolean field for each taxonomy term, which will define whether the description will be seen or not, and then to use the `taxonomy-term.tpl.php` to show or hide the description according to this field.<BR/>

The problem is that since we are working in OG (specifically in <a href="http://openscholar.gizra.com/" target=_blank>OpenSchoalr</a> project), we can't and should not add the field manually, and considering each of the authorized site users can create their own vocabulary, we need to add the field automatically when the vocabulary is being created.

Gladly, OG API is built for those kind of tasks, and with several steps we can achieve what we want.

First, we need to define the field that we need. Once it is defined, it can be exported and then used in code. While it might be possible to define the field manually, I believe it is always better to avoid guessing Drupal multi-dimentional-unvalidatible arrays, and to leave this task for the machine.
* Create a taxonomy vocabulary via `http://localhost/ogdev/www/admin/structure/taxonomy/add` and then add the field you need via `http://localhost/ogdev/www/admin/structure/taxonomy/<your vocabulary>/fields`.
@todo: add a screenshot.
* Use the features UI in order to create a new feature that includes the new field you just created. We won't use the feature as is, this is just a simple way to export the field configuration (I guess there are other ways as well).\
@todo: add a screenshot.
* Once you download the features file, look for the code inside the `hook_field_default_field_bases()` and `hook_field_default_field_instances()` implementations. This is the code we will use later on.

Second, we need to tell OG we have a new field, and to define how OG should use it. This is done using <a href="http://drupalcontrib.org/api/drupal/contributions!og!og.api.php/function/hook_og_fields_info/7" target=_blank>`hook_og_fields_info()`</a>. What you need to do is to create a new module (or use one of your own modules), and create an implementation of this hook, in which you will add the code exported from the last step (with minor changes). One important issue is the name of the field - it needs to be correct in all places, and it is what you will use later on.
@todo: add source code

Note that the best way to verify your configuration is correct is by using the OG UI in order to manually add this field to your structure via `http://localhost/ogdev/www/admin/config/group/fields`. If your configuration is correct (don't forget to clear cache...) you will see the new field in the fields list.
@todo: add screenshot
To be certain, just add it manually to a vocabulary and see whether it is being added properly.
@todo: add screenshot

Third, we want to add an action saying "whenever new taxonomy vocabulary is created, add our cool field to it". To do so we need to hook into Drupal core bundle creation (thankfully, vocabulary is an entity, and each new vocabulary is a bundle...), and this is done via <a href="https://api.drupal.org/api/drupal/modules!field!field.api.php/function/hook_field_attach_create_bundle/7" target=_blank>hook_field_attach_create_bundle</a>. What you need to do is to create an implmenetation for this hook in your module, and there to use the nicely-structured OG API `og_create_field` with the name of the field you defined in the last step.
@todo: add source code

Yep, "this is it" ! 
Is it ?
It is, as long as you build new site. Considering you might handle an existing OG site, that might already have a bunch of taxonomy vocabularies, you must handle all of them as well, and the best way to do it is automatically via the installation or upgrade process.
~todo: when to use hook_install and when to use hook_update_n, put source code for batch API in update and point to another post reagarding to batch API.
