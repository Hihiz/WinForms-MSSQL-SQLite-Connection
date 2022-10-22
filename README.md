# WinForms MSSQL - SQLite Connection
## WinForms ADO .NET &  EF CORE6
## ADO .NET
### 1. Подключить NuGet MSSQL
```
System.Data.SqlClient Author Microsoft
```
![NuGet System Data SqlClient](https://user-images.githubusercontent.com/98191494/190898120-92db2611-72c9-4d4d-92bb-f1baccc7cc98.PNG)

### 2. (Вариант 1) Сделать класс для работы с БД MSSQL
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

### 2. (Вариант 2) Без класса для работы с БД MSSQL
```C#
private void Form1_Load(object sender, EventArgs e)
{
  // Строка подключения
  string connectionString = "Data Source=название сервера;Initial Catalog=название бд;Trusted_Connection=True;";

  // Запрос
  string sqlQuery = "SELECT * FROM Users";

  using (SqlConnection sqlConnection = new SqlConnection(connectionString))
  {
    sqlConnection.Open();
    string cmd = sqlQuery;
    SqlCommand command = new SqlCommand(cmd, sqlConnection);
    SqlDataAdapter adapter = new SqlDataAdapter(command);
    DataTable dt = new DataTable();
    adapter.Fill(dt);
    // Вывод на грид
    dataGridViewUser.DataSource = dt;
    sqlConnection.Close();
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

### 2. (Вариант 1) Сделать класс для работы с БД SQLite
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

### 2. (Вариант 2) Без класса для работы с БД SQLite
```C#
private void Form1_Load(object sender, EventArgs e)
{
  // Строка подключения
  string connectionString = "Data Source=название бд.db;";

  // Запрос
  string sqlQuery = "SELECT * FROM Users";

  using (SQLiteConnection sqlConnection = new SQLiteConnection(connectionString))
  {
    sqlConnection.Open();
    string cmd = sqlQuery;
    SQLiteCommand command = new SQLiteCommand(cmd, sqlConnection);
    SQLiteDataAdapter adapter = new SQLiteDataAdapter(command);
    DataTable dt = new DataTable();
    adapter.Fill(dt);
    // Вывод на грид
    dataGridViewUser.DataSource = dt;
    sqlConnection.Close();
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

# EF CORE6
### 1. Подключить NuGet MSSQL или SQLite
```
Microsoft.EntityFrameworkCore.SqlServer
```
![ef1](https://user-images.githubusercontent.com/98191494/195999536-0dea49bc-c8fe-4a93-b3c9-0dcb9401e508.PNG)


```
Microsoft.EntityFrameworkCore.Sqlite
```
![ef2](https://user-images.githubusercontent.com/98191494/195999550-b46235f3-8629-4243-aa4c-1bc381fe4271.PNG)


## 2. При подходе DataBase First - Scaffold, нужно подключить пакет
```
Microsoft.EntityFrameworkCore.Tools
```
Microsoft.EntityFrameworkCore.Tools - необходим для создания классов по базе данных, то есть reverse engineering

### 3. Создание классов по базе данных
SQLite
```
Scaffold-DbContext "DataSource=название бд.db;" Microsoft.EntityFrameworkCore.Sqlite
```

MSSQL
```
Scaffold-DbContext "Data Source=название сервера;Initial Catalog=название БД;" Microsoft.EntityFrameworkCore.SqlServer
```

## При подходе Code First
### 1. Создать класс модели
```C#
public class User
{
  public long Id { get; set; }
  public string Login { get; set; }
  public string Password { get; set; }
  public string PasswordCopy { get; set; }
}
```

### 2. Создать класс контекста данных
```C#
public class ApplicationContext : DbContext
{
  public ApplicationContext()
  {
    // Database.EnsureDeleted();
   Database.EnsureCreated();
  }

  public DbSet<Users> users { get; set; }

  protected override void OnConfiguring(DbContextOptionsBuilder optionsBuilder)
  {
      // SQLite
      optionsBuilder.UseSqlite("Data Source=название бд.db");
      
      // MSSQL
      //optionsBuilder.UseSqlServer("Data Source=название сервера;Initial Catalog=название бд;Trusted_Connection=True;");
  }
}
```

### 3. Вывод данных из таблицы
```C#
private void Form1_Load(object sender, EventArgs e)
{
  using (AuthUserContext db = new AuthUserContext())
  {
    var users = db.Users.ToList();
    dataGridView1.DataSource = users;
  }
}
```

### 4. Добавление данных
```C#
AuthUserContext db = new AuthUserContext();

User user = new User
{
  Login = textBoxLogin.Text,
  Password = textBoxPassword.Text,
  PasswordCopy = textBoxPassword.Text
};

db.Users.Add(user);
db.SaveChanges();

MessageBox.Show($"Пользователь: {textBoxLogin.Text} добавлен");
```

### 5. Сравнение данных
```C#
using (AuthUserContext db = new AuthUserContext())
{
  if (db.Users.FirstOrDefault(user => user.Login == textBoxLogin.Text && user.Password == textBoxPassword.Text) != null)
  {
    MessageBox.Show("Вход выполнен", "Успешно");
  }
  else
  {
    MessageBox.Show("Неверный логин или пароль", "Ошибка");
  }
}
```


