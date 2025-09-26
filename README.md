# Fhir

O principal objetivo do FHIR é resolver um problema histórico na área da saúde: a interoperabilidade. Ou seja, a capacidade de diferentes sistemas de informação (prontuários eletrônicos, sistemas de laboratório, aplicativos de pacientes, etc.) trocarem dados de saúde de forma padronizada, segura e eficiente.

O FHIR foi construído com as tecnologias e a mentalidade da web moderna: - Foco em API: Utiliza uma abordagem API RESTful, familiar para qualquer desenvolvedor web. - Formatos Flexíveis: Usa formatos de dados leves e fáceis de manipular, como JSON e XML. - Foco na Implementação: Foi projetado para ser fácil de entender e implementar, com documentação rica e servidores de teste públicos.

## Conceitos Fundamentais

### Resources

Os Recursos são o coração do FHIR. Eles são "pedaços" de informação de saúde, definidos de forma modular e com um escopo bem definido. Pense neles como os "substantivos" do mundo da saúde.

Exemplos Comuns: - Patient: Informações demográficas sobre um paciente. - Practitioner: Informações sobre um profissional de saúde (médico, enfermeiro). - Organization: Informações sobre uma instituição (hospital, clínica, laboratório). - Observation: Resultados de exames, medições (pressão arterial, glicemia), sinais vitais. - Encounter: Um encontro clínico, como uma consulta ou internação. - MedicationRequest: Uma prescrição de medicamento.

Cada Recurso tem uma estrutura bem definida com elementos comuns e específicos, baseados em tipos de dados padronizados.

### Interações (API RESTful)

A comunicação com um servidor FHIR ocorre através de uma API RESTful, usando os verbos HTTP padrão: - CREATE: POST /patient - Cria um novo recurso de paciente. - READ: GET /patient/123 - Lê os dados do paciente com o ID "123". - UPDATE: PUT /patient/123 - Atualiza (substitui) os dados do paciente com o ID "123". - DELETE: DELETE /patient/123 - Remove o recurso do paciente com o ID "123". - SEARCH: GET /patient?name=Silva - Busca por pacientes cujo nome contém "Silva". A busca é extremamente poderosa e uma das maiores vantagens do FHIR.

### Perfis (Profiles)

Um Recurso base do FHIR é genérico para ser usado globalmente. Um Perfil (Profile) é uma camada de customização que adapta um Recurso para um contexto específico. Com um perfil, você pode: - Restringir: Tornar um campo opcional em obrigatório. - Estender: Adicionar novas informações usando a estrutura de Extension. - Definir Terminologias: Especificar que um campo deve usar um conjunto de códigos específico (ex: tipos de documentos válidos no Brasil). - Exemplo Prático: A RNDS (Rede Nacional de Dados em Saúde) do Brasil utiliza perfis FHIR para padronizar como as informações de vacinação, exames de Covid-19, etc., devem ser representadas. Um Patient no padrão brasileiro, por exemplo, terá obrigatoriamente um CPF.

### Formatos de Dados

FHIR suporta principalmente dois formatos para representar os recursos: - JSON (JavaScript Object Notation): O mais comum e preferido para aplicações web e mobile por ser mais leve e fácil de manipular. - XML (eXtensible Markup Language): Formato mais verboso, mas ainda amplamente suportado.

## Aplicações no Mundo Real

O FHIR não é apenas teoria. Ele já está sendo usado para: - Aplicativos de Pacientes: Permitir que pacientes acessem seus próprios dados de saúde de diferentes hospitais em um único app (ex: Apple Health Records usa FHIR). - Integração Hospitalar: Conectar o sistema de prontuário eletrônico a sistemas de farmácia, laboratório e radiologia dentro de um hospital. - Saúde Pública: Coletar dados anonimizados para vigilância epidemiológica e relatórios governamentais (o caso da RNDS no Brasil). - Pesquisa Clínica: Agregar dados de múltiplas fontes para estudos clínicos de forma padronizada. - Sistemas de Apoio à Decisão Clínica: Fornecer dados em tempo real para algoritmos que ajudam os médicos a tomar melhores decisões.

