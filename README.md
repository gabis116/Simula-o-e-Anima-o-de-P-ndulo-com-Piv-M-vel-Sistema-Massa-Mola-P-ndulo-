# Simulação e Animação de Pêndulo com Pivô Móvel (Sistema Massa-Mola-Pêndulo)

Este projeto implementa a integração numérica e a animação em tempo real de um sistema dinâmico não-linear composto por um **pêndulo acoplado a um suporte móvel preso a uma mola** (também conhecido como sistema carrinho-pêndulo ou pêndulo elástico horizontal).

O código resolve o sistema de Equações Diferenciais Ordinárias (EDOs) acopladas utilizando álgebra linear numérica e gera uma animação interativa da trajetória da massa.

##  Sobre o Modelo Físico e Matemático

O sistema possui dois graus de liberdade:

1. $x(t)$: O deslocamento horizontal do ponto de suspensão (pivô) preso à mola.

2. $\theta(t)$: O ângulo do pêndulo em relação à vertical.

### Formulação Matricial das EDOs

Para resolver o sistema de segunda ordem acoplado, as acelerações ($\ddot{x}$ e $\ddot{\theta}$) são isoladas em cada passo de tempo através da resolução do sistema linear $M \cdot \vec{a} = \vec{b}$:

$$
\begin{bmatrix} L & -L\cos\theta \\ -\cos\theta & L \end{bmatrix} \begin{bmatrix} \ddot{x} \\ \ddot{\theta} \end{bmatrix} = \begin{bmatrix} L\dot{\theta}\sin\theta - 2 - \omega_0^2 x \\ -g\sin\theta \end{bmatrix}
$$

Onde $\omega_0^2 = \frac{k}{m}$ representa o quadrado da frequência natural do sistema massa-mola.

### Cinemática da Massa

A posição cartesiana $(x_m, y_m)$ da massa na extremidade do pêndulo é calculada a partir da posição do pivô $x(t)$:

$$
x_m(t) = x(t) + L\sin(\theta(t))
$$

$$
y_m(t) = -L\cos(\theta(t))
$$

## Tecnologias e Bibliotecas

* [**NumPy**](https://numpy.org/): Operações vetoriais, funções trigonométricas e criação da malha temporal.

* [**SciPy (`linalg` e `integrate`)**](https://scipy.org/):

  * `scipy.linalg.solve`: Resolução rápida do sistema matricial de acelerações a cada iteração.

  * `scipy.integrate.odeint`: Integração numérica do sistema de EDOs de 1ª ordem.

* [**Matplotlib (`pyplot` e `animation`)**](https://matplotlib.org/): Renderização gráfica e criação da animação quadro a quadro com `FuncAnimation` e otimização `blit=True`.

##  Parâmetros e Condições Iniciais

Os parâmetros físicos definidos no script são:

| Parâmetro | Símbolo | Valor Padrão | Descrição | 
| ----- | ----- | ----- | ----- | 
| Gravidade | `g` | $9.81\text{ m/s}^2$ | Aceleração gravitacional | 
| Massa | `m` | $3\text{ kg}$ | Massa do sistema | 
| Constante Elástica | `k` | $10\text{ N/m}$ | Rigidez da mola horizontal | 
| Comprimento do Pêndulo | `L` | $3\text{ m}$ | Comprimento da haste do pêndulo | 
| Comprimento Natural | `L0` | $2\text{ m}$ | Parâmetro de referência geométrica | 

### Configuração Temporal e Estado Inicial

* **Intervalo de Tempo:** $0$ a $30\text{ s}$ com passo $h = 0.01\text{ s}$ ($3001$ quadros).

* **Vetor de Estado Inicial (**$Y_0$**):** Definido no código como `[x0, vx0, th0, vth0] = [0.5, np.radians(40), -1, 0]`. *(Nota: na função `dois_pendulos_mola`, a ordem das variáveis do vetor de estado é `[x, th, vx, vth]`).*

##  Como Executar

### 1. Instalação das Dependências

Certifique-se de ter o Python 3 instalado e execute:

```
pip install numpy scipy matplotlib

```

### 2. Execução do Script

Salve o código como `animacao_pendulo.py` e execute:

```
python animacao_pendulo.py

```

## Resultado Esperado

Ao executar o programa, uma janela gráfica em proporção real (`aspect='equal'`) será aberta exibindo:

* **Linha Vermelha com Marcadores (`o-r`):** A haste do pêndulo conectando o pivô móvel $(x, 0)$ à massa $(x_m, y_m)$.

* **Linha Azul Contínua (`-b`):** O rastro da trajetória percorrida pela massa no plano $xy$ ao longo dos $30$ segundos de simulação.
