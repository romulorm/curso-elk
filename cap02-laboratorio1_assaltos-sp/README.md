# ELASTIC SEARCH - LABORATÓRIO 01 - Assaltos em SP

Laboratório envolvendo um dataset de Assaltos em SP.

### Importando o dataset assaltos-sp

* Faça o download do dataset de Assaltos em SP no endereço: https://github.com/romulorm/curso-elk/blob/master/cap02-laboratorio1_assaltos-sp/assaltos-sp-dataset.csv
* Para importar o dataset, vá na opção **Machine Learning**, na seção Analytics do Kibana;
* Acesse a opção **Visualize data from a file** e selecione o arquivo do dataset;
* No preview do dataset, clique no botão Import;
* Atenção, inclua o nome do índice como **assaltos-sp** e não aperte o botão Import neste momento, selecione a aba Advanced;
* Teremos que alterar os mapeamentos dos campos para refletir os verdadeiros tipos de dados, a exemplo dos campos time e created_at que devem ser do tipo "date";
* Para facilitar, substitua o conteúdo do campo **Mappings** pelo conteúdo do arquivo https://github.com/romulorm/curso-elk/blob/master/cap02-laboratorio1_assaltos-sp/assaltos-sp-mappings.txt;
* Assim como feito acima, substitua o conteúdo do campo **Ingest Pipeline** pelo conteúdo do arquivo https://github.com/romulorm/curso-elk/blob/master/cap02-laboratorio1_assaltos-sp/assaltos-sp-ingest_pipeline.txt;
* Vai dar falha em um registro. Desconsidere.
* Clique em **View index in Discover**.


### Criando Mapas

#### Mapa de Assaltos em SP

* Para criar um mapa, acesse a opção **Maps**, na seção Analytics do Kibana;
* Clique no botão **Create map**;
* Clique no botão **Add layer**;
* Selecione a opção **Documents**;
* Selecione a data view **assaltos-sp** e clique no botão **Add layer**;
* Na próxima tela coloque o nome da camada como **assaltos**;
* Reduza a opacidade para 30%;
* Vamos criar Tooltip fields que serão mostrados quando o mouse estiver sobre o registro. Selecione: **time, titulo, bairro, valor_prejuizo**;
* Ajuste o tamanho dos registros. **Symbol size: 1**;
* Clique no botão **Save & Close**;
* Dê um zoom na área do Estado de São Paulo, passe o mouse pelos registros e observe os tooltips.

#### Mapa de calor (Heatmap)

* Clique no botão **Add layer**;
* Selecione a opção **Heat map**;
* Selecione a data view **assaltos-sp** e clique no botão **Add layer**;
* Em name coloque **mapa de calor**;
* Clique no botão **Save & Close**;
* Observe que quanto mais registros na área, mais vermelho aparecerá.
* Clique em Save, defina o nome do mapa como **Mapa de assaltos em SP** e escolha New para criar um Dashboard onde o mapa será exibido.
* Clique em **Save & Go to Dashboard**. Clique no botão Save novamente e defina o nome do dashboard para **Painel de Assaltos em SP**.
* Arraste o mapa para a direita de modo que ele ocupe toda a tela.


### Adicionando mais informações ao Dashboard


#### Gráfico Pizza/Torta "Top assaltos por bairro"

Criando um gráfico do tipo Pizza ou Torta com os bairros com mais ocorrências de assaltos.

* Acesse o botão **Create visualization** do Dashboard, ou a opção **Visualize Library**na seção Analytics do Kibana;
* Clique no botão **Create visualization**;
* Selecione **Lens**
* No tipo de gráfico, mude de **Bar vertical stacked** para **Pie**;
* Arraste o campo **bairro** para o campo "Slice by";
* No campo "Size by", clique em "Add or drag..." e selecione **Count** e depois em Close.
* Vamos ajustar a visualização retirando o agrupamento "Other" e retirando o bairro "São Paulo".
* Clique em **Top 5 values of bairro** e altere o campo **Number of values** para **10**;
* Clique em Advanced e ajuste os campos abaixo:
    - Desative **Group other values as Other**
    - Em "Exclude values", escreva **São Paulo**, que não é um bairro.
    - em "Name" coloque o nome **Top 10 assaltos por bairro**.
* Aperte "Close".

* Clique em Save. Coloque o Title como **Top 10 Assaltos por bairro**;
* Selecione o Dashboard **Painel de Assaltos em SP** salvo anteriormente, lique em **Save and go to Dashboard**.
* Clique em **Save** novamente para salvar o Dashboard.

---

#### Gráfico Contador (Metric)  "Top assaltos por bairro"

Criando contadores mostrando os bairros com mais ocorrências.