## Melhores Práticas de Implementação

Ao construir soluções com FHIR, seguir estas práticas é fundamental para o sucesso.

    1. Segurança em Primeiro Lugar
        - Use HTTPS Sempre: Todo o tráfego deve ser criptografado com TLS.
        - Autenticação e Autorização Robustas: O padrão SMART on FHIR é a melhor prática. Ele combina OAuth 2.0 e OpenID Connect para garantir que apenas usuários e aplicativos autorizados acessem os dados corretos, com escopos de permissão bem definidos.
    2. Priorize a Reutilização
        - Não Reinvente a Roda: Antes de criar um Perfil do zero, verifique se já existe um perfil nacional (como os da RNDS) ou internacional (como o US Core) que atenda às suas necessidades.
        - Use Ferramentas de Profiling: Utilize ferramentas como o Forge ou o Simplifier para criar e validar seus perfis.
    3. Gerenciamento de Identificadores
        - Utilize o tipo de dado Identifier corretamente para armazenar os diferentes identificadores de negócio (CPF, CNS, número de prontuário, etc.), especificando o sistema de cada um.
    4. Use Terminologias Padrão
        - Sempre que possível, vincule campos CodeableConcept a terminologias padrão (como LOINC, SNOMED CT) ou a conjuntos de valores (ValueSets) bem definidos em seu perfil. Isso garante o significado semântico dos dados.
    5. Otimize as Buscas
        - Use os parâmetros de busca padrão do FHIR. Evite criar parâmetros customizados a menos que seja estritamente necessário.
        - Implemente a paginação para lidar com grandes volumes de resultados, usando os parâmetros _count e os links next nos Bundles de resposta.
    6. Extensibilidade com Cautela
        - Use o elemento Extension para dados que não se encaixam na estrutura padrão do Recurso, mas documente-as claramente no seu Perfil (StructureDefinition).

## Interoperabilidade em Saúde

A interoperabilidade em saúde refere‑se à capacidade de diferentes sistemas, dispositivos e aplicativos trocarem informações clínicas de forma segura, padronizada e compreensível. O objetivo é que dados como prontuários eletrônicos (EHR), resultados de exames, prescrições e informações de dispositivos de monitoramento estejam disponíveis no ponto de cuidado adequado, independentemente da solução tecnológica utilizada.

### Conceitos

Antes de mais nada, é crucial dominar os "níveis" de interoperabilidade. Eles definem a qualidade e a profundidade com que os sistemas se comunicam.

    Nível 1: Interoperabilidade Técnica (Foundational)
        O que é: Garante que os sistemas consigam trocar dados. É a "tubulação".
        Foco: Protocolos de comunicação (HTTP, TCP/IP), infraestrutura de rede, APIs. Basicamente, um sistema consegue enviar um pacote de dados e outro consegue recebê-lo.

    Nível 2: Interoperabilidade Sintática (Structural)
        O que é: Garante que a estrutura da informação seja compreendida. É a "gramática".
        Foco: Formatos de dados (JSON, XML) e a ordem dos campos. O sistema receptor sabe onde encontrar o nome do paciente, a data de nascimento, etc. Padrões como HL7v2 e FHIR atuam fortemente aqui.

    Nível 3: Interoperabilidade Semântica (Semantic)
        O que é: Garante que o significado da informação seja o mesmo para ambos os sistemas. É o "dicionário" compartilhado e o nível mais crítico e desafiador.
        Foco: Terminologias e vocabulários controlados. Não basta saber onde está o campo "diagnóstico"; é preciso que "Infarto Agudo do Miocárdio" seja representado pelo mesmo código (ex: SNOMED CT) em ambos os sistemas, evitando ambiguidades.

    Nível 4: Interoperabilidade Organizacional
        O que é: Inclui os aspectos de governança, legais, éticos e de fluxo de trabalho que permitem a troca de dados entre diferentes organizações.
        Foco: Políticas de consentimento do paciente, leis de proteção de dados (como a LGPD no Brasil), acordos de confiança entre hospitais, e alinhamento de processos de negócio.

