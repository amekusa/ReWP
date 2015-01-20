# encoding: utf-8
# vim: ft=ruby expandtab shiftwidth=2 tabstop=2

Vagrant.require_version ">= 1.5"


#
# Configuration for the WordPress
#

VM_BOX               = "miya0001/vccw" # pre-installed box

DOCUMENT_ROOT        = "/var/www/wordpress" # path to your site's document-root (relative from Vagrantfile)

WP_VERSION           = ENV["wp_version"] || 'latest' # latest or 3.4 or later or http(s):// URL to zipfile
WP_LANG              = ENV["wp_lang"] || "en_US" # WordPress locale (e.g. ja)

WP_HOSTNAME          = "wordpress.local" # e.g example.com
WP_IP                = "192.168.33.10" # host ip address

WP_HOME              = "" # path to WP_HOME, e.g blank or /wp or ...
WP_SITEURL           = "" # path to WP_SITEURL, e.g or /wp or ...

WP_TITLE             = "Welcome to the Vagrant" # title
WP_ADMIN_USER        = "admin" # default user
WP_ADMIN_PASS        = "admin" # default user's password

WP_DB_PREFIX         = 'wp_' # Database prefix
WP_DB_HOST           = 'localhost' # or WP_IP and other
WP_DB_NAME           = 'wordpress'
WP_DB_USER           = 'wordpress'
WP_DB_PASS           = 'wordpress'

WP_DB_ROOT_PASS      = 'wordpress'

WP_DEFAULT_PLUGINS   = %w(dynamic-hostname wp-total-hacks tinymce-templates)
WP_DEFAULT_THEME     = ENV["wp_theme"] || '' # e.g. twentyfifteen
WP_OPTIONS           = {}
WP_REWRITE_STRUCTURE = '/archives/%post_id%'

WP_DIR               = '' # e.g. /wp or wp or other
WP_IS_MULTISITE      = false # enable multisite when true
WP_FORCE_SSL_ADMIN   = false # enable force ssl admin when true
WP_DEBUG             = true # enable debug mode
WP_SAVEQUERIES       = false # save the database queries to an array
WP_THEME_UNIT_TEST   = false # automatic import theme unit test data

WP_ALWAYS_RESET      = true # always reset database

WP_CHEF_COOKBOOKS_PATH = File.join(File.dirname(__FILE__), 'provision') # path to the cookbooks (e.g. ~/vccw)

if WP_LANG === 'ja' then
  WP_THEME_UNIT_TEST_DATA_URI = 'https://raw.githubusercontent.com/jawordpressorg/theme-test-data-ja/master/wordpress-theme-test-date-ja.xml'
else
  WP_THEME_UNIT_TEST_DATA_URI = 'https://wpcom-themes.svn.automattic.com/demo/theme-unit-test-data.xml'
end

# end configuration

Vagrant.configure(2) do |config|

  config.vm.box = VM_BOX
  #config.ssh.forward_agent = true

  config.vm.hostname = WP_HOSTNAME
  config.vm.network :private_network, ip: WP_IP

  config.vm.synced_folder "www/wordpress/", DOCUMENT_ROOT, :create => "true"

  if Vagrant.has_plugin?("vagrant-hostsupdater")
    config.hostsupdater.remove_on_suspend = true
  end

  config.vm.provider :virtualbox do |vb|
    vb.customize [
      "modifyvm", :id,
      "--natdnsproxy1", "on",
      "--natdnshostresolver1", "on"
    ]
  end

  config.vm.provision :chef_solo do |chef|

    chef.cookbooks_path = [
      File.join(WP_CHEF_COOKBOOKS_PATH, "cookbooks"),
      File.join(WP_CHEF_COOKBOOKS_PATH, "site-cookbooks")
    ]

    chef.json = {
      :apache => {
        :docroot_dir  => DOCUMENT_ROOT,
        :user         => 'vagrant',
        :group        => 'vagrant',
        :listen_ports => ["80", "443"]
      },
      :php => {
        :packages => %w(php php-cli php-devel php-mbstring php-gd php-xml php-mysql),
        :directives => {
            "default_charset"            => "UTF-8",
            "mbstring.language"          => "neutral",
            "mbstring.internal_encoding" => "UTF-8",
            "date.timezone"              => "UTC",
            "short_open_tag"             => "Off",
            "session.save_path"          => "/tmp"
        }
      },
      :mysql => {
        :bind_address           => "0.0.0.0",
        :server_debian_password => WP_DB_ROOT_PASS,
        :server_root_password   => WP_DB_ROOT_PASS,
        :server_repl_password   => WP_DB_ROOT_PASS
      },
      "wpcli" => {
        :wp_version        => WP_VERSION,
        :wp_host           => WP_HOSTNAME,
        :wp_home           => WP_HOME,
        :wp_siteurl        => WP_SITEURL,
        :wp_docroot        => DOCUMENT_ROOT,
        :locale            => WP_LANG,
        :admin_user        => WP_ADMIN_USER,
        :admin_password    => WP_ADMIN_PASS,
        :default_plugins   => WP_DEFAULT_PLUGINS,
        :default_theme     => WP_DEFAULT_THEME,
        :title             => WP_TITLE,
        :is_multisite      => WP_IS_MULTISITE,
        :force_ssl_admin   => WP_FORCE_SSL_ADMIN,
        :debug_mode        => WP_DEBUG,
        :savequeries       => WP_SAVEQUERIES,
        :theme_unit_test   => WP_THEME_UNIT_TEST,
        :theme_unit_test_data_url => WP_THEME_UNIT_TEST_DATA_URI,
        :gitignore         => File.join(DOCUMENT_ROOT, ".gitignore"),
        :always_reset      => WP_ALWAYS_RESET,
        :dbhost            => WP_DB_HOST,
        :dbname            => WP_DB_NAME,
        :dbuser            => WP_DB_USER,
        :dbpassword        => WP_DB_PASS,
        :dbprefix          => WP_DB_PREFIX,
        :options           => WP_OPTIONS,
        :rewrite_structure => WP_REWRITE_STRUCTURE
      },
      :vccw => {
        :wordmove => {
          :movefile        => File.join('/vagrant', "Movefile"),
          :url             => "http://" << File.join(WP_HOSTNAME, WP_DIR),
          :wpdir           => File.join(DOCUMENT_ROOT, WP_DIR),
          :dbhost          => WP_DB_HOST,
          :dbname          => WP_DB_NAME,
          :dbuser          => WP_DB_USER,
          :dbpassword      => WP_DB_PASS
        }
      },
      :rbenv => {
        "rubies"  => ['2.1.2'],
        "global"  => '2.1.2',
        "gems"    => {
          "2.1.2" => [
            {
              name: "bundler",
              options: "--no-ri --no-rdoc"
            },
            {
              name: "sass",
              options: "--no-ri --no-rdoc"
            },
            {
              name: "wordmove",
              options: "--no-ri --no-rdoc"
            }
          ]
        }
      }
    }

    chef.add_recipe "yum::remi"
    chef.add_recipe "iptables"
    chef.add_recipe "apache2"
    chef.add_recipe "apache2::mod_php5"
    chef.add_recipe "apache2::mod_ssl"
    chef.add_recipe "mysql::server"
    chef.add_recipe "mysql::ruby"
    chef.add_recipe "php::package"
    chef.add_recipe "wpcli"
    chef.add_recipe "wpcli::install"
    chef.add_recipe "vccw"

  end

end
