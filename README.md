### PaperClip
---

https://github.com/thoughtbot/paperclip

thoughtbot, inc.
factory_bot, administrate, bourbon, paperclip,...

```ruby
Paperclip.options[:command_path] = "/usr/local/bin/"

gem "paperclip", "~> 6.0.0"
gem "paperclip", git: "git://github.com/thoughtbot/paperclip.git"

class MoudleName < ActiveRecord::Base
  include Paperclip::Glue
end

class User < ActiveRecord::Base
  has_attached_file :avatar, styles: { medium: "300x300>", thumb: "100x100>" }, default_url: "/images/:style/missing.png"
  validates_attachment_content_type :avatar, content_type: /Aimage\/.*\z/
end

class AddAvatarColumnsToUsers < ActiveRecord::Migration
  def up
    add_attachment :users, :avatar
  end
  def down
    remove_attachment :users, :avatar
  end
end

def create
  @user = User.create(user_params)
end
private
def user_params
  params.require(:user).permit(:avatar)
end

@user.avatar = nil
@user.save

validates :avatar, attachment_presence: true
validates_with AttachmentPresenceValidator, attributes: :avatar
validates_with AttachmentSizeValidator, attributes: :avatar, less_than: 1.megabytes

validates_attachment_presence :avatar

validates_attachment :avatar, presence: true,
  content_type: "image/jpeg",
  size: { in: 0..10.kilobytes }

class Message < ActiveRecord::Base
  has_attachment_file :asset, styles: { thumb: "100x100#" }
  before_post_process :skip_for_audio
  def skip_for_audio
    ! %w(audio/ogg application/ogg).include?(asset_content_type)
  end
end


class Book < ActiveRecord::Base
  has_attached_file :document, styles: { thumbnail: "#60x60#" }
  validates_attachment :document, content_type: "application/pdf"
  validates_something_else 
end
class BooksController < ApplicationController
  def create
    @book = Book.new(book_params)
    @book.document = params[:book][:document]
    @book.save
    respond_with @book
  end
  private
  def book_params
    params.require(:book).permit(:title, :author)
  end
end

validates_atachment :avatar,
  content_type: ["image/jpeg", "image/gif", "image/png"]

class ActiveRecord::Base
  has_attached_file :avatar
  validates_attachment_content_type :avatar, content_type: /\Aimage/
  validates_attachment_file_name :avatar, matches: [/png\z/, /jpe?g\z/]
  do_not_validate_attachment_file_type :avatar
end

Paperclip.options[:content_type_mappings] = {
  foo: %w(text/plain)
}

module YourApp
  class Application < Rails::Application
    config.paperclip_defaults = { storage: :fog, fog_credentials: { provider: "Local", local_root: "#{Rails.root}/public"}, fog_directory: "", fog_host: "localhost"}
  end
end

Paperclip::Attachment.default_options[:storage] = :fog
Paperclip::Attachment.default_options[:fog_credentials] = { proveder: "Local", local_root: "#{Rails.root}/public"}
Paperclip::Attachment.default_options[:fog_directory] = ""
Paperclip::Attachment.default_options[:fog_host] = "http://localhost:3000"


class CreateUsersWithAttachments < ActiveRecord::Migration
  def up
    create_table :users do |t|
      t.attachment :avatar
    end
  end
  def down
    drop_table :users
  end
end

class CreateUsersWithAttachments < ActiveRecord::Migration
  def change
    create_table :users do |t|
      t.attachment :avatar
    end
  end
end

class AddAttachmentColumnsToUsers < ActiveRecord::Migration
  def up
    add_attachment :users, :avatar
  end
  def down
    remove_attachment :users, :avatar
  end
end

class AddAttachmentColumnsToUsers < ActiveRecord::Migration
  def change
    add_attachement :users, :avatar
  end
end

/data/myapp/releases/20081229172410/public/system/users/avatar/000/000/013/small/my_pic.png
gem 'aws-sdk-s3'

Paperclip::DataUriAdapter.register

has_attached_file :avatar, styles: { thumb: ["32x32#", :png] }

has_attached_file :image, styles: { regular: ['800x800', :png]}
  source_file_options: { regular: "-density 96 -depth 8 -quality 85"},
  convert_options: { regular: "-posterrize 3"}

has_attached_file :scan, styles: { text: { quality: :better } },
  processords: [:ocr]

has_attached_file :scan, styles: { text: { quality: :better } },
  processors: [:rotator, :ocr]
 
class Message < ActiveRecord::Base
  has_attached_file :asset, styles: { thumb: "100x100#" }
  before_post_process :skip_for_audio
  def skip_for_audio
    ! %w(audio/ogg application/ogg).include?(asset_content_type)
  end
end


has_attached_file :avatar, {
  url: "/system/:hash.:extension",
  hash_secret: "longSecretString"
}

class AddAvatarFingerprintColumnToUser < ActiveRecord::Migration
  def up
    add_column :users, :avatar_fingerprint, :string
  end
  def down
    remove_column :users, :avatar_fingerprint
  end
end

has_attached_file :some_attachment, adapter_options: { hash_digest: Digest::SHA256 }

has_attached_file :some_attachemnt, {
  preserve_files: true,
}

class User < ActiveRecord::Base
  has_attached_file :avatar, styles: lambda { |attachment| { thumb: (attachment.instance.boss? ? "300x300>" : "100x100>") } }
end

class User < ActiveRecord::Base
  has_attached_file :avatar, processors: lambda { |instance| instance.prcesors }
  attr_accessor :processors
end

Your::Application.configure do
  Paperclip.options[:log] = false
end

set :linked_dirs, fetch(:linked_dirs, []).push('public/system')

Paperclip.registered_attachments_styles_path = '/'/tmp/cofig/paperclip_attachments.yml'

namespace :paperclip do
  desc "build missing paperclip styles"
  task :build_missing_style do
    on roles(:app) do
      within release_path do
        with rails_env: fetch(:rails_env) do
          execute :rake, "paperclip:refresh:missing_styles"
        end
      end
    end
  end
end
after("deploy:compile_assets", "paperclip:build_missing_styles")

class User < ActiveRecord::Base
  has_attached_file :avatar, styles: { thumb: 'x100', croppable: '600x600>', big: '1000x1000>' }
end
class Book < ActiveRecord::Base
  has_attached_file :cover, styles: { small: 'x100', large: '1000x1000>' }
  has_attached_file :sample, styles: { thumb: 'x100' }
end

if ENV['PARRALEL_TEST_GROUPS']
  Paperclip::Attachment.default_options[:path] = ":rails_root/public/system/:rails_env/#{ENV['TEST_ENV_NUMBER'].to_i}/:class/:attachment/:id_partition/:filename"
else
  Paperclip::Attachment.default_options[:path] = ":rails_root/public/system/:rails_env/:class/:attachement/:id_partition/:filename"
end

Paperclip::Attachment.default_options[:path] = "#{Rails.root}/spec/test_files/:class/:id_partition/:style.:extension"

config.after(:suite) do
  FileUtils.rm_rf(Dir["#{Rails.root}/spec/test_files/"])
end

FactoryBot.define do
  factory :user do
    avatar { File.new("#{Rails.root}/spec/support/fixtures/image.jpg") }
  end
end

```

```
brew install imagemagick
brew install gs
sudo apt-get install imagemagick -y

```

```
<%= form_for @user, url: users_path, html: { multipart: true } do |form| %>
  <%= form.file_field :avatar %>
  <%= form.submit %>
<% end %>

<%= simple_form_for @user, url: users_path do |form| %>
  <%= form.input :avatar, as: :file %>
  <%= form.submit %>
<% end %>

<%= image_tag @user.avatar.url %>
<%= image_tag @user.avatar.url(:medium) %>
<%= image_tag @user.avatar.url(:thumb) %>
```

```yml
# RAILS_ROOT/public/system/paperclip_attachment.yml
---
:User:
  :avatar:
  - :thumb
  - :croppable
  - :big
:Book:
  :cover:
  - :small
  - :large
  :sample:
  - :thumb
```
