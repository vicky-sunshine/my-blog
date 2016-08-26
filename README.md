# Rails Girls Notes
## Environment
install homebrew
```sh
$ xcode-select --install
$ ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```
install rvm
```sh
$ brew install autoconf automake libtool libyaml readline libksba openssl
$ brew install rvm
```
利用 rvm 裝 ruby
```sh
$ rvm install ruby-head
$ rvm use ruby-head
```
裝 bundler 和 rake
```sh
$ gem install bundler
$ gem install rake
$ gem install rails --no-ri --no-rdoc
```

## Ruby Basic
可以去 [TryRuby](http://tryruby.org/) 或 [codecademy](https://www.codecademy.com/learn/ruby)


## Rails - Build a Blog!
參考資料

[Rails Girl - 第一個 app](http://railsgirls.tw/app)

[Rails Girl - App 上線至 Heroku](http://railsgirls.tw/heroku)
### Step1. 創新專案
```
rails new blog
cd blog
bundle install
rails server
```

### Step2. 新增文章
```
rails generate scaffold title:string content:text picture:string
# title 文章標題 string 字串
# content 文章內容 text 一大段的文字
# picture 文章圖片 string 之後會用其它套件讓它支援圖片
```
```rails
rake db:migrate # 在 database 裡創表格
rails server # 把網站跑起來
```

### Step3. 加入圖片
#### 加入套件
在 `Gemfile` 裡面加入
```
gem 'carrierwave'
```
在終端裡，執行：
```
bundle install
```

#### 處理上傳圖片的相關程式
```
rails generate uploader Picture
```

打開 `app/models/article.rb` 並在這行下面
```ruby
class Article < ActiveRecord::Base
```
加入
```ruby
  mount_uploader :picture, PictureUploader
```
打開 `app/views/articles/_form.html.erb` 並將這行
```erb
<%= f.text_field :picture %>
```
改成
```erb
<%= f.file_field :picture %>
```
可以再次開啟 `rails server` 看看

#### 顯示圖片
打開 `app/views/articles/show.html.erb` 並將這兩行
```erb
<strong>Picture:</strong>
<%= @idea.picture %>
```
改成
```erb
<%= image_tag(@article.picture_url, width: 600) if @article.picture.present? %>
```


