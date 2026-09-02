# Projeto Final - RAG: Comparando os modelos Gemma 3 (4B e 12B) executados via Ollama

#### Aluno: Fabiana Viana Salmaso        (https://github.com/fabianaestudoIA)
#### Orientadora: Evelyn Batista  (https://github.com/evysb)


Trabalho apresentado ao curso MASTER em Inteligência Artificial Generativa & Large Language Models da PUC-Rio
como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "PROJETO FINAL 2025.2".

### Link do Projeto:
- https://github.com/fabianaestudoIA/ProjetoFinalPUC

  
### Resumo do Projeto

<p align="justify">
Este Trabalho de Conclusão de Curso (TCC) propõe o desenvolvimento de um assistente inteligente baseado em Inteligência Artificial Generativa e Retrieval-Augmented Generation (RAG) e visa comparar os modelos Gemma 3 (4B e 12B) executados via Ollama. 
O projeto do RAG teve como inspiração a necessidade observada pelos usuários da empresa na qual trabalho, para encontrar informações em um sistema de gestão acadêmica onde reuni vários "Documentos" em PDF sobre regulamentos, contratos, manuais e normas.
</p>

<p align="justify">
Atualmente, os usuários precisam localizar manualmente os documentos e navegar por seu conteúdo para encontrar informações específicas, o que pode demandar tempo e dificultar o acesso rápido às informações desejadas. 
</p>

### 1. Introdução

<p align="justify">
A empresa onde trabalho, dispões de um sistema de gestão acadêmica onde reuni vários "Documentos" do sistema em formato PDF, incluindo regulamentos, contratos, manuais e normas. O crescente volume de documentos disponibilizados em sistemas de gestão acadêmica torna a busca por informações um processo, muitas vezes, demorado e pouco intuitivo para os usuários.
</p>
<p align="justify">
Diante desse cenário, este Trabalho de Conclusão de Curso propõe o desenvolvimento e a avaliação de um Retrieval-Augmented Generation (RAG) capaz de consultar os documentos e fornecer respostas em linguagem natural aos usuários.
</p>
<p align="justify">
Devido ao caráter confidencial das informações corporativas, não foi possível utilizar os documentos reais da organização durante o desenvolvimento e os testes do projeto. Para contornar essa limitação e preservar a segurança dos dados, a base de conhecimento foi composta por documentos públicos em formato PDF contendo informações relacionadas às regras de trânsito, obtidos a partir de fontes disponíveis na internet. Dessa forma, foi possível reproduzir um cenário semelhante ao ambiente corporativo sem comprometer informações sensíveis da instituição.
</p>
<p align="justify">
A arquitetura adotada foi baseada na técnica Retrieval-Augmented Generation (RAG), que combina recuperação de informações e geração de texto por modelos de linguagem. Nesse modelo, o sistema realiza inicialmente a busca dos trechos mais relevantes nos documentos da base de conhecimento e, em seguida, utiliza essas informações como contexto para a construção da resposta, aumentando a precisão e a confiabilidade dos resultados.
</p>
<p align="justify">
O RAG foi testado com dois modelos, o  Gemma 3 (4B e 12B) executados via Ollama. A avaliação foi realizada por meio de um conjunto padronizado de perguntas aplicadas aos dois modelos, permitindo analisar critérios como precisão das respostas, aderência ao contexto recuperado, capacidade de recuperação das informações e ocorrência de respostas incorretas ou não fundamentadas.
</p>

### 2. Modelagem
<p align="justify">
Para a implementação do protótipo, foi utilizado o ambiente Google Colab, facilitando o desenvolvimento e a execução dos experimentos. Os documentos PDF utilizados como base de conhecimento foram armazenados no diretório /content, enquanto a interface de interação com o usuário foi desenvolvida com o framework Gradio, proporcionando uma experiência simples e intuitiva. Adicionalmente, foi empregada a ferramenta LangSmith para observabilidade, monitoramento e análise das execuções do sistema, permitindo acompanhar o comportamento dos modelos durante os testes e avaliar a qualidade das respostas geradas.
</p>

