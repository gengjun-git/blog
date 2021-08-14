## Big Metadata: When Metadata is Big Data

### Big Query uses a distributed meta system to store its meta data, and has the following salient properties:
- Consistent

- Infinite scalable: supports tables of multiple petabytes size

- Performant: complexity is of the order of number of comlumns in a table rather than the total amount of data in it

### METADATA MANAGEMENT

- One table corresponds to one CMETA table

- Each column is maping to one CMETATYPE

- CMETA (metadata table) is organized such that each row corresponds to a block in the table