### Principais Padrões e Terminologias (Além do FHIR)

O FHIR é moderno, mas convive com padrões legados que ainda são dominantes em muitos hospitais.

    HL7 v2:
        O que é: O padrão mais utilizado no mundo para troca de mensagens de saúde (admissões, resultados de exames, prescrições). É o "workhorse" da interoperabilidade.
        Como funciona: Baseado em mensagens de texto com delimitadores (|, ^). É menos intuitivo que o FHIR, mas extremamente robusto e difundido. Um mecanismo de integração (Integration Engine) geralmente traduz mensagens HL7v2 para outros formatos, como FHIR.

    HL7 CDA (Clinical Document Architecture):
        O que é: Um padrão para criar documentos clínicos eletrônicos, como sumários de alta, relatórios de consulta e históricos de pacientes.
        Como funciona: Baseado em XML. Define a estrutura e o conteúdo semântico de um documento clínico para que ele possa ser lido e interpretado por qualquer sistema compatível.

    DICOM (Digital Imaging and Communications in Medicine):
        O que é: O padrão universal para imagens médicas (raio-x, tomografia, ressonância) e informações relacionadas.
        Foco: Não apenas armazena a imagem (pixel data), mas também metadados como ID do paciente, parâmetros de aquisição e muito mais. Ele define protocolos de rede para consultar e recuperar imagens de um PACS (Picture Archiving and Communication System).

    openEHR:
        O que é: Uma abordagem alternativa e poderosa para prontuários eletrônicos. Em vez de focar na troca de dados, foca em criar um modelo de dados clínicos persistente, flexível e à prova de futuro.
        Como funciona: Separa o modelo de informação (Reference Model) do conhecimento clínico (Arquétipos e Templates). Isso permite que clínicos modelem novos formulários e dados sem precisar de mudanças no software, o que é uma grande vantagem.

    Terminologias Clínicas (Crucial para a Semântica):
        SNOMED CT: O vocabulário clínico mais abrangente do mundo. Usado para diagnósticos, sinais, sintomas, procedimentos, etc.
        LOINC: Padrão universal para identificar testes de laboratório e observações clínicas.
        CID-10/CID-11 (ICD em inglês): Usada principalmente para faturamento, estatísticas e mortalidade.
        TUSS (Brasil): Terminologia Unificada da Saúde Suplementar, usada para faturamento de convênios no Brasil.

### Arquitetura e Tecnologias de Implementação

    Mecanismos de Integração (Integration Engines):
        São o coração da interoperabilidade em um hospital. Softwares como Mirth Connect (open source), Intersystems HealthShare ou Orion Health Rhapsody atuam como um hub central, recebendo mensagens em um padrão (ex: HL7v2 do sistema laboratorial) e transformando-as em outro (ex: FHIR para um novo aplicativo mobile).

    IHE (Integrating the Healthcare Enterprise):
        O que é: Uma iniciativa que não cria padrões, mas define "Perfis de Integração" que mostram como usar os padrões existentes (FHIR, HL7, DICOM) para resolver problemas clínicos do mundo real.
        Exemplo: O perfil PIX/PDQ define como usar HL7v2 para gerenciar a identidade de um paciente entre múltiplos sistemas, garantindo que "João da Silva" seja a mesma pessoa no sistema da farmácia e no da radiologia.

