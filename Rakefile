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
	properties[:primary] = args[:primary]
	properties[:major] = args[:major]
	properties[:minor] = args[:minor]
	JavaProperties.write(properties, "version.properties")
	psystem "git add ."
	psystem "git commit -m 'version upgraded'"
end

desc "tag a repository"
task :rc_tag, [:version] do |t, args|
	psystem "git tag v#{args[:version]}-rc"
	psystem "git push --tags"
end

desc "tag a repository"
task :stable_tag, [:version] do |t, args|
	psystem "git tag v#{args[:version]}"
	psystem "git push --tags"
end

# rake 'hq_tag[2.3.4]'
desc "tag hq repository"
task :hq_rc_tag, [:version] do |t, args|
	repos = ['psm', 'kona-secret']
	repos.each do |item|
		psystem "cd #{item}"
		psystem "git tag v#{args[:version]}-rc"
		psystem "git push --tags"
		psystem "cd -"
	end 
end

# rake 'hq_stable_tag[2.3.4]'
desc "stable tag hq repository"
task :hq_stable_tag, [:version] do |t, args|
	repos = ['psm', 'kona-secret']
	repos.each do |item|
		psystem "cd #{item}"
		psystem "git tag v#{args[:version]}"
		psystem "git push --tags"
		psystem "cd -"

	end 
end


# rake 'all_rc_tag[2.3.4]'
desc "stable all repository"
task :stable_tag, [:version] => [:hq_stable_tag] do |t, args|
	repos = ['kona-paypaas']
	repos.each do |item|
		psystem "cd #{item}"
		psystem "git tag v#{args[:version]}"
		psystem "git push --tags"
		psystem "cd -"

	end 
end





desc "it does a thing"
task :work, [:option] => [:environment] do |t, args|
	puts args
	puts args[:option]
end


