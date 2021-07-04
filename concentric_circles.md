# Who is Participaing?

A local community was interested in identifying all the participants in core activities.
We describe how we used SRP to create a spreadsheet to show this.

## SRP Query

To determine the individuals particpating, create a Custom Report "People Engaged in An Activity".
We used the following *Data Filters*:

```
AND
  Archived Is equal to No
  OR
    Currently facilitating A children's class Yes/No Is equal to Yes
    Currently facilitating A junior youth group Yes/No Is equal to Yes
    Currently facilitating A study circle Yes/No Is equal to Yes        
    Currently participating in A children's class Yes/No Is equal to Yes        
    Currently participating in A junior youth group Yes/No Is equal to Yes        
    Currently participating in A study circle Yes/No Is equal to Yes        
    Comments Contains #Host-DM        
```

Not that in this cluster, hosts of devotional gatherings are indicated using the comment "#Host-DM" in the Comment field of individual records.

We included the following items in the *Display Information*:

- First Names(s)
- Family Name
- Age Category
- Registered Baha'i
- Focus Neighborhood
- Locality
- Currently teaching a children's class
- Currently animating a junior youth group
- Currently facilitating a study circle
- Currently particicating in a children's class
- Currently participating in a junior youth group
- Currently participating in a study circle
- Comments

We used the following *Sort Order*:

```
Sort By Focus Neighborhood Ascending
Then By Family Name Ascending
Then By First Name(s) Ascending
```

We chose this order because in reviewing the results, we assumed that it would be easier to think groups of people if they were sorted by focus neighborhood.

## Spreadsheet

### Basic Spreadsheet

Once we ran the Custom Report, we exported the data to Excel.
We cut and pasted the data (including column titles) to a new Google sheet.
We use Google sheets to make it easier to share the final document withe specific individuals.

### Changes to Sheet

We modified the spreadsheet in order to highlight the information as follows:

- Added a column (column N) "Hosting Devotional Gathering" containg "Yes" if the Comment field contains the tag "#Host-DM". This can  be calculated from the Comments column (column M) as follows:

    ```
    =if(REGEXMATCH(M5,".*Host-DM.*"),"Yes","")
    ```

- Hide the `Comments` column (column M); the information of interest in now in the new column N.

- Hid the `Locality` column (column F) becuase the data was intended for a single locality and so was the same for all rows.

- Set *Text Wrapping* to `Wrap` for all column headers and adjust the column widths to be most attractive.

- Insert a row above the column headers using larger text to display the number of individuals in the columns:

    A1: Shows the number of individuals included in the table, calucated as:

    ```
    =COUNTA(A3:A200)
    ```

    C1: Shows the number of children, junior youth, youth, and adults, calculated as:

    ```
    =CONCATENATE(
        COUNTIF(C3:C200,"Child"),
        "/",COUNTIF(C3:C200,"Junior Youth"),
        "/",COUNTIF(C3:C200,"Youth"),
        "/",COUNTIF(C3:C200,"Adult")
    )
    ```

    D1: Shows the number of registered Baha'is calculated as:

    ```
    =COUNTA(D3:D200)
    ```

    G1 through L1: Show the number of individuals facilitating or participating in training instiute activities. These fields are also caclulated using COUNTA.

    N1: Shows the number of individuals hosting a devotional gathering. Because this column was calculated using IF, every entry has a value. Instead of COUNTA, COUNTIF can be used:

    ```
    =COUNTIF(N3:N200, "Yes")
    ```

- Froze the top two rows so that the summary and column headings are always visible using menu options:

```
View > Freeze > 2 rows
```

- Renamed the sheet "Participating and Sustaining

### A Second Sheet

The first sheet captured all currently engaged as participants or facilitators.
A second sheet was created to just capture the facilitators as follows.

- Duplicate the first sheet and renamed the copy as "Sustaining" meant to capture just those facilitating activities.

- Unhide columns F and M. Then delete columns J - M and column F.

- Delete all the data

- Reinsert the data by setting A3 to:

    ```
    =QUERY('Participating and Sustaining'!A3:N200,
    "SELECT A,B,C,D,E,G,H,I,N WHERE G='Yes' OR H='Yes'OR I='Yes'OR N='Yes'")
    ```

This approach is used so that when new data is generated, only the first sheet needs to be updated. The second sheet will be automatically updated.

## Considerations

1. You will update data over time.
To avoid cutting and pasting column by column make siure that the raw data can be copied to a single place in the target spreadsheet. Use forumlae to copy columns to new locations and hide columns you don't want to appear.

2. Here only 200 rows were included in the defined fomulae. This number should be large enough for as many indivudals as you expect.

3. If some columns (such as **Comments**) are considered sensitive, hiding them may not be sufficient.
Instead, create a second spreadsheet using `IMPORTRANGE` and `QUERY` selecting only the data you want to share.
This approach is also helpful to create multiple views over the same data.
For example, to create locality specific views with one query for the whole cluster.
