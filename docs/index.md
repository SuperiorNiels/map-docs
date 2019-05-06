# Whisper Southband API

## Commands

### Whisper dio

	whisper dio <target_id> <source_id> <rank> <whisper_node>

	* <target_id>: id of the node the dio should be sent to (hex).
	* <source_id>: id of the node the dio should be from (hex).
	* <rank>: rank to sent.
	* <whisper_node>: root or number

Examples:
>whisper dio 1 2 5000 root

Will execute the command on the root node: a dio will be sent to node 1.

>whisper dio 1 2 5000 4

Will execute the command on the whisper node with id 4: a dio will be sent to node 1.

### Whisper sixtop

#### Sixtop Add
	whisper 6p add <target_id> <source_id> <cellOptions> <cellInfo> <whisper_node>

	* <target_id>: id of the node the command should be sent to (hex).
	* <source_id>: id of the node the command should be from (hex).
	* <cellOptions>: TX or RX or TXRX
	* <cellInfo>: random or cell <slotOffset> <channel>
	* <whisper_node>: number

Examples:
>whisper 6p add 2 3 TX cell 10 10 4

Command will be sent to whisper node with id 4, a 6p add command will be sent to 2 and 3 will be impersonated. On 2 a TX cell will be added with slot offset 10 and channel 10.

>whisper 6p add 2 3 RX random 4

A random RX cell will be added on 2.

#### Sixtop Delete
	whisper 6p delete <target_id> <source_id> <cellOptions> <cellInfo> <whisper_node>

	* <target_id>: id of the node the command should be sent to (hex).
	* <source_id>: id of the node the command should be from (hex).
	* <cellOptions>: TX or RX or TXRX
	* <cellInfo>: random or cell <slotOffset> <channel>
	* <whisper_node>: number


#### Sixtop List
	whisper 6p list <target_id> <source_id> <cellOptions> <max_nr_cells> <listing_offset> <whisper_node>

	* <target_id>: id of the node the command should be sent to (hex).
	* <source_id>: id of the node the command should be from (hex).
	* <cellOptions>: TX or RX or TXRX
	* <max_nr_cells>: max. number of cells to list.
	* <listing_offset>: listing offset.
	* <whisper_node>: number

#### Sixtop Clear
	whisper 6p clear <target_id> <source_id> <whisper_node>

	* <target_id>: id of the node the command should be sent to (hex).
	* <source_id>: id of the node the command should be from (hex).
	* <whisper_node>: number

### Whisper link testing

	whisper link <node1> <node2>

	* <node1>: node the ping request will be sent to.
	* <node2>: node the ping request should pass before reaching node1.


## Change the firmware of nodes in openvisualizer
The firmware of each node can be changed by editing the file: **openvisualizer/bin/openVisualizerApp.py** (line 90)

In the follwing piece of code the first node (0) is started with the root firmware, the 4 node (3) is started with the whisper node firmware, the other nodes start with the normal OpenWSN firmware.
```python
if self.simulatorMode:
    # in "simulator" mode, motes are emulated
    sys.path.append(os.path.join(self.datadir, 'sim_files'))
    import openwsn.oos_openwsn
    import whisper_node.oos_openwsn
    import whisper_root.oos_openwsn

    self.moteProbes       = []
    for i in range(self.numMotes):
        if i == 0:
            # Start a whisper root node
            MoteHandler.readNotifIds(
                os.path.join(self.datadir, 'sim_files', 'whisper_root', 'openwsnmodule_obj.h'))
            moteHandler = MoteHandler.MoteHandler(whisper_root.oos_openwsn.OpenMote())
            self.simengine.indicateNewMote(moteHandler)
            self.moteProbes += [moteProbe.moteProbe(emulatedMote=moteHandler)]
            print "Started a whisper root node"
        elif i == 3:
            # Start a whisper node
            MoteHandler.readNotifIds(
                os.path.join(self.datadir, 'sim_files', 'whisper_node', 'openwsnmodule_obj.h'))
            moteHandler = MoteHandler.MoteHandler(whisper_node.oos_openwsn.OpenMote())
            self.simengine.indicateNewMote(moteHandler)
            self.moteProbes += [moteProbe.moteProbe(emulatedMote=moteHandler)]
            print "Started a whisper node"
        else:
            # Start a normal openwsn node
            MoteHandler.readNotifIds(
                os.path.join(self.datadir, 'sim_files', 'openwsn', 'openwsnmodule_obj.h'))
            moteHandler = MoteHandler.MoteHandler(openwsn.oos_openwsn.OpenMote())
            self.simengine.indicateNewMote(moteHandler)
            self.moteProbes += [moteProbe.moteProbe(emulatedMote=moteHandler)]
            print "Started an openwsn node"
```