### Construção do Pipeline RAG
<p align="justify">
Inicialmente, os documentos que compõem a base de conhecimento passaram por um processo de segmentação (chunking). Essa etapa é fundamental para melhorar a granularidade da recuperação de informações, uma vez que documentos extensos podem dificultar a identificação de trechos específicos relacionados à consulta do usuário. Para essa finalidade, foi utilizada a classe RecursiveCharacterTextSplitter, configurada com um tamanho de fragmento (chunk_size) de 1.250 caracteres e uma sobreposição (chunk_overlap) de 200 caracteres entre chunks consecutivos.
</p>
<p align="justify">
A estratégia de sobreposição adotada visa preservar o contexto semântico entre os fragmentos, reduzindo a possibilidade de perda de informações relevantes localizadas nas extremidades dos textos. Dessa forma, conteúdos que poderiam ser separados durante o processo de divisão permanecem parcialmente replicados em chunks adjacentes, aumentando a qualidade da recuperação posterior.
</p>
<p align="justify">
Após a segmentação dos documentos, foi realizada a etapa de vetorização dos dados textuais por meio da geração de embeddings. Para essa finalidade, optou-se pela utilização do modelo all-MiniLM-L6-v2, disponibilizado pela biblioteca Sentence Transformers. A escolha desse modelo foi motivada pelo seu equilíbrio entre eficiência computacional e qualidade semântica das representações vetoriais geradas.
</p>
<p align="justify">
Os embeddings são representações numéricas densas dos textos, capazes de capturar relações semânticas e contextuais entre palavras, frases e documentos. Diferentemente de métodos tradicionais de busca baseados apenas em correspondência lexical, a utilização de embeddings permite que consultas semanticamente semelhantes sejam associadas a conteúdos relevantes, mesmo quando não há coincidência exata entre os termos utilizados.
</p>
<p align="justify">
Em seguida, os embeddings gerados foram armazenados em um banco vetorial utilizando o ChromaDB, uma solução amplamente empregada em arquiteturas RAG devido à sua simplicidade de integração e capacidade de persistência. A criação do repositório vetorial foi realizada por meio do método Chroma.from_documents(), que recebe os chunks gerados e seus respectivos embeddings, armazenando-os em um diretório persistente denominado chroma_db.
</p>
<p align="justify">
A persistência dos vetores em disco representa uma vantagem significativa para o sistema, pois elimina a necessidade de reconstrução completa do índice a cada reinicialização da aplicação. Dessa forma, a base vetorial pode ser reutilizada em diferentes execuções, reduzindo o tempo de processamento e aumentando a eficiência operacional da solução.
</p>
<p align="justify">
Por fim, foi configurado o componente de recuperação de informações (Retriever), responsável por realizar buscas semânticas no banco vetorial. A partir do método as_retriever(), o sistema passou a disponibilizar uma interface capaz de localizar os chunks mais relevantes para uma determinada consulta, utilizando métricas de similaridade entre os embeddings da pergunta e os embeddings armazenados na base documental.
</p>
<p align="justify">
O retriever desempenha papel central na arquitetura RAG, pois atua como mecanismo intermediário entre a consulta do usuário e o modelo de linguagem generativa. Quando uma pergunta é submetida ao sistema, o retriever identifica os fragmentos de texto semanticamente mais relevantes e os encaminha como contexto adicional para o modelo de geração. Dessa forma, as respostas produzidas passam a ser fundamentadas em informações efetivamente presentes na base de conhecimento, reduzindo alucinações e aumentando a confiabilidade dos resultados.
</p>
<p align="justify">
Em síntese, o pipeline desenvolvido foi composto por quatro etapas principais: 
- Segmentação dos documentos em chunks com preservação de contexto
- Geração de embeddings semânticos utilizando o modelo all-MiniLM-L6-v2
- Armazenamento dos vetores no banco ChromaDB
- Recuperação de informações por meio de um retriever semântico. 
</p>
<p align="justify">
Essa arquitetura possibilita a construção de sistemas de busca inteligente e geração contextualizada de respostas, constituindo uma abordagem eficiente para aplicações que demandam acesso rápido e preciso a grandes volumes de informação textual.
</p>


#### Fluxo do Pipeline RAG
##### 1. Carregamento dos documentos.
##### 2. Divisão dos documentos em chunks de 1.250 caracteres com sobreposição de 200 caracteres.
##### 3. Geração dos embeddings utilizando o modelo Sentence Transformer all-MiniLM-L6-v2.
##### 4. Armazenamento dos embeddings no banco vetorial ChromaDB.
##### 5. Persistência do índice vetorial no diretório ./chroma_db.
##### 6. Inicialização do Retriever.
##### 7. Recuperação dos chunks mais relevantes para cada consulta.
##### 8. Envio do contexto recuperado para o modelo de linguagem responsável pela geração das respostas.


### 3. Resultados

