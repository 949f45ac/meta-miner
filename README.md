# meta-miner
Meta Miner: allows to add algo switching support to *any* stratum miner.

Does not add any extra mining fees.

## Check mm.js builtin help

```
Usage: mm.js [<config_file.json>] [options]
Adding algo switching support to *any* stratum miner
<config_file.json> is file name of config file to load before parsing options (mm.json by default)
Config file and options should define at least one pool and miner:
Options:
        --pool=<pool> (-p):             <pool> is in pool_address:pool_port format, where pool_port can be <port_number> or ssl<port_number>
        --host=<hostname>:              defines host that will be used for miner connections (localhost 127.0.0.1 by default)
        --port=<number>:                defines port that will be used for miner connections (3333 by default)
        --user=<wallet> (-u):           <wallet> to use as pool user login (will be taken from the first miner otherwise)
        --pass=<miner_id>:              <miner_id> to use as pool pass login (will be taken from the first miner otherwise)
        --perf_<algo_class>=<hashrate>  Sets hashrate for perf <algo_class> that is: cn/r, cn/2, cn, cn/half, cn/rwz, cn/zls, cn/double, cn/gpu, cn-pico, cn-lite, cn-heavy, rx/wow
        --algo_min_time=<seconds>       Sets <seconds> minimum time pool should keep our miner on one algo (0 default, set higher for starting miners)
        --miner=<command_line> (-m):    <command_line> to start smart miner that can report algo itself
        --<algo>=<command_line>:        <command_line> to start miner for <algo> that can not report it itself
        --watchdog=<seconds> (-w):      restart miner if is does not submit work for <seconds> (600 by default, 0 to disable)
        --hashrate_watchdog=<percent>:  restart miner if is hashrate dropped below <percent> value of of its expected hashrate (0 by default to disable)
        --miner_stdin:                  enables stdin (input) in miner
        --quiet (-q):                   do not show miner output during configuration and also less messages
        --verbose (-v):                 show more messages
        --debug:                        show pool and miner messages
        --log=<file_name>:              <file_name> of output log
        --no-config-save:               Do not save config file
        --help (-help,-h,-?):           Prints this help text
```

Check https://github.com/xmrig/xmrig-proxy/blob/master/doc/STRATUM_EXT.md#14-algorithm-names-and-variants for list of possible algo names.

## Sample mm.json (to use with xmrig v2.10.0+ located in the same directory)

```
{
 "miner_port": 3333,
 "pools": [
   "gulf.moneroocean.stream:10001"
 ],
 "algos": {
   "cn/r": "./xmrig --config=config.json",
   "cn/0": "./xmrig --config=config.json",
   "cn/1": "./xmrig --config=config.json",
   "cn/2": "./xmrig --config=config.json",
   "cn/xtl": "./xmrig --config=config.json",
   "cn/msr": "./xmrig --config=config.json",
   "cn/xao": "./xmrig --config=config.json",
   "cn/rto": "./xmrig --config=config.json",
   "cn/half": "./xmrig --config=config.json",
   "cn/wow": "./xmrig --config=config.json",
   "cn/rwz": "./xmrig --config=config.json",
   "cn/zls": "./xmrig --config=config.json",
   "cn/double": "./xmrig --config=config.json",
   "cn/gpu": "./xmrig --config=config-gpu.json",
   "cn-lite/1": "./xmrig --config=config-lite.json",
   "cn-lite/0": "./xmrig --config=config-lite.json",
   "cn-heavy/0": "./xmrig --config=config-heavy.json",
   "cn-heavy/xhv": "./xmrig --config=config-heavy.json",
   "cn-heavy/tube": "./xmrig --config=config-heavy.json",
   "cn-pico/trtl": "./xmrig --config=config-pico.json",
   "rx/wow": "./xmrig --config=config-randomwow.json",
 },
 "algo_perf": {
   "cn/r": 34,
   "cn/2": 37.4,
   "cn": 36.3,
   "cn/half": 73.5,
   "cn/gpu": 6,
   "cn/rwz": 49.6,
   "cn/zls": 49.6,
   "cn/double": 18.7,
   "cn-pico": 394.7,
   "cn-lite": 114.9,
   "cn-heavy": 18.2
 },
 "user": "44qJYxdbuqSKarYnDSXB6KLbsH4yR65vpJe3ELLDii9i4ZgKpgQXZYR4AMJxBJbfbKZGWUxZU42QyZSsP4AyZZMbJBCrWr1",
 "pass": "x",
 "log_file": null,
 "watchdog": 600,
 "hashrate_watchdog": 0
}
```

