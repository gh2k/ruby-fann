require "bundler/gem_tasks"

require 'rake/testtask'
require 'rdoc/task'
require 'rake/clean'

NAME = 'ruby_fann'

# rule to build the extension: this says
# that the extension should be rebuilt
# after any change to the files in ext
file "lib/#{NAME}/#{NAME}.so" =>
  Dir.glob("ext/#{NAME}/*{.rb,.c}") do
    Dir.chdir("ext/#{NAME}") do
      # this does essentially the same thing
      # as what RubyGems does
      ruby "extconf.rb"
      sh "make"
    end
  end
  cp "ext/#{NAME}/#{NAME}.so", "lib/#{NAME}"
#end

# make the :test task depend on the shared
# object, so it will be built automatically
# before running the tests
task :test => "lib/#{NAME}/#{NAME}.so"

# use 'rake clean' and 'rake clobber' to
# easily delete generated files
CLEAN.include('ext/**/*{.o,.log,.so}')
CLEAN.include('ext/**/Makefile')
CLOBBER.include('lib/**/*.so')

# the same as before
Rake::TestTask.new do |t|
  t.libs << 'test'
end

desc "Run tests"
task :default => :test

Rake::RDocTask.new do |rd|
  rd.main = "README.txt"
  rd.rdoc_files.include("README.txt", "lib/**/*.rb", "ext/**/*.c")
  rd.title = "Ruby FANN Gem documentation"
end

rubyforge_username = "stevenmiers"
rubyforge_project = "ruby-fann"
desc 'Upload rdoc files to rubyforge'
task :upload_docs do
  host = "#{rubyforge_username}@rubyforge.org"
  remote_dir = "/var/www/gforge-projects/#{rubyforge_project}/"
  local_dir = 'html'
  sh %{scp -r #{local_dir}/ #{host}:#{remote_dir}}
  #sh %{rsync -aCv #{local_dir}/ #{host}:#{remote_dir}}
end

