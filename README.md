# IMDB-Top-1000

Top 1000 películas IMDB

Este repositorio contiene una lista de las 1000 películas mejor calificadas en IMDB, junto con información relevante como el título, año de lanzamiento, género, director y calificación. Descargué una base de datos de Kaggle, creada por Harshit Shankhdhar.

Cargamos la base y mostramos las primeras filas:

df_imdb \<- read.csv("imdb_1000.csv")

head(df_imdb)

Seleccionamos las columnas relevantes: series_title, released_year, genre, director, imdb_rating, Star1, Star2, Star3, Star4

selected_columns \<- c("series_title", "released_year", "genre", "director", "imdb_rating", "Star1", "Star2", "Star3", "Star4")

Y creamos un nuevo objeto con ellas:

df_select \<- df_imdb \|\> select("Series_Title", "Released_Year", "Genre", "Director", "IMDB_Rating", "Star1", "Star2", "Star3", "Star4")

Seleccionamos las películas con calificación mayor a 8.5 usando:

df_high_rating \<- df_select \|\> filter(IMDB_Rating \> 8.5)

Mostramos las primeras filas del nuevo objeto usando:

head(df_high_rating)

Ahora las ordenamos por año de lanzamiento usando:

df_sorted \<- df_high_rating \|\> arrange(Released_Year) head(df_sorted)

Creamos un gráfico de barras con ggplot2 para visualizar la cantidad de películas por año de lanzamiento

ggplot(df_sorted, aes(x = Released_Year)) + geom_bar(fill = "blue") + labs(title = "Cantidad de películas por año de lanzamiento", x = "Año de lanzamiento", y = "Cantidad de películas") + theme_minimal()

Ahora revisaremos la cantidad de películas por género usando:

df_genre_count \<- df_sorted \|\> group_by(Genre) \|\> summarise(count = n()) \|\> arrange(desc(count))

Lo visualizaremos en un gráfico de barras:

ggplot(df_genre_count, aes(x = reorder(Genre, -count), y = count)) + geom_bar(stat = "identity", fill = "green") + labs(title = "Cantidad de películas por género", x = "Género", y = "Cantidad de películas") + theme_minimal() + theme(axis.text.x = element_text(angle = 45, hjust = 1))

Queremos saber cuáles son los directores con más películas en esta lista. Usamos:
df_director_count <- df_sorted |> 
group_by(Director) |> 
summarise(count = n()) |> 
arrange(desc(count)) |> 
head(10)

Y lo visualizamos con un gráfico de barras:
ggplot(df_director_count, aes(x = reorder(Director, -count), y = count)) + geom_bar(stat = "identity", fill = "red") + labs(title = "Top 10 directores con más películas", x = "Director", y = "Cantidad de películas") + theme_minimal() + theme(axis.text.x = element_text(angle = 45, hjust = 1))

Ahora queremos ver los actores más frecuentes en estas películas. Usamos:

df_actors <- df_sorted |> 
select(Star1, Star2, Star3, Star4) |> 
pivot_longer(cols = everything(), names_to = "Star", values_to = "Actor") |> 
group_by(Actor) |> summarise(count = n()) |> 
arrange(desc(count)) |> head(10)

Y lo visualizamos con un gráfico de barras:

ggplot(df_actors, aes(x = reorder(Actor, -count), y = count)) + geom_bar(stat = "identity", fill = "purple") + labs(title = "Top 10 actores con más películas", x = "Actor", y = "Cantidad de películas") + theme_minimal() + theme(axis.text.x = element_text(angle = 45, hjust = 1))