#### Análise Comparativa dos Resultados dos Modelos de Linguagem em Ambiente RAG
<p align="justify">
Com o objetivo de avaliar o desempenho de dois Modelos de Linguagem de Grande Escala (Large Language Models - LLMs) em um cenário de Retrieval-Augmented Generation (RAG), foi conduzido um experimento no qual ambos os modelos responderam ao mesmo conjunto de 10 perguntas. As questões utilizadas nos testes estão descritas no arquivo "analise qualitativa entre os modelos Gemma 3 - 4B e 12B.xlsx", elaborado especificamente para avaliar a capacidade dos modelos em recuperar informações relevantes da base documental e gerar respostas fundamentadas no contexto recuperado.
</p>

####A seguir a listagem das 10 perguntas utilizadas para realizar a análise comparativas entre os modelos Gemma 3 - 4B e 12B


<p align="justify">
Durante a execução dos experimentos, cada pergunta foi submetida individualmente a cada modelo, utilizando o mesmo pipeline RAG e a mesma base de conhecimento, garantindo assim condições equivalentes de avaliação. O objetivo foi analisar aspectos como precisão da recuperação, aderência ao contexto fornecido, completude das respostas e capacidade de evitar informações não presentes nos documentos de referência.
</p>
<p align="justify">
As perguntas foram formuladas de modo a exigir que os modelos utilizassem exclusivamente as informações disponíveis na base documental recuperada pelo mecanismo de busca semântica. Como exemplo, uma das questões presentes no arquivo foi:
</p>

"Responda usando exclusivamente com base nos conteúdos fornecidos, informe os deveres do condutor."
<p align="justify">
Esse tipo de pergunta foi projetado para verificar a capacidade do sistema RAG de localizar os trechos mais relevantes nos documentos indexados e fornecer ao modelo de linguagem contexto suficiente para a geração de uma resposta precisa e alinhada às informações disponíveis. Além disso, a restrição explícita de utilizar exclusivamente o conteúdo recuperado permitiu avaliar a ocorrência de alucinações, fenômeno em que o modelo produz informações não fundamentadas na base de conhecimento.
</p>
<p align="justify">
Ao empregar um conjunto padronizado de perguntas, conforme apresentado no arquivo "analise qualitativa entre os modelos Gemma 3 - 4B e 12B.xlsx", tornou-se possível realizar uma análise qualitativa entre os modelos avaliados, identificando diferenças de desempenho relacionadas tanto à recuperação de informações quanto à qualidade da resposta gerada no contexto de uma arquitetura RAG. Essa metodologia contribui para a obtenção de resultados mais consistentes e comparáveis, fornecendo evidências concretas sobre a efetividade de cada modelo na utilização do conhecimento disponibilizado pela base documental.
</p>
<p align="justify">
Os resultados indicaram diferenças entre os modelos, especialmente no que se refere à ocorrência de alucinações. Embora ambos tenham demonstrado capacidade de compreender as perguntas e utilizar o contexto fornecido pelo mecanismo de recuperação, o Modelo Gemma 3 (4B) 
apresentou uma frequência maior de respostas incorretas, evidenciando dificuldades na etapa de seleção e utilização das evidências recuperadas pelo mecanismo RAG.
</p>
<p align="justify">
Por outro lado, o Modelo Gemma 3 (12B) apresentou um comportamento mais consistente e aderente às informações efetivamente recuperadas, demonstrando maior cautela na elaboração das respostas. Quando confrontado com limitações ou lacunas na base de conhecimento, o modelo tendeu a restringir suas respostas ao conteúdo disponível, reduzindo significativamente a inserção de informações incorretas ou especulativas.
</p>
#### Observabilidade com LangSmith

#### Análise do Tempo Médio de Latência
<p align="justify">
A avaliação do tempo de latência dos modelos foi realizada com o apoio da ferramenta LangSmith, utilizada para monitorar e registrar detalhadamente a execução de cada consulta submetida ao pipeline RAG. A observabilidade fornecida pela plataforma permitiu acompanhar métricas de desempenho em tempo real, incluindo o tempo total necessário para que cada modelo processasse a pergunta, recuperasse o contexto relevante e gerasse a resposta final.
</p>
<p align="justify">
Para a realização da análise, foram submetidas aos modelos  Gemma 3 (4B e 12B) as 10 perguntas descritas no experimento. O tempo de latência de cada interação foi registrado individualmente por meio do LangSmith, possibilitando a obtenção de dados precisos sobre o desempenho de cada modelo. Após a coleta dos resultados, foi calculada a média aritmética dos tempos observados para cada conjunto de respostas. Os resultado podem ser observados na figura 2 e os registros podem ser consultados no arquivo "analise qualitativa entre os modelos Gemma 3 - 4B e 12B.xlsx", na aba LangSmith.
</p>

