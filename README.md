# Laboratório AI 900 - Azure Cognitive Search: Utilizando AI Search para indexação e consulta de Dados

Olá seja muito bem vindo a mais um laboratório! Neste aprendemos como configurar e realizar pesquisas utilizando o Azure Cognitive Search. 

## 📝 Para que serve o Azure Cognitive Search?

O Azure Cognitive Search é um serviço de pesquisa baseado na nuvem da Microsoft que fornece recursos avançados de pesquisa de texto e de dados. Ele permite que os desenvolvedores incorporem facilmente capacidades avançadas de pesquisa em aplicativos e sites, proporcionando aos usuários a capacidade de encontrar informações relevantes de maneira rápida e eficiente.

## 📝 Principais Recursos do Azure Cognitive Search:

- **Pesquisa Avançada:** Suporta pesquisa em texto livre, pesquisa geoespacial, sugestões de pesquisa, realce de resultados, autocompletar, sinônimos, classificação e paginação.

- **Integração de IA:** Oferece integração opcional de inteligência artificial para extrair informações de texto e estruturar conteúdo.

- **Ferramentas de Desenvolvedor:** Fornece ferramentas e SDKs para várias linguagens de programação, facilitando a integração e o desenvolvimento de aplicativos de pesquisa.

- **Facetas e Filtros Dinâmicos:** Permite aos usuários refinar os resultados da pesquisa por meio de facetas dinâmicas e filtros.

- **Escalabilidade:** Pode lidar com conjuntos de dados de qualquer escala, desde pequenos até muito grandes, com capacidade de dimensionamento automático.

## 📝 Para que ele é usado?

Ele é usado por diversas áreas, dentre elas as 3 principais são:

- **E-commerce:** Aprimorar a experiência do usuário por meio de pesquisa avançada de produtos.

- **Saúde:** Permitir a pesquisa eficiente em registros médicos e documentos clínicos.

- **Jurídico:** Facilitar a descoberta e análise de documentos legais.

## 📝 Como configurar uma pesquisa?

Para configurarmos uma pesquisa no Azure Cognitive Search, primeiro precisamos criar um recurso do Azure AI Search.

**Nota:** Este passo a passo foi feito no portal do Azure traduzido para o Português do Brasil. Se você usou o padrão (inglês) ou outro idioma, alguns nomes podem ser um pouco diferentes, fique a vontade para traduzir se preferir.

Passo a passo: 

