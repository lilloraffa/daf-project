# Dataset Services
Dataset is one of the fundamental entities of DAF. Here we discuss the microservices designed to perform operations on Datasets.

## Dataset Catalog Manager
The Catalog Manager is an API manage the creation, update and deletion of datasets in DAF. In particular, it takes care of the metadata information associated to a Dataset ([see here for more info on metadata](https://github.com/lilloraffa/daf-datamgmt/blob/master/datamgmt/metadata/dataset_metadata.md)).

### API Structure
`catalogds/add/{info}` => `CatOpsReport`
It creates a new Dataset in the Dataset Catalog. It returns an object (JSON) of type `CatOpsReport` that provides info on the insert operation. In case the input source of the data has been provided. It perform coherence checks that are necessary to save the new dataset (e.g. check vs an associated StandardSchema).   
`{info}` is an object (JSON) containing metadata info for the Dataset to be created.

`catalogds/update/{dataset URI o ID}/{info}` => `CatOpsReport`
It updates the info of an existing Dataset. It returns an object (JSON) of type `CatOpsReport` that provides info on the update operation.
`{dataset URI o ID}` can be either the dataset URI or its associated ID.  
`{info}` is an object (JSON) containing metadata info for the Dataset to be created.

`catalogds/get/{query info}/[{return info}]` => `List[DatasetCat]`  
It looks for one or more datasets matching the `{query info}` parameters. It is also possible (optional) to specify which info will be retrieved, otherwise the whole dataset info will be passed. It returns a list of `DatasetCat` object, containing metadata info of the Dataset returned.
`{query info}` it is an object (JSON) with the info to be used to look up the dataset from the Catalog.  
`[{return info}]` it is an optional parameter/object (JSON) to restrict the fields to be returned.

## Data Ingestion Manager
The Data Ingestion Manager API takes care of the actual ingestion of data associated to a Dataset (which, as a reminder, is an entity made of a combination of metadata and data). In particular, the API takes as input data and info needed to identify the Dataset to which the data needs to be associated with. Before actually storing the data in DAF, the API performs a set of coherence checks between the metadata contained in the catalogue and the data schema implied in the input data.

`ingestionMgmt/addWithInfo/{DatasetCat}` => `IngestionReport`  
It uses the info contained in the object `DatasetCat` to perform an ingestion of the data referenced in the field `operational -> input_src`. It returns an object (JSON) of type `IngestionReport` containing info on the ingestion process performed and the associated dataset (particularly wrt URI and other calcolated fields that will be needed to finalize the storing of the metadata in the catalog).
`{DatasetCat}` is an object containing catalog info about the dataset to which the incoming data ill be associated.  

`ingestionMgmt/add/{dataset query info}/{input file path}` => `IngestionReport`  
It uses `{dataset query info}` to look up for the dataset in the Catalog. If more than one dataset is found, then an error will be logged and the ingestion will be terminated.  
`{dataset query info}` it is an object (JSON) with the info to be used to look up the dataset from the Catalog.  
`{input file path}` the path of the file containing the data to be ingested. This should normally be placed in the designated entry point folder of HDFS.


## Dataset Manager
The Dataset Manager API manages operations related to the Dataset entity, such as: returning the data  of the dataset (or a sample of it) in a specified format, create a specific view on top of it, get the dataset schema in a given format (e.g. AVRO), create a new dataset based on an existing one but saved into a different storage mechanism or based on a transformation of the existing dataset, etc.

`datasetMgmt/getData/{dataset URI o ID}/[{query type}]/[{output data format}]/[{sql query on data}]` => `DatasetData`  
It retrieves the data of the dataset and allows for options specifying the data format (i.e. JSON, CSV, etc.) of the output, the type of query to be performed (i.e. a bulk operation, a sample, etc.), and a SQL query (other querying languages may be considered for the future) to perform specific queries on the dataset.  
`{dataset URI o ID}` can be either the dataset URI or its associated ID.  
`[{query type}]` is optional and specifies the type of data result wanted. For example, `bulk` will retrieve the entire dataset (access rights to bulk operation should be carefully set), `sample` will retrieve a 5% sample of the entire dataset, `sample10` will retrieve the 10%, `sample100R` will retrieve the first 100 rows, etc.  
`[{output data format}]` is an optional field specifying the data format the output will have, e.g. `json`, `csv`, `avro`, etc.  
`[{sql query on data}]` is optional and allows to specify a SQL query on the data.

`datasetMgmt/feat_jdbc/{enble/disable}` => `DsFeatReport`  
API that enable or disable the feature "Expose a JBDC connector for the dataset"
