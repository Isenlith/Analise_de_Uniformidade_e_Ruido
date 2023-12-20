# Uniformidade e Ruído do PET/CT

Este código em Python tem como objetivo processar imagens DICOM de aquisições em PET/CT para análise de Uniformidade e Ruído de distintas reconstruções.

| :books: Vitrine.Dev |     |
| -------------  | --- |
| :sparkles: Nome        | **Uniformidade e Ruído do PET/CT**
| :label: Tecnologias | python, cv2, pydicom, pandas, plotly

<!-- Inserir imagem com a #vitrinedev ao final do link -->
![](https://vitrinedev.s3.amazonaws.com/uniformidade-ruido.png#vitrinedev)

## Detalhes do projeto

### Bibliotecas utilizadas
O código usa as seguintes bibliotecas:

- os: para lidar com operações de diretório
- pydicom: para ler arquivos DICOM
- plotly.express: para visualizar os resultados
- matplotlib.pyplot: para visualizar a ROI
- numpy: para operações numéricas
- cv2: para processamento de imagem
- csv: para escrever dados em um arquivo csv
- math: para realizar operações matemáticas
- pandas: para ler os dados do arquivo csv.

### Funcionalidades
1. O código primeiro define o caminho para o diretório onde os arquivos DICOM estão armazenados e importa as bibliotecas necessárias.

2. Em seguida, a função get_images_data é definida, que recebe um caminho como entrada e retorna uma matriz NumPy de dados de pixel para cada imagem no diretório, bem como uma lista dos nomes dos arquivos. Essa função usa os.listdir para obter uma lista de todos os arquivos DICOM no diretório e pydicom.dcmread para ler os dados de pixel de cada imagem.

3. A função de máscara é então definida, que aplica uma máscara circular a um slice da imagem de entrada em uma coordenada (x, y) especificada com um raio dado, retornando apenas os pixels dentro da máscara.

4. O código então carrega as informações do cabeçalho DICOM para o primeiro arquivo no diretório usando pydicom.dcmread e extrai informações relevantes, como o nome do paciente, a data do estudo, a altura e a largura da imagem, o espaçamento de pixel, a espessura do slice, a descrição da série e o número de slices.

5. Em seguida, o código calcula o raio de uma região de interesse (ROI) circular com uma área de 500 mm² e o traduz para um número de pixels com base no espaçamento de pixel. O centro da imagem é então definido e os raios e ângulos de ROIs centrados no centro da imagem são definidos em coordenadas polares.

6. O código então calcula o número de slices a serem puladas para ter uma distância de 40 mm entre o slice central e os slices acima e abaixo dela. O slice central é então definida, bem como os slices 40 mm acima e abaixo dela.

7. O código então aplica as máscaras circulares em cada slice, calcula o valor médio de pixel dentro de cada máscara e armazena-o em uma lista. Os valores médios de pixel são usados para calcular o valor médio médio de pixel em todas as máscaras, que é então armazenado em $uROI_{TOT}$. Com essa informação, é calculado $SD_{uROImean}$ a partir da equação abaixo:

$$SD_{uROImean} = \sqrt{\frac{1}{n-1} \sum_{i=1}^n \left(\frac{uROI_{mean}}{uROI_{TOT} - 1} \right)^2}$$

8. O código então calcula o tamanho da ROI para a análise de ruído, visualiza-a e calcula o Coeficiente de Variação (CV) para os pixels dentro da ROI, seguindo a seguinte relação:

$$CV = \frac{SD_{nROI}}{nROI_{mean}} \times 100\%$$

9. O código verifica se os dados já foram exportados para um arquivo csv. Se não, exporta os dados para um arquivo csv chamado INSCER.csv.

10. Por fim, o código lê o arquivo INSCER.csv e exibe os dados em um dataframe do pandas.

### Como utilizar
Para usar este código, você precisará ter um diretório de arquivos DICOM que deseja analisar.

1. Defina o caminho para o diretório que contém os arquivos DICOM.
2. Execute o código.
3. O código gerará um arquivo INSCER.csv no mesmo diretório do código. Este arquivo conterá o $SD_{uROImean}$, o CV e a descrição da série para o arquivo DICOM em questão.
4. O código também exibirá uma visualização da ROI e um dataframe do pandas dos dados exportados.

Observação: Este código foi projetado para analisar arquivos DICOM específicos, portanto, talvez seja necessário modificá-lo para seu caso de uso específico. Além disso, o código analisa uma série por vez. Ou seja, é necessário alterar o caminho para o diretório da próxima série para que a análise seja aplicada em uma reconstrução distinta.
O arquivo .csv de saída gerado não é sobrescrito pelos resultados anteriores. Será sempre incluído uma nova linha com informações da próxima série (duplicatas não serão incluídas).

Este código pode ser útil para avaliar a qualidade de imagens médicas e comparar diferentes séries ou protocolos de aquisição.
