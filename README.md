# CPS703 - Arquitetura de Computadores 2
Arquivos de Resultado experimental do trabalho prático da disciplina de mestrado do PESC/COPPE - UFRJ.

## Objetivo

Analisar as mudanças de performance causadas por alterações nas definições de hardware da arquitetura RISC-V.

## Ferramentas
- [MARS RISC-V](https://github.com/bucaps/marss-riscv)
- Debian Based Linux (ex.: Ubuntu)
- [Coremark](https://www.eembc.org/coremark/)
- Python
- Jupyter

## Simulação
Começei lendo [esse](https://marss-riscv-docs.readthedocs.io/en/latest/sections/running-full-system.html#) tutorial que tem o passo a passo de instalação do simulador, configuração das imagens RISC-V, como rodar o simulador com o arquivo de configuração da imagem e também como instalar, configurar e rodar o becnhmark supracitado.

### Tentativas de Instalação das ferramentas 
Tentei instalar numa máquina RedHat num primeiro momento, o que foi infrutífero, pois as bibliotecas padrão necessárias não existiam com o mesmo nome na distribuição como, talvez, existisse incompatibilidade.

Então na segunda vez que instalei o simulador o fiz numa máquina Debian (Ubuntu), o que funcionou perfeitamente.

Posteriormente também testei a instalação num Ubuntu instalado no WSL do Windows 10, que também funcionou perfeitamente.

Importante notar que, a máquina final a qual rodei os experimentos, instalei a versão 32 e 64 da máquina RISC-V, uma vez que dado os problemas de execução mencionados na seção seguinte, acabei testando as duas estruturas.

### Subindo a Simulação a partir das imagens
Sobre esse tópico, ao longo do tutorial não é mencionado sobre a necessidade de copiar a pasta com o arquivo de imagem para a pasta *./configs/*, pois quando executei o comando abaixo de dentro da pasta *src* do MARS
```bash
./marss-riscv -rw -ctrlc -sim-mem-model base ../configs/<ARQUIVO_CONFIG>
```
Este retornou um erro indicando não encontrar a imagem necessária. Portanto é necessário copiar os arquivos derivados do arquivo baixado *marss-riscv-images.tar.gz* 
Após abrir o prompt, a primeira coisa a ser feita é definir a data do sistema (VM Guest Machine). E sempre que era necessário subir novamente a simulação, é necessário esse passo.
```bash
date --set="16 Jun 2023 23:05:00"
```

### Rodando o Coremark na Simulação
Após todas as configurações dos repositórios do *Coremark* e do *marss-riscv-utils*, o comando para executar a simulação estava em Stall permanente. Testei isso ao longo de dois dias esperando mais de **DUAS** horas antes de fechar a tentativa. Uma variação do comando defeituoso e sua correção estão abaixo.

```bash
# comando que entra em stall permanente
simstart; ./coremark.exe  0x0 0x0 0x66 0 7 1 2000 > ./run1.log; simstop;

# comando corrigido com o 4º argumento (Nº de iterações) diferente de zero:
simstart; ./coremark.exe  0x0 0x0 0x66 500 7 1 2000 > ./run1.log; simstop;
```
Rodar o benchmark dentro desse *environment* gera saídas bem estruturadas diretamente na pasta *src* do MARS.
![](./results/saidas_csv_benchmak_simulation.png)



## Avaliação de Performance
A partir dos arquivos de log e CSV que o benchmark rodando dentro da simulação gerava, foi possível comparar os desempenhos entre as levemente diferentes arquiteturas utilizando o Coremark. 

Os Gráficos gerados estão abaixo.

### Gráficos
![alt text](./results/Conditional%20Branches%20Taken.png "grafico1")
![alt text](./results/Conditional%20Branches%20Predicted%20Correctly.png "grafico2")
![alt text](./results/Conditional%20Branches%20Predicted%20Wrong.png "grafico3")
![alt text](./results/Unconditional%20Branches%20Predicted%20Correctly.png "grafico4")
![alt text](./results/Unconditional%20Branches%20Predicted%20Wrong.png "grafico5")
![alt text](./results/L1%20icache%20Reads.png "grafico6")
![alt text](./results/L1%20icache%20Read%20Misses.png "grafico7")
![alt text](./results/L2%20cache%20Reads.png "grafico8")
![alt text](./results/L2%20cache%20Read%20Misses.png "grafico9")
![alt text](./results/L1%20cache%20Writes.png "grafico10")
![alt text](./results/L1%20cache%20Write%20Misses.png "grafico10")
