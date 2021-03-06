desc 'Build Compass output'
task :compass => [:'compass:clean'] do
  `bundle exec compass compile -c config/compass.rb`
end

desc 'Clean everything'
task :clean => [:'compass:clean', :'docs:clean'] do
  puts 'Cleaned.'
end

namespace :compass do
  desc 'Clean Compass output'
  task :clean do
    `rm -rf public/stylesheets/*`
  end
  
  desc 'Start Compass watching the stylesheets directory'
  task :watch => [:'compass:clean'] do
    `bundle exec compass watch -c config/compass.rb`
  end
end

desc 'Start local server'
task :server  => [:compass] do
  `bundle exec shotgun config.ru`
end

desc 'Import docs. Requires SSToolkit repo at ../sstoolkit and appledoc in $PATH'
task :docs => [:'docs:clean'] do
  appledoc_options = [
    '--output temp/documentation',
    '--project-name SSToolkit',
    '--project-company \'Sam Soffes\'',
    '--company-id com.samsoffes',
    '--keep-intermediate-files',
    '--create-html',
    '--templates ~/Library/Application\ Support/appledoc/Templates/',
    '--no-repeat-first-par',
    '--no-create-docset',
    '--verbose']
  
  `appledoc #{appledoc_options.join(' ')} ../sstoolkit/SSToolkit/*.h`
  `mv temp/documentation/html public/documentation`
  `rm -rf temp/documentation`
  puts 'Imported.'
end

namespace :docs do
  desc 'Clean docs'
  task :clean do
    `rm -rf public/documentation`
  end
end

desc 'Deploy to Heroku'
task :deploy do
  unless `git status -s`.length == 0
    puts "WARNING: There are uncommitted changes"
    puts "Commit any changes before deploying."
    exit
  end
  
  `git push heroku master`
  `git push origin master`
end
