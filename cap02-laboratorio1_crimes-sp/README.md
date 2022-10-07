# ELASTIC SEARCH - LABORATÓRIO 01 - CRIMES SP

Laboratório envolvendo um dataset de Crimes em SP.

### Importando o dataset crimes-sp

* Faça o download do dataset de Assaltos em SP no endereço: https://github.com/romulorm/elk-docs/raw/master/cap02-laboratorio1_crimes-sp/crimes-sp-dataset.csv
* Para importar o dataset, vá na opção **Machine Learning**, na seção Analytics do Kibana;
* Acesse a opção **Visualize data from a file** e selecione o arquivo do dataset;
* No preview do dataset, clique no botão Import;
* Atenção, inclua o nome do índice como **crimes-sp** e não aperte o botão Import neste momento, selecione a aba Advanced;
* Teremos que alterar os mapeamentos dos campos para refletir os verdadeiros tipos de dados, a exemplo dos campos time e created_at que devem ser do tipo "date";
* Para facilitar, substitua o conteúdo do campo **Mappings** pelo conteúdo do arquivo https://github.com/romulorm/elk-docs/blob/master/cap02-laboratorio1_crimes-sp/crimes-sp-mappings.txt;
* Assim como feito acima, substitua o conteúdo do campo **Ingest Pipeline** pelo conteúdo do arquivo  https://github.com/romulorm/elk-docs/blob/master/cap02-laboratorio1_crimes-sp/crimes-sp-ingest_pipeline.txt;
* Clique em **View index in Discover**.

### Criando Mapas

#### Mapa da Criminalidade

* Para criar um mapa, acesse a opção **Maps**, na seção Analytics do Kibana;
* Clique no botão **Create map**;
* Clique no botão **Add layer**;
* Selecione a opção **Documents**;
* Selecione a data view **crimes-sp** e clique no botão **Add layer**;
* Na próxima tela coloque o nome da camada como **criminalidade**;
* Reduza a opacidade para 30%;
* Vamos criar Tooltip fields que serão mostrados quando o mouse estiver sobre o registro. Selecione: **time, titulo, bairro, valor_prejuizo**;
* Ajuste o tamanho dos registros. **Symbol size: 1**;
* Clique no botão **Save & Close**;
* Dê um zoom na área do Estado de São Paulo, passe o mouse pelos registros e observe os tooltips.

#### Mapa de calor (Heatmap)

* Clique no botão **Add layer**;
* Selecione a opção **Heat map**;
* Selecione a data view **crimes-sp** e clique no botão **Add layer**;
* Em name coloque **mapa de calor**;
* Clique no botão **Save & Close**;
* Observe que quanto mais registros na área, mais vermelho aparecerá.
* Clique em Save, defina o nome do mapa como **Mapa de assaltos em SP** e escolha New para criar um Dashboard onde o mapa será exibido.
* Clique em Save novamente para salvar o Dashboard atual. Defina o nome de **Painel da Criminalidade em SP**.

### Adicionando mais informações ao Dashboard


##### Gráfico Pizza "Top assaltos por bairro"

Criando um gráfico do tipo Pizza ou Torta com os bairros com mais ocorrências.

* Acesse a opção **Visualize Library**, na seção Analytics do Kibana;
* Clique no botão **Create visualization**;
* Selecione **Lens**
* No tipo de gráfico, mude de "Bar vertical stacked" para **Pie**;
* Arraste o campo **bairro** para o campo "Slice by";
* No campo "Size by", clique em "Add or drag..." e selecione **Count**;
* Vamos ajustar a visualização retirando o agrupamento "Other" e retirando o bairro "São Paulo".
* Clique em **Top 5 values of bairro** e altere o campo **Number of values** para **10**;
* Clique em Advanced e ajuste os campos abaixo:
    - Desative "Group other values as Other"
    - Em "Exclude values", escreva "São Paulo", que não é um bairro.
* Aperte "Close";
* Clique em "Save" e coloque o nome "Top assaltos por bairro".
* Selecione o Dashboard **Painel da Criminalidade em SP** salvo anteriormente.
* Clique em "Save" novamente para salvar o Dashboard.

##### Gráfico Contador (Metric)  "Top assaltos por bairro"

Criando contadores mostrando os bairros com mais ocorrências.

* Acesse a opção **Visualize Library**, na seção Analytics do Kibana;
* Clique no botão **Create visualization**;
* Selecione **Lens**
* No tipo de gráfico, mude de "Bar vertical stacked" para **Metric**;
* Arraste o campo **Records** para o campo "Primary metric";
* Arraste o campo **Bairro** para o campo "Break down by";
* Clique em **Top 5 values of bairro** e altere o campo **Number of values** para **9**;
* Clique em Advanced e ajuste os campos abaixo:
    - Desative "Group other values as Other"
    - Em "Exclude values", escreva "São Paulo", que não é um bairro.
* Aperte "Close";
* Arraste o campo **registrou_bo** para o campo "Secondary metric";
* Clique em **Unique count of registrou_bo** e altere a Function para **Count**;
* Clique em Advanced e em "Filter by" coloque "registrou_bo:"VERDADEIRO";
* Aperte "Close";
* Será mostrado o número de Ocorrências em que foi registrado o Boletim de Ocorrências.
* Clique em "Save" e coloque o nome "Contador assaltos por bairro".
* Selecione o Dashboard **Painel da Criminalidade em SP** salvo anteriormente.
* Clique em "Save" novamente para salvar o Dashboard.