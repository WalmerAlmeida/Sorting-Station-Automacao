# Sorting-Station-Automacao

## Descrição textual e esquemática do problema a ser automatizado;
### **Problema**

  Automatizar um sistema para classificar e ordenar pacotes por cores no cenário “sorting station” do Factory I/O.

### **Equipamentos**

  Para o correto funcionamento do processo de ordenação, são necessários os seguintes equipamentos:

1. Um emitter, responsável por produzir, de forma aleatória, os pacotes que serão classificados e ordenados;
2. Uma esteira A, responsável por levar os pacotes do emitter até o vision sensor;
3. Uma esteira B, responsável por transportar os pacotes até os ordenadores (esteiras auxiliares);
4. Um vision sensor, responsável por realizar o reconhecimento visual da cor de cada pacote e;
5. 3 esteiras auxiliares (ordenadores), uma para cada uma das 3 cores de pacotes. Cada uma das 3 esteiras deverá “empurrar” o pacote para o seu determinado grupo (metal, verde ou azul);

### **Funcionamento**

1. O processo inicia-se ao pressionar o botão “start”
    Com o acionamento do botão, o emitter irá iniciar a produção de 3 tipos de pacotes distintos de forma aleatória;
2.  Enquanto o emitter produz novos pacotes, a esteira A deve conduzi-los até o vision sensor, cuja função é identificar a cor do pacote;
3.  Após identificar uma determinada cor, o vision sensor “emite” o comando para que o ordenador (esteira auxiliar) responsável por essa cor seja ativado. A esteira B, então, conduz o pacote até o ordenador em funcionamento para que o pacote seja endereçado ao seu grupo correspondente;
4.  No sistema, há, ainda, a presença de uma stop blade, que servirá como uma cancela de segurança. A stop blade deverá ser tratada como um circuito normalmente fechado, ou seja, sua posição inicial e normal é levantada (ativa);
5.  A stop blade deverá ser desativada (baixada) se e somente se as duas condições abaixo forem satisfeitas:
    1.  a entrada do vision sensor é diferente de zero, ou seja, há algum pacote no range de detecção do sensor e;
    2. não há ordenador ativo;
6. A stop blade sempre deverá voltar à posição inicial após um atraso de 0.65seg para que não haja conflito entre a interação stop blade/pacote;
	 



## Descrição Lógica

O código ladder implementado foi dividido em 4 partes, a parte do painel onde são exibidas as informações do funcionamento para o usuário, a parte das Esteiras que engloba a lógica de ativação utilizada, a ordenação que é o trecho responsável por levar os pacotes para o local correto, e por último, o trecho que descreve o funcionamento da "Stop blade" que é utilizado na ordenação.

### Painel

Inicialmente o botão "Start button" é precionado e como mostrado no trecho da "Figura 1", a luz do "Start light" é ligada e mantido ativo até alguém apertar o botão "Stop button" que por sua vez liga a luz do "Stop light" de forma análoga ao "Start light".

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Painel.png />
  
  **Figura 1**: Painel.
</div>

### Esteiras

Além de ativar as luzes corretas no Painel, o "Start button" é responsável por ativar todas as esteiras, incluíndo as presentes nos ordenadores, até alguém apertar o botão "Stop button" que desativa todas as esteiras, como mostrado no trecho da "Figura 2".

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Esteiras1.png />
  
  **Figura 2**: Esteiras parte 1.
</div>

Na "Figura 3", observamos que a esteira "Entry conveyuor" funciona de forma um pouco diferente, tendo em vista que se faz necessário a utilização de uma variável auxiliar "Entry conveyour memory 2" que serve para desligar a esteira de entrada(Entry conveyuor) quando houver algum pacote sendo ordenado na esteira de saída(Exit conveyour) e houver a detecção(pelo Vision sensor) de outro pacote na espera para ser ordenado.

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Esteiras2.png />
  
  **Figura 3**: Esteiras parte 2.
</div>

### Ordenação

Para ativar o ordenador, o “Vision sensor” verifica se existe algum pacote com uma determinada cor, essa verificação ocorre analisando se o valor obtido pelo “Vision sensor” corresponde à algum pacote com a cor correspondente, ou seja, se

| cor   | números dos pacotes | ordenador ativado |
| ----- | ------------------- | ----------------- |
| azul  | 1, 2 ou 3           | 1º                |
| verde | 4, 5 ou 6           | 2º                |
| cinza | 7, 8 ou 9           | 3º                |

