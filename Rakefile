require 'bundler/gem_tasks'

begin
  require 'rspec/core/rake_task'

  RSpec::Core::RakeTask.new(:spec)

  task :test => :spec
  task :default => :spec
rescue LoadError => e
  warn "#{e.path} is not available"
end

begin
  require 'rubocop/rake_task'

  RuboCop::RakeTask.new(:rubocop) do |t|
    t.options = ['--display-cop-names', '--fail-level', 'W']
  end

  task :default => :rubocop
rescue LoadError => e
  warn "#{e.path} is not available"
end

namespace :build do
  desc 'Transcompile to JavaScript using Opal'
  task :js do
    require 'opal'

    builder = Opal::Builder.new(compiler_options: {
      dynamic_require_severity: :error,
    })
    builder.append_paths 'lib'
    builder.build 'asciidoctor-interdoc-reftext'

    out_file = 'js/asciidoctor-interdoc-reftext.js'

    mkdir_p(File.dirname(out_file), verbose: false)
    File.binwrite out_file, builder.to_s
    File.binwrite "#{out_file}.map", builder.source_map
  end
end
