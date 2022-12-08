# Sorting-Station-Automacao



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

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Ordenacao1.png />
  
  **Figura 4**: Ordenação parte 1.
</div>

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Ordenacao2.png />
  
  **Figura 5**: Ordenação parte 2.
</div>

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Ordenacao3.png />
  
  **Figura 6**: Ordenação parte 3.
</div>

### Stop blade

<div align="center">
  <img src=https://github.com/WalmerAlmeida/Sorting-Station-Automacao/blob/main/imgs/Stop_blade.png />
  
  **Figura 7**: Stop blade.
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
