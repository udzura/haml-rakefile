require 'rubygems'
require 'rake'

module DoConv
  FILE_NAME_REGEXP = {
    :haml => /(?:\.html?)?\.haml$/,
    :sass => /(?:\.css)?\.sass$/,
    :html2haml => /\.html$/,
    :css2sass => /\.css$/,
    :asciidoc => /\.txt$/
  }
  FILE_DEST_SUFFIX = {
    :haml => ".html",
    :sass => ".css",
    :html2haml => ".haml",
    :css2sass => ".sass",
    :asciidoc => ".html"
  }

  def do_conv(bintype, fpath)
    dest_fpath = File.join(
      File.dirname(fpath),
      File.basename(fpath).gsub(FILE_NAME_REGEXP[bintype], FILE_DEST_SUFFIX[bintype])
    )
    if File.exist? dest_fpath
      f = open(dest_fpath, "w")
      f.close
      puts "(!) truncated #{dest_fpath}."
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
      do_conv(:html2haml, fpath)
    when /\.sass$/
      do_conv(:css2sass, fpath)
    end
  end
  puts "Convert Successful"
end

desc "rollback converted html/css into haml/sass"
task :rollback do
  check_bin :html2haml, :css2sass
  Rake::FileList.new("**/*.html", "**/*.css").each do |fpath|
    case fpath
    when /\.html$/
      do_conv(:html2haml, fpath)
    when /\.css$/
      do_conv(:css2sass, fpath)
    end
  end
  puts "Rollback Successful"
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

namespace :rollback do
  desc "rollback converted htmls into haml"
  task :haml do
    check_bin :html2haml
    Rake::FileList.new("**/*.html").each do |fpath|
      do_conv(:html2haml, fpath)
    end
    puts "Rollback Successful"
  end
  task :sass do
    check_bin :css2sass
    Rake::FileList.new("**/*.css").each do |fpath|
      do_conv(:css2sass, fpath)
    end
    puts "Rollback Successful"
  end
end