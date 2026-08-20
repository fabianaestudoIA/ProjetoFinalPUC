# Projeto Final - RAG: Comparando os modelos Gemma 3 (4B e 12B) executados via Ollama

#### Aluno: Fabiana Viana         (https://github.com/fabianaestudoIA)
#### Orientadora: Evelyn Batista  (https://github.com/evysb)


Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".

- Link do Projeto:
- https://github.com/fabianaestudoIA/ProjetoFinalPUC

  
### Resumo

Resumo do Projeto

Este Trabalho de Conclusão de Curso (TCC) propõe o desenvolvimento de um assistente inteligente baseado em Inteligência Artificial Generativa e Retrieval-Augmented Generation (RAG) e visa comparar os modelos Gemma 3 (4B e 12B) executados via Ollama 
O projeto do RAG teve como inspiração a necessidade observada pelos usuários da empresa na qual trabalho, para encontrar informações em um sistema de gestão acadêmica onde reuni vários "Documentos" em PDF sobre regulamentos, contratos, manuais e normas.

Atualmente, os usuários precisam localizar manualmente os documentos e navegar por seu conteúdo para encontrar informações específicas, o que pode demandar tempo e dificultar o acesso rápido às informações desejadas. 


### 1. Introdução


A empresa onde trabalho, dispões de um sistema de gestão acadêmica onde reuni vários "Documentos" do sistema em formato PDF, incluindo regulamentos, contratos, manuais e normas. O crescente volume de documentos disponibilizados em sistemas de gestão acadêmica torna a busca por informações um processo, muitas vezes, demorado e pouco intuitivo para os usuários.

Diante desse cenário, este Trabalho de Conclusão de Curso propõe o desenvolvimento e a avaliação de um Retrieval-Augmented Generation (RAG) capaz de consultar os documentos e fornecer respostas em linguagem natural aos usuários.

Devido ao caráter confidencial das informações corporativas, não foi possível utilizar os documentos reais da organização durante o desenvolvimento e os testes do projeto. Para contornar essa limitação e preservar a segurança dos dados, a base de conhecimento foi composta por documentos públicos em formato PDF contendo informações relacionadas às regras de trânsito, obtidos a partir de fontes disponíveis na internet. Dessa forma, foi possível reproduzir um cenário semelhante ao ambiente corporativo sem comprometer informações sensíveis da instituição.

A arquitetura adotada foi baseada na técnica Retrieval-Augmented Generation (RAG), que combina recuperação de informações e geração de texto por modelos de linguagem. Nesse modelo, o sistema realiza inicialmente a busca dos trechos mais relevantes nos documentos da base de conhecimento e, em seguida, utiliza essas informações como contexto para a construção da resposta, aumentando a precisão e a confiabilidade dos resultados.

O RAG foi testado com dois modelos, o  Gemma 3 (4B e 12B) executados via Ollama. A avaliação foi realizada por meio de um conjunto padronizado de perguntas aplicadas aos dois modelos, permitindo analisar critérios como precisão das respostas, aderência ao contexto recuperado, capacidade de recuperação das informações e ocorrência de respostas incorretas ou não fundamentadas.


### 2. Modelagem

Para a implementação do protótipo, foi utilizado o ambiente Google Colab, facilitando o desenvolvimento e a execução dos experimentos. Os documentos PDF utilizados como base de conhecimento foram armazenados no diretório /content, enquanto a interface de interação com o usuário foi desenvolvida com o framework Gradio, proporcionando uma experiência simples e intuitiva. Adicionalmente, foi empregada a ferramenta LangSmith para observabilidade, monitoramento e análise das execuções do sistema, permitindo acompanhar o comportamento dos modelos durante os testes e avaliar a qualidade das respostas geradas.


### Construção do Pipeline RAG

Inicialmente, os documentos que compõem a base de conhecimento passaram por um processo de segmentação (chunking). Essa etapa é fundamental para melhorar a granularidade da recuperação de informações, uma vez que documentos extensos podem dificultar a identificação de trechos específicos relacionados à consulta do usuário. Para essa finalidade, foi utilizada a classe RecursiveCharacterTextSplitter, configurada com um tamanho de fragmento (chunk_size) de 1.250 caracteres e uma sobreposição (chunk_overlap) de 200 caracteres entre chunks consecutivos.

A estratégia de sobreposição adotada visa preservar o contexto semântico entre os fragmentos, reduzindo a possibilidade de perda de informações relevantes localizadas nas extremidades dos textos. Dessa forma, conteúdos que poderiam ser separados durante o processo de divisão permanecem parcialmente replicados em chunks adjacentes, aumentando a qualidade da recuperação posterior.

Após a segmentação dos documentos, foi realizada a etapa de vetorização dos dados textuais por meio da geração de embeddings. Para essa finalidade, optou-se pela utilização do modelo all-MiniLM-L6-v2, disponibilizado pela biblioteca Sentence Transformers. A escolha desse modelo foi motivada pelo seu equilíbrio entre eficiência computacional e qualidade semântica das representações vetoriais geradas.

