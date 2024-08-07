## [Tópico T04b] - Requisitos de Dados - `BD Livraria Virtual`
###### *by Prof. Plinio Sa Leitao-Junior (INF/UFG)*

### Livro Digital _vs._ Serviço _vs._ Software _vs._ Banco de Dados

:sparkles: **LIVRO DIGITAL**<br>
`Livro digital` (livro eletrônico, _e-book_) é qualquer conteúdo de informação, semelhante a um livro, existindo ou não sua versão em papel, cujo conteúdo é codificado em formato digital, que pode ser lido em equipamentos eletrônicos.

:sparkles: **SERVIÇO**<br>
`Livraria Virtual` é um serviço prestado àqueles que desejam ter a facilidade e a praticidade no manuseio e na leitura de livros digitais. Basicamente, o usuário pode alugar ou comprar (o mesmo que alugar por tempo indeterminado) livros digitais, para lê-los usando um _e-reader_<sup>(\*)</sup>.<br>
<sup>(\*)</sup> _e-reader é um dispositivo eletrônico móvel projetado para ler e-books e periódicos digitais_.

:sparkles: **SOFTWARE**<br>
Para viabilizar o serviço **Livraria Virtual**, vários _softwares_ serão necessários, tais como: software para _e-reader_, software para manutenção (aquisição, descarte, etc.) do acervo digital, software para a gestão financeira do serviço, dentre outros.

:sparkles: **BANCO DE DADOS**<br>
O fornecimento do serviço requer um banco de dados, capaz de viabilizar as funcionalidades dos diversos _softwares_ utilizados.

<hr style="border:2px solid blue">

### Perfis de Usuário

:star2: `Associado`<br>
O usuário que primariamente utiliza o serviço é chamado de ASSOCIADO (pessoa física), aquele interessado na leitura de livros digitais. Basicamente, representa a pessoa que se associa ao serviço, mediante um valor monetário pago mensalmente, onde tal concede o direito de uso do serviço.

:star2: `Editora`<br>
O usuário que fornece os livros digitais é chamado EDITORA, visando a incorporá-los ao acervo de livros do serviço.

:star2: `Gestor`<br>
O usuário responsável pela gestão administrativa e financeira do serviço é denominado GESTOR.

<hr style="border:2px solid blue">

### Demandas Informacionais

:star2: `Associado`<br>
Quantos são os recortes do livro X ?<br>
Quais os meus recortes no livro X ?<br>
Quais os títulos mais adquiridos [no período X] ?<br>
Quais os títulos mais alugados no [período X] ?<br>
Quais os títulos abandonados no [período X] ?<br>
Quais os títulos mais desejados no [período X] ?<br>
Quais os títulos com melhores avaliações no [período X] ?<br>

:star2: `Editora`<br>

:star2: `Gestor`<br>
Quais as sessões de uso em cada software no [período X] ?<br>

#### Conceitos
Livro digital<br>
Equipamentos eletrônicos<br>
Livraria virtual<br>
Alugar ou comprar<br>
e-reader, software manutenção, software gestão<br>
associado, editora, gestor<br>
associado aluga e compra livro digital<br>
livro e recorte de livro<br>
período X<br>
títulos mais adquiridos no período X<br>
títulos mais alugados no período X<br>
- SELECT L.ISBN , L.Titulo, COUNT(*) As QtdeAluguel<br>
FROM TRANSACAO AS T JOIN LIVRO A L<br>
            ON T.ISBN = L.ISBN<br>
WHERE Tipo = "A"  (Same as DataHoraFim IS NOT NULL)<br>
AND      DataHoraInic BETWEEN :Data1 AND :Data2<br>
GROUP BY L.ISBN , L.Titulo<br>
ORDER BY QtdeAluguel DESC

títulos abandonados no período X<br>
títulos mais desejados no período X<br>
títulos com melhores avaliações no período X<br>

### DER Livraria Virtual

Arquivo EERCASE [Aqui](https://github.com/plinioleitao/bd-2024-1-bcc/blob/main/media/bd-2024-1-der-livraria-virtual.eer)

<img src="../media/bd-2024-1-der-livraria-virtual.jpg" width="600">

|Esquema Lógico|
|-|
|USUARIO (Ident, TipoUsuario, Nome, Email, SenhaCripto)<br>USUARIO (Ident) IS PRIMARY KEY|
|ASSOCIADO (Ident, Nome, Email, SenhaCripto, CPF)<br>ASSOCIADO (Ident) IS PRIMARY KEY<br>ASSOCIADO (Ident) REFERENCES USUARIO (Ident)|
|LIVRO (ISBN, Titulo, Edicao, NumPag, AnoPublic)<br>LIVRO (ISBN) IS PRIMARY KEY|
|TRANSACAO (Ident, ISBN, DataHoraInic, DataHoraFim)<br>TRANSACAO (Ident) IS PRIMARY KEY<br>TRANSACAO (ISBN) REFERENCES LIVRO (ISBN)|
|EDITORA (Ident, Nome, Email, SenhaCripto, CNPJ)<br>EDITORA (Ident) IS PRIMARY KEY<br>EDITORA (Ident) REFERENCES USUARIO (Ident)|
|RECORTE  (IdentAssociado, DataHora, PagInic, PosInic, PagFim, PosFim, ISBN)<br>RECORTE  (IdentAssociado, DataHora) IS PRIMARY KEY<br>RECORTE  (IdentAssociado) REFERENCES ASSOCIADO (Ident)<br>RECORTE (ISBN) REFERENCES LIVRO (ISBN)|
|AVALIACAO (IdentTransacao, DataHora, Comentario, NumEstrelas)<br>AVALIACAO (IdentTransacao) IS PRIMARY KEY<br>AVALIACAO (IdentTransacao) REFERENCES Transacao (ident)|
