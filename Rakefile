$:.unshift File.join( File.dirname( __FILE__ ))
$:.unshift File.join( File.dirname( __FILE__ ), 'lib')

require 'rake'
require 'rake/testtask'
require 'unimidi'

namespace(:test) do
  task :all => [:unit, :integration]

  Rake::TestTask.new(:integration) do |t|
    t.libs << "test"
    t.test_files = FileList["test/integration/**/*_test.rb"]
    t.verbose = true
  end

  Rake::TestTask.new(:unit) do |t|
    t.libs << "test"
    t.test_files = FileList["test/unit/**/*_test.rb"]
    t.verbose = true
  end
end

Rake::Task['test'].enhance ["test:all"]

platforms = ["generic", "x86_64-darwin10.7.0", "i386-mingw32", "java", "i686-linux"]

task(:build) do
  require "unimidi-gemspec"
  platforms.each do |platform|
    UniMIDI::Gemspec.new(platform)
    filename = "unimidi-#{platform}.gemspec"
    system "gem build #{filename}"
    system "rm #{filename}"
  end
end

task(:release => :build) do
  platforms.each do |platform|
    system "gem push unimidi-#{UniMIDI::VERSION}-#{platform}.gem"
  end
end

task(:default => [:test])