Os embeddings são representações numéricas densas dos textos, capazes de capturar relações semânticas e contextuais entre palavras, frases e documentos. Diferentemente de métodos tradicionais de busca baseados apenas em correspondência lexical, a utilização de embeddings permite que consultas semanticamente semelhantes sejam associadas a conteúdos relevantes, mesmo quando não há coincidência exata entre os termos utilizados.

Em seguida, os embeddings gerados foram armazenados em um banco vetorial utilizando o ChromaDB, uma solução amplamente empregada em arquiteturas RAG devido à sua simplicidade de integração e capacidade de persistência. A criação do repositório vetorial foi realizada por meio do método Chroma.from_documents(), que recebe os chunks gerados e seus respectivos embeddings, armazenando-os em um diretório persistente denominado chroma_db.

A persistência dos vetores em disco representa uma vantagem significativa para o sistema, pois elimina a necessidade de reconstrução completa do índice a cada reinicialização da aplicação. Dessa forma, a base vetorial pode ser reutilizada em diferentes execuções, reduzindo o tempo de processamento e aumentando a eficiência operacional da solução.

Por fim, foi configurado o componente de recuperação de informações (Retriever), responsável por realizar buscas semânticas no banco vetorial. A partir do método as_retriever(), o sistema passou a disponibilizar uma interface capaz de localizar os chunks mais relevantes para uma determinada consulta, utilizando métricas de similaridade entre os embeddings da pergunta e os embeddings armazenados na base documental.

O retriever desempenha papel central na arquitetura RAG, pois atua como mecanismo intermediário entre a consulta do usuário e o modelo de linguagem generativa. Quando uma pergunta é submetida ao sistema, o retriever identifica os fragmentos de texto semanticamente mais relevantes e os encaminha como contexto adicional para o modelo de geração. Dessa forma, as respostas produzidas passam a ser fundamentadas em informações efetivamente presentes na base de conhecimento, reduzindo alucinações e aumentando a confiabilidade dos resultados.

Em síntese, o pipeline desenvolvido foi composto por quatro etapas principais: 
- Segmentação dos documentos em chunks com preservação de contexto
- Geração de embeddings semânticos utilizando o modelo all-MiniLM-L6-v2
- Armazenamento dos vetores no banco ChromaDB
- Recuperação de informações por meio de um retriever semântico. 


Essa arquitetura possibilita a construção de sistemas de busca inteligente e geração contextualizada de respostas, constituindo uma abordagem eficiente para aplicações que demandam acesso rápido e preciso a grandes volumes de informação textual.

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

Análise Comparativa dos Resultados dos Modelos de Linguagem em Ambiente RAG

Com o objetivo de avaliar o desempenho de dois Modelos de Linguagem de Grande Escala (LLMs) em um cenário de Retrieval-Augmented Generation (RAG), foi conduzido um experimento no qual ambos os modelos responderam ao mesmo conjunto de 10 perguntas. As questões foram elaboradas para exigir a recuperação e interpretação de informações presentes na base de conhecimento utilizada pelo sistema RAG, permitindo observar a capacidade de cada modelo em fundamentar suas respostas em evidências recuperadas.

Os resultados indicaram diferenças entre os modelos, especialmente no que se refere à ocorrência de alucinações. Embora ambos tenham demonstrado capacidade de compreender as perguntas e utilizar o contexto fornecido pelo mecanismo de recuperação, o Modelo Gemma 3 (4B) 
apresentou uma frequência maior de respostas incorretas, evidenciando dificuldades na etapa de seleção e utilização das evidências recuperadas pelo mecanismo RAG.

Por outro lado, o Modelo Gemma 3 (2B) apresentou um comportamento mais consistente e aderente às informações efetivamente recuperadas, demonstrando maior cautela na elaboração das respostas. Quando confrontado com limitações ou lacunas na base de conhecimento, o modelo tendeu a restringir suas respostas ao conteúdo disponível, reduzindo significativamente a inserção de informações incorretas ou especulativas.



### 4. Conclusões

Embora nenhum modelo tenha alcançado desempenho perfeito, o Modelo Gemma 3 (2B) demonstrou maior precisão na identificação das fontes adequadas e melhor capacidade de fundamentar suas respostas nos dados disponibilizados pelo sistema RAG.

Os resultados obtidos indicam que a qualidade de um sistema baseado em RAG não depende exclusivamente do desempenho do modelo de linguagem, mas também de sua capacidade de interpretar corretamente o contexto recuperado e selecionar evidências pertinentes à consulta realizada. Nesse aspecto, o Modelo Gemma 3 (2B) mostrou-se mais robusto e confiável, apresentando menor incidência de respostas incorretas decorrentes de recuperação inadequada de informações e maior aderência ao conhecimento efetivamente disponível na base documental.

Em síntese, a análise evidencia que, embora ambos os modelos tenham sido capazes de responder à maioria das questões propostas, o Modelo Gemma 3 (2B) apresentou desempenho melhor em termos de precisão, contextualização e confiabilidade das respostas, enquanto o Modelo  Gemma 3 (4B) apresentou maior suscetibilidade à utilização de informações irrelevantes e à falha na identificação de conteúdos existentes na base de conhecimento. Esses resultados reforçam a importância de avaliar não apenas a capacidade generativa dos LLMs, mas também sua eficiência na integração com mecanismos de recuperação de informações em cenários RAG.


Matrícula: 252100064

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
