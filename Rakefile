require 'rspec/core/rake_task'
require 'rubocop/rake_task'
require 'foodcritic'
require 'kitchen'
require 'emeril/rake'

# Style tests. Rubocop and Foodcritic
namespace :style do
  desc 'Run Ruby style checks'
  RuboCop::RakeTask.new(:ruby)

  desc 'Run Chef style checks'
  FoodCritic::Rake::LintTask.new(:chef) do |t|
    t.options = {
      :fail_tags => ['any'],
      :tags => []
    }
  end
end

desc 'Run all style checks'
task :style => ['style:chef', 'style:ruby']

# Rspec and ChefSpec
desc 'Run ChefSpec examples'
RSpec::Core::RakeTask.new(:spec)

# Integration tests. Kitchen.ci
namespace :integration do
  desc 'Run Test Kitchen with Digital Ocean'
  task :digitalocean do
    Kitchen.logger = Kitchen.default_file_logger
    Kitchen::Config.new.instances.each do |instance|
      instance.test(:always)
    end
  end
end

desc 'Run all tests on Travis'
task :travis => %w(style spec)

# Default
task :default => %w(style spec integration:digitalocean)
