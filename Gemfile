# frozen_string_literal: true

source "https://rubygems.org"

gemspec

gem "bootstrap", "~> 5.3.0" # ✅ Added Bootstrap gem
gem "html-proofer", "~> 5.0", group: :test
gem "jekyll-seo-tag", "~> 2.8" # ✅ Added SEO plugin to fix Liquid error
gem "jekyll-include-cache" # ✅ Added to support include_cached tag

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", :platforms => [:mingw, :x64_mingw, :mswin]