1. Entre no [portal do Azure](https://portal.azure.com/).

2. Na página inicial, clique no botão **+ Criar um recurso**, pesquise **Azure AI Search** e crie um recurso Azure AI Search com as seguintes configurações:

    - **Assinatura:** sua assinatura do Azure.
    - **Grupo de recursos:** Selecione um grupo ou crie um novo grupo de recursos com um nome único.
    - **Nome do serviço:** Um nome único.
    - **Localização:** Escolha qualquer região disponível(Recomendo utilizar a região West US, os preços podem ser mais baratos do que no Brasil).
    - **Nível de preços:** Básico.

3. Selecione **Revisar + criar** e depois de ver a resposta **validação realizada com sucesso**, selecione **Criar**.

4. Após a conclusão da implantação, selecione **Ir para o recurso**. Na página de visão geral do Azure AI Search, você pode adicionar índices, importar dados e pesquisar índices criados.

Agora você precisa criar um recurso de serviços de IA do Azure. Você precisará provisionar um recurso de serviços de IA do Azure que esteja no mesmo local que seu recurso do Azure AI Search. Sua solução de pesquisa usará esse recurso para enriquecer os dados no armazenamento de dados com insights gerados por IA.

1. Retorne à [página inicial](https://portal.azure.com/) do portal do Azure. Clique no botão **＋Criar um recurso** e pesquise os **serviços de IA do Azure**. Selecione criar um plano de serviços de IA do Azure . Você será levado a uma página para criar um recurso de serviços de IA do Azure. Configure-o com as seguintes configurações:

    - **Assinatura:** sua assinatura do Azure.
    - **Grupo de recursos:** O mesmo grupo de recursos que seu recurso do Azure AI Search.
    - **Região:** o mesmo local do recurso do Azure AI Search.
    - **Nome:** Um nome exclusivo.
    - **Nível de preços:** Padrão S0
Marcar a caixa, **confirmo que li e compreendi todos os termos abaixo:** Selecionado.

2. Selecione **Revisar + criar.** Depois de ver a resposta **Validação realizada com sucesso**, selecione **Criar**.

3. Aguarde a conclusão da implantação e visualize os detalhes da implantação.

Por último, crie uma conta de armazenamento:

1. Retorne à [página inicial](https://portal.azure.com/) do portal do Azure e selecione o botão **+ Criar um recurso**.

2. Procure conta de armazenamento e crie um recurso **de conta de armazenamento** com as seguintes configurações:

    - **Assinatura**: sua assinatura do Azure.
    - **Grupo de recursos:** O mesmo grupo de recursos que os recursos do Azure AI Search e dos serviços Azure AI.
    - **Nome da conta de armazenamento:** um nome exclusivo.
    - **Localização:** Escolha qualquer localização disponível(Recomendo utilizar a região West US, os preços podem ser mais baratos do que no Brasil).
    - **Desempenho:** Padrão
    - **Redundância:** armazenamento localmente redundante (LRS).

Pronto, sua pesquisa já está configurada.

## 📝 Como carregar documentos para o armazenamento do Azure

1. No painel do menu esquerdo, selecione **Containers**.

2. Selecione **+ Contêiner**. Um painel do seu lado direito é aberto.

3. Insira as seguintes configurações e clique em Criar :
    - **Nome:** O nome padrão virá como "Coffee-Reviews", porém o portal do Azure não irá deixar utilizar esse nome por conter letras maiúsculas e traço, nesse caso renomeie para "coffeereviwes".
    - **Nível de acesso público:** Container (acesso de leitura anônimo para containers e blobs).
    - **Avançado:** sem alterações.

4. Em uma nova guia do navegador, baixe as avaliações de café compactadas clicando [aqui](https://aka.ms/mslearn-coffee-reviews) ou copie e cole o seguinte link "https://aka.ms/mslearn-coffee-reviews" e após extraia os arquivos para a pasta de avaliações que você irá usar.

5. No portal do Azure, selecione o contêiner de avaliações de café . No contêiner, selecione **Carregar**.

6. No painel **Carregar blob**, selecione **Selecionar um arquivo**.

7. Na janela do Explorer, selecione **todos** os arquivos na pasta de avaliações que você acabou de extrair, selecione **Abrir** e, em seguida, selecione **Carregar**.

8. Depois que o upload for concluído, você poderá fechar o painel **Upload blob**. Seus documentos estão agora em seu contêiner de armazenamento de avaliações de café.

## 📝 Como indexar documentos

Depois de armazenar os documentos, você poderá usar o Azure AI Search para extrair insights dos documentos. O portal do Azure fornece um assistente de importação de dados . Com este assistente, você pode criar automaticamente um índice e um indexador para fontes de dados suportadas. Você usará o assistente para criar um índice e importar seus documentos de pesquisa do armazenamento para o índice do Azure AI Search.

1. No portal do Azure, navegue até o recurso Azure AI Search. Na página **Visão geral**, selecione **Importar dados**.

2. Na página **Conectar-se aos seus dados**, na lista **Fonte de Dados**, selecione **Azure Blob Storage**. Preencha os detalhes do armazenamento de dados com os seguintes valores:

    - **Fonte de dados:** Armazenamento de Blobs do Azure
    - **Nome da fonte de dados :** coffee-customer-data
    - **Dados a extrair:** Conteúdo e metadados
    - **Modo de análise:** Padrão
    - **Cadeia de conexão :** *Selecione **Escolha uma conexão existente**. Selecione sua conta de armazenamento, selecione o contêiner de **avaliações de café** e clique em **Selecionar**.
    - **Autenticação de identidade gerenciada:** Nenhuma
    - **Nome do contêiner:** esta configuração é preenchida automaticamente depois que você escolhe uma conexão existente .
    - **Pasta Blob:** deixe em branco .
    - **Descrição:** Avaliações sobre Fourth Coffee Shops.

3. Selecione **Próximo: Adicionar habilidades cognitivas (opcional)**.

4. Na secção **Anexar Serviços Cognitivos**, selecione o seu recurso de serviços Azure AI.

5. Na seção **Adicionar enriquecimentos**:

    - Altere o **ome da qualificação**para **offee-skillset**.
    - Marque a caixa de seleção **abilitar OCR e mesclar todo o texto no campo merged_content**.
        - Observação: É importante selecionar **Habilitar OCR** para ver todas as opções de campo enriquecido.

    - Certifique-se de que o **campo Dados de origem** esteja configurado como **merged_content**.
    - Altere o **nível de granularidade de enriquecimento** para **Páginas (blocos de 5.000 caracteres)**.
    - Não selecione Habilitar enriquecimento incremental.

    Selecione os seguintes campos enriquecidos:

|Habilidade Cognitiva| 	|Parâmetro|	|Nome do campo|
|--------------------|--|---------|-|-------------|
|Extraia nomes de locais|	|| 	|Localizações|
|Extraia frases-chave|	|| 	|rases chave|
|Detectar sentimento|	 ||	|sentimento|
|Gerar tags de imagens|	 ||	|imagemTags|
|Gere legendas de imagens|	|| 	|legenda da imagem|

6. Em **alvar enriquecimentos em um armazenamento de conhecimento**, selecione:
    - Projeções de imagem
    - Documentos
    - Páginas
    - Frases chave
    - Entidades
    - Detalhes da imagem
    - Referências de imagem

7. Selecione **projeções de blob do Azure: Documento**. Uma configuração para o nome do contêiner com as exibições preenchidas automaticamente do contêiner de armazenamento de conhecimento . Não altere o nome do contêiner.

8. Selecione **róximo: Personalizar índice de destino**. Altere o **nome do índice** para **coffee-index**.

9. Certifique-se de que a **chave** esteja configurada como **metadata_storage_path**. Deixe o ** do sugeridor** em branco e o **modo de pesquisa** preenchido automaticamente.

10. Revise as configurações padrão dos campos de índice. Selecione **filtrável** para todos os campos que já estão selecionados por padrão.

11. Selecione **Próximo: Criar um indexador**.

12. Altere o **nome do indexador** para **coffee-indexer**.

13. Deixe a **programação** definida como **Uma vez**.

14. Expanda as **opções avançadas**. Certifique-se de que a opção **Base-64 Encode Keys** esteja selecionada, pois as chaves de codificação podem tornar o índice mais eficiente.

15. Selecione **Enviar** para criar a fonte de dados, o conjunto de habilidades, o índice e o indexador. O indexador é executado automaticamente e executa o pipeline de indexação, que:
    - Extrai os campos de metadados do documento e o conteúdo da fonte de dados.
    - Executa o conjunto de habilidades cognitivas para gerar campos mais enriquecidos.
    - Mapeia os campos extraídos para o índice.

16. Volte à página de recursos do Azure AI Search. No painel esquerdo, em **Gerenciamento de pesquisa** , selecione **Indexadores** . Selecione o **indexador de café** recém-criado . Espere um minuto e selecione **↻ Atualize** até que o **Status** indique sucesso.

17. Selecione o nome do indexador para ver mais detalhes.

> Com isso, sabemos como configurar uma pesquisa, como carregar documentos para o armazenamento do Azure e como indexar documentos, para mais detalhes além deste, sugiro que você pesquise e estude a documentação clicando [aqui](https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/11-ai-search.html#azure-resources-needed).

## 📲 Conete-se comigo
[![Perfil DIO](https://img.shields.io/badge/-Meu%20Perfil%20na%20DIO-000?style=for-the-badge&logo=gitbook&logoColor=white)](https://www.dio.me/users/juninho_snowpb)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/RogerioFelipeJr)
[![LinkedIn](https://img.shields.io/badge/LinkedIn-0077B5?style=for-the-badge&logo=linkedin&logoColor=white)](https://www.linkedin.com/in/rogerio-felipe-jr/)
[![Gmail](https://img.shields.io/badge/Gmail-333333?style=for-the-badge&logo=gmail&logoColor=white)](mailto:rogeriofj7@gmail.com)
[![Instagram](https://img.shields.io/badge/-Instagram-%23E4405F?style=for-the-badge&logo=instagram&logoColor=white)](https://www.instagram.com/rogeriolucena1/)
[![WhatsApp](https://img.shields.io/badge/WhatsApp-25D366?style=for-the-badge&logo=whatsapp&logoColor=white)](https://wa.me/+5581993846505)

## 💻 Habilidades

![HTML5](https://img.shields.io/badge/HTML5-E34F26?style=for-the-badge&logo=html5&logoColor=white)
![CSS3](https://img.shields.io/badge/CSS3-1572B6?style=for-the-badge&logo=css3&logoColor=white)
![JavaScript](https://img.shields.io/badge/JavaScript-F7DF1E?style=for-the-badge&logo=javascript&logoColor=black)
![Java](https://img.shields.io/badge/java-%23ED8B00.svg?style=for-the-badge&logo=openjdk&logoColor=white)
![React](https://img.shields.io/badge/React-20232A?style=for-the-badge&logo=react&logoColor=61DAFB)
![Next](https://img.shields.io/badge/Next-black?style=for-the-badge&logo=next.js&logoColor=white)
![Bootstrap](https://img.shields.io/badge/-boostrap-0D1117?style=for-the-badge&logo=bootstrap&labelColor=0D1117)
![MySQL](https://img.shields.io/badge/MySQL-00000F?style=for-the-badge&logo=mysql&logoColor=white)
![PostgreSQL](https://img.shields.io/badge/PostgreSQL-000?style=for-the-badge&logo=postgresql)
![Git](https://img.shields.io/badge/GIT-E44C30?style=for-the-badge&logo=git&logoColor=white)
[![GitHub](https://img.shields.io/badge/GitHub-100000?style=for-the-badge&logo=github&logoColor=white)](https://github.com/)
![Figma](https://img.shields.io/badge/Figma-696969?style=for-the-badge&logo=figma&logoColor=figma)
![NodeJS](https://img.shields.io/badge/node.js-6DA55F?style=for-the-badge&logo=node.js&logoColor=white)


![GitHub Stats](https://github-readme-stats.vercel.app/api?username=RogerioFelipeJr&theme=transparent&bg_color=000&border_color=30A3DC&show_icons=true&icon_color=30A3DC&title_color=E94D5F&text_color=FFF)