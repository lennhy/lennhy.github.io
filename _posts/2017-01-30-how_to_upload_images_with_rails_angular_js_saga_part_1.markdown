---
layout: post
title:  "How to upload single images with Rails, Angular js, Paperclip & Base64"
date:   2017-01-30 16:56:58 -0500
---

I have a comic app that I want to accept image uploads for the publishers, users and comic book pages. After several attempts to upload images with rails and angular I realized how complicated this process could be given the specific toolbox. To start there were not many tutorials availale on the specific subject. In addition, the tutorials were limited leaving you to, well figure out the rest. 

With enough research I found 3 helpful methods to upload and persist a single image or set of images to the database. Although a seperate server would have been the more ideal place to store images I started with the database first. The three methods are with [HTML5 file API](https://developer.mozilla.org/en-US/docs/Using_files_from_web_applications), [ng-file-upload](https://github.com/danialfarid/ng-file-upload) module and the [angular-base64-upload](https://www.npmjs.com/package/angular-base64-upload) module. 

At first I wanted to use the HTML5 file API but I didn't want to have to write all that extra logic to manage the images so instead I used a gem. Paperclip is a rails gem that makes it easier to upload and manage files. it has a has_attached_file attribute that provides useful attributes like file size of the image, it's content type etc...

When I set up the paperclip attachments inside of the comic model, I was able to upload a single attachment. So far most of the tutorials are for single file upload not multiple. The reason being that paperlclip doesn't do mutiple attachments for one file. 

After doing more research I found that I needed to add another model that has a belongs_to relationship to the main model. The model with the belongs_to relationship would contain the images but more on that in part 2 of the saga. 
Read more at [pluralsight](https://www.pluralsight.com/guides/ruby-ruby-on-rails/handling-file-upload-using-ruby-on-rails-5-api#fix7rj1eqCBOImxm.99) 

**For single file upload we will start with the following tools.**
Tools
1. Paperclip 
2. Use ng-upload
3. Use base64 

**Installation instructions for paperclip**

Install [ImageMagick](http://www.imagemagick.org/script/index.php)

On Linux, use apt-get:
sudo apt-get install imagemagick -y
On Windows download it [here](https://www.imagemagick.org/script/download.php#windows)


Paperclip
Add gem "paperclip" to your gem file
bundle install

Add the below attribute to the main model comic model
*Comic Model*

  ```
has_attached_file :cover, :styles => {large: "1000x1000>", medium: "300x300>", thumb: "150x150#" },
                            :default_style => :thumb, :default_url=> "/images/:style/cover.png"

  validates_attachment_content_type :cover, content_type: /\Aimage\/.*\z/
```


Paperclip will wrap up to four attributes (all prefixed with that attachment's name, so you can have multiple attachments per model). These attributes are:

cover_file_name
cover_file_size
cover_content_type
cover_updated_at

Run rails g paperclip comic cover.
FYI Comic is the main model where the single image attachment (*cover*) will be stored. *Cover* is the paperclip attachment that will hold the single image.

This will add a migration file with two migrations: add_attachment and remove_attachment. 

run rake db:migrate

Add cover to your JSON comic serializer if you have one.
*ComicSerializer.rb*


```
class ComicSerializer < ActiveModel::Serializer
  attributes :id, :title, :description, :issue, :volume, :page_count, :issue_date, :graphic_novel, :region,  :users,  :genres, :created_at, :cover
  has_many :users, serializer: UserSerializer
  has_many :genres, serializer: GenreSerializer
  has_many :ratings, serializer: RatingSerializer
  has_one :region
end
```


**Make sure to permit your params with strong params**
*ComicController.rb*


```
 def author_params
    params.require(:author).permit(:bio, :name, :avatar)
  end
```

**install ng-file-upload module**

with npm the command is : npm install ng-file-upload
with bower the command is bower install ng-file-upload-shim --save(for non html5 suppport) and 
bower install ng-file-upload --save
the shim part is for browsers that are not compatible with the file api


**App.js include the ngFileUpload module**

*App.js*

```
.module('app',['ui.router', 'templates', 'ngMessages', 'Devise','ngRoute', 'ngFileUpload'])
```


**In your ComicController.js include 'Upload'**

*ComicController.js*

```
function NewBookController(BookService, regions, genres, $scope, Upload, $http) {
```


**Create a JSON object with the keys for the comic model and empty values**

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
       genre_ids: [],
       cover: {}
  };
```
	

All the keys will be filled with data in the corresponding form fields. Note cover has an empty hash to hold the relevant attachment attributes. 

**Below is the input for the attachment  'cover'**

  ```
<label>Upload a Cover Page</label>
  <img ngf-thumbnail="cover"/>
  <div class="button" ngf-select ng-model="cover" name="cover" ngf-pattern="'image/*'"
  ngf-accept="'image/*'" ngf-max-size="20MB" ngf-min-height="100">Select</div><br>
```


The ng-model is the part where the image will be upload via the ng-file-upload angular module.

The ngf-thumbnail-part will display a preview of the image after it has been submitted. 

Finally we create an http.post service to send the image to the rails api which will be passed via the rails route 

*routes.rb*

`post   '/comics', to: 'comics#post'`
 
*ComicController.js*

```
vm.createBook = function() {
     BookService
       //  before submit form
       .httpCreateBook(vm.book)
         .then(function (res) {
           var arr=[];
              for(let i=0; i < res.data.comic.pages.length; i++){
                arr.push(res.data.comic.pages[i].url);
              };
              vm.upload = arr;
              console.log(res);
              $('ul').prepend("<li>You have successfully created a new comic!</li>");
         },function(error){
               console.log(error)
               $('ul').append("<li>Looks like You are are missing something!</li>");
         })
   }
}
```

After the form is submitted the data from the form fields is passed into the vm.book object and sent to the rails api via the http.post post service above.

Here is the rest of the post service in the serivce.js file

*Service.js* 


```
this.httpCreateBook = function(data) {
    var req = {
     method: 'POST',
     url: '/comics',
     data: {comic:data},
     headers: {
       'Content-Type': 'application/json'
     }
    }
    return $http(req)
    .then(successCallback)
    .catch(errorCallback)

    function successCallback(data){
      console.log(data)
    }

    function errorCallback(error){
      console.log(error)
    }
}
```



Back in the rails ComicController
*ComicController.rb*

The create action 

```
def create
    binding.pry
      comic = Comic.new(comic_params)
      comic.users << current_user
      if comic.save
        render json: { message:'you have successfully created a new comic', status: 'ok'}, notice: "You successfully created a new Comic!"
      else
        render json: {errors: comic.errors.full_messages}, status: :unprocessable_entity
      end
  end
```


**Now in the view you can display the image with the .cover attribute or .cover.url attribute inside of ng-src **


```
<img ngf-thumbnail="file">
<img ng-src="{{ vm.cover }}"/>
```


In the next blog *How to upload multiple images* i will explain how to incorporate the base64, why we use it and how to upload multiple images. 
