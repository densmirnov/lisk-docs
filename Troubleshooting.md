### This is a place holder for the troubleshooting document to come.

# Troubleshooting issues with Lisk

The purpose of this document is to provide clarification around various issues that may be encountered during the installation and normal execution of Lisk.

##Table of Contents

#### Installation Issues

##### Prerequisite Checks Failed
##### Postgres Database Initialization Failed
##### Lisk Failed to Start

#### Runtime Issues

##### error: Forever detected script was killed by signal: SIGKILL


#### Fork Issues

##### Cause 1

```
info 2016-04-04 21:32:10 Fork { delegate: 'a48bad28661406130df30ea016ed1f59561a6200500000a4db86ed45436f46fc',
  block:
   { id: '14121550417616863194',
     timestamp: 31282330,
     height: 442544,
     previousBlock: '15794674487202740739' },
  cause: 1 }
```

`Fork Cause 1` indicates the node has received a block which has the correct block height, but the previous block id is different, hence a fork is detected. In this circumstance there are two outcomes.

1. The node will discard the block and continue searching for the correct block.
2. If the node forged the forking block, it will retain the block and be forked indefinitely.

Mitigation

In the instance of the second error, the node will continue to search other nodes in vain unable to find a common block. The node will need to be rebuilt using `bash lisk.sh rebuild`


##### Cause 2

##### Cause 3

```
error 2016-04-05 07:23:54 Can't verify slot: 17900401180294209692
log 2016-04-05 07:23:54 Failed to load blocks, ban 60 min 45.55.201.46:7000
info 2016-04-05 07:24:03 Check blockchain on 85.25.194.204:7000
info 2016-04-05 07:24:13 Check blockchain on 13.76.247.171:7000
info 2016-04-05 07:24:13 Looking for common block with 13.76.247.171:7000
info 2016-04-05 07:24:14 Found common block 15747904430520320645 (at 446243) with peer 13.76.247.171:7000
info 2016-04-05 07:24:14 Fork { delegate: '45bc77403105fffe822314b305ac7ca6db7a4acf10e56bb4e02cfd22a43b3385',
  block:
   { id: '17900401180294209692',
     timestamp: 31321330,
     height: 446244,
     previousBlock: '15747904430520320645' },
  cause: 3 }
```

`Fork Cause 3` indicates that there is corruption in the database is missing voting data. This means that when processing the next block in the chain it expects one delegate to have forged the block for that slot and timestamp. In this case that expectation is wrong.

Mitigation

This error is unrecoverable. The node will need to be rebuilt using `bash lisk.sh rebuild`

##### Cause 5

```
{"level":"info",
"message":"Fork",
"timestamp":"2016-04-04 01:30:40",
"data": {"delegate":"018dac7f44fe0a48b21dccb8b5f10f3053e4496aa19ad31dbe9dc8c216c3222f",
"block": {"id":"13079141691421699733",
"timestamp":31188640,
"height":433968,
"previousBlock":"10210628004998625034"},
"cause":5}}
```

`Fork Cause 5` indicates that the node has received a block from a peer which failed to validate due to the block id not matching with nodes expected next block id even though the block is at the right height and the previous block data matches.

In the case of a `Fork Cause 5` the block is not validated and given a confirmation like a normal block. It is discarded instead.

Mitigation

There is none, this is an informational error only.

## error: Forever detected script was killed by signal: SIGKILL

```
error: Forever detected script was killed by signal: SIGKILL
module.js:489
    throw err;
          ^
SyntaxError: /home/lisk/lisk-main/config.json: Unexpected token #
    at Object.parse (native)
    at Object.Module._extensions..json (module.js:486:27)
    at Module.load (module.js:355:32)
    at Function.Module._load (module.js:310:12)
    at Module.require (module.js:365:17)
    at require (module.js:384:17)
    at c (/home/lisk/lisk-main/app.js:1:216)
    at b (/home/lisk/lisk-main/app.js:1:1107)
    at Object.<anonymous> (/home/lisk/lisk-main/app.js:10:22075)
    at c (/home/lisk/lisk-main/app.js:1:477)
error: Forever detected script exited with code: 1
```
                                                     
