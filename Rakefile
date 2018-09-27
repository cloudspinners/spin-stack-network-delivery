require 'rake/clean'
require 'cloudspin/stack/rake'
require 'cloudspin/stack/artefact'

CLEAN.include('work')
CLEAN.include('build')
CLEAN.include('dist')
CLEAN.include(
  Dir['state/**/*'].
    select { |d| File.directory? d }.
    select { |d| (Dir.entries(d) - %w[ . .. ]).empty? }
)
CLOBBER.include('state')


include Cloudspin::Stack::Rake

environment_list = ['sandbox', 'test', 'staging']

namespace :network do
  environment_list.each { |environment|
    namespace "#{environment}" do
      network_stack = StackTask.new(
        environment,
        role: 'network',
        definition_location: '../spin-stack-network/src'
      ).instance
      InspecTask.new(stack_instance: network_stack)
    end
  }
end

namespace :statebucket do
  StackTask.new(
    role: 'statebucket',
    definition_location: '../spin-stack-s3bucket/src',
    configuration_files: [
      './stack-statebucket-defaults.yaml',
      './stack-statebucket-local.yaml'
    ]
  )
end

namespace :account do
  StackTask.new(
    role: 'account',
    definition_location: '../spin-stack-iam-roles/src'
  )
end

namespace :pipeline do
  StackTask.new(
    role: 'pipeline',
    definition_location: '../spin-stack-codepipeline/src'
  )
end
