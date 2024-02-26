Neste exercício, você usará o recurso de aprendizado de máquina automatizado no Azure Machine Learning
para treinar e avaliar um modelo de aprendizado de máquina. Em seguida, você implantará e testará o modelo
treinado.
Este exercício deve levar aproximadamente 30 minutos para ser concluído.
Criar um espaço de trabalho do Azure Machine Learning
Para usar o Azure Machine Learning, você precisa provisionar um espaço de trabalho do Azure Machine Learning
na sua assinatura do Azure. Então você poderá usar o estúdio Azure Machine Learning para trabalhar com os
recursos em seu espaço de trabalho.
�� Dica : Se você já tiver um espaço de trabalho de Aprendizado de Máquina do Azure, poderá usá-lo e pular para a próxima
tarefa.
1. Assine para oPortal do AzureA que se a https://portal.azure.com usando suas credenciais da Microsoft.
2. Selecione a+ Criar um recurso, a busca porAprendizado de máquina, e criar um novoAprendizado de
Máquinas dorecurso com as seguintes configurações:
◦ Assinatura: Sua assinatura do Azure.
◦ Grupo de recursos: Crie ou selecione um grupo de recursos.
◦ Nome : Insira um nome exclusivo para o seu workspace.
◦ Região: Selecione a região geográfica mais próxima.
◦ Conta de armazenamento: Observe a nova conta de armazenamento padrão que será criada para o
seu espaço de trabalho.
◦ Keyeb dobrás: Observe o novo cofre de chaves padrão que será criado para o seu workspace.
◦ Insights do aplicativo: Observe o novo recurso padrão de insights de aplicativos que será criado para
o seu espaço de trabalho.
◦ Registro de contêineres: Nenhum (será criado automaticamente na primeira vez que você implantar
um modelo em um contêiner).
3. Selecione Revisão + criar e, em seguida, selecione Criar. Aguarde a criação do seu espaço de trabalho (sá
pode demorar alguns minutos) e, em seguida, vá para o recurso implantado.
4. Selecione Estúdio de inicialização (ou abra uma nova guia do navegador e navegue até https://
ml.azure.com e entre no estúdio Azure Machine Learning usando sua conta da Microsoft). Feche todas as
mensagens exibidas.
5. No estúdio de Machine Learning do Azure, você deve ver sua área de trabalho recém-criada. Se não,
selecione Todos os espaços de trabalho no menu à esquerda e, em seguida, selecione o espaço de
trabalho que você criou.
Use machine learning automatizado para treinar um modelo
O aprendizado de máquina automatizado permite que você experimente vários algoritmos e parâmetros para
treinar vários modelos e identificar o melhor para seus dados. Neste exercício, você usará um conjunto de dados
de detalhes históricos do aluguel de bicicletas para treinar um modelo que prevê o número de aluguel de
bicicletas que deve ser esperado em um determinado dia, com base em características sazonais e
meteorológicas.
Criar um espaço
de trabalho do
Azure Machine
Learning
Use machine
learning
automatizado
para treinar um
modelo
Reveja o melhor
modelo
Implantar e
testar o modelo
Teste o serviço
implantado
Limpeza
mslearn-ai-fundamentals (empredios) https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html
1 of 5 25/02/2024, 23:07
�� CitationCitação: Os dados utilizados neste exercício são derivados da Capital Bikeshare e são utilizados de acordo com o
contrato de licença de dados publicado.
1. No estúdio de Machine Learning do Azure, veja a página ML automatizada (em criação).
2. Criar uma nova tarefa de ML Automatizada com as seguintes configurações, usando Next conforme
necessário para progredir através da interface do usuário:
Configurações básicas :
◦ Nome de trabalho : mslearn-bike-automl
◦ Novo nome de experimento : mslearn-bike-rental
◦ Descrição: Aprendizado de máquina automatizado para previsão de aluguel de bicicletas
◦ Tags : Nenhum
Tipo & dados da tarefa:
◦ Selecione o tipo de tarefa : Regressão
◦ Selecione o conjunto de dados: Crie um novo conjunto de dados com as seguintes configurações:
◦ Tipo de dados:
▪ Nome : aluguel de bicicletas
▪ Descrição : Dados históricos de aluguer de bicicletas
▪ Tipo : Tabular
◦ Fonte de dados:
▪ Selecione a partir de arquivos web
◦ URL da Web:
▪ URL da Web: https://aka.ms/bike-rentals
▪ Ignorar a validação de dados : não selecione
◦ Configurações:
▪ Formato de arquivo : Delimitado
▪ Delimitador : Coma
▪ Encoding : UTF-8 (versão para o usuário)
▪ Cabeçadores de coluna : Apenas primeiro arquivo tem cabeçalhos
▪ Ignorar as linhas : Nenhum
▪ Dataset contém dados multi-linha : não selecione
◦ Schema ('):
▪ Inclua todas as colunas que não sejam Caminho
▪ Revise os tipos detectados automaticamente
Selecione Criar. Após a criação do conjunto de dados, selecione o conjunto de dados de aluguel de
bicicletas para continuar a enviar o trabalho ML Automatizado.
Configurações de tarefa :
◦ Tipo de tarefa : Regressão
◦ Dataset : aluguel de bicicletas
◦ Coluna alvo : Locação (ingerto)
◦ Configurações adicionais de configuração:
◦ Mémétrica primária : Erro quadrado médio de raiz normalizado
◦ Explique o melhor modelo: Não selecionado
◦ Use todos os modelos suportados: Não selecionado. Você restringirá o trabalho para tentar
apenas alguns algoritmos específicos.
◦ Modelos permitidos: Selecione apenas RandomForest e LightGBM - normalmente você
gostaria de tentar o maior número possível, mas cada modelo adicionado aumenta o tempo que
mslearn-ai-fundamentals (empredios) https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html
2 of 5 25/02/2024, 23:07
leva para executar o trabalho.
◦ Limites:Expanda esta seção
◦ Testes máximos: 3
◦ Ensaios simultâneos máximos: 3
◦ O símdia de Max : 3
◦ Limite de pontuação métrica: 0,085 (para que, se um modelo atingir uma raiz normalizada
escore métrico de erro quadrado médio de 0,085 ou menos, o trabalho termina.)
◦ Tempo limite : 15
◦ Tempo limite de iteração : 15
◦ Habilite a rescisão antecipada : Selecionado
◦ Validação e teste:
◦ Tipo de validação : Divisões de validação de trem
◦ Percentagem de dados de validação: 10
◦ Testar o conjunto de dados : Nenhum
Compute :
◦ Selecione o tipo de computação : Serverless
◦ Tipo de máquina virtual: CPU
◦ Nível da máquina virtual: Dedicado
◦ Tamanho da máquina virtual : Standard_DS3_V2
◦ Número de instâncias: 1
Se a sua assinatura restringir os tamanhos de VM disponíveis para você, escolha qualquer tamanho
disponível.
3. Submeta o trabalho de treino. Ele começa automaticamente.
4. Esperar que o trabalho termine. Pode demorar um pouco – agora pode ser um bom momento para uma
pausa para um café!
Reveja o melhor modelo
Quando o trabalho automatizado de aprendizado de máquina for concluído, você pode revisar o melhor modelo
treinado.
1. Na guia Visão geral do trabalho automatizado de aprendizado de máquina, observe o melhor resumo do
modelo.
�� Nota Você pode ver uma mensagem sob o status “Aviso: pontuação de saída especificada pelo usuário alcançada...”.
Esta é uma mensagem esperada. Por favor, continuem para o próximo passo.
mslearn-ai-fundamentals (empredios) https://microsoftlearning.github.io/mslearn-ai-fundamentals/Instructions/Labs/01-machine-learning.html
3 of 5 25/02/2024, 23:07
2. Selecione o texto em Nome do Algoritmo para o melhor modelo para visualizar seus detalhes.
3. Selecione a guia Métricas e selecione os gráficos de resíduos e previstos_true se ainda não estiverem
selecionados.
Revise os gráficos que mostram o desempenho do modelo. O gráfico de resíduos mostra os resíduos (as
diferenças entre os valores previstos e reais) como um histograma. O gráfico_true previsto compara os
valores previstos em relação aos valores reais.
Implantar e testar o modelo
1. Sobre oModelo de Modeltabulação para o melhor modelo treinado pelo seu trabalho automatizado de
aprendizado de máquina, selecioneDeploy (Deploy)e usar oServiço da Webopção para implantar o
modelo com as seguintes configurações:
◦ Nome : preditual-rentals
◦ Descrição : Predict cycle rentals
◦ Tipo de computação : Instância do recipiente do Azure
◦ Habilitação de autenticação : Selecionado
2. Aguarde o início da implantação - isso pode levar alguns segundos. O status de Implantação para o
endpoint preditor-alugis será indicado na parte principal da página como Executando.
3. Aguarde que o status de Implantação mude para Sucedido. Isso pode levar de 5 a 10 minutos.
Teste o serviço implantado
Agora você pode testar o seu serviço implantado.
1. No Azure Machine Learning studio, no menu esquerdo, selecione Endpoints e abra o ponto de
extremidade preditor-augur em tempo real.
2. Na página de ponto de extremidade preditual-rentals em tempo real, visualize a guia Teste.
3. Nos dados de entrada para testar o painel de endpoint, substitua o modelo JSON pelos seguintes dados
de entrada: vide doc .json nessa pasta
4. Clique no botão Testar.
5. Analse os resultados do teste, que incluem um número previsto de locações com base nos recursos de
entrada - semelhante a isso:
Código Do Código  Cópia
{
"Results": [
444.27799000000000
]
}
O painel de teste levou os dados de entrada e usou o modelo que você treinou para retornar o número
previsto de aluguéis.
Vamos rever o que você fez. Você usou um conjunto de dados de dados históricos de aluguel de bicicletas para
treinar um modelo. O modelo prevê o número de aluguel de bicicletas esperados em um determinado dia, com
base em características sazonais e meteorológicas.
Limpeza
O serviço Web que você criou está hospedado em uma instância de contêiner do Azure. Se você não pretende
experimentá-lo ainda mais, você deve excluir o endpoint para evitar o acúmulo de uso desnecessário do Azure.
1. No Azure Machine Learning studio, na guia Endpoints, selecione o ponto de extremidade preditor-
auguel. Em seguida, selecione Excluir e confirmar que você deseja excluir o endpoint.
A exclusão da sua computação garante que sua assinatura não será cobrada por recursos de computação.
No entanto, será cobrada uma pequena quantia para o armazenamento de dados, desde que o espaço de
trabalho do Azure Machine Learning exista na sua assinatura. Se você tiver terminado de explorar o Azure
Machine Learning, poderá excluir o espaço de trabalho do Azure Machine Learning e os recursos
associados.
Para excluir seu workspace:
1. No portal do Azure, na página Grupos de recursos, abra o grupo de recursos especificado ao criar sua
área de trabalho do Azure Machine Learning.
2. Clique em Excluir grupo de recursos, digite o nome do grupo de recursos para confirmar que você deseja
excluí-lo e selecione Excluir.
