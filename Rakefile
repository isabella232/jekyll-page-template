file "master.zip" => [:'clean:zip'] do
  puts "Downloading latest version of template"
  `wget https://github.com/opener-project/jekyll-page-template/archive/master.zip`
  Rake::Task["clean:zip"].reenable
end

file "template" =>["master.zip", :'clean:template'] do
  puts "Expanding latest version of template."
  `unzip master.zip`
  `mv jekyll-page-template-master template`
  rm "template/Rakefile", :force=>true
  Rake::Task["clean:template"].reenable
end

task :copy => ["template"] do
  puts "Copying over template files."
  `cp -R template/* .`
  `cp -R custom/* .`
  puts "All files have been put in place"
end

task :update => [:copy] do
  Rake::Task["clean"].invoke
  puts "Done, it was a pleasure working with you. Enjoy the result..."
end

namespace :update do
  task :force => [:'remove_template'] do
    Rake::Task["update"].invoke
  end
end

task :remove_template => ["template"] do
  puts "Removing all template files."
  files = FileList["template/*"]
  files.each do |filename|
    rm_r filename.gsub(/^template/,'.'), :force=>true
  end
end

task :clean => [:'clean:zip', :'clean:template'] do
  puts "The maid has left the building, all is shiny and clean. Don't forget to thank her."
end

namespace :clean do
  task :zip do
    puts "removing the template zip"
    rm "master.zip", :force=>true
  end

  task :template do
    puts "Removing the template folder"
    rm_r "template", :force=>true
  end
end

task :default => [:update]
task :clobber => [:remove_template, :clean]
