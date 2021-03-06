# Build a Space X clone

brew install ffmpeg
![SuperxSchool](https://ws1.sinaimg.cn/large/006tNbRwgy1fuqdj0idotj31kw0s57l9.jpg)
![SuperxSchool](https://ws2.sinaimg.cn/large/006tNbRwgy1fuqdj01ujfj31kw0r41kx.jpg)

SESSION 1
```
Will use the Terminal and Atom as text editor
Build the basic functionality
Use Scaffold to easily create contriller，model，views，routes， and migration files。
Copy gemfiles from in the description below
Need ffmpeg and imagemagick installed for video audio and image processing
```
第一部分：本地构建一个新的专案
```
rails new RocketzCorp
cd RocketzCorp
git init
git add .
git commit -m "first commit"
```
---

第二部分：本地查看新的专案
```
atom .
rails server
http://localhost:3000/
```
---

第三部分：云端构建一个新的专案
```
git remote add origin https://github.com/shenzhoudance/RocketzCorp.git
git push -u origin master
```
---

第四部分：构建一个新的分支
```
git checkout -b xw-0827
rails g scaffold Mission title:string subtitle:string boxtitle:string boxinfo:text
rake db:migrate
rake routes
http://localhost:3000/missions
```

第五部分：修改 root 路由器
config/routes.rb
```
Rails.application.routes.draw do
  root "missions#index"
  resources :missions
end
```
http://localhost:3000/

第六部分：添加 Gemfile 路由器
```
gem 'jquery-rails', '~> 4.3', '>= 4.3.1'
gem 'bootstrap', '~> 4.0.0.beta'
gem 'sprockets-rails', '~> 3.2', '>= 3.2.1'

gem 'paperclip', '~> 6.0'
gem 'paperclip-av-transcoder'
gem 'aws-sdk-s3', '~> 1.9'
```

app/assets/stylesheets/application.scss
```
@import "bootstrap";
```

app/assets/javascripts/application.js
```
//= require jquery3
//= require popper
//= require bootstrap-sprockets
```
```
bundle install
rails g paperclip missions banner picone pictwo video audio
rake db:migrate
```

app/controllers/missions_controller.rb
```
def mission_params
  params.require(:mission).permit(:title, :subtitle, :boxtitle, :boxinfo
                                  :banner, :picone, :pictwo, :video, :audio)
end
```

app/models/mission.rb
```
class Mission < ApplicationRecord
# picone validation
has_attached_file :picone, styles: {large: "1500x500>", medium: "500x400>", thumb: "200x150#"}
validates_attachment_content_type :picone, content_type: /\Aimage\/.*\z/
end
```

第七部分：修改 view mission 样式
app/views/missions/-form.html.erb
```
<%= form_with(model: mission, local: true, html: {multipart: true}) do |form| %>
  <% if mission.errors.any? %>
    <div id="error_explanation">
      <h2><%= pluralize(mission.errors.count, "error") %> prohibited this mission from being saved:</h2>

      <ul>
      <% mission.errors.full_messages.each do |message| %>
        <li><%= message %></li>
      <% end %>
      </ul>
    </div>
  <% end %>

  <div class="field">
    <%= form.label :title %>
    <%= form.text_field :title, id: :mission_title %>
  </div>

  <div class="field">
    <%= form.label :subtitle %>
    <%= form.text_field :subtitle, id: :mission_subtitle %>
  </div>

  <div class="field">
    <%= form.label :boxtitle %>
    <%= form.text_field :boxtitle, id: :mission_boxtitle %>
  </div>

  <div class="field">
    <%= form.label :picone %>
    <%= form.file_field :picone %>
  </div>


  <div class="field">
    <%= form.label :boxinfo %>
    <%= form.text_area :boxinfo, id: :mission_boxinfo %>
  </div>

  <div class="actions">
    <%= form.submit %>
  </div>
<% end %>


```

app/views/missions/index.html.erb
```
<p id="notice"><%= notice %></p>

<h1>Missions</h1>

<table>
  <thead>
    <tr>
      <th>Title</th>
      <th>Subtitle</th>
      <th>Boxtitle</th>
      <th></th>
      <th>Boxinfo</th>
      <th colspan="3"></th>
    </tr>
  </thead>

  <tbody>
    <% @missions.each do |mission| %>
      <tr>
        <td><%= mission.title %></td>
        <td>
        <% if mission.picone.present? %>
        <%= image_tag mission.picone.url(:thumb), style:"width:100%;" %>
        <% else %>
        No Image
        <% end %>
        </td>
        <td><%= mission.subtitle %></td>
        <td><%= mission.boxtitle %></td>
        <td><%= mission.boxinfo %></td>
        <td><%= link_to 'Show', mission %></td>
        <td><%= link_to 'Edit', edit_mission_path(mission) %></td>
        <td><%= link_to 'Destroy', mission, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </tr>
    <% end %>
  </tbody>
</table>

<br>

<%= link_to 'New Mission', new_mission_path %>
```
app/views/missions/show.html.erb
```
<p id="notice"><%= notice %></p>

<p>
  <strong>Title:</strong>
  <%= @mission.title %>
</p>

<p>
  <strong>Subtitle:</strong>
  <%= @mission.subtitle %>
</p>

<p>
  <strong>Boxtitle:</strong>
  <% if @mission.picone.present? %>
  <%= image_tag @mission.picone.url(:medium) %>
  <% else %>
  No Image
  <% end %>
</p>

<p>
  <strong>Boxinfo:</strong>
  <%= @mission.boxinfo %>
</p>

<%= link_to 'Edit', edit_mission_path(@mission) %> |
<%= link_to 'Back', missions_path %>
```



#在第一单元的训练当中，我们主要完成了两个核心功能的制作：
>1、基本博客的功能体系
2、图片每户的功能体系

以上的两个功能是最基础的功能体系，后续的所有的功能体系，都是在这个功能体系的维度上不断的叠加的结果。

SESSION 1b
```
Creating and styling the boxes
Edit Bootstrap card properties
Add Navigation partial and Navigation bar
First real Git Commit
```

在第二个部分完成的基本首页的页面的美化以及完成导航条的引入；

https://www.bootstrapdash.com/bootstrap-4-tutorial/navbar/

app/views/shared/-navigation.html.erb
```
<nav class="navbar navbar-expand-md navbar-dark fixed-top bg-dark">
  <button class="navbar-toggler navbar-toggler-left" type="button" data-toggle="collapse" data-target="#navbarNav" aria-controls="navbarNav" aria-expanded="false" aria-label="Toggle navigation">
    <span class="navbar-toggler-icon"></span>
  </button>
  <a class="navbar-brand" href="#">SuperxSchool</a>
  <div class="collapse navbar-collapse" id="navbarNav">
    <ul class="navbar-nav mr-auto mt-2 mt-md-0">
      <li class="nav-item active">
        <a class="nav-link" href="#">Home
          <span class="sr-only">(current)</span></a>
      </li>
      <li class="nav-item">
        <a class="nav-link" href="#">Link</a>
      </li>
      <li class="nav-item">
        <a class="nav-link disabled" href="#">Disabled</a>
      <li >
        <a class="nav-link dropdown-toggle" href="http://superxschool.com" id="navbarDropdownMenuLink" data-toggle="dropdown" aria-haspopup="true" aria-expanded="false">
          Dropdown link
        </a>
        <div class="dropdown-menu" aria-labelledby="navbarDropdownMenuLink">
          <a class="dropdown-item" href="#">Action 1</a>
          <a class="dropdown-item" href="#">Action 2</a>
          <a class="dropdown-item" href="#">Action 3</a>
        </div>
      </li>
    </li>
  </ul>
  <form class="form-inline">
    <input class="form-control mr-sm-2" type="text" placeholder="Search">
    <button class="btn btn-success my-2 my-sm-0" type="submit">Search</button>
  </form>
</div>
</nav>

```

app/views/layouts/application.html.erb
```
<!DOCTYPE html>
<html>
  <head>
    <title>RocketzCorp</title>
    <%= csrf_meta_tags %>

    <%= stylesheet_link_tag    'application', media: 'all', 'data-turbolinks-track': 'reload' %>
    <%= javascript_include_tag 'application', 'data-turbolinks-track': 'reload' %>
  </head>

  <body>
    <%= render 'shared/navigation' %>
    <%= yield %>
  </body>
</html>
```

app/assets/stylesheets/application.scss
```
@import "bootstrap";
@import "missions";

body{
 min-hright: 75rem;
 padding-top: 3.5rem;
}

.bg-dark
{
  background-color: #050505;
}
```

app/assets/stylesheets/missions.scss
```
.card
{
  -webkit-box-shadow: 0 1px 5px 0 rgba(0,0,0,0.30);
  -moz-box-shadow: 0 1px 5px 0 rgba(0,0,0,0.30);
  -ms-box-shadow:0 1px 5px 0 rgba(0,0,0,0.30);
  -o-box-shadow:0 1px 5px 0 rgba(0,0,0,0.30);
  box-shadow:0 1px 5px 0 rgba(0,0,0,0.30);
  padding:6%;
}


.card-title
{
  padding-top: 1rem;
  text-transform: uppercase;
}
```

针对于页面的修改，主要在 idnex、show、edit、new以及form的表单上完成样式的修改；
```
<section class="container">
  <div class="row">
    <div class="col-md-12">
    </div><!-- .col-md-12 -->
  </div><!-- .row -->

<div class="row">
  <% @missions.each do |mission| %>
  <div class="col-md-4">
    <div class="card">
      <div class="card-topper">
      <% if mission.picone.present? %>
      <%= image_tag mission.picone.url(:thumb), stle:"width:100%;" %>
      <% else %>
      No Image
      <% end %>
    </div><!-- .card-topper -->
    <div class="card-block">
        <h4 class="card-title"><%= mission.boxtitle %></h4>

        <td><%= link_to 'LEADL MORE', mission %></td>
        <td><%= link_to 'Edit', edit_mission_path(mission) %></td>
        <td><%= link_to 'Destroy', mission, method: :delete, data: { confirm: 'Are you sure?' } %></td>
      </div><!-- .card-block -->
    </div><!-- .card -->
    </div><!-- .col-md-4 -->

    <% end %>
</div><!-- .row -->



<%= link_to 'New Mission', new_mission_path %>
</section>
```
app/views/missions/show.html.erb
```
<section class="container">
  <div class="row">
    <div class="col-md-12">
      <p id="notice"><%= notice %></p>

<p>
  <strong>Title:</strong>
  <%= @mission.title %>
</p>

<p>
  <strong>Subtitle:</strong>
  <%= @mission.subtitle %>
</p>

<p>
  <strong>Boxtitle:</strong>
  <% if @mission.picone.present? %>
  <%= image_tag（@mission.picone.(:medium)) %>
  <% else %>
  No Image
  <% end %>
</p>

<p>
  <strong>Boxinfo:</strong>
  <%= @mission.boxinfo %>
</p>

<%= link_to 'Edit', edit_mission_path(@mission) %> |
<%= link_to 'Back', missions_path %>

</div><!-- .col-md-12 -->
</div><!-- .row -->
```

SESSION 2
Style the input form for the boxes
Add links in ruby ti the Navigation banner
Fun Astronomical Fact
Git Commit in the end

主要是做导航条的修改；


SESSION 3

Add footer with footer partial
bootstrap styling plus some custom
put in search and newsletter sign up forms
(will add ruby code next time)

app/views/shared/-footer.html.erb
```
<footer class="footer">
  <div class="container">
    <div class="row">
      <div class="col-lg-3">
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
      </div>
      <!-- .col-lg-3 -->

      <div class="col-lg-3">
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
      </div>
      <!-- .col-lg-3 -->

      <div class="col-lg-3">
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
      </div>
      <!-- .col-lg-3 -->

    </div>
    <!-- .row -->
    <div class="row top-margin bottom-margin">
      <div class="col-lg-6">
        <form class="form-inline mt-2 mt-md-0">
          <input class="form-control mr-sm-2" type="text" placeholder="sign up for latest news" aria-label="newsletter">
          <button class="btn btn-outline-info my-2 my-sm-0" type="submit">sunmit</button>
        </form>
      </div>  <!-- .col-lg-6 -->
      <div class="col-lg-6">
        <form class="form-inline mt-2 mt-md-0 floatr">
          <input class="form-control mr-sm-2 bg-dark" type="text" placeholder="sign up for latest news" aria-label="newsletter">
          <button class="btn btn-outline-info my-2 my-sm-0" type="submit">search</button>
        </form>
      </div> <!-- .col-lg-6 -->
       </div> <!-- row top-margin -->
    </div>  <!-- .container -->
    <section class="bg-dark">
      <div class="container">
        <div class="row">
          <div class="col-lg-6">
            <span color="#eeeeee">
              FOLLOW US : TWITTER | FACEBOOK | YOUTUBE
              <span></div>
              <!-- .col-lg-6 -->
              <div class="col-lg-6">
                <span color="#eeeeee">
                  2018 &copy SuperxSchool
                  <span></div>
                  <!-- .col-lg-6 -->

                </footer>

```

SESSION 4
set up search functionality
search：route，search template，controller，model；
changesearch form to ruby syntax
add custom counter for number of mission articles
matching the search input
complete an exercise for counter to next time

修改 root 路由器的变量
```
Rails.application.routes.draw do
  root "missions#index"
  resources :missions do
    collection do
      git :search #create a route for search!
    end
  end
end
```

app/views/shared/-footer.html.erb
```
<footer class="footer">
  <div class="container">
    <div class="row">
      <div class="col-lg-3">
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
      </div>
      <!-- .col-lg-3 -->

      <div class="col-lg-3">
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
      </div>
      <!-- .col-lg-3 -->

      <div class="col-lg-3">
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
        <h5>hello footer</h5>
      </div>
      <!-- .col-lg-3 -->

    </div>
    <!-- .row -->
    <div class="row top-margin bottom-margin">
      <div class="col-lg-6">
        <form class="form-inline mt-2 mt-md-0">
          <input class="form-control mr-sm-2" type="text" placeholder="sign up for latest news" aria-label="newsletter">
          <button class="btn btn-outline-info my-2 my-sm-0" type="submit">sunmit</button>
        </form>
      </div>  <!-- .col-lg-6 -->
      <div class="col-lg-6">
       <div class="form-control mr-2 mt-md-0 floatr">
         <% form_tag search_missions_path, method: :get do %>
         <% text_field_tag :search, nil, placeholder: "seacrch rocketz" %>
         <%= submit_tag "search", class:"btn btn-outline-info my-2 my-sm-0" %>
         <% end %>
       </div>

        <!--<form class="form-inline mt-2 mt-md-0 floatr">
          <input class="form-control mr-sm-2 bg-dark" type="text" placeholder="sign up for latest news" aria-label="newsletter">
          <button class="btn btn-outline-info my-2 my-sm-0" type="submit">search</button>
        </form> -->
      </div> <!-- .col-lg-6 -->
       </div> <!-- row top-margin -->
    </div>  <!-- .container -->
    <section class="bg-dark">
      <div class="container">
        <div class="row">
          <div class="col-lg-6">
            <span color="#eeeeee">
              FOLLOW US : TWITTER | FACEBOOK | YOUTUBE
              <span></div>
              <!-- .col-lg-6 -->
              <div class="col-lg-6">
                <span color="#eeeeee">
                  2018 &copy SuperxSchool
                  <span></div>
                  <!-- .col-lg-6 -->

                </footer>

```

session 4
set up serach functionality
search : route,search template,controller,model
change search  form to ruby syntax
add custom counter for number of mission articles matching the search input
complete an exercise for counter to next time

.count method
certain methods can give an intruder access to the database - better with custom code

config/routes.rb
```
Rails.application.routes.draw do
  root "missions#index"
  resources :missions do
    collection do
      get :search #create a route for search!
    end
  end
end
```

app/controllers/missions_controller.rb
```
class MissionsController < ApplicationController
  before_action :set_mission, only: [:show, :edit, :update, :destroy]

  # GET /missions
  # GET /missions.json
  def index
    @missions = Mission.all.order("created_at DESC".LIMIT(3))
  end

  # GET /missions/1
  # GET /missions/1.json
  def show
  end

  def search
    if params[:search].block?
      @mission = Mission.all.order("created_at DESC".LIMIT(20))
    else
     @mission = Mission.search(params)
    end
  end
  # GET /missions/new
  def new
    @mission = Mission.new
  end

  # GET /missions/1/edit
  def edit
  end

  # POST /missions
  # POST /missions.json
  def create
    @mission = Mission.new(mission_params)

    respond_to do |format|
      if @mission.save
        format.html { redirect_to @mission, notice: 'Mission was successfully created.' }
        format.json { render :show, status: :created, location: @mission }
      else
        format.html { render :new }
        format.json { render json: @mission.errors, status: :unprocessable_entity }
      end
    end
  end

  # PATCH/PUT /missions/1
  # PATCH/PUT /missions/1.json
  def update
    respond_to do |format|
      if @mission.update(mission_params)
        format.html { redirect_to @mission, notice: 'Mission was successfully updated.' }
        format.json { render :show, status: :ok, location: @mission }
      else
        format.html { render :edit }
        format.json { render json: @mission.errors, status: :unprocessable_entity }
      end
    end
  end

  # DELETE /missions/1
  # DELETE /missions/1.json
  def destroy
    @mission.destroy
    respond_to do |format|
      format.html { redirect_to missions_url, notice: 'Mission was successfully destroyed.' }
      format.json { head :no_content }
    end
  end

  private
    # Use callbacks to share common setup or constraints between actions.
    def set_mission
      @mission = Mission.find(params[:id])
    end

    # Never trust parameters from the scary internet, only allow the white list through.
    def mission_params
      params.require(:mission).permit(:title, :subtitle, :boxtitle, :boxinfo,
                                      :banner, :picone, :pictwo, :video, :audio)
    end
end

```
app/views/missions/search.html.erb
```
<section class="container">
  <div class="row">
    <div class="col"></div>
      <div class="col-md-10" style="margin-bottom:1rem;">
        <h2 style="margin-top:2rem;"><u>Your search result</u></h2>
        <table class="table table-hover table-striped table-responsive">
          <thead>

          </thead>

          <tbody>
            <% c = 0 %>
            <% @missions.each do |mission| %>
            <tr>
              <td>
                <% if mission.picone.present? %>
                <%= image_tag mission.picone.url(:thumb), stle:"width:100%;" %>
                <% else %>
                No Image
                <% end %>
              </td>
              <td><%= mission.boxtitle %></td>
              <td><%= truncate(mission.boxinfo, length:100) %></td>
              <td><%= link_to 'LEADL MORE', mission %></td>
            </tr>
            <% c = c + 1 %>
            <% end %>
            <h4> there are <%= c %> containing your search! </h4>
          </tbody>
        </table>

      </div><!-- .col-md-10 -->
      <div class="col"></div>
    </div><!-- row -->
</section>

```
app/models/mission.rb
```
class Mission < ApplicationRecord
# picone validation
has_attached_file :picone, styles: { large: "1500x500>", medium: "500x400>", thumb: "200x150#" }
validates_attachment_content_type :picone, content_type: /\Aimage\/.*\z/
end
#for searching
def self.search(params)
  mission =Mission.where("title LINK ? or subtitle LINK ? or boxtitle LINK ？ or boxinfo LINK ？"),
  "%#{params[:search]}%","%#{params[:search]}%","%#{params[:search]}%",
  "%#{params[:search]}%", ) if params[:search].present?
  missions
end
```

SESSION 5
Creating date for template app
needed to demonstrate and test functionality
modifying seeds.rb in the db folder
use rand（）medthod for randomly select an
image from an array
go through some errors

在商业的环境下，我们所需要思考的核心的问题，首先是产品的问题，其次就是运营的问题，最后就是公司的财务的问题，我们只要可以解决以上的三个维度的问题，我们就可以非常轻松的完成一家公司的运作。

>所以我们在打造互联网产品的时候，其实最关键的问题就是：
1、我们应该如何打造我们的产品，一个技术体系的问题；
2、我们应该如何完成产品的运营，一个销售体系的问题；
3、我们应该如何完成货币的分配，一个公司经营的问题；



#打造我们的功能体系
在这个单元框架里面，主要谈到了过于 list 的文档的使用，在这个过程中，
需要构架一个 list 然后 开始 协调 root 接下来在 model 里面看 rb ，然后修改我们的 controller，这个维度体系跑下来，一个功能的架构就完成了。
#添加我们的数据结构
然后完成的是数据的快速填充，使用 seed.rb 的玩法 加上
rake seed:db, 完成数据的全部的填充；
#完成云端的产品部署
所以在我们完成功能的打造的时候，其实都是使用这个操作的流程来完成，从而完成代码的推送和云端功能的迭代，在这个过程中，会产生大量的数据，需要使用云端的服务器来保存我们的数据，从而完成价值体系的交换。

在互联网的世界里面，其实数据就是货币，具备大量有价值的数据，就具备了流量，具备流量的产品就可以具备子简单的广告价值，从而完成产品的最简单的利润的获取。


session 6
last time : asked you to generate about 25 boxes
add custom pagination(no gem)
next and previous buttons
modify controller and view
add an extra seach form on missionlist page

在这个章节的核心在于优化 list 的检索的功能体系，属于一个不断的优化的过程。
回顾其中的核心的框架来说，其实在第一章节里面就基本上完成了整个框架模型的搭建，所以一般情况下如果想要快速的完成一个产品的开发，在经验非常足的情况下， 的确可以在一个星期的时间就上线第一版的原型，如果一个产品一直不可以完成快速的开发，只能说明我们在完成开发的时候，思考的太多余复杂，其实本身就是一个不对的过程。


session 7

continue with the custom pagination
display the page numer and total number of pages
use division and division-modulo
modulo is a divisin property - diplays the reminder
talk about interger and float （decimal） methods

a mathematical operator - related to division
the answer is the remainder of a divisin

ex: 6 divided by 3 = 2(6/3 = 2)
no remaider > modulo = 0

ex:5/2 =2.5 >
integer or decimal remainder:
integer:5 % 2 = 1
decimal(float): 5% 2.0 = 0.5
