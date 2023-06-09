# Anotações sobre Cloud Computing

## Instrutores

[Ricardo Merces](https://www.linkedin.com/in/ricardomerces/) - curso ["Cloud Onboarding: trabalhando com os principais provedores"](https://www.alura.com.br/curso-online-cloud-onboarding-trabalhando-principais-provedores)

## Definições

**Cloud Computing**: disponibilização/entrega, sob demanda, de serviços de computação, banco de dados, páginas, e quaisquer outros recursos de TI.

- Sob demanda, ou seja, **escalável**;

- Provedor: você não tem o hardware físico. Você contrata **serviços em nuvem** e acessa os provedores via internet.

A contratação de serviços em nuvem substitui os Data Centers das empresas: elimina os custos com infraestrutura física (investimento inicial) e manutenção (investimento contínuo). Os custos agora são em cima do uso dos serviços, seus **gastos são pelo tempo de uso**.

- custos acessíveis para pequenas empresas;

- apesar disso, existem outras opções de planos e precificações, de acordo com as necessidades e tamanhos de cada empresa. 

    - Exemplos: instâncias *spot* (serviços compartilhados com outros usuários) e *reservadas* (quando você planeja utilizar os serviços por um período de anos específico).
	
- presença global: possibilidade de implantar seu aplicativo em provedores situados em diferentes regiões do globo, para que sejam acessados com alta velocidade. 

## Modelos de infraestrutura

**Modelo de Nuvem Pública**: utilizo um provedor para colocar toda a infraestrutura que eu preciso.

**Modelo Híbrido**: parte da infra fica dentro da empresa, parte fica na nuvem.

**Modelo On-premises**: infra está toda na empresa (premise: instalação, um local físico).

## Sopa de letrinhas

**IaaS (Infrastructure as a Service)**: subir uma máquina virtual para colocar uma aplicação.

**PaaS (Platform as a Service)**: elevo o escopo, irei subir a aplicação, sem me preocupar com infraestrutura (o provedor toma conta de provisionar as máquinas virtuais e a infra necessária para que a aplicação rode).

**SaaS (Software as a Service)**: aqui não tem a ver com deploy, mas sim com a *utilização* de um software com algum tipo de infraestrura na nuvem. Ex.: Dropbox, Google Docs, Google Colab.

**EC2 (Elastic Computer 2)**: é um nome comumente ouvido quando se fala sobre cloud. É referente a uma máquina virtual.

**Amazon S3**: é o nome dado ao serviço de storage (armazenamento) da Amazon. No Google Cloud é chamado de *Cloud Storage* e no Azure, de *Blob*.

**CLI:** Command Line Interface.

## Provedores Cloud

Principais provedores, em ordem de dominância do mercado (2022): 

- AWS (Amazon);

- Azure (Microsoft);

- Google Cloud;

- Oracle e IBM também são provedores, em menor escala.

Qual o melhor? Depende de cada aplicação e necessidade.

- oferecem serviços gratuitos limitados (período gratuito, ou serviços específicos gratuitos, ou limite de uso). Todos permitem criar uma conta gratuita, mas é **necessário informar um cartão de crédito internacional**;

- é possível configurar alertas para que não seja cobrado no cartão ou que avise os limites de uso, mas mesmo assim, tem esse risco de estar com um cartão vinculado à sua conta e você se esquecer disso....

Para não ter que pagar para usar ou evitar informar um cartão de crédito para uso gratuito, há as opções de estudo gratuitas:

- https://cloud.google.com/edu/students

- https://aws.amazon.com/pt/training/digital/

- https://docs.microsoft.com/pt-br/learn/azure/

*Fiz só a parte inicial do curso, para ter uma noção da área. Para acompanhar o restante das aulas de forma prática, é necessário abrir uma conta e vincular um cartão, então eu só assisti aos vídeos, fiz os exercícios teóricos e não acompanhei os cursos seguintes.*