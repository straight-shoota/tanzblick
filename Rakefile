# coding: utf-8
require 'yaml'
require 'open3'

CONFIG = YAML.load_file("_config.yml")

dest_dir = CONFIG['destination'] || "/tmp/jekyll_#{File.basename(__dir__)}"

CONFIG['git'] ||= {}
git_branch = CONFIG['git']['branch'] || 'gh-pages'

def git_repo
  CONFIG['git']['repo']
end

task :clean do
  system "rm -r #{dest_dir}"
end

task :build => :clean do
  system "jekyll build --destination #{dest_dir}"
end

task :serve do
  run_service "jekyll serve --force_polling --destination #{dest_dir}"
end

task :deploy do
  system "git push -u origin #{git_branch}"
end

task :setup_git do
  system "git remote add origin #{git_repo}"
end

def run_service(*cmd)
  Open3.popen2e(*cmd) do |stdin, stdout_and_stderr, thread|
    begin
      stdout_and_stderr.each do |line|
        puts line
      end
    rescue SystemExit, Interrupt
      puts "Execution iterrupted."
    ensure
      thread.kill
    end
  end
end
