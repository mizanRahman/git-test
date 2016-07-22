require 'date'

desc 'release with all features'
task :release =>[:fork_release, :merge] do

end


def psystem(command)
	puts command
	system command
end

# lib/tasks/migrate_topics.rake
desc 'merge feature branch to develop'
task :fork_release, [:version] do |t, args|
	psystem "git tag before-release-2.1.9#{Time.now.to_i}"
	psystem 'git tag'
	psystem "git checkout -b release/release-#{args[:version]} origin/develop"
	puts 'release branch forked'
end

desc 'merge feature branch to develop'
task :merge, [:feature] do |t, args|
	psystem "git checkout develop"
	psystem "git pull"
	psystem "git pull origin #{args[:feature]}"
end

desc "enlist sql file changes"
task :sql_changes, :from do |t, args|
	ppsystem "git checkout develop"
	puts "git diff #{args[:from]} --name-only server-config/db"
end

desc "enlist config file changes"
task :config_changes, [:from] do |t, args|
	puts "git checkout develop"
	puts "git diff #{args[:from]} --name-only server-config/wildfly"
end


desc "Task description"
task :task_name => [:dependent, :tasks] do
	
end

task :build do |t, args|
	puts "#{ENV['ok']}"
  puts "Current env is #{ENV['ok']}"
end