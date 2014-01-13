Active Admin Multi-Upload
========

### Support for multiple image upload for a nested resource in ActiveAdmin

_This gem was built to work with Carrierwave & ActiveAdmin, and is based on the excellent jquery-fileupload-system._

ActiveAdminMultiUpload is a version of jquery-fileupload-ui built to work with nested objects and ActiveAdmin 1.0. It supports all major features including file-previews and progress bars, and has been built so as to have the simplest implentation possible. It has been tested with Rails 4.0.2 and Ruby 2.1.0

**Please Note: Version 1.0.0 of this software only includes support for nested associations (eg. `Gallery.pictures`) and not standalone uploads. Feature will come in a future release.**

Getting Started
========

Carrierwave
--------

This gem relies on [Carrierwave](https://github.com/carrierwaveuploader/carrierwave) to work. If you are unfamiliar with Carrierwave then I recommend have a look through it before using this gem. You can also check out [This Railscast](http://railscasts.com/episodes/253-carrierwave-file-uploads) by Ryan Bates to help you get started.

Installation
--------

Add the gem to your Gemfile

`gem "active_admin_multi_upload"`

Run `bundle install`.

Run the generator on the Model with the Uplaoder mounted on it

`rails g active_admin_multi_upload:resource YourModel`



Setup
--------

For the sake of simplification for all examples below I am going to use the sample data.

* `Picture` will be the model that has an uploader associated with its `:image` attribute
* `Gallery` has many `:pictures`

You can also assume the `@gallery` refers to the current gallery that we are creating/editing.

**Substitute these with your own models and associations**

### Run the generator

### Allow the Params

In `/admin/gallery.rb` add `permit_params picture_ids: []`

### Add the uploader

Within your form render the `active_admin_multi_upload/upload_form`

    <%= f.inputs "Pictures" do %>
      <%= render "active_admin_multi_upload/upload_form", :resource => @gallery, :association => "pictures", attribute: "image" , options: {} %>
    <% end %>

Options
---------

At the moment the following options can be passed in to the above partial:

* `:existing_uploads` - *String*. Used to define the existing association, in this case `@gallery.pictures`.  This defaults to `:resource.:association` so is normally not required.
* `:input_id_prefix` - *String*. Used to dynamically set the id prefix of the inputs where files are uploaded. If left blank the default will include the name of the `:resource`, the `:association`, and the `id` of the file that is uploaded. eg. `gallery_picture_ids_9`
* `:input_name` - *String*. Used to set the name of the file input. Usually set dynamically. In the example above would default to `"picture[image]"`.
* `:post_url` - *String*. Used to set a custom path a `create` action. Defaults to one set dynamically in the gem, and can cause problems if changed, so handle with care.
* `:uploaded_ids_form_input_name` - *String*. Used for nested attributes. Defaults to `"#{resource.class.name.downcase}[#{association.singularize}_ids][]"`

All of these are optional, and sensible defaults are in place so that they should not be required. However they have been included for your customization.



This project rocks and uses MIT-LICENSE.