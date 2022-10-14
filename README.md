# WinForms MSSQL - SQLite Connection
## WinForms
### 1. Подключить NuGet MSSQL
```
System.Data.SqlClient Author Microsoft
```
![NuGet System Data SqlClient](https://user-images.githubusercontent.com/98191494/190898120-92db2611-72c9-4d4d-92bb-f1baccc7cc98.PNG)

### 2. Сделать класс для работы с БД MSSQL
```C#
public class DB
{
public SqlConnection sqlConnection = new SqlConnection(@"Data Source=Test\SQLEXPRESS;Initial Catalog=NameDataBase;Integrated Security=True");
  
  public SqlConnection GetConnection()
  {
    return sqlConnection;
  }
  
  public DataTable Query(string sqlQuery)
  {
    SqlDataAdapter adapter = new SqlDataAdapter();
    DataTable table = new DataTable();
    SqlCommand command = new SqlCommand(sqlQuery, GetConnection());
    adapter.SelectCommand = command;
    adapter.Fill(table);
    return table;
  }
  
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
}
```

### 3. Строка подключения
```C#
SqlConnection sqlConnection = new SqlConnection(@"Data Source=Test\SQLEXPRESS;Initial Catalog=NameDataBase;Integrated Security=True");
```

### 4. Вернуть строку подключения
```C#
public SqlConnection GetConnection()
{
  return sqlConnection;
}
```

### 5. Функция для выполнения запроса
```C#
public DataTable Query(string sqlQuery)
{
  SqlDataAdapter adapter = new SqlDataAdapter();
  DataTable table = new DataTable();
  SqlCommand command = new SqlCommand(sqlQuery, GetConnection());
  adapter.SelectCommand = command;
  adapter.Fill(table);
  return table;
}
```

### 6. Функция для вывода таблицы на Grid
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

## Пример кода
### Авторизация логин пароль
![AuthButton](https://user-images.githubusercontent.com/98191494/190898404-0c50e0dc-f615-4608-a991-265fa67d62c7.PNG)
```C#
if (textBoxLogin.Text.Length > 0)
{
  if (textBoxPassword.Text.Length > 0)
  {
    string query = $"SELECT Login, Password FROM Accounts WHERE Login = '{textBoxLogin.Text}' AND Password = '{textBoxPassword.Text}'";
    DataTable dt = dataBase.Query(query);

      if (dt.Rows.Count > 0)
      {
        MessageBox.Show("Вход выполнен");
      }
   }
}
```


### Регистрация
![RegButton](https://user-images.githubusercontent.com/98191494/190898546-cfe0a1ea-6398-4518-8381-6b9e2e73d471.PNG)

```C#
try
{
  string query = $"INSERT INTO Accounts VALUES ('{textBoxRegLogin.Text}', '{textBoxRegPass.Text}')";
  dataBase.Query(query);
  MessageBox.Show($"{textBoxRegLogin.Text} зарегистрирован");
}
catch (Exception ex)
{
    MessageBox.Show(ex.Message);
}
```


## Подключение SQLite
### 1.Подключить NuGet SQLite
```
System.Data.SQLite
```
![NuGet System Data SQLite](https://user-images.githubusercontent.com/98191494/195941990-567c41cf-c1cb-4d6a-8767-be879587714d.PNG)

### 2. Сделать класс для работы с БД SQLite
```C#
public class DB
{
  SQLiteConnection connection = new SQLiteConnection("Data Source=AuthUser.db;");

  SQLiteConnection GetConnection()
  {
    return connection;
  }

public DataTable Query(string sqlQuery)
{
  SQLiteDataAdapter adapter = new SQLiteDataAdapter();
  DataTable dt = new DataTable();
  SQLiteCommand command = new SQLiteCommand(sqlQuery, GetConnection());
  adapter.SelectCommand = command;
  adapter.Fill(dt);
  return dt;
}

public void Display(string query, DataGridView dgv)
{
  string sql = query;
  SQLiteConnection connection = GetConnection();
  SQLiteCommand command = new SQLiteCommand(sql, connection);
  SQLiteDataAdapter adapter = new SQLiteDataAdapter(command);
  DataTable dt = new DataTable();
  adapter.Fill(dt);
  dgv.DataSource = dt;
  connection.Close();
  }
}
```

### 3. Строка подключения
```C#
SQLiteConnection connection = new SQLiteConnection("Data Source=AuthUser.db;");
```

### 4. Вернуть строку подключения
```C#
SQLiteConnection GetConnection()
{
  return connection;
}
```

### 5. Функция для выполнения запроса
```C#
 public DataTable Query(string sqlQuery)
{
  SQLiteDataAdapter adapter = new SQLiteDataAdapter();
  DataTable dt = new DataTable();
  SQLiteCommand command = new SQLiteCommand(sqlQuery, GetConnection());
  adapter.SelectCommand = command;
  adapter.Fill(dt);
  return dt;
}
```

### 6. Функция для вывода таблицы на Grid
```C#
public void Display(string query, DataGridView dgv)
{
  string sql = query;
  SQLiteConnection connection = GetConnection();
  SQLiteCommand command = new SQLiteCommand(sql, connection);
  SQLiteDataAdapter adapter = new SQLiteDataAdapter(command);
  DataTable dt = new DataTable();
  adapter.Fill(dt);
  dgv.DataSource = dt;
  connection.Close();
}
```
