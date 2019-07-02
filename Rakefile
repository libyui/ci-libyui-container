# the tasks initialization done in "libyui/rake" reads some
# packaging files which do not exist in this container "package",
# create some dummy files before loading so it does not fail
# TODO: fix the "libyui/rake" and remove this workaround
FAKE_SPEC_FILE = "package/fake.spec".freeze
FAKE_VERSION_FILE = "VERSION.cmake"
FAKE_VERSION_CONTENT = "SET(VERSION_MAJOR \"42\")\n" \
"SET(VERSION_MINOR \"42\")\nSET(VERSION_PATCH \"42\")".freeze

begin
  # just an empty file is enough
  File.write(FAKE_SPEC_FILE, "")
  File.write(FAKE_VERSION_FILE, FAKE_VERSION_CONTENT)

  require "libyui/rake"
ensure
  # ensure the fake files are deleted
  File.delete(FAKE_SPEC_FILE) if File.exist?(FAKE_SPEC_FILE)
  File.delete(FAKE_VERSION_FILE) if File.exist?(FAKE_VERSION_FILE)
end

Libyui::Tasks.configuration do |conf|
  # lets ignore license check for now
  conf.skip_license_check << /.*/

  conf.package_name = "ci-libyui-container"
  conf.obs_target = "containers"
end

# do not create a tarball, this project builds a Docker container
Rake::Task["tarball"].clear_actions

# building at Jenkins does not work for some reason, disable it for now
Rake::Task["osc:build"].clear if ENV["JENKINS_HOME"]

