require 'date'
require 'java-properties'


def psystem(command)
	puts command
	system command
end

def tag_head(version)
	psystem "git pull"
	psystem "git tag v#{version}"
	psystem "git push --tags"
end

def tag(repo, version, main_branch)
		# psystem "cd #{repo}"
		psystem "git checkout #{main_branch}"
		tag_head version
		# psystem "cd -"
end 

def tag_all(repos, version, main_branch='develop') 
	repos.each do |repo|
		Dir.chdir(repo) do
			tag(repo,version, main_branch)
		end
	end
end

def tag_rc_all(repos, version, main_branch='develop') 
	rc_version = version+'-rc'
	tag_all repos, rc_version, main_branch
end

def tag_rc_hq(version) 
	repos = ['psm', 'kona-secret']
	tag_rc_all repos, version, 'master'
end

def tag_rc_kpp(version) 
	repos = ['kona-paypaas']
	tag_rc_all repos, version
end

#==============================================================
#================ Tasks =======================================
#==============================================================

# lib/tasks/migrate_topics.rake
desc 'merge feature branch to develop'
task :fork_release, [:version] do |t, args|
	psystem "git checkout -b release/release-#{args[:version]}"
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
	puts "Flyway migration script changes"
	puts "==============================="
	psystem "git diff #{args[:from]} --name-only server-config/sql"
end

desc "Post flyway migration script changes"
task :sqls, :from do |t, args|
	puts "Post flyway sql script changes"
	puts "==============================="
	psystem "git diff #{args[:from]} --name-only server-config/post-flyway"
end

desc "enlist config file changes"
task :configs, [:from] do |t, args|
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
	psystem "git pull"
	psystem "vi version.gradle"
	psystem "git add ."	
	psystem "git commit -m 'version upgraded'"
end



# rake 'hq_tag[2.3.4]'
# will tag v2.3.34-rc
desc "tag hq repository"
task :hq_rc_tag, [:version] do |t, args|
	tag_rc_hq args[:version]
end

desc "tag hq repository"
task :kpp_rc_tag, [:version] do |t, args|
	tag_rc_kpp args[:version]
end

# rake 'hq_stable_tag[2.3.4]'
# will tag v2.3.4
desc "tag hq repository"
task :hq_tag, [:version] do |t, args|
	repos = ['psm', 'kona-secret']
	tag_all repos, args[:version], 'master'
end

# rake 'hq_stable_tag[2.3.4]'
# will tag v2.3.4
desc "tag kpp repository"
task :kpp_tag, [:version] do |t, args|
end

desc "tag all repositories"
task :tag_rc_all, [:version] => [:kpp_rc_tag, :hq_rc_tag] do |t, args|
end

desc "tag all repositories"
task :tag_all,[:version] => [:kpp_tag, :hq_tag] do |t, args|
end

desc "give a git tag"
task :tag,[:version] do |t, args|
	tag_head args[:version]
end

desc "Merge Hotfix to release branch"
task :merge_hotfix,[:branch, :release_branch] => [:merge] do |t, args|
	psystem "git checkout #{args[:release_branch]}"
	psystem "git pull"
	psystem "git pull origin #{args[:branch]}"
	psystem "git push origin #{args[:release_branch]}"
end


desc "push branches"
task :push, [:version] do |t, args|
	branches = ['develop', "release/release-#{args.version}"]
	branches.each do |branch|
		psystem "git checkout #{branch}"
		psystem "git pull"
		psystem "git push origin #{branch}"
	end 
end


desc "Release"
task :release, [:version, :from] => [:version_upgrate, :fork_release, :configs, :migrations, :sqls, :tag_rc_all, :push] do |t, args|
	
end

# desc "test2"
# task :t2,[:p1] do |t, args|
# 	puts "param1: #{args[:p1]}"
# end

# desc "test1"
# task :t1,[:p1,:p2] => [:t2] do |t, args|
# 	puts "param2: #{args[:p2]}"
# end

# # Rakefile
# directory "db"
# 	file "db/my.db" do
# 	  sh "echo 'Hello db' > db/my.db"
# 	end


# task :report => "db/my.db" do 
# 	puts "ik"
# end


desc 'just a test'
task :demo, [:version] do |t, args|
	repos = ["repositories/app", "repositories/kpp"]
	repos.each do |repo|
		Dir.chdir(repo) do
			psystem "pwd"
			psystem "git status"
			psystem "git log -1"
		end
	end
end

##
# git archive --output=export1.zip HEAD $(git diff --name-only --diff-filter=A HEAD~1 HEAD)

