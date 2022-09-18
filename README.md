# MSSQL-Connection
## WinForms WPF
### 1. Подключить NuGet MSSQL
```
System.Data.SqlClient Author Microsoft
```
![NuGet System Data SqlClient](https://user-images.githubusercontent.com/98191494/190898120-92db2611-72c9-4d4d-92bb-f1baccc7cc98.PNG)

### 2. Строка подключения
```C#
SqlConnection sqlConnection = new SqlConnection(@"Data Source=Test\SQLEXPRESS;Initial Catalog=NameDataBase;Integrated Security=True");
```

### 3. Вернуть строку подключения
```C#
public SqlConnection GetConnection()
{
  return sqlConnection;
}
```

### 4. Функция для выполнения запроса
```C#
public DataTable Query(string sqlQuery)
{
  SqlDataAdapter adapter = new SqlDataAdapter();
  ataTable table = new DataTable();
  SqlCommand command = new SqlCommand(sqlQuery, GetConnection());
  adapter.SelectCommand = command;
  adapter.Fill(table);
  return table;
}
```

### 5. Функция для вывода таблицы на Grid
```C#
public void Display(string query, DataGridView dgv)
{
  string sql = query;
  SqlConnection connection = GetConnection();
  SqlCommand command = new SqlCommand(sql, connection);
  SqlDataAdapter adapter = new SqlDataAdapter(command);
  DataTable dt = new DataTable();
  adapter.Fill(dt);
  dgv.DataSource = dt;
  connection.Close();
}
```

![AuthButton](https://user-images.githubusercontent.com/98191494/190898404-0c50e0dc-f615-4608-a991-265fa67d62c7.PNG)
![RegButton](https://user-images.githubusercontent.com/98191494/190898546-cfe0a1ea-6398-4518-8381-6b9e2e73d471.PNG)
