# frozen_string_literal: true

source "https://rubygems.org"

ruby "3.3.9"                # sesuaikan dengan versi rbenv yang kamu pakai (opsional tapi disarankan)

gem "bundler"

# Jika kamu ingin memastikan Jekyll terpasang secara eksplisit, uncomment baris berikut:
# gem "jekyll", "~> 4.4"

gem "jekyll-theme-chirpy", "~> 7.3", ">= 7.3.1"
gem "html-proofer", "~> 5.0", group: :test

platforms :mingw, :x64_mingw, :mswin, :jruby do
  gem "tzinfo", ">= 1", "< 3"
  gem "tzinfo-data"
end

gem "wdm", "~> 0.2.0", platforms: [:mingw, :x64_mingw, :mswin]