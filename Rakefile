DATE = Time.now.strftime("%Y-%m-%d")
TIME = Time.now.strftime("%H:%M:%S")
POST_DIR = '_posts'

desc "Generate new post"
task :new_post do

  puts 'Please write the post title'
  @name = STDIN.gets.chomp
  @title = @name.downcase.strip.gsub(' ', '-')
  @file = "#{POST_DIR}/#{DATE}-#{@title}.markdown"

  if File.exists?("#{file}")
    raise 'file already exists'
  else
    File.open(@file, 'w') do |file|
      file.write(
        "---\nlayout: post\ntitle: \"#{@name}\"\ndate: #{DATE} #{TIME}\ncategories:\n---\n"
      )
    end
  end
end
