require "bundler/gem_tasks"
require "rspec/core/rake_task"

if /darwin/ =~ RUBY_PLATFORM
  task :spec do
    sh "rsdl -S rspec #{ENV['SPEC_OPTS']} #{ENV['SPEC']}"
  end

  task :guard do
    rspec_path = 'spec/rspec'
    File.open(rspec_path, 'w') do |f|
      f.write(<<-EOS)
#!/bin/sh
bundle exec rsdl -S rspec $@
      EOS
    end
    chmod(0755, rspec_path)
    begin
      sh "bundle exec guard"
    ensure
      rm_rf(rspec_path)
    end
  end
else
  RSpec::Core::RakeTask.new(:spec)

  task :guard do
    sh "bundle exec guard"
  end
end

task :rubocop do
  files = `git ls-files | grep -e '.rb$'`
  sh "rubocop #{files.split(/\s+/m).join(' ')}"
end

task :default => [:rubocop, :spec]