* Acesse a opção **Visualize Library**, na seção Analytics do Kibana;
* Clique no botão **Create visualization**;
* Selecione **Lens**
* No tipo de gráfico, mude de "Bar vertical stacked" para **Metric**;
* Arraste o campo **Records** para o campo "Primary metric";
* Arraste o campo **registrou_bo** para o campo "Secondary metric";
* Arraste o campo **bairro** para o campo "Break down by".

* Clique em **Top 5 values of bairro** e altere o campo **Number of values** para **12**;
* Clique em Advanced e ajuste os campos abaixo:
    - Desative "Group other values as Other"
    - Em "Exclude values", escreva "São Paulo", que não é um bairro.
    - em "Name" coloque o nome **Top 12 bairros**.
    - Em Layout columns, altere para **4**;
* Aperte "Close";

* Clique em **Unique count of registrou_bo** e altere a Function para **Count**;
* Clique em Advanced e em "Filter by" coloque **registrou_bo:"VERDADEIRO"**;
* Em Name coloque **Contador registro de BO**;
* Aperte "Close";
* Verifique que além do total de ocorrências do bairro, será mostrado o número de Ocorrências em que foi registrado o Boletim de Ocorrência.

* Clique em **Count of records** e altere a Function para **Count**;
* Em Name coloque **Contador assalto/B.O.**;
* Aperte "Close";

* Clique em Save. Coloque o Title como **Contador de Assaltos por bairro**;
* Selecione o Dashboard **Painel de Assaltos em SP** salvo anteriormente, lique em **Save and go to Dashboard**.
* Clique em **Save** novamente para salvar o Dashboard.

---

#### Tabela "Top 10 prejuizo/bairro/sexo"

Criando uma tabela mostrando o prejuízo das vítimas desmembrado por bairro e por sexo.

* Acesse a opção **Visualize Library**, na seção Analytics do Kibana;
* Clique no botão **Create visualization**;
* Selecione **Lens**
* No tipo de gráfico, mude de "Bar vertical stacked" para **Table**;
* Arraste o campo **bairro** para o campo "Rows";
* Arraste o campo **sexo** para o campo "Columns";
* Arraste o campo **valor_prejuizo** para o campo "Metrics";

* Clique em **Median of valor_prejuizo** no campo Metrics;
* Selecione a Function **Sum**;
* No campo Name coloque **Soma do prejuízo**;
* Em Value format escolha Number e em Decimals digite 2
* Em Summary Row coloque Sum e em Summary Label digite Total
* Aperte "Close";

* Clique em **Top 5 values of bairro** e altere o campo **Number of values** para **10**;
* No campo **Rank by** deixe selecionado **Soma do prejuízo**;
* Clique em Advanced e ajuste os campos abaixo:
    - Desative "Group other values as Other"
    - Em "Exclude values", escreva "São Paulo", que não é um bairro.
* No campo Name coloque **Top 10 prejuizo/bairro/sexo**;
* Aperte "Close";

* Clique em **Top 3 values of sexo** e altere o campo **Number of values** para **2**;
* No campo **Rank by** deixe selecionado **Soma do prejuízo**;
* No campo Name coloque **Sexo**;
* Aperte "Close";

* Clique em Save. Coloque o Title como **Top 10 prejuizo/bairro/sexo**;
* Selecione o Dashboard **Painel de Assaltos em SP** salvo anteriormente, lique em **Save and go to Dashboard**.
* Clique em **Save** novamente para salvar o Dashboard.

---

#### Mapa de palavras "Top 10 ocorrências por bairro"

Criando um Mapa de palavras (tag cloud) mostrando com os bairros com maiores ocorrências de assaltos.

* Acesse a opção **Visualize Library**, na seção Analytics do Kibana;
* Clique no botão **Create visualization**;
* Selecione **Aggregation based**;
* Selecione **Tag cloud**;
* Selecione o índice **assaltos-sp**;
* Em Metrics, expanda **Tag size**, e em Custom label, escreva **Contador**;
* Em Buckets, clique em Add, clique em Tags e selecione **Terms**;
* Em Field, selecione **bairro**;
* Em Size, digite **15**;
* Em Custom label, escreva **Bairros com maior índice de assaltos**;
* Expanda Advanced;
* Em Exclude, digite **São Paulo**;
* Clique em **Update**;
* Clique em **Save**;
* Digite o nome **Mapa de palavras/bairros**;
* Clique em Save e selecione o Dashboard **Painel de Assaltos em SP** salvo anteriormente.
* Clique em **Save and go to Dashboard**;
* No Dashboard, clique em **Save** novamente para salvar o Dashboard.
