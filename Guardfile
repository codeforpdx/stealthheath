guard :minitest, spring: "bin/rails test", all_on_start: false do
  watch(%r{^test/(.*)/?(.*)_test\.rb$})

  watch('test/test_helper.rb')   { 'test' }
  watch('config/routes.rb')      { interface_tests }
  watch(%r{app/views/layouts/*}) { interface_tests }

  watch(%r{^app/models/(.*?)\.rb$}) do |matches|
    "test/models/#{matches[1]}_test.rb"
  end

  watch(%r{^app/controllers/(.*?)_controller\.rb$}) do |matches|
    resource_tests(matches[1])
  end

  watch(%r{^app/views/([^/]*?)/.*\.html\.erb$}) do |matches|
    [
      "test/controllers/#{matches[1]}_controller_test.rb"
    ] + integration_tests(matches[1])
  end

  watch(%r{^app/helpers/(.*?)_helper\.rb$}) do |matches|
    integration_tests(matches[1])
  end
end

  def integration_tests(resource = :all)
    if resource == :all
      Dir["test/integration/*"]
    else
      Dir["test/integration/#{resource}_*.rb"]
    end
  end

  # Returns all tests that hit the interface.
  def interface_tests
    integration_tests << "test/controllers/"
  end

  def controller_test(resource)
    "test/controllers/#{resource}_controller_test.rb"
  end

  # Returns all tests for the given resource.
  def resource_tests(resource)
    integration_tests(resource) << controller_test(resource)
  end
