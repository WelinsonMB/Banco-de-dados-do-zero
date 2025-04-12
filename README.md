# 🛠️ Projeto de Banco de Dados: Mecânica

Este projeto foi desenvolvido como parte do desafio **"Construa um Projeto Lógico de Banco de Dados do Zero"** da formação SQL no bootcamp da **DIO (Digital Innovation One)**. O objetivo é criar e popular um banco de dados relacional para uma **oficina mecânica**, permitindo a gestão de clientes, veículos, ordens de serviço, equipes, mecânicos, serviços realizados e peças trocadas.

---

## 🧱 Estrutura do Banco de Dados

### 📂 Banco: `mecanica`
Foi criado com a instrução:
```sql
CREATE DATABASE mecanica;
USE mecanica;
```

### 🧾 Tabelas Criadas:

- **cliente**: Dados dos clientes da oficina.
- **veiculo**: Veículos dos clientes.
- **equipe**: Equipes que atuam nas ordens de serviço.
- **mecanico**: Mecânicos que compõem as equipes.
- **equipe_mecanico**: Relacionamento N:N entre mecânicos e equipes.
- **OS (ordem de serviço)**: Registro dos serviços prestados.
- **servico**: Serviços oferecidos pela oficina.
- **servico_OS**: Relação N:N entre ordens de serviço e serviços executados.
- **peca**: Peças utilizadas nas manutenções.
- **peca_OS**: Relação N:N entre ordens de serviço e peças utilizadas.

---

## 📥 População de Dados

Foram inseridos dados fictícios (com auxílio de IA) para fins de testes e consultas SQL:

- 5 clientes com veículos associados
- 2 equipes de mecânicos
- 5 mecânicos com diferentes especialidades
- 5 ordens de serviço
- Serviços e peças relacionados a cada OS

---

## 🔍 Consultas Realizadas

### 📌 Recuperações Simples (SELECT):
```sql
SELECT * FROM cliente;
SELECT Modelo, placa, ano FROM veiculo;
```

### 🎯 Filtros com WHERE:
```sql
SELECT * FROM cliente WHERE Snome = 'Oliveira';
SELECT * FROM veiculo WHERE ano >= 2015;
```

### 🧮 Expressões para Atributos Derivados:
```sql
SELECT 
  c.Pnome AS 'Nome do Cliente',
  v.Modelo AS 'Modelo do Veículo',
  s.DescServico AS 'Descrição do Serviço',
  p.Pnome AS 'Peça Trocada',
  (s.ValorMaoDeObra + p.ValorPeca) AS 'Total Parcial'
FROM cliente c
JOIN veiculo v USING(idCliente)
JOIN OS ord USING(idVeiculo)
JOIN servico_OS so USING(idOS)
JOIN servico s ON s.idServico = so.idServico
JOIN peca_OS po ON ord.idOS = po.idOS
JOIN peca p ON p.idPeca = po.idPeca;
```

### 📊 Ordenações com ORDER BY:
```sql
SELECT * FROM veiculo ORDER BY ano ASC;
```

### 📈 Filtros com HAVING:
```sql
SELECT idEquipe, COUNT(*) AS TotalMecanicos
FROM Equipe_Mecanico
GROUP BY idEquipe
HAVING COUNT(*) > 2;
```

### 🔗 Junções (JOINs):
```sql
SELECT s.DescServico AS 'Descrição do serviço',
       CONCAT(m.Pnome, ' ', m.Snome) AS 'Nome do Mecânico'
FROM servico_OS sos
JOIN servico s USING(idServico)
JOIN OS ON sos.idOS = OS.idOS
JOIN equipe e ON os.idEquipe = e.idEquipe
JOIN Equipe_Mecanico em ON e.idEquipe = em.idEquipe
JOIN mecanico m ON em.idMecanico = m.idMecanico;
```

---

## 🧠 Considerações Finais

Este banco de dados é uma simulação voltada ao aprendizado de SQL e modelagem relacional. Ele cobre conceitos como:

- Entidades e relacionamentos (1:N e N:N)
- Integridade referencial
- Chaves primárias e estrangeiras
- Consultas SQL com junções, filtros, ordenações e agregações

> **Observação:** Todos os dados inseridos são fictícios e foram gerados com o apoio da inteligência artificial para fins didáticos.

