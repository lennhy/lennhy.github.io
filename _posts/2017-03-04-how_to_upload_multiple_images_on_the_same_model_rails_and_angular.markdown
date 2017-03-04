---
layout: post
title:  "How to upload multiple images on the same model Rails & Angular"
date:   2017-03-04 20:34:13 +0000
---


In my earlier blog [How to upload single images with Rails, Angular js, Paperclip & Base64](http://codelenn.com/2017/01/30/how_to_upload_images_with_rails_angular_js_saga_part_1/) I explained how to upload single images. To continue where was left off I will now show you how to upload several images for a single model in Angular Js and Rails. 

Just to recap from my earlier blog post the tools we need are:

1. Paperclip ruby gem 
2. Angular-Base64 module
3. ng-file-upload module

For instllations of these tools see [How to upload single images with Rails, Angular js, Paperclip & Base64](http://codelenn.com/2017/01/30/how_to_upload_images_with_rails_angular_js_saga_part_1/) for details.

The key here is to remember that the difference between uploading one image vs serveral images is to create a many to one relationship where in my case a comic book model has many pages while a page model has one image attachment. 

Step 1
First we build a page model that has a belongs to relationship with the comic model. This page model will contain all the images for the comic model.

```
class Page < ApplicationRecord
  belongs_to :comic
  # validates :pages, presence: true

  has_attached_file :image, :styles => {large: "1000x1000>", medium: "300x300>", thumb: "150x150#" },
                            :default_style => :thumb, :default_url=> "/images/:style/cover.png", preserve_files: false

  validates_attachment_content_type :image, content_type: /\Aimage\/.*\z/

end

```

In the comic model I create a has many relationship with the page model:

```
class Comic < ApplicationRecord
  include Decoder::InstanceMethods
  has_many :pages
```

In the controller I create an object that will contain all the data from the form input. 

```
  vm.book = {
         title: '',
         description: '',
         issue:'',
         volume:'',
         page_count:'',
         issue_date:'',
         graphic_novel:'',
         region_id: null,
         cover: {},
         genre_ids: [],
         pages: []
    };
```

The key values are empty. 

In the form: 
```
  <!-- mulitple upload -->
  <label>Upload Pages all pages must be numbered in chronological order</label>
  <ul ng-repeat="image in vm.upload">
    <div id="preview_pages"></div>
    <img ng-src="{{image.url}}">
  </ul>
  <input type="file" ng-model="vm.book.pages"  id="pages"  accept="image/*" multiple base-sixty-four-input>

  <input type="submit" value="create"/>
```

The important part here is the *ng-model="vm.book.pages"*
This input will receive the data from the files we choose to upoad and put it into the pages key value in the vm.book model. 

Next we use our book service to send the data object to the rails backend api just like we did in my [previous blog post](http://codelenn.com/2017/01/30/how_to_upload_images_with_rails_angular_js_saga_part_1/) for single image attachments.

In the comic controller we make a provate method that accepts the whitelisted params 

 ```
 def comic_params
     params.require(:comic).permit(
                :title,
                :description,
                :issue,
                :volume,
                :page_count,
                :issue_date,
                :graphic_novel,
                :region_id,
                :cover,
                :genre_ids => [],
                :pages=>[]
        )
    end
```

Only this time we include the pages array as a parameter. This contains the file objects collected from the form.

Next we create a decoder module using the base64 module to encode the data. You can easily do this inside the model or controller but we are using seperation of concerns as well as good practice for organizing our code. 

File: controllers/concerns/decoder.rb

```
module Decoder

  module InstanceMethods

    def decode_base64(image)
      Rails.logger.info 'decoding base64 file'
      # decode base64 string
      decoded_data = Base64.decode64( image[:base64])
      # create 'file' understandable by Paperclip
      data = StringIO.new(decoded_data)
      data.class_eval do
        attr_accessor :content_type, :original_filename
      end

      # set file properties
      data.content_type = image[:filetype] ##<StringIO:0x007f9be40a1d88>
      data.original_filename = image[:filename]

      # return data to be used as the attachment file (paperclip)
      data ##<StringIO:0x007fc91718b590 @content_type="image/jpeg", @original_filename="lenn.jpg">
    end

  end

end
```

Here we see that the image which is really the pages array fromt the whitelisted params is being accepted as an argument. Then decoded into a string by the base 64 module. Note: base64 allows you to embed images right in your HTML, CSS, or JavaScript.  We then use the StringIO so that the paperclip gem can properly interpret it. Then we add the two attributes that come with paperclip gem with the ruby .class_eval method.

Because we want to create a one to many realtionship we need to create a new object for each file image that we get from the pages params. So in the comic model we create a method that takes in the pages as pages_attributes and  creates a new  instance of the Page object. We use the decoder module method decode_base64 that we created. Then we save into the comic.pages attribute so now our current comic has several new pages. See below:

```
 def pages_attributes(pages_attributes)
      pages_attributes.each do |image|
        if image.blank?

        else
          new_page = Page.new
          new_page.image  = decode_base64(image) # save resource and render response ...
          self.pages << new_page
        end
      end
    end
```

Now in our comics controller we can see the above method and the module method put into action. 
```
  def create
    comic = Comic.new(comic_params)
    # save resource and render response
    comic.cover = comic.decode_base64(params[:comic][:cover])
    comic.pages_attributes(params[:comic][:pages])

    comic.users << current_user
    if comic.save
      render json: { message:'you have successfully created a new comic', status: 'ok'}, notice: "You successfully created a new Comic!"
    else
      render json: {errors: comic.errors.full_messages}, status: :unprocessable_entity
    end
  end
```
To display the images we simply write an ng repeat in the html.
```
<div ng-repeat="page in vm.book.pages">
  <span><img ng-src='{{page.image}}'/></span>
</div>
```
