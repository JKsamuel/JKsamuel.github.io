---
title: Fecthing a data from local database
tags: 
 - C#
 - ADO.NET
 - SqlDataAdapter
 - WindowsForm
 - .NET Framework
description: This was one of the assignments from my course. I fill out this page to review and recall what I wrote and studied.
---

# C#: SQL Data Access

* ADO.NET: ADO.NET is based on the ActiveX Data Object (ADO). These are NET data access classes.
* Entity Framework(EF): A professional ORM framework for.NET, the Entity Framework (EF) produces intermediate classes that map relational data, SQL, and object-oriented languages.
  * ORM: Object-Relational Mapping
* LINQ: In order to make ORM easier to use, LINQ TO SQL supports the subset function of the Entity Framework and makes ORM mapping classes easy for LINQ.

## Using SqlDataAdapter class

**The SqlDataAdapter class** imports data from SQL Server to the client, disconnects it, and makes the data available for use. After importing data, **SqlDataReader is a connected mode feature**, but **SqlDataAdapter is a disconnected mode mechanism**. The SqlDataAdapter stores the imported data in the DataSet, which is a memory-mapped data object. This **DataSet object** can be used as a data binding source for grid-like controls (for example, the **Data GridView controls** in WinForms). That is, data is sprayed into the grid screen from the DataSet object's table data.

![assignment5](/assets/img/lab5bGif.gif)

## Prepare a Database
I looked through the database that was provided to me in order to complete school assignments. The database is about Doctor Who, a popular BBC series, that includes the names, ages, photos, companies, and episode information of previous Doctor Who actors. This database is made up of three tables.
   1. DOCTOR: DOCTORID, ACTOR, SERIES, AGE, DEBUT, PICTURE)
   2. COMPNION: NAME, ACTOR, DOCTORID, STORYID
   3. EPISODE: STORYID, SEASON, SEASONYEAR, TITLE

## Create Classes
I created 4 Classes including Connect, Doctor, Companion, and Episode.

```c#
//Coonect.cs
internal class Connect
{
    string connectionString = @"Data Source=.\SQLEXPRESS; Initial Catalog=LocalDataBase; Integrated Security=True";

    public DataSet GetInformation(string table)
    {
        DataSet dataset = new DataSet();

        sqlConnection connection = new SqlConnection(connectionString);
        connection.Open();

        string sql = $"SELECT * FROM {table}";

        // Initial dataset
        SqlDataAdapter adapter = new SqlDataAdapter(sql, connection);

        // Return DataSet usign Fill() method
        adapter.Fill(dataset);

        connection.Close();

        return dataset;
    }
}
```

## Form1.cs
```csharp
// Form1_Load event handler
private void Form1_Load(object sender, EventArgs e)
    {
        Connect connect = new Connect();

        DataSet dataSetDoctor = connect.GetInformation(DOCTOR);
        DataTable doctorTable = dataSetDoctor.Tables[0];
        foreach(DataRow dataRow in doctorTable.Rows)
        {
            Doctor doctor = new Doctor(dataRow["DOCTORID"], dataRow["ACTOR"], dataRow["SERIES"], dataRow["AGE"], dataRow["DEBUT"], dataRow["PICTURE"]);
            doctors.Add(doctor);
            cmbID.Items.Add(dataRow["DOCTORID"]);
            byte[] photo = (byte[])dataRow["Picture"]; 
            MemoryStream stream = new MemoryStream(photo); 
            Image image = Image.FromStream(stream);
            doctor.Picture = image;
        }

        DataSet dataSetCompanion = connect.GetInformation(COMPANION);
        DataTable companionsTable = dataSetCompanion.Tables[0];
        foreach (DataRow dataRow in companionsTable.Rows)
        {
            Companion companion = new Companion(dataRow["NAME"], dataRow["ACTOR"], dataRow["DOCTORID"], dataRow["STORYID"]);
            companions.Add(companion);
        }

        DataSet dataSetEpisode = connect.GetInformation(EPISODE);
        DataTable episodeTable = dataSetEpisode.Tables[0];
        foreach (DataRow dataRow in episodeTable.Rows)
        {
            Episode episode = new Episode(dataRow["STORYID"], dataRow["SEASON"], dataRow["SEASONYEAR"], dataRow["TITLE"]);
            episodes.Add(episode);
        }
    }

private void cmbID_SelectedIndexChanged(object sender, EventArgs e)
    {
        listOutput.Items.Clear();
        int index = cmbID.SelectedIndex;
        inputInformation(index);
    }

// Create a method for combobox event
private void inputInformation(int index)
{
    int series = (int)doctors[index].Series;
    int age = (int)doctors[index].Age;
    object docNumber = index + 1; 

    var yearEpisode =
        from company in companions
        join doc in doctors on company.DoctorID equals doc.DoctorID
        join ep in episodes on company.StoryID equals ep.StoryID
        where doc.DoctorID == doctors[index].DoctorID
        select new { ep.SeasonYear, ep.Title, company.Name, company.Actor };
    
    int size = yearEpisode.Count();
    int[] yearSeason = new int[size];
    string[] firstEpisode = new string[size];
    
    int i = 0;

    foreach(var history in yearEpisode)
    {
        string firstLine = $"{history.Name} ({history.Actor})";
        string secondLine = $"'{history.Title}' ({history.SeasonYear})";
        string thirdLine = "";
        listOutput.Items.Add(firstLine);
        listOutput.Items.Add(secondLine);
        listOutput.Items.Add(thirdLine);
        
        yearSeason[i] = (int)history.SeasonYear;
        firstEpisode[i] = (string)history.Title;
        i++;
    }
    doctorPictureBox.Image = (Image)doctors[index].Picture;
    txtActorName.Text = (string)doctors[index].ActorName;
    txtYear.Text = yearSeason[0].ToString();
    txtSeries.Text = series.ToString();
    txtAge.Text = age.ToString();
    txtTitle.Text = (string)firstEpisode[0];
    
}
```