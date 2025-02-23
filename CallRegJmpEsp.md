# CALL REG e JMP ESP

- Tudo que apontar o padrão pode ser usado como trampolim para execução de códigos
- Não necessariamente é preciso sobrescrever o endereço de retorno com o endereço do nosso código.
- Buscar no código CALL ESP ou JMP ESP
- *kernelbase.dll* pode ser uma boa de olhar o CALL ESP

A técnica chamada **CALL REG** é utilizada na criação de exploits para manipular o fluxo de execução de um programa, geralmente com o objetivo de explorar uma vulnerabilidade e obter controle sobre o sistema. A técnica envolve o uso da instrução **CALL** seguida de um registrador (REG), onde o registrador contém um endereço de memória específico.

Aqui está um resumo de como a técnica funciona:

### 1. **Entendimento da Instrução CALL:**

- A instrução **CALL** é utilizada em linguagens de baixo nível, como Assembly, para invocar uma função ou sub-rotina. Ela empurra o endereço da próxima instrução para a pilha e, em seguida, desvia o fluxo de execução para o endereço de destino.
- Normalmente, a instrução **CALL** é seguida por um endereço estático ou uma label de função. No entanto, no caso do **CALL REG**, a instrução usa um registrador que contém o endereço de destino.

### 2. **Manipulação do Registrador:**

- Para usar essa técnica, um atacante precisa manipular o conteúdo de um registrador específico (como EAX, ECX, EDX, etc.) para apontar para um endereço de memória controlado.
- Se o atacante conseguir injetar um payload ou shellcode em uma localização previsível, ele pode fazer com que o registrador aponte para essa localização.

### 3. **Execução do Payload:**

- Após o registrador ser manipulado para apontar para o código malicioso, a execução da instrução **CALL REG** redireciona o fluxo de execução para o payload.
- Isso permite que o atacante execute código arbitrário no contexto do programa vulnerável.

### 4. **Utilidade em Exploits:**

- A técnica **CALL REG** é especialmente útil quando um atacante tem controle sobre um registrador específico, mas não pode alterar diretamente o ponteiro de instrução (EIP/RIP) ou quando há mitigações que dificultam o redirecionamento direto do fluxo de execução.
- Ela é comum em ataques que envolvem técnicas de retorno (como ROP - Return-Oriented Programming) ou em contextos onde o ASLR (Address Space Layout Randomization) está ativo, mas o atacante conseguiu obter uma fuga parcial de informações sobre o layout da memória.

### 5. **Exemplo Prático:**

- Suponha que um exploit envolve uma vulnerabilidade que permite ao atacante controlar o conteúdo de um registrador como EAX. O atacante pode preparar o ambiente de forma que EAX aponte para um buffer contendo o shellcode. Então, ao usar **CALL EAX**, o shellcode é executado.

Essa técnica, como muitas outras no desenvolvimento de exploits, requer um entendimento profundo da arquitetura do processador e do sistema operacional, bem como da vulnerabilidade que está sendo explorada.

A técnica que envolve o uso da instrução **JMP** seguida do registrador **ESP** é conhecida como **JMP ESP**. Essa técnica é bastante utilizada na exploração de vulnerabilidades, especialmente em buffer overflows, para redirecionar o fluxo de execução para o código injetado (como um shellcode).

### Como Funciona a Técnica JMP ESP:

1. **Contexto da Vulnerabilidade:**
    - Em um ataque de buffer overflow, um atacante consegue sobrescrever o conteúdo da pilha (stack) até o ponto de alterar o valor do ponteiro de instrução (EIP). No entanto, o valor a ser inserido no EIP precisa redirecionar o fluxo de execução para o código malicioso.
2. **Uso do Registrador ESP:**
    - **ESP** é o registrador que aponta para o topo da pilha. Quando o buffer é sobrecarregado, o shellcode geralmente acaba sendo colocado na pilha, imediatamente após o endereço sobrescrito do EIP.
3. **Instrução JMP ESP:**
    - Ao utilizar a instrução **JMP ESP**, o código faz com que o fluxo de execução pule diretamente para o endereço onde o registrador ESP está apontando.
    - Se o atacante puder sobrescrever o EIP com o endereço de uma instrução **JMP ESP** existente em um módulo carregado na memória, o fluxo de execução será redirecionado para o topo da pilha, onde o shellcode pode estar localizado.

### Exemplos Práticos:

- Imagine um cenário onde o atacante descobriu uma vulnerabilidade de buffer overflow. Ao explorar essa vulnerabilidade, o atacante controla o EIP. Para executar o shellcode, o atacante pode sobrescrever o EIP com o endereço de uma instrução **JMP ESP** em um módulo carregado (como uma DLL). Quando o fluxo de execução atinge esse ponto, o **JMP ESP** redireciona para o shellcode, permitindo que o código malicioso seja executado.

### Utilidade e Prevenção:

- **JMP ESP** é uma técnica eficaz, especialmente em sistemas mais antigos ou em cenários onde proteções como ASLR (Address Space Layout Randomization) não estão ativas.
- Prevenções incluem o uso de técnicas de segurança como ASLR, DEP (Data Execution Prevention), e Stack Canaries, que tornam a exploração por **JMP ESP** mais difícil.

Essa técnica é bem conhecida entre os desenvolvedores de exploits e é frequentemente utilizada em conjunção com outras técnicas de exploração.
