require 'rubygems'
require 'rake'

module DoConv
  FILE_NAME_CONVERT = {
    :haml => /(?:\.html?)?\.haml$/,
    :sass => /(?:\.css)?\.sass$/,
    :asciidoc => /.txt$/
  }
  FILE_DEST_SUFFIX = {
    :haml => ".html",
    :sass => ".css",
    :asciidoc => ".html"
  }

  def do_conv(bintype, fpath)
    dest_fpath = File.join(
      File.dirname(fpath),
      File.basename(fpath).gsub(FILE_NAME_CONVERT[bintype], FILE_DEST_SUFFIX[bintype])
    )
    if File.exist? dest_fpath
      f = open(dest_fpath, "w")
      f.close
      puts "# truncated #{dest_fpath}."
    end
    if bintype == :asciidoc
      sh "#{bintype} #{fpath}"
    else
      sh "#{bintype} #{fpath} #{dest_fpath}"
    end
    sh "rm #{fpath}"
  end

  def check_bin(*args)
    args.each do |bin|
      raise "#{bin} executable file is not in PATH!" if `which #{bin}`.empty?
    end
  end
end

include DoConv

desc "convert haml/sass"
task :convert do
  check_bin :haml, :sass
  Rake::FileList.new("**/*.haml", "**/*.sass").each do |fpath|
    case fpath
    when /\.haml$/
      do_conv(:haml, fpath)
    when /\.sass$/
      do_conv(:sass, fpath)
    end
  end
  puts "Convert Successful"
end


namespace :convert do
  desc "convert haml only"
  task :haml do
    check_bin :haml
    Rake::FileList.new("**/*.haml").each do |fpath|
      do_conv(:haml, fpath)
    end
    puts "Convert Successful"
  end
  desc "convert sass only"
  task :sass do
    check_bin :sass
    Rake::FileList.new("**/*.sass").each do |fpath|
      do_conv(:sass, fpath)
    end
    puts "Convert Successful"
  end
  desc "convert asciidoc (beta)"
  task :asciidoc do
    check_bin :asciidoc
    Rake::FileList.new("**/*.txt").each do |fpath|
      do_conv(:asciidoc, fpath)
    end
    puts "Convert Successful"
  end
end