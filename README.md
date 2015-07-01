Exadata provides a lot of useful metrics to monitor the Cells and you may want to retrieve historical values for some metrics. To do so, you can use the “LIST METRICHISTORY” command through CellCLI on the cell.

But as usual, visualising the metrics is even more better. To do so, I created a perl script that extracts the historical metrics in CSV format so that you can graph them with the visualisation tool of your choice.

**Let’s see the help:**

```sh
Usage: ./csv_exadata_metrics_history.pl [cell=|groupfile=] [serial] [type=] [name=] [objectname=] [name!=] [objectname!=] [ago_unit=] [ago_value=]

 Parameter                 Comment                                                      Default
 ---------                 -------                                                      -------
 CELL=                     comma-separated list of cells
 GROUPFILE=                file containing list of cells
 SERIAL                    serialize execution over the cells (default is no)
 TYPE=                     Metrics type to extract: Cumulative|Rate|Instantaneous       ALL
 NAME=                     Metrics to extract (wildcard allowed)                        ALL
 OBJECTNAME=               Objects to extract (wildcard allowed)                        ALL
 NAME!=                    Exclude metrics (wildcard allowed)                           EMPTY
 OBJECTNAME!=              Exclude objects (wildcard allowed)                           EMPTY
 AGO_UNIT=                 Unit to retrieve historical metrics back: day|hour|minute    HOUR
 AGO_VALUE=                Value associated to Unit to retrieve historical metrics back 1

utility assumes passwordless SSH from this cell node to the other cell nodes
utility assumes ORACLE_HOME has been set (with celladmin user for example)

Example : ./csv_exadata_metrics_history.pl cell=cell
Example : ./csv_exadata_metrics_history.pl groupfile=./cell_group
Example : ./csv_exadata_metrics_history.pl cell=cell objectname='CD_disk03_cell'
Example : ./csv_exadata_metrics_history.pl cell=cell name='.*BY.*' objectname='.*disk.*'
Example : ./csv_exadata_metrics_history.pl cell=enkcel02 name='.*DB_IO.*' objectname!='ASM' name!='.*RQ.*' ago_unit=minute ago_value=4
Example : ./csv_exadata_metrics_history.pl cell=enkcel02 type='Instantaneous' name='.*DB_IO.*' objectname!='ASM' name!='.*RQ.*' ago_unit=hour ago_value=4
Example : ./csv_exadata_metrics_history.pl cell=enkcel01,enkcel02 type='Instantaneous' name='.*DB_IO.*' objectname!='ASM' name!='.*RQ.*' ago_unit=minute ago_value=4 serial
```

**The main options/features are:**

- You can specify the cells on which you want to collect the metrics thanks to the cell orgroupfile parameter.
- You can choose to serialize the execution over the cells thanks to the serial parameter.
- You can choose the type of metrics you want to retrieve (Cumulative, rate or instantaneous) thanks to the type= parameter.
- You can focus on some metrics thanks to the name= parameter (wildcard allowed).
- You can exclude some metrics thanks to the name!= parameter (wildcard allowed).
- You can focus on some metricobjectname thanks to the objectname= parameter (wildcard allowed).
- You can exclude some metricobjectname thanks to the objectname!= parameter (wildcard allowed).
- You can choose the unit to retrieve metrics back (day, hour, minute) thanks to theago_unit= parameter.
- You can choose the value associated to the unit to retrieve metrics back thanks to theago_value= parameter
