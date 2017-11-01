# nifi-pravega 
Persist and read streaming data with Nifi and Pravega for data burst

#
# Abstract 
#
Purpose of the project is to use very fast and low latency storage primitives of Pravega (software defined storage) to 
persist streaming data. 

The use case in mind is to handle data storms for streams that would cause the system to overload and/or d+ropping events or panic.

Pravega acts as flow buffer that controls the ingest into the event processing system and guarentees continuity without any data loss.

Nifi has build-in capabilites to manage and protects the data in repositories with local storage. While Nifi-Pravege provides survival for
data ingest while data is exploding before it is ingested. 

Note: To protect NIFI repositories on local storage other software defined storage primitives like ScaleIO can be used. 

#
# Concept
#

Data Flow:

Source data --> Nifi/Minifi Pravega Processor (flood gate) --> Nifi Processors transfering / routing --> store into a sink or database 

The Nifi Pravega Processor has configurable (static, DistributedMapCache or file) flood gate open/close and feed back rate (events per second).

The Pravega processor measures the ingest and opens the flood gate if the events per second of data hitting the system is reached and then sends all data to Pravega. After open the flood gate the porcessor start to sequentially feed back the data a the healthy rate to Nifi. 

After the data burst is over and all data in Pravega is digested by Nifi (back to normal) the flood gate is closed. 
#
# Streams
#



