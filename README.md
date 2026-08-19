# Projeto Final - RAG: Comparando os modelos Gemma 3:4B e Gemma 3:12B do Ollama

#### Aluno: Fabiana Viana         (https://github.com/fabianaestudoIA)
#### Orientadora: Evelyn Batista  (https://github.com/evysb)


Trabalho apresentado ao curso [BI MASTER](https://ica.puc-rio.ai/bi-master) como pré-requisito para conclusão de curso e obtenção de crédito na disciplina "Projetos de Sistemas Inteligentes de Apoio à Decisão".


- [Link para o código]:https://github.com/fabianaestudoIA/ProjetoFinalPUC

  
### Resumo

Resumo do Projeto

Este Trabalho de Conclusão de Curso (TCC) propõe o desenvolvimento de um assistente inteligente baseado em Inteligência Artificial Generativa e Retrieval-Augmented Generation (RAG) e visa comparar os modelos Gemma 3:4B e Gemma 3:12B do Ollama. 
O projeto do RAG teve como inspiração a necessidade observada pelos usuários da empresa na qual trabalho, para encontrar informações em um sistema de gestão acadêmica onde reuni vários "Documentos" em PDF sobre regulamentos, contratos, manuais e normas.

Atualmente, os usuários precisam localizar manualmente os documentos e navegar por seu conteúdo para encontrar informações específicas, o que pode demandar tempo e dificultar o acesso rápido às informações desejadas. 


### 1. Introdução


A empresa onde trabalho, dispões de um sistema de gestão acadêmica onde reuni vários "Documentos" do sistema em formato PDF, incluindo regulamentos, contratos, manuais e normas. O crescente volume de documentos disponibilizados em sistemas de gestão acadêmica torna a busca por informações um processo, muitas vezes, demorado e pouco intuitivo para os usuários.

Diante desse cenário, este Trabalho de Conclusão de Curso propõe o desenvolvimento e a avaliação de um Retrieval-Augmented Generation (RAG) capaz de consultar os documentos e fornecer respostas em linguagem natural aos usuários.

Devido ao caráter confidencial das informações corporativas, não foi possível utilizar os documentos reais da organização durante o desenvolvimento e os testes do projeto. Para contornar essa limitação e preservar a segurança dos dados, a base de conhecimento foi composta por documentos públicos em formato PDF contendo informações relacionadas às regras de trânsito, obtidos a partir de fontes disponíveis na internet. Dessa forma, foi possível reproduzir um cenário semelhante ao ambiente corporativo sem comprometer informações sensíveis da instituição.

A arquitetura adotada foi baseada na técnica Retrieval-Augmented Generation (RAG), que combina recuperação de informações e geração de texto por modelos de linguagem. Nesse modelo, o sistema realiza inicialmente a busca dos trechos mais relevantes nos documentos da base de conhecimento e, em seguida, utiliza essas informações como contexto para a construção da resposta, aumentando a precisão e a confiabilidade dos resultados.

O RAG foi testado com dois modelos da plataforma Ollama, o Gemma 3:4B e o Gemma 3:12B. A avaliação foi realizada por meio de um conjunto padronizado de perguntas aplicadas aos dois modelos, permitindo analisar critérios como precisão das respostas, aderência ao contexto recuperado, capacidade de recuperação das informações e ocorrência de respostas incorretas ou não fundamentadas.


### 2. Modelagem

Para a implementação do protótipo, foi utilizado o ambiente Google Colab, facilitando o desenvolvimento e a execução dos experimentos. Os documentos PDF utilizados como base de conhecimento foram armazenados no diretório /content, enquanto a interface de interação com o usuário foi desenvolvida com o framework Gradio, proporcionando uma experiência simples e intuitiva. Adicionalmente, foi empregada a ferramenta LangSmith para observabilidade, monitoramento e análise das execuções do sistema, permitindo acompanhar o comportamento dos modelos durante os testes e avaliar a qualidade das respostas geradas.

Os resultados obtidos permitiram identificar diferenças de desempenho entre os modelos avaliados, contribuindo para a compreensão do potencial de aplicação de soluções baseadas em RAG em ambientes acadêmicos e corporativos. Dessa forma, o trabalho demonstra como a combinação de modelos de linguagem, recuperação de documentos e observabilidade pode auxiliar na construção de assistentes inteligentes capazes de facilitar o acesso à informação de maneira eficiente, segura e escalável.


### 3. Resultados

Análise Comparativa dos Resultados dos Modelos de Linguagem em Ambiente RAG

Com o objetivo de avaliar o desempenho de dois Modelos de Linguagem de Grande Escala (LLMs) em um cenário de Retrieval-Augmented Generation (RAG), foi conduzido um experimento no qual ambos os modelos responderam ao mesmo conjunto de 10 perguntas. As questões foram elaboradas para exigir a recuperação e interpretação de informações presentes na base de conhecimento utilizada pelo sistema RAG, permitindo observar a capacidade de cada modelo em fundamentar suas respostas em evidências recuperadas.

Os resultados indicaram diferenças entre os modelos, especialmente no que se refere à ocorrência de alucinações. Embora ambos tenham demonstrado capacidade de compreender as perguntas e utilizar o contexto fornecido pelo mecanismo de recuperação, o Modelo 1 
apresentou uma frequência maior de respostas incorretas, evidenciando dificuldades na etapa de seleção e utilização das evidências recuperadas pelo mecanismo RAG.

Por outro lado, o Modelo 2 apresentou um comportamento mais consistente e aderente às informações efetivamente recuperadas, demonstrando maior cautela na elaboração das respostas. Quando confrontado com limitações ou lacunas na base de conhecimento, o modelo tendeu a restringir suas respostas ao conteúdo disponível, reduzindo significativamente a inserção de informações incorretas ou especulativas.



### 4. Conclusões

Embora nenhum modelo tenha alcançado desempenho perfeito, o Modelo 2 demonstrou maior precisão na identificação das fontes adequadas e melhor capacidade de fundamentar suas respostas nos dados disponibilizados pelo sistema RAG.

Os resultados obtidos indicam que a qualidade de um sistema baseado em RAG não depende exclusivamente do desempenho do modelo de linguagem, mas também de sua capacidade de interpretar corretamente o contexto recuperado e selecionar evidências pertinentes à consulta realizada. Nesse aspecto, o Modelo 2 mostrou-se mais robusto e confiável, apresentando menor incidência de respostas incorretas decorrentes de recuperação inadequada de informações e maior aderência ao conhecimento efetivamente disponível na base documental.

Em síntese, a análise evidencia que, embora ambos os modelos tenham sido capazes de responder à maioria das questões propostas, o Modelo 2 apresentou desempenho melhor em termos de precisão, contextualização e confiabilidade das respostas, enquanto o Modelo 1 apresentou maior suscetibilidade à utilização de informações irrelevantes e à falha na identificação de conteúdos existentes na base de conhecimento. Esses resultados reforçam a importância de avaliar não apenas a capacidade generativa dos LLMs, mas também sua eficiência na integração com mecanismos de recuperação de informações em cenários RAG.


Matrícula: 252100064

Pontifícia Universidade Católica do Rio de Janeiro

Curso de Pós Graduação *Business Intelligence Master*
