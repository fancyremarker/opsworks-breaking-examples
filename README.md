# opsworks-breaking-examples

Examples of configurations that break OpsWorks.

## aws/opsworks-cookbooks#97

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
1. Add a layer. In configuring the layer, add "opsworks-breaking-examples::issue97" to the "Configure" section of "Custom Chef Recipes."

## Copyright and License

MIT License, see [LICENSE](LICENSE) for details.

Copyright (c) 2014 [Frank Macreery](https://github.com/fancyremarker).
