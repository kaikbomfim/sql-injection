### SQL Injection

SQL Injection é uma técnica de ataque que envolve a inserção de código SQL malicioso em uma entrada de usuário, principalmente em aplicações web, para manipular a execução de consultas SQL. Isso pode levar a acessos não autorizados, exposição de informações confidenciais, corrupção de dados e até mesmo à destruição do banco de dados.

### Caso de Uso

O exemplo abaixo trata-se de um código em Java para autenticação de acesso de usuários em um sistema.

```java
import java.sql.*;

public class AuthenticatorLogin {
    // Método para validar se um usuário e senha estão corretos
    public boolean validarUsuario(String usuario, String senha) {

        // Criando uma string com a consulta SQL para verificar o usuário e a senha
        String query = "SELECT * FROM usuarios WHERE usuario='" + usuario + "' AND senha='" + senha + "'";

        try {
            // Estabelecendo uma conexão com o banco de dados MySQL
            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/banco_de_dados", "usuario", "senha");

            // Criando um objeto de instrução (statement) para enviar consultas SQL ao banco de dados
            Statement statement = connection.createStatement();

            // Executando a consulta SQL e obtendo um conjunto de resultados (ResultSet)
            ResultSet resultSet = statement.executeQuery(query);

            // Verificando se há algum resultado retornado
            return resultSet.next(); // Se houver pelo menos uma linha no resultado, retorna verdadeiro; caso contrário, retorna falso

        } catch (SQLException e) {
            // Em caso de erro ao executar a consulta SQL ou estabelecer conexão com o banco de dados, imprime o erro
            e.printStackTrace();
            return false; // Retorna falso, indicando que a validação falhou devido a um erro
        }
    }
}
```

### Problema:

Neste código, os valores de usuário e senha são diretamente concatenados à string de consulta SQL, o que torna o código vulnerável a ataques de SQL Injection.

### Solução: 

A solução para prevenir ataques de SQL Injection é utilizar consultas parametrizadas. Com consultas parametrizadas, os valores dos parâmetros são tratados separadamente da estrutura da consulta SQL, eliminando assim a possibilidade de manipulação maliciosa dos dados de entrada.

```java
import java.sql.*;

public class AuthenticatorLogin {
    public boolean validarUsuario(String usuario, String senha) {

        // Consulta parametrizada
        String query = "SELECT * FROM usuarios WHERE usuario=? AND senha=?";

        try {
            // Estabelecendo conexão com o banco de dados
            Connection connection = DriverManager.getConnection("jdbc:mysql://localhost:3306/banco_de_dados", "usuario", "senha");

            // Criando um PreparedStatement com a consulta parametrizada
            PreparedStatement preparedStatement = connection.prepareStatement(query);

            // Definindo os parâmetros da consulta
            preparedStatement.setString(1, usuario);
            preparedStatement.setString(2, senha);

            // Executando a consulta
            ResultSet resultSet = preparedStatement.executeQuery();

            // Verificando se há resultados
            return resultSet.next();
        } catch (SQLException e) {
            e.printStackTrace();
            return false;
        }
    }
}
```

Neste código, a consulta SQL foi modificada para usar ? como marcadores de posição para os parâmetros. Em seguida, um objeto PreparedStatement é criado, e os valores dos parâmetros são definidos utilizando métodos apropriados, como setString(). Essa abordagem garante que os valores dos parâmetros sejam tratados de forma segura, reduzindo significativamente o risco de SQL Injection.

