# Projeto Kafka e Zookeeper: Tutorial Passo a Passo

Seja bem-vindo(a)! Este repositório contém um tutorial completo para você configurar e rodar o **Zookeeper** e o **Apache Kafka** localmente, usando o **WSL (Windows Subsystem for Linux)** ou outro ambiente Linux. O objetivo aqui é facilitar sua vida com um passo a passo simples, para que qualquer pessoa, desde iniciantes até quem já tem mais experiência, consiga iniciar esses serviços essenciais para o processamento de dados em tempo real. Vamos juntos nessa!

## Índice
1. [Pré-requisitos](#pré-requisitos)
2. [Instalação do Zookeeper e Kafka](#instalação-do-zookeeper-e-kafka)
3. [Iniciando o Zookeeper e Kafka](#iniciando-o-zookeeper-e-kafka)
4. [Comandos úteis do Kafka](#comandos-úteis-do-kafka)
5. [Como testar a configuração](#como-testar-a-configuração)
6. [Conclusão](#conclusão)

---

## Pré-requisitos

Antes de colocar a mão na massa, é importante garantir que você tem tudo o que precisa instalado. Aqui estão os pré-requisitos:

1. **Java 8 ou superior**: O Apache Kafka e Zookeeper precisam do Java para funcionar. Se você ainda não tem o Java instalado, pode fazer isso facilmente no WSL (ou em qualquer distribuição Linux) com os seguintes comandos:

   ```bash
   sudo apt update
   sudo apt install default-jre
   ```

   Esse comando vai atualizar os pacotes e instalar a versão padrão do Java Runtime Environment (JRE). Se você já tem o Java instalado, pode pular essa parte.

2. **WSL (Windows Subsystem for Linux)**: Caso você esteja usando Windows, é necessário ter o WSL configurado para rodar o Ubuntu ou outra distribuição Linux. Verifique se você já tem o WSL instalado e o Ubuntu configurado. Se não tiver, você pode seguir [este guia oficial da Microsoft](https://docs.microsoft.com/pt-br/windows/wsl/install) para configurar.

3. **Apache Kafka e Zookeeper**: Vamos baixar e configurar o Kafka e o Zookeeper nos próximos passos, então fique tranquilo(a), que explico tudo em detalhes.

---

## Instalação do Zookeeper e Kafka

### 1. **Baixando o Kafka e Zookeeper**

Primeiro, vamos fazer o download do Apache Kafka, que já vem com o Zookeeper. Siga os passos abaixo:

   - Acesse o site oficial do Apache Kafka: [https://kafka.apache.org/downloads](https://kafka.apache.org/downloads).
   - Baixe a versão mais recente (com Scala 2.x), que é compatível com o que vamos configurar.
   
   Agora, no terminal do WSL, execute os comandos para baixar e descompactar o Kafka:

   ```bash
   wget https://downloads.apache.org/kafka/2.8.0/kafka_2.13-2.8.0.tgz
   tar -xvzf kafka_2.13-2.8.0.tgz
   cd kafka_2.13-2.8.0
   ```

   - O comando `wget` baixa o arquivo.
   - O comando `tar` descompacta o arquivo `.tgz`.
   - O `cd` move você para o diretório onde o Kafka foi extraído.

### 2. **Configuração inicial**

O Kafka já vem com arquivos de configuração prontos para o Zookeeper e para ele mesmo. Esses arquivos ficam na pasta `config/`. Para rodar tudo na configuração padrão (que já é suficiente para testes locais), não precisamos alterar nada. Se quiser, mais tarde você pode explorar essas configurações, mas por enquanto vamos seguir com o padrão.

---

## Iniciando o Zookeeper e Kafka

Agora que tudo está instalado e configurado, é hora de rodar os serviços.

### 1. **Iniciar o Zookeeper**

O Zookeeper é essencial para o Kafka, pois ele é responsável por coordenar e manter o estado do cluster. Vamos iniciar o Zookeeper com o seguinte comando:

```bash
bin/zookeeper-server-start.sh config/zookeeper.properties
```

Este comando inicia o Zookeeper usando as configurações do arquivo `zookeeper.properties`. Deixe essa janela do terminal aberta, pois o Zookeeper precisa continuar rodando enquanto o Kafka estiver ativo.

### 2. **Iniciar o Kafka**

Com o Zookeeper rodando, vamos iniciar o Kafka. Abra uma **nova aba** ou **nova janela** do terminal e execute o comando:

```bash
bin/kafka-server-start.sh config/server.properties
```

Este comando vai iniciar o servidor Kafka, que estará pronto para começar a produzir e consumir mensagens. Assim como o Zookeeper, o Kafka precisa continuar rodando, então mantenha essa janela aberta.

---

## Comandos úteis do Kafka

Agora que o Kafka está rodando, vamos explorar alguns comandos básicos para interagir com ele.

### 1. **Criar um novo tópico no Kafka**

Um "tópico" no Kafka é como um canal de comunicação onde as mensagens serão enviadas. Vamos criar um tópico chamado `meu-topico` com o seguinte comando:

```bash
bin/kafka-topics.sh --create --topic meu-topico --bootstrap-server localhost:9092 --partitions 1 --replication-factor 1
```

- `--partitions 1` indica que o tópico terá apenas uma partição.
- `--replication-factor 1` significa que não haverá replicação (útil para um ambiente de teste local).

### 2. **Listar todos os tópicos existentes**

Para verificar os tópicos que já foram criados no Kafka, use o seguinte comando:

```bash
bin/kafka-topics.sh --list --bootstrap-server localhost:9092
```

Esse comando vai listar todos os tópicos que estão rodando no servidor Kafka.

### 3. **Produzir mensagens em um tópico**

Agora, vamos produzir (enviar) algumas mensagens para o tópico `meu-topico`. Execute o comando abaixo:

```bash
bin/kafka-console-producer.sh --topic meu-topico --bootstrap-server localhost:9092
```

Após rodar esse comando, o terminal ficará aguardando para você digitar suas mensagens. Cada linha que você digitar será enviada como uma mensagem para o tópico.

### 4. **Consumir mensagens de um tópico**

Para ler (ou consumir) as mensagens que foram enviadas para o tópico, rode o seguinte comando:

```bash
bin/kafka-console-consumer.sh --topic meu-topico --from-beginning --bootstrap-server localhost:9092
```

Esse comando vai mostrar todas as mensagens enviadas para o tópico, desde o início.

---

## Como testar a configuração

Agora vamos testar tudo para garantir que está funcionando:

1. **Inicie o Zookeeper** em uma aba/janela do terminal.
2. **Inicie o Kafka** em outra aba/janela.
3. **Crie um tópico** com o comando de criação de tópicos.
4. **Produza algumas mensagens** no tópico.
5. **Consuma essas mensagens** para verificar se elas estão sendo lidas corretamente.

Se tudo estiver funcionando, você verá as mensagens que produziu sendo exibidas no terminal onde o consumidor foi rodado.

---

## Conclusão

E aí, deu tudo certo? Se você seguiu os passos, já deve ter o Kafka e o Zookeeper rodando perfeitamente no seu ambiente local. Vimos como configurar, rodar, produzir e consumir mensagens de um tópico. A partir daqui, você pode expandir seu projeto, integrando o Kafka com outras ferramentas, como o Apache Spark, ou criando tópicos mais avançados com múltiplas partições.

Caso tenha alguma dúvida ou sugestão, sinta-se à vontade para abrir uma issue ou contribuir com este repositório. Vamos construir juntos!

---

**Contribuições**:
Contribuições são sempre bem-vindas! Se você tiver alguma ideia, sugestão ou quiser adicionar novas funcionalidades, crie um pull request e vamos aprimorar este projeto juntos.

---

### Links úteis:
- [Documentação oficial do Kafka](https://kafka.apache.org/documentation/)
- [Tutorial básico do Kafka](https://kafka.apache.org/quickstart)
- [Guia sobre Zookeeper](https://zookeeper.apache.org/doc/current/zookeeperStarted.html)
