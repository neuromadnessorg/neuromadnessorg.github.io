require "rubygems"
require "tmpdir"

require "bundler/setup"
require "jekyll"

GITHUB_REPONAME = "neuromadnessorg/neuromadnessorg.github.io"

desc "Generate blog files"
task :generate do
  Jekyll::Site.new(Jekyll.configuration({
    "source"      => ".",
    "destination" => "_site"
  })).process
end

desc "Generate and publish blog to gh-pages"
task :publish => [:generate] do
  Dir.mktmpdir do |tmp|
    cp_r "_site/.", tmp

    pwd = Dir.pwd
    Dir.chdir tmp

    system "git init"
    system "git config user.name 'neuromadnessorg'; git config user.email 'neuromadnessorg@gmail.com'"
    system "git add ."
    message = "Site updated at #{Time.now.utc}"
    system "git commit -m #{message.inspect} --author 'neuromadnessorg <neuromadnessorg@gmail.com>'"
    system "git remote add origin git@neuromadnessorg:#{GITHUB_REPONAME}.git"
    system "git push origin master --force"

    Dir.chdir pwd
  end
end

desc "Create new post"
task :post => [:create] do
  puts "Creating new post: #{filename}"
  open(filename, 'w') do |post|
      post.puts "---"
      post.puts "layout: post"
      post.puts "title: \"#{title.gsub(/-/,' ')}\""
      post.puts "cover: "
      post.puts "cover_width: "
      post.puts "---"
      post.puts "{% include JB/setup %}"
  end
end

