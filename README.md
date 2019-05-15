### solr-import-export-json

# Import/Export (or Restore/Backup) a Solr collection from/to a json file.

As the title states, this little project will help you to save your collection in json format and restore it where and when you need.

Please report issues at https://github.com/freedev/solr-import-export-json/issues

### Build

To build this project, [Java 8](https://www.azul.com/downloads/zulu/), [Git](https://git-scm.com/) and [Gradle](https://gradle.org/) are required.

```bash
    git clone https://github.com/freedev/solr-import-export-json.git
    cd solr-import-export-json
    gradle build
``` 

The steps above will create the distributions in the `./build/distributions/` directory.


### Install

This tool requires only Java 8 (e.g. Zulu OpenJDK).

Just copy to the target machine the distribution produced in the build task above(or download the distribution from GitHub) and unpack it.

### Usage

From the application home (where the distribution was unpacked), run for Unix systems:
```bash
./bin/solr-import-export --help
```
or run for Windows: 
```bash
bin\solr-import-export.bat --help
```

A list of **available** command line parameters:


     usage: myapp [-a <arg>] [-d] [-D] [-f <arg>] [-h] [-k <arg>] [-o <arg>]
        [-s <arg>] [-S <arg>]
     solr-import-export-json

     -a,--actionType <arg>           action type [import|export|backup|restore]
     -b,--blockSize <arg>            block size (default 5000 documents)
     -c,--commitDuringImport <arg>   Commit progress after specified number of
                                     docs. If not specified, whole work will
                                     be committed.
     -d,--deleteAll                  delete all documents before import
     -D,--dryRun                     dry run test
     -f,--filterQuery <arg>          filter Query during export
     -F,--dateTimeFormat <arg>       set custom DateTime format (default YYYY-MM-dd'T'HH:mm:sss'Z' )
     -h,--help                       help
     -i,--includeFields <arg>        simple comma separated fields list to be
                                     used during export. If not specified all,
                                     the existing fields are used
     -k,--uniqueKey <arg>            specify unique key for deep paging
     -o,--output <arg>               output file
     -s,--solrUrl <arg>              Solr url
     -S,--skipFields <arg>           comma separated fields list to skip during
                                     export/import, this field accepts start and end
                                     wildcard *. So you can specify skip all fields
                                     starting with name_*
     -t,--timeZone <arg>             set custom TimeZone as required by
                                     'TimeZone.getTimeZone()'(default 'UTC' )
     -x,--skipCount <arg>            Number of documents to be skipped when
                                     loading from file. Useful when an error
                                     occurs, so loading can continue from last
                                     successful save.



### Real life examples

Export all documents into a json file

    ./bin/solr-import-export -s http://localhost:8983/solr/collection -a export -o /tmp/collection.json

Export all documents into a json file using a different timeZone using IDs as defined by: https://docs.oracle.com/javase/7/docs/api/java/util/TimeZone.html#getAvailableIDs()

    ./bin/solr-import-export -s http://localhost:8983/solr/collection -a export -t "Europe/Berlin" -o /tmp/collection.json

Import documents from json

    ./bin/solr-import-export -s http://localhost:8983/solr/collection -a import -o /tmp/collection.json 

Export part of documents, like adding a `fq`  Solr parameter to the export

     ./bin/solr-import-export -s http://localhost:8983/solr/collection -a export -o /tmp/collection.json --filterQuery field:value

Import documents from json but first delete all documents in the collection

     ./bin/solr-import-export -s http://localhost:8983/solr/collection -a import -o /tmp/collection.json --deleteAll

Export documents and skip few fields. In the example the will be skipped the fields: `field1_a`, all the fields starting with `field2_` and all the fields ending with `_date`

     ./bin/solr-import-export -s http://localhost:8983/solr/collection -a export -o /tmp/collection.json --skipFields field1_a,field2_*,*_date

Import documents, skip first 49835000 records from file, commit every 200000 documents, block size 5000 (faster than default 500) 

    ./bin/solr-import-export -s http://localhost:8983/solr/collection -a import -o /tmp/collection.json -x 49835000 -c 200000 -b 5000 
