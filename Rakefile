require 'date'
require 'java-properties'



def psystem(command)
	puts command
#	system command
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
task :merge, [:branch] do |t, args|
	psystem "git checkout develop"
	psystem "git pull"
	psystem "git pull origin #{args[:branch]}"
end

desc "Flyway migration script changes"
task :migrations, :from do |t, args|
	# psystem "git checkout develop"

	puts "Flyway migration script changes"
	puts "==============================="
	psystem "git diff #{args[:from]} --name-only server-config/sql"
end

desc "Post flyway migration script changes"
task :sqls, :from do |t, args|
	# psystem "git checkout develop"
	
	puts "Post flyway sql script changes"
	puts "==============================="
	psystem "git diff #{args[:from]} --name-only server-config/post-flyway"
end

desc "enlist config file changes"
task :configs, [:from] do |t, args|
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

desc "open vi editor and record new version"
task :version_upgrate do |t, args|
	psystem "vi version.gradle"
	psystem "git add ."	
	psystem "git commit -m 'version upgraded'"
end



def tag(repo, version)
		psystem "cd #{repo}"
		psystem "git checkout master"
		psystem "git pull"
		psystem "git tag v#{version}"
		psystem "git push --tags"
		psystem "cd -"
end 


def tag_all(repos, version) 
	repos.each do |repo|
		tag(repo,version)
	end 
end

# desc "tag a repository"
# task :rc_tag, [:version] do |t, args|
# 	psystem "git tag v#{args[:version]}-rc"
# 	psystem "git push --tags"
# end

# desc "tag a repository"
# task :stable_tag, [:version] do |t, args|
# 	psystem "git tag v#{args[:version]}"
# 	psystem "git push --tags"
# end

# rake 'hq_tag[2.3.4]'
# will tag v2.3.34-rc

desc "tag hq repository"
task :hq_rc_tag, [:version] do |t, args|
	repos = ['psm', 'kona-secret']
	rc_version = args[:version]+'-rc'
	tag_all repos, rc_version
end

# rake 'hq_stable_tag[2.3.4]'
# will tag v2.3.4
desc "tag hq repository"
task :hq_tag, [:version] do |t, args|
	repos = ['psm', 'kona-secret']
	tag_all repos, args[:version]
end

desc "Merge Hotfix to release branch"
task :merge_hotfix,[:branch, :release_branch] => [:merge] do |t, args|
	psystem "git checkout #{args[:release_branch]}"
	psystem "git pull"
	psystem "git pull origin #{args[:branch]}"
	psystem "git push origin #{args[:release_branch]}"
end


desc "test2"
task :t2,[:p1] do |t, args|
	puts "param1: #{args[:p1]}"
end

desc "test1"
task :t1,[:p1,:p2] => [:t2] do |t, args|
	puts "param2: #{args[:p2]}"
end

# Rakefile
directory "db"
	file "db/my.db" do
	  sh "echo 'Hello db' > db/my.db"
	end


task :report => "db/my.db" do 
	puts "ik"
end
