require 'time'

desc 'create a new draft post'
task :post do
    title = ENV['TITLE']
    slug = "#{Date.today}-#{title.downcase.gsub(/[^\w]+/, '-')}"

    file = File.join(
        File.dirname(__FILE__),
        '_posts',
        slug + '.markdown'
    )

    File.open(file, "w") do |f|
        f << <<-EOS.gsub(/^     /, '')
    ---
    date: "#{Date.today}"
    date_published: "#{Date.today}"
    date_updated: "#{Date.today}"
    slug: "#{title.downcase.gsub(/[^\w]+/,'-')}"
    title: #{title}
    tags: []
    type: "post"
    ---

        EOS
    end

end