## General configuration guidelines

* Configure your miners to connect to the single localhost:3333 (non SSL/TLS) pool.

* For best results separate xmr-stak CPU and GPU miners (by using --noCPU, --noAMD, --noNVIDIA options).

* Prepare your miner config files that give the best performance for your hardware on cryptonight, cryptonight-lite, cryptonight-heavy and cryptonight-pico algorithm classes.

* If you have several miners on one host use mm.js --port option to assign them to different ports.

* Additional mm.js pools will be used as backup pools.

* To rerun benchmark for specific algorithm class use --perf_<algo_class>=0 option.

## Usage examples on Windows

Place mm.exe or mm.js (with nodejs installed) into unpacked miner directory either by:

* Download and unpack the latest mm-vX.X.zip from https://github.com/MoneroOcean/meta-miner/releases

* Download and install nodejs using https://nodejs.org/dist/v8.11.3/node-v8.11.3-x64.msi installator and download and unpack https://raw.githubusercontent.com/MoneroOcean/meta-miner/master/mm.js


### Usage example with xmrig-amd on Windows

* Download and unpack the lastest xmrig-amd (https://github.com/xmrig/xmrig-amd/releases/download/v2.14.1/xmrig-amd-2.14.1-msvc-win64.zip).

* Modify config.json file in xmrig-amd directory this way and adjust it for the best threads performance (out of scope of this guide):

	* Set "url" to "localhost:3333"
	* Set "user" to "44qJYxdbuqSKarYnDSXB6KLbsH4yR65vpJe3ELLDii9i4ZgKpgQXZYR4AMJxBJbfbKZGWUxZU42QyZSsP4AyZZMbJBCrWr1" (put your Monero address)

* Copy config.json to config-lite.json, put "algo" to "cryptonight-lite/1" in config-lite.json and adjust it for the best threads performance (out of scope of this guide).

* Copy config.json to config-heavy.json, put "algo" to "cryptonight-heavy/0" in config-heavy.json and adjust it for the best threads performance (out of scope of this guide).

* Copy config.json to config-pico.json, put "algo" to "cryptonight-pico/trtl" in config-pico.json and adjust it for the best threads performance (out of scope of this guide).

* Copy config.json to config-gpu.json, put "algo" to "cryptonight/gpu" in config-gpu.json and adjust it for the best threads performance (out of scope of this guide).

* Run Meta Miner (or use "node mm.js" instead of mm.exe):

```shell
mm.exe -p=gulf.moneroocean.stream:10001 -m="xmrig-amd.exe --config=config.json" -m="xmrig-amd.exe --config=config-heavy.json" -m="xmrig-amd.exe --config=config-lite.json" -m="xmrig-amd.exe --config=config-pico.json" -m="xmrig-amd.exe --config=config-gpu.json" -m="xmrig-amd.exe --config=config.json"
```

### Usage example with xmr-stak (AMD only) on Windows

* Download and unpack the lastest xmr-stak (https://github.com/fireice-uk/xmr-stak/releases/download/2.9.0/xmr-stak-win64-2.9.0.zip).

* Configure xmr-stak this way (put your Monero address):

```
- Please enter the currency that you want to mine:
  ...
cryptonight_r
- Pool address: e.g. pool.example.com:3333
localhost:3333
- Username (wallet address or pool login):
44qJYxdbuqSKarYnDSXB6KLbsH4yR65vpJe3ELLDii9i4ZgKpgQXZYR4AMJxBJbfbKZGWUxZU42QyZSsP4AyZZMbJBCrWr1
```

* Enable hashrate output by setting "verbose_level" to 4 in config.txt so it can be collected by mm.js (also set "flush_stdout" to true for older xmr-stak versions).

* Copy amd.txt to amd-lite.txt, amd-heavy.txt, amd-gpu.txt and amd-pico.txt and adjust all of them for the best threads performance (out of scope of this guide).

* Run Meta Miner (or use "node mm.js" instead of mm.exe):

```shell
mm.exe -p=gulf.moneroocean.stream:10001 --cn/r="xmr-stak.exe --noCPU --currency cryptonight_r --amd amd.txt" --cn/2="xmr-stak.exe --noCPU --currency cryptonight_v8 --amd amd.txt" --cn/msr="xmr-stak.exe --noCPU --currency cryptonight_masari --amd amd.txt" --cn-lite/1="xmr-stak.exe --noCPU --currency cryptonight_lite_v7 --amd amd-lite.txt" --cn-heavy/0="xmr-stak.exe --noCPU --currency cryptonight_heavy --amd amd-heavy.txt" --cn-heavy/xhv="xmr-stak.exe --noCPU --currency cryptonight_haven --amd amd-heavy.txt" --cn-heavy/tube="xmr-stak.exe --noCPU --currency cryptonight_bittube2 --amd amd-heavy.txt" --cn/gpu="xmr-stak.exe --noCPU --currency cryptonight_gpu --amd amd-gpu.txt" --cn-pico/trtl="xmr-stak.exe --noCPU --currency cryptonight_turtle --amd amd-pico.txt"
```
* To run Meta Miner for xmr-stak CPU/GPU use this command (need to create cpu-*.txt configs for CPU in this case as well based on cpu.txt with adjusted thread configuration):

```shell
mm.exe -p=gulf.moneroocean.stream:10001 --cn/r="xmr-stak.exe --noCPU --currency cryptonight_r --amd amd.txt" --cn/2="xmr-stak.exe --currency cryptonight_v8 --cpu cpu.txt --amd amd.txt" --cn/msr="xmr-stak.exe --currency cryptonight_masari --cpu cpu.txt --amd amd.txt" --cn-lite/1="xmr-stak.exe --currency cryptonight_lite_v7 --cpu cpu-lite.txt --amd amd-lite.txt" --cn-heavy/0="xmr-stak.exe --currency cryptonight_heavy --cpu cpu-heavy.txt --amd amd-heavy.txt" --cn-heavy/xhv="xmr-stak.exe --currency cryptonight_haven --cpu cpu-heavy.txt --amd amd-heavy.txt" --cn-heavy/tube="xmr-stak.exe --currency cryptonight_bittube2 --cpu cpu-heavy.txt --amd amd-heavy.txt" --cn/gpu="xmr-stak.exe --currency cryptonight_gpu --cpu cpu-gpu.txt --amd amd-gpu.txt" --cn-pico/trtl="xmr-stak.exe --currency cryptonight_turtle --cpu cpu-pico.txt --amd amd-pico.txt"
```

## Usage examples on Linux (Ubuntu 16.04)

Get node and Meta Miner (mm.js) in the miner directory:

```shell
sudo apt-get update
sudo apt-get install -y nodejs-legacy
wget https://raw.githubusercontent.com/MoneroOcean/meta-miner/master/mm.js
chmod +x mm.js
```

### Usage example with xmrig on Linux

* Get xmrig:

```shell
wget https://github.com/xmrig/xmrig-amd/releases/download/v2.14.1/xmrig-amd-2.14.1-xenial-x64.tar.gz
tar xf xmrig-2.14.1-xenial-amd64.tar.gz
cd xmrig-2.14.1/
```

* Prepare configs for different algorithms (put your Monero address):

```shell
sed -i 's/"url": *"[^"]*",/"url": "localhost:3333",/' config.json
sed -i 's/"user": *"[^"]*",/"user": "44qJYxdbuqSKarYnDSXB6KLbsH4yR65vpJe3ELLDii9i4ZgKpgQXZYR4AMJxBJbfbKZGWUxZU42QyZSsP4AyZZMbJBCrWr1",/' config.json
cp config.json config-heavy.json
cp config.json config-lite.json
cp config.json config-pico.json
cp config.json config-gpu.json
cp config.json config-randomwow.json
sed -i 's/"algo": *"[^"]*",/"algo": "cryptonight-heavy\/0",/' config-heavy.json
sed -i 's/"algo": *"[^"]*",/"algo": "cryptonight-lite\/1",/' config-lite.json
sed -i 's/"algo": *"[^"]*",/"algo": "cryptonight-pico\/trtl",/' config-pico.json
sed -i 's/"algo": *"[^"]*",/"algo": "cryptonight\/gpu",/' config-gpu.json
sed -i 's/"algo": *"[^"]*",/"algo": "randomx\/wow",/' config-randomwow.json
```

* Run Meta Miner:

```shell
./mm.js -p=gulf.moneroocean.stream:10001 -m="./xmrig --config=config.json" -m="./xmrig --config=config-heavy.json" -m="./xmrig --config=config-lite.json" -m="./xmrig --config=config-pico.json" -m="./xmrig --config=config-gpu.json"
```

### Usage example with xmr-stak (CPU only) on Linux

* Get xmr-stak:

```shell
wget https://github.com/fireice-uk/xmr-stak/releases/download/2.9.0/xmr-stak-linux-2.9.0-cpu.tar.xz
tar xf xmr-stak-linux-2.9.0-cpu.tar.xz
cd xmr-stak-linux-2.9.0-cpu
```

* Configure xmr-stak this way (put your Monero address):

```
- Please enter the currency that you want to mine:
  ...
cryptonight_r
- Pool address: e.g. pool.example.com:3333
localhost:3333
- Username (wallet address or pool login):
44qJYxdbuqSKarYnDSXB6KLbsH4yR65vpJe3ELLDii9i4ZgKpgQXZYR4AMJxBJbfbKZGWUxZU42QyZSsP4AyZZMbJBCrWr1
```

* Enable hashrate output so it can be collected by mm.js and disable output flush (for older xmr-stak versions):

```shell
sed -i 's/"verbose_level" : 3,/"verbose_level" : 4,/' config.txt
sed -i 's/"flush_stdout" : false,/"flush_stdout" : true,/' config.txt
```

* Prepare and adjust configs for different algorithms:

```shell
cp cpu.txt cpu-lite.txt
cp cpu.txt cpu-heavy.txt
cp cpu.txt cpu-pico.txt
cp cpu.txt cpu-gpu.txt
```

* Run Meta Miner:

```shell
./mm.js -p=gulf.moneroocean.stream:10001 --cn/2="./bin/xmr-stak --currency cryptonight_v8 --cpu cpu.txt" --cn/msr="./bin/xmr-stak --currency cryptonight_masari --cpu cpu.txt" --cn-lite/1="./bin/xmr-stak --currency cryptonight_lite_v7 --cpu cpu-lite.txt" --cn-heavy/0="./bin/xmr-stak --currency cryptonight_heavy --cpu cpu-heavy.txt" --cn-heavy/xhv="./bin/xmr-stak --currency cryptonight_haven --cpu cpu-heavy.txt" --cn-heavy/tube="./bin/xmr-stak --currency cryptonight_bittube2 --cpu cpu-heavy.txt" --cn/half="./bin/xmr-stak --currency cryptonight_v8_half --cpu cpu.txt" --cn/gpu="./bin/xmr-stak --currency cryptonight_gpu --cpu cpu-gpu.txt" --cn-pico/trtl="./bin/xmr-stak --currency cryptonight_turtle --cpu cpu-pico.txt"
```

## Developer Donations

If you'd like to make a one time donation, the addresses are as follows:

* XMR - ```44qJYxdbuqSKarYnDSXB6KLbsH4yR65vpJe3ELLDii9i4ZgKpgQXZYR4AMJxBJbfbKZGWUxZU42QyZSsP4AyZZMbJBCrWr1```
* AEON - ```WmsEg3RuUKCcEvFBtXcqRnGYfiqGJLP1FGBYiNMgrcdUjZ8iMcUn2tdcz59T89inWr9Vae4APBNf7Bg2DReFP5jr23SQqaDMT```
* ETN - ```etnkQMp3Hmsay2p7uxokuHRKANrMDNASwQjDUgFb5L2sDM3jqUkYQPKBkooQFHVWBzEaZVzfzrXoETX6RbMEvg4R4csxfRHLo1```
* SUMO - ```Sumoo1DGS7c9LEKZNipsiDEqRzaUB3ws7YHfUiiZpx9SQDhdYGEEbZjRET26ewuYEWAZ8uKrz6vpUZkEVY7mDCZyGnQhkLpxKmy```
* GRFT - ```GACadqdXj5eNLnyNxvQ56wcmsmVCFLkHQKgtaQXNEE5zjMDJkWcMVju2aYtxbTnZgBboWYmHovuiH1Ahm4g2N5a7LuMQrpT```
* MSR - ```5hnMXUKArLDRue5tWsNpbmGLsLQibt23MEsV3VGwY6MGStYwfTqHkff4BgvziprTitbcDYYpFXw2rEgXeipsABTtEmcmnCK```
* ITNS - ```iz53aMEaKJ25zB8xku3FQK5VVvmu2v6DENnbGHRmn659jfrGWBH1beqAzEVYaKhTyMZcxLJAdaCW3Kof1DwTiTbp1DSqLae3e```
* WOW - ```Wo3yjV8UkwvbJDCB1Jy7vvXv3aaQu3K8YMG6tbY3Jo2KApfyf5RByZiBXy95bzmoR3AvPgNq6rHzm98LoHTkzjiA2dY7sqQMJ```
* XMV - ```4BDgQohRBqg2wFZ5ezYqCrNGjgECAttARdbh1fNkuAbd3HnNkSgas11QD9VFQMzbnvDD3Mfcky1LAFihkbEYph5oGAMLurw```
* RYO - ```RYoLsi22qnoKYhnv1DwHBXcGe9QK6P9zmekwQnHdUAak7adFBK4i32wFTszivQ9wEPeugbXr2UD7tMd6ogf1dbHh76G5UszE7k1```
* XTL - ```Se3Qr5s83AxjCtYrkkqg6QXJagCVi8dELbHb5Cnemw4rMk3xZzEX3kQfWrbTZPpdAJSP3enA6ri3DcvdkERkGKE518vyPQTyi```
* XHV - ```hvxyEmtbqs5TEk9U2tCxyfGx2dyGD1g8EBspdr3GivhPchkvnMHtpCR2fGLc5oEY42UGHVBMBANPge5QJ7BDXSMu1Ga2KFspQR```
* TUBE - ```bxcpZTr4C41NshmJM9Db7FBE5crarjaDXVUApRbsCxHHBf8Jkqjwjzz1zmWHhm9trWNhrY1m4RpcS7tmdG4ykdHG2kTgDcbKJ```
* LOKI - ```L6XqN6JDedz5Ub8KxpMYRCUoQCuyEA8EegEmeQsdP5FCNuXJavcrxPvLhpqY6emphGTYVrmAUVECsE9drafvY2hXUTJz6rW```
* TRTL - ```TRTLv2x2bac17cngo1r2wt3CaxN8ckoWHe2TX7dc8zW8Fc9dpmxAvhVX4u4zPjpv9WeALm2koBLF36REVvsLmeufZZ1Yx6uWkYG```
* BTC - ```3BzvMuLStA388kYZ9nudfm8L22937dSPS3```
* BCH - ```qrhww48p5s6zw9twhc7cujgwp7vym2k4vutem6f92p```
* ETH - ```0xCF8BABC074C487Ae17F9Ce0394eab492E6A35658```
* LTC - ```MCkjQo99VzoeZQ1piDzLDb4uqNSDRZpx55```
