require 'date'
require 'java-properties'
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

desc "Flyway migration script changes"
task :migration_scripts, :from do |t, args|
	# psystem "git checkout develop"

	puts "Flyway migration script changes"
	puts "==============================="
	psystem "git diff #{args[:from]} --name-only server-config/sql"
end

desc "Post flyway migration script changes"
task :manual_sqls, :from do |t, args|
	# psystem "git checkout develop"
	
	puts "Post flyway sql script changes"
	puts "==============================="
	psystem "git diff #{args[:from]} --name-only server-config/post-flyway"
end

desc "enlist config file changes"
task :config_changes, [:from] do |t, args|
	puts "git checkout develop"

	puts "Configuration script changes"
	puts "==============================="
	psystem "git diff #{args[:from]} --name-only server-config/config"
end


desc "enlist config file changes"
task :migrate_version, [:primary, :major, :minor] do |t, args|
	properties = JavaProperties.load("version.properties")
	# puts properties[:primary] # => "bar"
	# prim = gets
	properties[:primary] = args[:primary]
	properties[:major] = args[:major]
	properties[:minor] = args[:minor]
	JavaProperties.write(properties, "version.properties")
	psystem "git add ."
	psystem "git commit -m 'version upgraded'"
end


desc "it does a thing"
task :work, [:option] => [:environment] do |t, args|
	puts args
	puts args[:option]
end


