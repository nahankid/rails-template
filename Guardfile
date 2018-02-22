# A sample Guardfile
# More info at https://github.com/guard/guard#readme

# Run just the livereload monitor with: `guard -P livereload`
guard :livereload do
  watch(%r{app/views/.+\.(erb|haml|slim)$})
  watch(%r{app/helpers/.+\.rb})
  watch(%r{public/.+\.(css|js|html)})
  watch(%r{config/locales/.+\.yml})
  # Rails Assets Pipeline
  watch(%r{(app|vendor)(/assets/\w+/(.+\.(css|js|html|png|jpg))).*}) { |m| "/assets/#{m[3]}" }
  watch(%r{(app|vendor)(/assets/\w+/(.+)\.(scss))}) { |m| "/assets/#{m[3]}.css" }
end

guard :minitest, :spring => "bin/rails test" do
  watch(%r{^app/(.+)\.rb$})                               { |m| "test/#{m[1]}_test.rb" }
  watch(%r{^app/controllers/application_controller\.rb$}) { "test/controllers" }
  watch(%r{^app/controllers/(.+)_controller\.rb$})        { |m| "test/integration/#{m[1]}_test.rb" }
  watch(%r{^app/views/(.+)_mailer/.+})                    { |m| "test/mailers/#{m[1]}_mailer_test.rb" }
  watch(%r{^lib/(.+)\.rb$})                               { |m| "test/unit/lib/#{m[1]}_test.rb" }
  watch(%r{^lib/tasks/(.+)\.rake$})                       { |m| "test/unit/lib/tasks/#{m[1]}_test.rb" }
  watch(%r{^test/.+_test\.rb$})
  watch(%r{^test/test_helper\.rb$}) { "test" }
  watch(%r{^test/support/.+\.rb}) { "test" }
end

guard 'brakeman', run_on_start: true do
  watch(%r{^app/.+\.(erb|haml|rhtml|rb)$})
  watch(%r{^config/.+\.rb$})
  watch(%r{^lib/.+\.rb$})
  watch('Gemfile')
end

guard :rubocop, cli: ['-a', '--except', 'Lint/Debugger'] do
  watch(/.+\.rb$/)
  watch(/.+\.jbuilder$/)
  watch(%r{(?:.+/)?\.rubocop\.yml$}) { |m| File.dirname(m[0]) }
  watch(%r{^test/(.*)\/?test_(.*)\.rb$})
  watch(%r{^lib/(.*/)?([^/]+)\.rb$})     { |m| "test/#{m[1]}test_#{m[2]}.rb" }
  watch(%r{^test/test_helper\.rb$})      { 'test' }
end

guard :bundler do
  require 'guard/bundler'
  require 'guard/bundler/verify'
  helper = Guard::Bundler::Verify.new

  files = ['Gemfile']
  files += Dir['*.gemspec'] if files.any? { |f| helper.uses_gemspec?(f) }

  # Assume files are symlinked from somewhere
  files.each { |file| watch(helper.real_path(file)) }
end

guard 'bundler_audit', run_on_start: true do
  watch('Gemfile.lock')
end

### Guard::Sidekiq
#  available options:
#  - :verbose
#  - :queue (defaults to "default") can be an array
#  - :concurrency (defaults to 1)
#  - :timeout
#  - :environment (corresponds to RAILS_ENV for the Sidekiq worker)
guard 'sidekiq', concurrency: 4, queue: %w[critical default low] do
  watch(%r{^workers/(.+)\.rb$})
  watch(%r{^jobs/(.+)\.rb$})
end
