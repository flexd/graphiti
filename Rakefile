require 'rubygems'
require 'bundler'
begin
  Bundler.setup(:default, :development)
rescue Bundler::BundlerError => e
  $stderr.puts e.message
  $stderr.puts "Run `bundle install` to install missing gems"
  exit e.status_code
end

require 'rake'
require File.join(File.dirname(__FILE__), 'graphiti')

require 'rake/testtask'
Rake::TestTask.new(:test) do |test|
  test.libs << 'lib' << 'test'
  test.pattern = 'test/**/test_*.rb'
  test.verbose = true
end

task :default => :test
namespace :server do
  desc 'Stop the unicorn server'
  task :stop do
    puts "Stopping unicorn: #{`kill -QUIT $(cat $(grep pid /opt/graphiti/config/unicorn.rb| cut -d \ -f 2 | sed "s/^([\"'])(.)\1\$/\2/g"))`}"
  end
  task :restart do
    puts "Restarting unicorn: #{`kill -USR2 $(cat $(grep pid /opt/graphiti/config/unicorn.rb| cut -d \ -f 2 | sed "s/^([\"'])(.)\1\$/\2/g"))`}"
  end
  task :start do
    puts "Starting unicorn: #{`bundle exec unicorn -c config/unicorn.rb -E production -D`}"
  end
end
namespace :graphiti do

  desc 'Rebuild Metrics List'
  task :metrics do
    list = Metric.all true
    puts "Got #{list.length} metrics"
  end

  desc 'Send email reports per dashboard. Needs `reports` settings in settings.yml'
  task :send_reports do
    Dashboard.send_reports
  end

end
