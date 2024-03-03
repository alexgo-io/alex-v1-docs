# Threshold Sampling

Threshold sampling allows Bitcoin Oracle to derive consensus from the network of non-trusted nodes without having to download all the consensus data from its data layer. Bitcoin Oracle engages in several rounds of random sampling for subset of the network and, with each successful round, consensus confidence grows. When Bitcoin Oracle achieves a set confidence threshold, an event is deemed to have reached consensus, subject to validation by all trusted nodes.
