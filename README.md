# htmltab2stata
           ----------------------------------------------------------------------------------------------------------------
          help htmltab2stata                                                                         v. 1.0 - January 2019
          ----------------------------------------------------------------------------------------------------------------
          Title

              htmltab2stata - Converting html tables into a Stata dataset.

          Contents

              Syntax
              Description
              Options
              Examples
              About


          Syntax

              The general syntax is:

                htmltab2stata , url(url) [tablenumber(integer) firstrow href]

          Options

              url(url) the url of the html website to be processed.  The url has to be a downloadable html website.  url
                  can be a webadress or a local html file.

              tablenumber(integer) number of table within the html document.  Default is 1, i.e. the first table is
                  processed.

              firstrow Use firstrow of table as variable names.

              href Links enclosed in <a href=....></a> are added to the content transferred to Stata.

          Description

              htmltab2stata parses html code from websites.  It detects tables enclosed with the html <table> environment
              and transforms the table into a Stata dataset.  To do so, htmltab2stata parses the html code and uses <tr>
              as row identifiers and code encolosed in <td> as columns.  It only transfers content which is not enclosed
              in < > to Stata, unless option href is used for links.  Empty cells remain empty in the Stata dataset.

          Examples

              For all examples, the following table in html code saved in table.html is processed:

                     Content
                     Table 1
                     <table>
                      <tr><th>Country</th><th>Population</th><th>GDP</th></tr>
                       <tr><td>Country A</td><td>10</td><td>100</td></tr>
                       <tr><td>Country B</td><td>20</td><td>5</td></tr>
                       <tr><td>Country C</td><td>500</td><td>10</td></tr>
                     </table>
                     More Content
                     Table 2
                     <table>
                       <tr><th>Firstname</th><th>Surname</th><th>Webpage</th></tr>
                       <tr><td>Adam</td><td>Smith</td><td>none</td></tr>
                       <tr><td>Allan</td><td>Richards</td><td><a href="www.google.com">webpage</a></td></tr>
                       <tr><td>Richard</td><td>Johnson</td><td></td></tr>
                     </table>
           

              Loading Table into Stata as a dataset:

                     htmltab2stata , url(table.html)

              Returns:

                     .list
                     
                          +---------------------------------+
                          |    myvar1       myvar2   myvar3 |
                          |---------------------------------|
                       1. |   Country   Population      GDP |
                       2. | Country A           10      100 |
                       3. | Country B           20        5 |
                       4. | Country C          500       10 |
                          +---------------------------------+


              To use the first column as variable names the option firstrow is required.

                     htmltab2stata , url(table.html) firstrow

                     .list
                     
                          +----------------------------+
                          |   Country   Popula~n   GDP |
                          |----------------------------|
                       1. | Country A         10   100 |
                       2. | Country B         20     5 |
                       3. | Country C        500    10 |
                          +----------------------------+


              To process Table 2, use the first row as variable names and add the url of the hyperlink as text to the
                  contents:

                     htmltab2stata , url(table.html) firstrow tablenumber(2) href

                     . list
                     
                          +----------------------------------------------+
                          | Firstn~e    Surname                  Webpage |
                          |----------------------------------------------|
                       1. |     Adam      Smith                     none |
                       2. |    Allan   Richards   www.google.com webpage |
                       3. |  Richard    Johnson                          |
                          +----------------------------------------------+


          Author

              Jan Ditzen (Heriot-Watt University)
              Email: j.ditzen@hw.ac.uk
              Web: www.jan.ditzen.net

          Changelog

              This version: 1.0
