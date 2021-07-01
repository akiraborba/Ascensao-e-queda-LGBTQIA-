#### A temática

A comunidade LGBTQIA+ é um grupo dinâmico, em contante movimento e aprofundamento de suas problemáticas e bandeiras. Esse dinamismo se reflete em como o grupo se auto-denomina. Os nomes sempre são campos de disputa. Para produzir insights sobre essa fluidez dos nomes, um gráfico de popularidade de termos de pesquisa no Google Brasil foi criado.

#### Construindo a Base de Dados

Estes dados foram obtidos através do repositório aberto Google Trends. Apenas os dados do Brasil foram coletados, entre 2004 e 2021. O Google Trends gera um arquivo em separado para cada termo de interesse, e estes arquivos precisavam ser combinados.

```{r}
all.data <- list()
for (i in 1:13) {
    all.data[i] <- as.data.frame(read.csv(
	file = paste("multiTimeline(", i, ").csv",
	sep = ""),
	encoding = "Latin-1", check.names = T))
}
```

Claro que, após combinados em uma grande planilha, estes dados precisavam ser organizados de uma maneira amigável à plataformas gráficas. Neste caso, as visualizações foram construídas em Plotly, mas o processo seria o mesmo para Tableau ou Power Bi.

```{r}
dates <- as.Date("2004-01-01") + months(0:209)
queer.rows <- vector()

for(i in 1:13) {
  new.rows <- as.vector(all.data[[i]][2:211])
  queer.rows <- c(queer.rows, new.rows)
  
}
queer.rows <- as.numeric(queer.rows)
queer.rows[is.na(queer.rows)] <- 0
queer.names <- c(
"LGBTQ", "LBGTQIA", 
"GLS", "Gay", "Bissexual", 
"Lésbica", "Transexual", "LGBT", 
"Travesti", "Queer", "Assexual", 
"Intersexual", "Não-binário")
queer.data <- data.frame(
termo.de.pesquisa = rep(queer.names, each = 210), 
n.buscas = queer.rows, tempo = rep(dates, times = 13))
```

Finalmente, o gráfico é construído. Todo este processo foi feito em linguagem R. O gráfico originalmente foi feito usando o pacote GGplot, e depois convertido em Plotly.

```{r}
queer.plot <- ggplot(queer.data) + 
           geom_line(aes(x = tempo, y = n.buscas, color = termo.de.pesquisa)) +
           labs(x = "Tempo, em meses",
                y = "Buscas",
                title = "Popularidade dos termos de pesquisa",
                subtitle = "De 0 a 100, sendo 100 o pico da popularidade do termo") +
           theme(legend.title = element_blank(), legend.position = "bottom")
ggplotly(queer.plot)
```
#### Explore o Arco-Íris!

Você pode clicar nos termos de busca para que eles apareçam e
desapareçam. Arraste o mouse com botão direito pressionado para dar zoom
em determinado período de tempo. Os valores estão entre 0 e 100, sendo
100 o momento em que o termo esteve mais popular. Os dados vão de
janeiro de 2004 a junho de 2021. Para melhor visualização, três gráficos foram gerados: siglas, sexualidades e gêneros.

"https://chart-studio.plotly.com/~arthurlunabcf/1.embed"

"https://chart-studio.plotly.com/~arthurlunabcf/3.embed"

"https://chart-studio.plotly.com/~arthurlunabcf/4.embed"

#### As cores da Moda

Entre as siglas, GLS está em decadência desde dezembro de 2004. LGBT e LGBTQIA disputam a popularidade nesse momento. O caminho do meio, LBGTQ, está, bom, no meio. Como diria Heidi Klum, *“In fashion, one day you are in, and in the next day you are out”*.

#### Antes invisíveis, hoje no topo

Grupos marginalizados dentro da comunidade LGBTQIA+ estão conquistando seu espaço. Lésbicas, Bissexuais, Assexuais, Intersexs e Não-Binários, historicamente apagados e críticos de que a sigla LGBTQIA+ é na verdade GGGGG+, nunca foram tão buscados quanto neste momento. A luta continua, mas certamente mais atenção é dada a estas pessoas da comunidade do que se dava em 2004.

#### O poder da representatividade

O termo Transexual tem um comportamento muito diferente dos demais, um pico indiscutível e agudo em janeiro de 2011. Por quê? Por que em janeiro de 2011 Ariadna foi apresentada como concorrente no BBB11, a primeira transexual no programa. Nunca se buscou tanto sobre transexuais como naquela época.Para quem acha que representatividade na grande mídia não importa, bem, o Google discorda.

#### Consequências no mundo real

Esta breve análise torna evidente a necessidade de atualizar não só as nomenclutras utilizadas como referência à população LGBTQIA+, mas também a necessidade de considerar populações dentro dessas comunidades, antes invisibilizadas. A pesquisa torna claro que termos como Lésbica, Não-Binário, Intersex e Travesti vêm ganhando mais espaço online, e qualquer empresa ou iniciativa que desconsidere essas pessoas está não apenas desrespeitando e negando a dignidade de um enorme grupo de pessoas, mas está sendo ultrapassada.

#### Tá passada?

Que outros padrões você vê aí nestes dados? Curtiu o bafo? Tem um projeto que demanada análise de dados dentro dos temas de inclusão e diversidade? Manda um e-mail para <arthurlunabcf@outlook.com> ou conversa comigo no [LinkedIn]("https://www.linkedin.com/in/arthur-luna-borba-10050438/").
