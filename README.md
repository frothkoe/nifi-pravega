# nifi-pravega 
Persist and read streaming data with Nifi and Pravega for data burst

#
# Abstract 
#
Purpose of the project is to use very fast and low latency storage primitives of Pravega (software defined storage see: http://pravega.io)
to persist streaming data. 

The use case in mind is to handle data storms for streams that would cause the system to overload and/or dropping events or panic.

This custom Nifi Processor for Pravega acts as flow buffer that controls the ingest into the stream processing system (Nifi) and 
guarentees continuity without data loss and strict ordering.

Nifi has build-in capabilites to manage and protects the data in repositories with local storage. While Nifi-Pravege provides survival for
data ingest while data is exploding before it is ingested. 

Note: To protect NIFI repositories on local storage other software defined storage primitives like ScaleIO can be used. 

#
# Concept
#

Data Flow:

Source data --> Nifi/Minifi Pravega Processor (flood gate) --> Nifi Processors transfering / routing --> store into a sink or database 

The Nifi Pravega Processor has configurable properties for the flood gate to open/close and controle rate (events per second).

The Pravega processor measures the ingest and opens the flood gate if the events per second of data hitting the system is reached 
and starts sending all data to Pravega. After open the flood gate the porcessor start to sequentially feed back the data a the healthy rate to Nifi. Pravega manages the data storage.

After the data burst is over and all data in Pravega is digested by Nifi (back to normal) the flood gate is closed. 
#
# Streams
#



