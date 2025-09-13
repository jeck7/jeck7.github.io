source 'https://rubygems.org'

# Jekyll version
gem "jekyll", "~> 4.3.0"

# Theme
gem "jekyll-theme-architect", "~> 0.2.0"

# GitHub Pages compatibility
gem "github-pages", group: :jekyll_plugins

# Essential plugins
group :jekyll_plugins do
  gem "jekyll-feed", "~> 0.12"
  gem "jekyll-sitemap", "~> 1.4"
  gem "jekyll-seo-tag", "~> 2.8"
  gem "jekyll-gist", "~> 1.5"
  gem "jekyll-paginate", "~> 1.1"
  gem "jekyll-optional-front-matter", "~> 0.3"
  gem "jekyll-readme-index", "~> 0.3"
  gem "jekyll-default-layout", "~> 0.1"
  gem "jekyll-titles-from-headings", "~> 0.5"
  gem 'jekyll-admin', group: :jekyll_plugins
end

# Windows and JRuby compatibility
platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

# Performance-booster for watching directories on Windows
gem "wdm", "~> 0.1.1", :platforms => [:mingw, :x64_mingw, :mswin]

# Lock `http_parser.rb` gem to `v0.6.x` on JRuby builds
gem "http_parser.rb", "~> 0.6.0", :platforms => [:jruby]

gemspec
