# heads-vagrant
Build heads easily using vagrant


## Usage guide
* Install vagrant and a virtualization provider of your choice (virtualbox, hyperv and libvirt are supported)
* Clone this repository
* Run `vagrant up` in repo
* After heads rom is built shut down the build VM

## Notes
* By default the VM builds heads for Thinkpad X230. If you want to build heads for a different platform provide `board` argument to `vagrant up` like this: `vagrant --board=BOARD up`. You can find a list of supported boards [here](https://github.com/osresearch/heads/tree/master/boards)