### Principais dores (desafios)

    1. Fragmentação de Sistemas
        - Hospitais, clínicas e laboratórios utilizam soluções proprietárias incompatíveis.
        - Falta de padrões uniformes dificulta a troca de dados.
    2. Ausência ou implementação incompleta de padrões
        - Uso parcial de FHIR, HL7 v2/v3 ou CDA gera inconsistências.
        - Definições personalizadas (customizações) criam barreiras de integração.
    3. Governança e políticas de privacidade
        - Conformidade com LGPD, HIPAA e outras regulamentações impõe requisitos rígidos para consentimento e auditoria.
        - Processos burocráticos retardam a habilitação de interfaces.
    4. Qualidade e semântica dos dados
        - Dados incompletos ou não normalizados geram interpretações equivocadas.
        - Falta de terminologias padronizadas (SNOMED CT, LOINC, RxNorm) impede a semântica comum.
    5. Segurança e confiança
        - Risco de vazamento ou adulteração de informações sensíveis requer criptografia robusta e autenticação forte (OAuth 2.0/OpenID Connect).
        - Falta de confiança entre organizações impede compartilhamento aberto.
    6. Custo e recursos técnicos
        - Implementar APIs FHIR envolve investimento em desenvolvimento, treinamento e manutenção.
        - Escassez de profissionais com expertise em arquitetura FHIR aumenta o custo total.

### Casos de uso mais recorrentes

| Caso de Uso                      | Descrição                                                                         | Benefícios                                                       |
| -------------------------------- | --------------------------------------------------------------------------------- | ---------------------------------------------------------------- |
| Consulta a histórico do paciente | Um médico acessa o prontuário completo do paciente via API FHIR Patient/Encounter | Redução de duplicidade de exames; decisão clínica mais informada |
| Integração laboratório‑hospital  | Resultados laboratoriais são enviados em tempo real usando FHIR Observation       | Agilidade no diagnóstico; melhora na coordenação do cuidado      |
| Prescrição eletrônica (e‑Rx)     | Sistema de prescrição envia recursos MedicationRequest para farmácias             | Redução de erros de medicação; rastreabilidade                   |
| Monitoramento remoto             | Dispositivos wearables enviam dados vitais via FHIR Observation                   | Gestão proativa de doenças crônicas; intervenções antecipadas    |
| Transferência entre instituições | Uso de Bundle para exportar/importar todo o registro ao transferir o paciente     | Continuidade do cuidado; diminuição de perda de informação       |
| Pesquisa baseada em dados reais  | Agregação anonimizada de recursos FHIR para estudos observacionais                | Aceleração da pesquisa clínica; insights populacionais           |

### Insights para promover melhorias

    1. Adoção plena do padrão FHIR
        - Implementar todas as “clinical resources” relevantes (Patient, Encounter, Observation, MedicationRequest, etc.).
        - Utilizar os “profiles” oficiais (US Core, International Patient Summary) como base para customizações mínimas.

    2. Governança baseada em APIs
        - Definir catálogos internos de APIs com versionamento claro e políticas de ciclo de vida (depreciação programada).
        - Estabelecer contratos (SLAs) entre fornecedores internos/externos.

    3. Camada semântica unificada
        - Padronizar o uso das terminologias SNOMED CT, LOINC e RxNorm nos recursos FHIR.
        - Implementar serviços de mapeamento automático quando houver código local.

    4. Arquitetura orientada a eventos
        - Complementar as APIs RESTful com mensagens baseadas em HL7 v2 ou FHIR Messaging/Subscriptions para notificações em tempo real.

    5. Segurança “by design”
        - Adotar OAuth 2.0 + OpenID Connect para autorização/autenticação granular (scopes por recurso).
        - Aplicar criptografia TLS 1.3 em todas as comunicações e auditoria detalhada das chamadas API.

    6. Capacitação e comunidade
        - Investir em treinamentos internos sobre modelagem FHIR e desenvolvimento HL7‑compatible.
        - Incentivar a participação em grupos open‑source (FHIR Community on GitHub) para reutilizar implementações já validadas.

    7. Estratégia incremental
        - Começar por “use cases” críticos (ex.: resultados laboratoriais) antes de expandir para todo o EHR.
        - Medir métricas chave (tempo médio de acesso ao dado, taxa de erros de integração) para ajustar o roadmap.

