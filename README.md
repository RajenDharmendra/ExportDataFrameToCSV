# ExportDataFrameToCSV
Export data-frame from Apache Spark
Apache Spark is a great tool for working with a large amount of data like terabytes and petabytes in a cluster. It’s also very useful in local machine when gigabytes of data do not fit your memory. Normally we use Spark for preparing data and very basic analytic tasks. However, it is not advanced analytical features or even visualization. So, therefore, you have to reduce the amount of data to fit your computer memory capacity. It turns out that Apache Spark still lack the ability to export data in a simple format like CSV.

                df.write.
                    format("com.databricks.spark.csv").
                    option("header", "true").
                save("myfile.csv")
 
It also support reading from csv file with similar API
   
   
              val df = sqlContext.read.format("com.databricks.spark.csv").option("header", "true").option("inferSchema", 
              "true").load("file.csv")
              
              
You could also write some custom code to create the output string using mkString, but it won't be safe if you encounter special characters and won't be able to handle quote, etc..

              df.map(x => x.mkString("|")).saveAsTextFile("file.csv") 
   
This code creates a directory myfile.csv with several CSV files and metadata files. If you need single CSV file, you have to implicitly create one single partition.



                  df.write.
                      repartition(1).
                      format("com.databricks.spark.csv").
                      option("header", "true").
                  save("myfile.csv")
                  
