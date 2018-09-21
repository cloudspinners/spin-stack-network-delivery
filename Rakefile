require 'rake/clean'
require 'cloudspin/stack/rake'
require 'cloudspin/stack/artefact'

CLEAN.include('work')
CLEAN.include('build')
CLEAN.include('dist')
CLOBBER.include('state')


include Cloudspin::Stack::Rake

environment_list = ['sandbox', 'test', 'staging']

namespace :statebucket do
  environment_list.each { |environment|
    namespace "#{environment}" do
      StackTask.new(
        environment,
        definition_folder: '../spin-stack-s3bucket/src',
        configuration_files: [
          './stack-statebucket-defaults.yaml',
          './stack-statebucket-local.yaml',
          "environments/stack-statebucket-#{environment}.yaml"
        ]
      )
    end
  }

  namespace :all do
    desc 'Dry run for all of the statebuckets'
    task :dry => environment_list.map { |env| "statebucket:#{env}:dry" }

    desc 'Plan all of the statebuckets'
    task :plan => environment_list.map { |env| "statebucket:#{env}:plan" }

    desc 'Up for all of the statebuckets'
    task :up => environment_list.map { |env| "statebucket:#{env}:up" }

    desc 'Down for all of the statebuckets'
    task :down => environment_list.map { |env| "statebucket:#{env}:down" }
  end
end

namespace :account do
  StackTask.new(
    definition_folder: '../spin-stack-iam-roles/src',
    configuration_files: [
      './stack-account-defaults.yaml',
      './stack-account-local.yaml'
    ]
  )
end

namespace :pipeline do
  StackTask.new(
    definition_folder: '../spin-stack-codepipeline/src',
    configuration_files: [
      './stack-pipeline-defaults.yaml',
      './stack-pipeline-local.yaml'
    ]
  )
end
