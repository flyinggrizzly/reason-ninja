require 'html-proofer'

# Deploy. Depends on the build and s3_push tasks
namespace :deploy do
  desc 'it deploys the site with current posts'
  task :current do
    Rake::Task['build'].invoke
    Rake::Task['s3_push'].invoke
  end

  desc 'it deploys the site with future posts published'
  task :future do
    Rake::Task['build:future'].invoke
    Rake::Task['s3_push'].invoke
  end
end
# Default task for :deploy namespace
task :deploy do
  Rake::Task['deploy:current'].invoke
end

### BUILD
namespace :build do
  desc 'it builds the site with current posts'
  task :current do
    sh 'bundle exec jekyll build'
  end

  desc 'it builds the site with future posts'
  task :future do
    sh 'bundle exec jekyll build --future'
  end
end
# Default task for the :build namespace
task :build do
  Rake::Task['build:current'].invoke
end

### S3_PUSH
desc 'it pushes the generated site to s3 and Cloudfront'
task :s3_push do
  sh "bundle exec s3_website push"
end

### HTML_PROOF
desc 'it checks validity of the generated HTML'
task :html_proof do
  sh "bundle exec jekyll build"
  options = { :assume_extension   => true,
              :http_status_ignore => [999],
              :internal_domains   => ['flyinggrizzly.io',
                                      'www.flyinggrizzly.io'] }
  HTMLProofer.check_directory("./_site", options).run
end
