require_relative 'lib/simp/rpm/spec_builder'
require_relative 'lib/simp/rpm/spec_builder_config'

HOST_DIST   = %x[rpm -E '%dist'].strip.sub(/^\./,'')[0,3]
namespaces  = []
dist_dirs   = []

config_hash = SIMP::RPM::SpecBuilderConfig.load_config('things_to_build.yaml')
config_hash.each do |proj, dist_configs|
  namespace proj do
    dist_configs.each do |dist_config|
      Dir["#{proj}#{dist_config[:dist]||''}/*.spec"].each do |spec|
        project_dir = File.dirname(spec)
        rpm_dist = project_dir.split('.')[1] || nil
        next unless rpm_dist == HOST_DIST
        namespaces << "#{proj}:#{rpm_dist}"
        dist_dirs  << File.expand_path('dist',project_dir)
        namespace rpm_dist do
          CLEAN << File.expand_path('dist',File.dirname(spec))
          builder = SIMP::RPM::SpecBuilder.new({proj => dist_config},
                                               File.expand_path(project_dir))
          builder.define_rake_tasks spec
        end
      end
    end
  end
end

namespace :pkg do
  dist_dir = File.expand_path('dist',__dir__)
  CLEAN << dist_dir
  desc "builds all #{HOST_DIST} RPM packages"
  task :rpm => namespaces.map{|ns| "#{ns}:pkg:rpm" } do
     Dir.chdir __dir__
     mkdir_p dist_dir
     Dir["{#{dist_dirs.join(',')}}/*.rpm"].each{|f| copy(f, dist_dir)}
  end
end