Além disso, durante a verificação do pacote, é realizado também a análise da “Stop blade”, onde caso exista um pacote com uma das cores(azul, verde ou cinza) e verificado que a "Stop blade" está aberta, a variável “Timer memory” é ativada e mantida assim, até o sensor “At exit” detectar que algum pacote já foi ordenado. Durante a ativação dos ordenadores são aplicados os timers TON e TOF, que aplicam os delays de 0.65s no início e 1s no final do “Sorter turn”, respectivamente. Os ordenadores das demais cores funcionam de forma análoga ao código exibido na "Figura 4".

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Ordenacao1.png />
  
  **Figura 4**: Ordenação parte 1.
</div>

### Stop blade

Quanto ao funcionamento da "Stop blade", temos que ela é ativada sempre que não existe nenhum pacote no "Vision sensor" ou quando algum pacote está sendo ordenado na esteira de saída(Exit conveyour).

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Stop_blade.png />
  
  **Figura 5**: Stop blade.
</div>

## Tabela de Endereçamento

| Artefato                 | Tipo | Endereço |
| ------------------------ | ---- | -------- |
| At exit                  | Bool | %I0.0    |
| Start button             | Bool | %I0.1    |
| Stop button              | Bool | %I0.3    |
| Vision sensor            | DINT | %ID30    |
| Entry conveyour          | Bool | %Q0.0    |
| Stop blade               | Bool | %Q0.1    |
| Exit conveyour           | Bool | %Q0.2    |
| Sorter 1 turn            | Bool | %Q0.3    |
| Sorter 1 belt            | Bool | %Q0.4    |
| Sorter 2 turn            | Bool | %Q0.5    |
| Sorter 2 belt            | Bool | %Q0.6    |
| Sorter 3 turn            | Bool | %Q0.7    |
| Sorter 3 belt            | Bool | %Q1.0    |
| Start light              | Bool | %Q1.1    |
| Stop light               | Bool | %Q1.3    |
| Timer 1 memory           | Bool | %Q1.4    |
| Timer 2 memory           | Bool | %Q1.5    |
| Timer 3 memory           | Bool | %Q1.6    |
| Entry conveyour memory 2 | Bool | %Q1.7    |
| Entry conveyour memory 1 | Bool | %Q2.0    |

## Tabela Verdade

| %I0.0 | %I0.1 | %I0.3 | %ID30 == 1 ou %ID30 == 2 ou %ID30 == 3 | %ID30 == 4 ou %ID30 == 5 ou %ID30 == 6 | %ID30 == 7 ou %ID30 == 8 ou %ID30 == 9 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| ----- | ----- | ----- | -------------------------------------- | -------------------------------------- | -------------------------------------- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- | ----- |
| 1 | 1 | 1 | 1 | - | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 1 | 1 | - | 1 | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 1 | 1 | - | - | 1 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 1 | 1 | 0 | 0 | 0 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 1 | 0 | 1 | - | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 1 | 0 | - | 1 | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 1 | 0 | - | - | 1 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 1 | 0 | 0 | 0 | 0 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 0 | 1 | 1 | - | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 0 | 1 | - | 1 | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 0 | 1 | - | - | 1 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 0 | 1 | 0 | 0 | 0 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 0 | 0 | 1 | - | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 0 | 0 | - | 1 | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 0 | 0 | - | - | 1 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 1 | 0 | 0 | 0 | 0 | 0 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 1 | 1 | 1 | - | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 1 | 1 | - | 1 | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 1 | 1 | - | - | 1 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 1 | 1 | 0 | 0 | 0 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 1 | 0 | 1 | - | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 1 | 0 | - | 1 | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 1 | 0 | - | - | 1 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 1 | 0 | 0 | 0 | 0 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 0 | 1 | 1 | - | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 0 | 1 | - | 1 | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 0 | 1 | - | - | 1 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 0 | 1 | 0 | 0 | 0 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 0 | 0 | 1 | - | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 0 | 0 | - | 1 | - | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 0 | 0 | - | - | 1 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |
| 0 | 0 | 0 | 0 | 0 | 0 | %Q0.0 | %Q0.1 | %Q0.2 | %Q0.3 | %Q0.4 | %Q0.5 | %Q0.6 | %Q0.7 | %Q1.0 | %Q1.1 | %Q1.3 | %Q1.4 | %Q1.5 | %Q1.6 | %Q1.7 | %Q2.0 |

## Vídeo da Simulação e explicação(entrar com conta da ic.ufal.br)

[![Sorting Station](https://imgur.com/a/CWDzz3s)](https://www.youtube.com/embed/nb5dSuqP0Ik "Sorting Station")
