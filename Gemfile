source "https://rubygems.org"

# Ruby 3.x 호환성을 위해 상단에 배치
gem "bigdecimal", "~> 3.1"
gem "webrick", "~> 1.9"

# Windows 환경에서 파일 변화를 감지하기 위한 도구
gem 'wdm', '>= 0.1.0' if Gem.win_platform?

# Jekyll 플러그인 그룹
group :jekyll_plugins do
  gem 'jekyll-feed'
  gem 'jekyll-sitemap'
  gem 'jekyll-paginate'
  gem 'jekyll-seo-tag'
  gem 'jekyll-archives'
end

# 기타 라이브러리 (플러그인 그룹 밖으로 이동)
gem 'kramdown'
gem 'rouge'
gem 'ffi', '>= 1.15.0'
gem 'eventmachine', '1.2.7', platforms: [:ruby]