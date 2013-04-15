# Capistrao defaults
load 'deploy'

require 'capistrano/ext/multistage'
require 'capistrano/chocopoche'

# Base configuration
set :application,   "short_url"
set :repository,    "git@github.com:chocopoche/short_url.git"
set :use_sudo,      false
ssh_options[:forward_agent] = true

# Folders to rsync with files:download
set :files_rsync,    files_rsync    + %w(web/qr)

# Symlinks to create after deploy:update_code
set :files_symlinks, files_symlinks + %w(web/qr)

# # Won't work with the cli command `csync` because the default stage task
# # will be invoked, but it should not
# set :default_stage,  'vm'

# Files to be generated on setup
set :files_tpl, [
  {
    :template => "config/deploy/templates/nginx.conf.erb",
    :dest     => "config/nginx.conf"
  },
  {
    :template => "config/deploy/templates/parameters.yml.erb",
    :dest     => "config/parameters.yml"
  }
]

set :tld, "tmb.io"

# Generate the api doc after deployment if the script exists
after 'deploy', :roles => :app do
  run "cd #{current_path} && test -f vendor/bin/sami.php && vendor/bin/sami.php update config/sami.php"
end
