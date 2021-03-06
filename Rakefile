require 'bundler'
Bundler::GemHelper.install_tasks

require 'rake'
require "rspec/core/rake_task"

task :default => :spec

desc "Run functional specs"
RSpec::Core::RakeTask.new(:spec_functional) do |spec|
  spec.pattern = 'spec/*_spec.rb'
  spec.rspec_opts = ['--options', "\"#{File.dirname(__FILE__)}/spec/spec.opts\""]
end

desc "Run unit specs"
RSpec::Core::RakeTask.new(:spec_unit) do |spec|
  spec.pattern = 'spec/unit/*_spec.rb'
  spec.rspec_opts = ['--options', "\"#{File.dirname(__FILE__)}/spec/spec.opts\""]
end

desc "Run all specs"
task :spec do
  if ENV['TRAVIS'] # travis handles the environments for us
    Rake::Task[:spec_unit].execute
    Rake::Task[:spec_functional].execute
  else
    ['3_2', '4_0'].each do |version|
      Bundler.with_clean_env do
        puts "Running tests with ActiveSupport #{version.sub('_', '.')}"
        sh "env BUNDLE_GEMFILE=active_support_#{version} bundle install"
        sh "env BUNDLE_GEMFILE=active_support_#{version} bundle exec rake spec_unit spec_functional"
      end
    end
  end
end
