# Object storage vs. block storage: How are they different?

>[!TIP]
>This is an optional reading for the AZ-104 exam.

Object storage and block storage are two types of cloud storage — meaning, remote data storage that can be accessed via an Internet connection. Object storage is highly scalable and customizable, but not always fast. Block storage is fast, but usually more expensive than object storage. Which one better fits an organization's use case depends on a number of factors. Overall, object storage is typically used for large volumes of unstructured data, while block storage works best with transactional data and small files that need to be retrieved often.

Think of block storage as a compact parking garage with valet parking, and object storage as a massive, open parking lot with acres of spaces. The Block Storage Garage, as we can call it, allows drivers to quickly retrieve their cars; but it has limited space for vehicles, and expanding capacity would involve constructing a new garage and hiring more valets, which is expensive. The Object Storage Lot, in contrast, allows as many drivers to park as desired. However, some of the cars may end up at the far end of the parking lot, and it could take some time for drivers to retrieve them.

## How object storage and block storage work

Block storage divides files and data into equally sized blocks. Each block has a unique identifier, stored in a data lookup table. When data needs to be retrieved, the data lookup table is used to find the required blocks, which are then reassembled into their original form.

Think of it this way: the data lookup table is like the key box where valets keep keys for each car. When a driver needs their car, the valet grabs the key and looks up where the car is in order to retrieve it quickly. Similarly, block storage uses unique identifiers stored in the data lookup table to rapidly find and retrieve data.

Block storage is fast, and it is often preferred for applications that regularly need to load data from the backend.

Object storage is a method for saving large volumes of unstructured data, including sensor data, audio files, logs, video and photo content, webpages, and emails. Each file or segment of data is saved as an "object," and each object includes metadata and a unique name or identifier for data retrieval. (Imagine how a driver might write down their space number in a large parking lot in order to remember where their vehicle is.)

All objects are stored together in a "data lake" (also called a "data pool"). Data lakes are flat — there is no file hierarchy, just as a large parking lot is flat, with no ramps or additional levels.

>[!IMPORTANT]
>[Object Storage vs Block Storage](https://www.cloudflare.com/en-gb/learning/cloud/object-storage-vs-block-storage/)
