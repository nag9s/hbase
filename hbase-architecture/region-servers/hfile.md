HFile represents the real data storage fle. The fles contain a variable number of data

blocks and a fxed number of fle info blocks and trailer blocks. The index blocks

records the offsets of the data and meta blocks. Each data block contains a magic

header and a number of serialized KeyValue instances.

