task :default => :generate

desc 'Create new post with rake "post[post-name]"'
task :post, [:title] do |t, args|
  if args.title then
    new_post(args.title)
  else
    puts 'rake "post[post-name]"'
  end
end

desc 'Build site with Jekyll'
task :generate => :clean do
  `jekyll build`
end

desc 'Start server'
task :server => :clean do
  `jekyll serve`
end

desc 'Deploy with rake "depoly[comment]"'
task :deploy, [:comment] => :generate do |t, args|
  if args.comment then
    `git commit . -m '#{args.comment}' && git push`
  else
    `git commit . -m 'new deployment' && git push`
  end
end

desc 'Clean up'
task :clean do
  `rm -rf _site`
end

def new_post(title)
  time = Time.now
  filename = "_posts/" + time.strftime("%Y-%m-%d-") + title.downcase.strip.gsub(' ', '-').gsub(/[^\w-]/, '') + '.markdown'
  if File.exists? filename then
    puts "Post already exists: #{filename}"
    return
  end
  File.open(filename, "wb") do |f|
    f << <<-EOS
---
title: #{title}
layout: post
tags:
  - 
---


EOS
  %x[echo "#{filename}" | xsel]
  end
  puts "created #{filename}"
  `git add #{filename}`
  `open #{filename}`
end
