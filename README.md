# opsworks-breaking-examples

Examples of configurations that break OpsWorks.

## [aws/opsworks-cookbooks#97](/aws/opsworks-cookbooks/issues/97)

In this example, a custom cookbooks archive containing Git repositories among its subdirectories (as with almost any archive produced by `berks package`), will fail to include any of these Git repositories among the available cookbooks.

To reproduce the bug:

1. Clone this repository:

        git clone git@github.com:fancyremarker/opsworks-breaking-examples.git
        cd opsworks-breaking-examples/

1. Run `berks package` and upload the resulting file, `package.tar.gz`, to an HTTP URL.
1. Create a new stack in OpsWorks, making sure to set:
    - Default operating system: Ubuntu 12.04 LTS
    - (Advanced) Use custom Chef cookbooks: Yes
    - (Advanced) Repository URL: (the URL from Step 1)
1. Add a layer. In configuring the layer, add "opsworks-breaking-examples::issue97" to the "Setup" section of "Custom Chef Recipes."
1. Add an instance to the layer and start it. The Setup command will fail, and its logs will include a backtrace similar to the following:

        [2014-01-14T16:06:34+00:00] INFO: New Run List expands to ["opsworks_initial_setup", "ssh_host_keys", "ssh_users", "mysql::client", "dependencies", "ebs", "opsworks_ganglia::client", "opsworks_stack_state_sync", "opsworks-breaking-examples::issue97", "deploy::default", "test_suite", "opsworks_cleanup"]
        [2014-01-14T16:06:34+00:00] ERROR: Caught exception while compiling OpsWorks custom run list: Chef::Exceptions::CookbookNotFound - Cookbook opsworks-breaking-examples not found. If you're loading opsworks-breaking-examples from another cookbook, make sure you configure the dependency in your metadata - /opt/aws/opsworks/releases/20131222191101_216/vendor/gems/chef-11.4.4/bin/../lib/chef/cookbook/cookbook_collection.rb:38:in `initialize'
        /opt/aws/opsworks/releases/20131222191101_216/vendor/bundle/ruby/1.8/gems/ohai-6.16.0/lib/ohai/mash.rb:77:in `call'
        /opt/aws/opsworks/releases/20131222191101_216/vendor/bundle/ruby/1.8/gems/ohai-6.16.0/lib/ohai/mash.rb:77:in `default'
        /opt/aws/opsworks/releases/20131222191101_216/vendor/bundle/ruby/1.8/gems/ohai-6.16.0/lib/ohai/mash.rb:77:in `default'
        /opt/aws/opsworks/releases/20131222191101_216/vendor/gems/chef-11.4.4/bin/../lib/chef/run_context/cookbook_compiler.rb:265:in `[]'

## Copyright and License

MIT License, see [LICENSE](LICENSE) for details.

Copyright (c) 2014 [Frank Macreery](https://github.com/fancyremarker).
