Fluxo de Testes
=======================================================

Antes de dar a tarefa como concluída, antes mesmo de ser aberto a **Pull Request** de sua branch, deve-se tomar o cuidado se o que está sendo entregue atende ao que foi solicitado e também se está tudo funcionando de forma correta.

Um dos meios que utilizamos nos nossos projetos é o desenvolvimento dos testes automatizados, eles por sí só (de desenvolvidos corretamente) asseguram que a entrega está funcional e não impactou o funcionamento do restante do projeto.

Porém, algumas boas práticas são bem-vindas, como as metodologias de teste que serão abordadas abaixo e são recomendadas para evitar que a tarefa retorne.

Teste Unitário
-------------

O teste unitário consiste em verificar as partes pequenas de uma aplicação, seriam os componentes desenvolvidos. Essas partes podem ser: funcionalidade de cadastro, funcionalidade de exportação de planilha.

Exemplo da “Funcionalidade de Cadastro”–Caminho feliz:

- Testar se os campos estão aceitando os valores corretamente. Ex: campo de telefone aceitando somente número.
- Testar se está aplicando a máscara corretamente. Ex: campo de telefone aceitando forma “(xx) x xxxx-xxxx”
- Testar a complementação dos campos. Ex: ao digitar um CEP preencher os campos de “Estado, Cidade e Rua” automaticamente.

Teste de Integração
-------------

O teste de integração consiste em verificar se as funcionalidades continuam a funcionar após a sua integração. Por exemplo: foi desenvolvido a funcionalidade de cadastro, após a sua integração com o banco de dados é verificado se ao finalizar o cadastro foi guardado o registro corretamente.

Exemplo “Funcionalidade de Cadastro –Integração com o BD” – Caminho feliz:

- Testar se foi registrado corretamente o cadastro do usuário. 
- Testar se foi registrado as informações com máscara.

Esse tipo de teste é para comprovar que após fazer a integração os componentes não parem de funcionar.

Teste de Regressão
-------------

Existem ainda o teste de regressão que consiste em fazer uma revalidação. Por exemplo: foi desenvolvido o cadastro do usuário, porém o cliente solicitou uma melhoria no cadastro, uma inclusão de mais dois campos.

O teste de regressão tem o intuito de verificar que após a inclusão dos campos o cadastro não pare de funcionar, verificar se os campos são registrados corretamente no BD também.

Os testes de integração são semelhantes aos testes unitários e testes de integração. Esse tipo de testes é necessário em qualquer melhoria implementada ou quando o tech lead enxerga a necessidade de novos testes.