### Resumo rápido

    - Interoperabilidade é um campo multidisciplinar. Envolve não apenas tecnologia (código, APIs), mas também um profundo entendimento dos processos de saúde, dos padrões de dados e das relações organizacionais e legais.
    - A interoperabilidade na saúde depende da adoção consistente de padrões como FHIR combinados a terminologias clínicas unificadas.
    - As maiores dores são fragmentação tecnológica, governança rígida e qualidade semântica dos dados.
    - Casos típicos incluem troca automática de resultados laboratoriais, prescrições eletrônicas e monitoramento remoto.
    - Melhorias eficazes surgem ao investir em governança API‑first, segurança avançada (OAuth 2.0), capacitação contínua e estratégias incrementais focadas nos usos críticos.
    - Existem muitos recursos gratuitos — documentos oficiais da HL7®, comunidades online e repositórios open‑source — que podem acelerar a implementação e a maturidade da interoperabilidade nos ambientes clínicos.

## Semântica

São padrões de linguagem universais. Possibilita as pessoas e sistemas interpretarem as informações com o mesmo significado. - CID10 - CIAP2 - SNOMED - RNDS

## Sintática

São padrões que fornecem a estrutura da informação consistente e que deve ser seguida por todos que adotam o padrão. - FHIR -> Interoperabilidade entre sistemas - openEHR -> Utilizado para estruturar o prontuário em si

Diferença entre integrar e interoperar: - Integração -> Sistemas enviam e recebem diversas informações de forma distinta, não padronizada - Interoperabilidade -> Sistemas utilizam uma linguagem única e padronizada

## HL7 FHIR

### Paradigmas

    - Documentos -> Agrupam diversos recursos para representar um registro de saúde.
    - API Rest -> Suporta requisições http, realizando assim operações CRUD no servidor FHIR.
    - Mensagens -> A comunicação FHIR é movida através de eventos que podem ser síncronos e assíncronos. Lógica de pergunta e resposta.
    - Serviços -> Umas das características do FHIR é conseguir separar a camada de dados da camada de aplicação. Permitindo criar serviços através das informações dos servidores FHIR.

## SMART Guideline Components

## Fontes de Conteúdo e Comunidades

Para se aprofundar, siga estas fontes:

    Organizações Oficiais:
        - Documentação completa do padrão FHIR: https://hl7.org/fhir
        - Perfil US Core: https://www.hl7.org/fhir/us/core
        - International Patient Summary: https://www.hl7.org/fhir/uv/ips
        - HL7 International: www.hl7.org (fonte de todos os padrões HL7)
        - IHE International: www.ihe.net (para entender os perfis de integração)
        - openEHR Foundation: www.openehr.org (para a abordagem de arquétipos)
        - SNOMED International: www.snomed.org
        - https://rnds-guia.saude.gov.br (RNDS)
        - Regenstrief Institute: loinc.org (mantenedor do LOINC)

    Livros Essenciais:
        - "Principles of Health Interoperability: SNOMED CT, HL7 and FHIR" por Tim Benson: Considerado uma das bíblias do assunto.
        - "HL7v2: An Introduction": Existem vários guias introdutórios sobre o padrão.
        - Livros sobre "Health Informatics" (Informática em Saúde) costumam ter capítulos dedicados à interoperabilidade.

    Cursos e Certificações:
        - Coursera/edX: Busque por especializações em "Health Informatics" ou "Digital Health". A Georgia Tech, por exemplo, oferece cursos excelentes na área.
        - HL7 International: Oferece certificações em FHIR, HL7v2 e CDA.

    Comunidades e Blogs (Muito Importante):
        - Blog do Grahame Grieve: O "pai do FHIR" mantém um blog (www.healthintersections.com.au) com discussões profundas sobre o padrão e o futuro da interoperabilidade.
        - HL7 Brasil e Comunidades Locais: Participe de fóruns e grupos no Telegram/WhatsApp da comunidade de interoperabilidade brasileira. É o melhor lugar para entender os desafios e as soluções aplicadas no Brasil.
