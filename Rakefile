# Added by Josh Nichols, a.k.a. technicalpickles
require 'rake'

files = ['doc/apidock.txt', 'plugin/apidock.vim']

desc 'Install plugin and documentation'
task :install do

  # {{{ logic to modify plugin browser settings by platform
  in_lines = File.open( files.last ).read.split("\n")

  def get_platform_regex
    if RUBY_PLATFORM =~ /linux/
      /LINUX/
    elsif RUBY_PLATFORM =~ /(win|w)32$/
      /WINDOWS/
    else
      /OSX/
    end
  end

  def detect_settings_index in_lines
    in_lines.each_with_index do |s,i| 
      return i if s =~ get_platform_regex
    end
  end
 
  line_to_uncomment = detect_settings_index(in_lines)+1
  in_lines[line_to_uncomment] = in_lines[line_to_uncomment][2..1000]

  File.open( files.last, 'w' ) do |fw|
    in_lines.each do |line|
      fw.puts line
    end
  end
  # }}}

  # {{{ install by platform
  vimfiles = if ENV['VIMFILES']
               ENV['VIMFILES']
             elsif RUBY_PLATFORM =~ /(win|w)32$/
               File.expand_path("~/vimfiles")
             else
               File.expand_path("~/.vim")
             end
  files.each do |file|
    target_file = File.join(vimfiles, file)
    FileUtils.mkdir_p File.dirname(target_file)
    FileUtils.cp file, target_file

    puts "  Copied #{file} to #{target_file}"
  end
  # }}}

end