![Imagem sobre Análise do Tempo Médio de Latência ](https://github.com/fabianaestudoIA/ProjetoFinalPUC/blob/main/imgLatenciaLangSmith.png)
                                                        <div align="center">**Figura 2**</div>

<p align="justify">
Os resultados obtidos indicaram que o Modelo Gemma 3 (4B) apresentou um tempo médio de latência de 16,15 segundos, enquanto o Modelo Gemma 3 (12B) registrou uma latência média de 88,77 segundos. Observa-se, portanto, uma diferença significativa entre os modelos, sendo que o segundo levou aproximadamente 5,5 vezes mais tempo para processar e responder às consultas realizadas.
</p>
<p align="justify">
Essa diferença de desempenho pode ser explicada, em grande parte, pelas características arquiteturais dos modelos avaliados. O Gemma 3 (12B) possui aproximadamente três vezes mais parâmetros do que versões menores, como o Gemma 3 (4B). Em modelos de linguagem, o número de parâmetros está diretamente relacionado à quantidade de cálculos necessários durante o processo de inferência. Dessa forma, quanto maior o modelo, maior é o volume de dados processados a cada etapa de geração de texto, resultando em maior demanda de recursos computacionais e, consequentemente, em aumento do tempo de resposta.
</p>
<p align="justify">
Além disso, a geração de cada token exige sucessivas operações matemáticas sobre toda a estrutura do modelo. Como o Gemma 3(12B) possui uma quantidade significativamente maior de parâmetros, o tempo necessário para produzir cada token tende a ser superior quando comparado a modelos menores.
</p>
<p align="justify">
Os resultados observados demonstram um importante compromisso entre capacidade do modelo e eficiência computacional. Embora modelos maiores possuam potencial para respostas mais elaboradas e maior capacidade de compreensão contextual, eles também apresentam custos computacionais superiores, refletidos em maiores tempos de latência. 
</p>


### 4. Conclusões
<p align="justify">
Embora nenhum modelo tenha alcançado desempenho perfeito, o Modelo Gemma 3 (4B) demonstrou maior precisão na identificação das fontes adequadas e melhor capacidade de fundamentar suas respostas nos dados disponibilizados pelo sistema RAG.
</p>
<p align="justify">
Os resultados obtidos indicam que a qualidade de um sistema baseado em RAG não depende exclusivamente do desempenho do modelo de linguagem, mas também de sua capacidade de interpretar corretamente o contexto recuperado e selecionar evidências pertinentes à consulta realizada. Nesse aspecto, o Modelo Gemma 3 (4B) mostrou-se mais robusto e confiável, apresentando menor incidência de respostas incorretas decorrentes de recuperação inadequada de informações e maior aderência ao conhecimento efetivamente disponível na base documental.
</p>
<p align="justify">
Em síntese, a análise evidencia que, embora ambos os modelos tenham sido capazes de responder à maioria das questões propostas, o Modelo Gemma 3 (4B) apresentou desempenho melhor em termos de precisão, contextualização e confiabilidade das respostas, enquanto o Modelo  Gemma 3 (4B) apresentou maior suscetibilidade à utilização de informações irrelevantes e à falha na identificação de conteúdos existentes na base de conhecimento. Esses resultados reforçam a importância de avaliar não apenas a capacidade generativa dos LLMs, mas também sua eficiência na integração com mecanismos de recuperação de informações em cenários RAG. 
</p>
<p align="justify">
Outro ponto relevante que a análise evidênciou sobre os dados coletados por meio do LangSmith foi que o aumento do número de parâmetros impacta diretamente a latência dos modelos avaliados. Os valores médios obtidos, de 16,15 segundos para o Modelo Gemma 3 (4B) e 88,77 segundos para o Modelo Gemma 3(12B), confirmam a influência do porte do modelo sobre o desempenho computacional da solução RAG, fornecendo subsídios importantes para a escolha do modelo mais adequado de acordo com os requisitos de tempo de resposta e qualidade esperados pela aplicação.
</p>


#### Matrícula: 252100064

#### Pontifícia Universidade Católica do Rio de Janeiro

#### Curso de Pós Graduação *MASTER em Inteligência Artificial Generativa & Large Language Models da PUC-Rio*

