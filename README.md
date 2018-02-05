# LaTeX
\documentclass[10pt,a4paper]{article}
\usepackage[utf8]{inputenc}
\usepackage[polish]{babel}
\usepackage[OT4]{polski}
\usepackage{listings}
\usepackage{color}
\usepackage{tabularx}
\usepackage{graphicx}
\usepackage[left=2cm,right=2cm,top=2cm,bottom=2cm]{geometry}
\author{Jakub Nowakowski}
\title{GNUPLOT- tutorial}
\begin{document}

\maketitle

\section{Wprowadzenie}

	Zacznijmy od tego czym jest tytułowy Gnuplot. Gnuplot to zaawansowany (w dodatku darmowy) program do tworzenia wykresów dwu i trójwymiarowych. Jest dostępy zarówno na platformę Linux jak i Windows. Wykresy tworzone przy pomocy tego
programu spełniają standardy przyjęte przy publikowaniu wyników w czasopismach naukowych.
Więcej informacji na temat oprogramowania gnuplot (oraz pliki do pobrania) można znaleźć na stronie
domowej projektu: 
\newline
www.gnuplot.info.
\newline
Poniżej widnieje efekt końcowy wraz z listą poleceń przykładowego ćwiczenia w Gnuplocie:

\begin{lstlisting}

rgb(r,g,b) = int(r)*65536 + int(g)*256 + int(b)
set border 0
unset xtics; unset ytics; unset ztics
set rmargin 5; set lmargin 5; set bmargin 2
set angle degrees
xrgb(r,g,b) = (g-b)/255. * cos(30.)  
yrgb(r,g,b) = r/255. - (g+b)/255. * sin(30.)
set arrow 1 from 0,0 to 0,1 nohead lw 3 lc rgb "red" back
set arrow 2 from 0,0 to cos(-30), sin(-30) nohead lw 3 lc rgb "green" back
set arrow 3 from 0,0 to cos(210), sin(210) nohead lw 3 lc rgb "blue" back
set title "RGB color information read from data file"
plot 'rgb_variable.dat' using (xrgb($1,$2,$3)):(yrgb($1,$2,$3)):(rgb($1,$2,$3)) \
     with points pt 7 ps 4 lc rgb variable notitle
\end{lstlisting}
     
\begin{center}
\includegraphics[scale=0.5]{gnuplot1}
\end{center}

\newpage
\section{Pierwsze kroki}

Istnieje wiele nakładek graficznych pozwalających korzystać z Gnuplota w sposób uproszczony, jednak
zasadniczo program ten obsługiwany jest przy pomocy komend wydawanych w wierszu poleceń. Po uruchomieniu programu w konsoli poleceniem "gnuplot" możemy wyświetlić listę dostępnych opcji wpisując komendę:
\newline
"help"
\newline
Następnie należy postępować zgodnie ze wskazówkami ukazującymi się na ekranie i wybrać interesujący
nas temat. Można również uzyskać pomoc dla konkretnej funkcji wpisując np.:
\newline
"help plot", "help set" itd.
\newline
\newline
\subsection{Podstawowe polecenia}
\subsubsection{plot}
Podstawowym poleceniem gnuplota jest komenda plot, która pozwala na wygenerowanie wykresu funkcji
matematycznej bądź wizualizację danych zawartych w pliku tekstowym.
W celu przetestowania tej funkcji możemy wykonać następujące polecenie:
\newline
plot cos(x)
\newline
\subsubsection{set}
Polecenie set ma wiele zastosowań. Za jego pomocą możemy zmieniać liczbę próbek, tytuł, nazwy wartości, zakresy i wiele wiele innych opcji.
\newline
Przykładowe komendy z użyciem polecenia set:
\newline
\begin{lstlisting}
plot sin(x) # wyswietlamy wykres sin(x)
set title "Sinusoida" # ustawienie tytulu wykresu
set xlabel "X" # ustawienie opisu osi x
set ylabel "Y" # ustawienie opisu osi y
set xrange [-10:10] # zakres osi x
set yrange [-1:1] # zakres osi y
set autoscale # automatyczne skalowanie osi
set xtics ("0" 0,"180" pi,"360" 2*pi) # polozenie znacznikow osi x
replot # ponowne narysowanie wykresu - odswiezenie
\end{lstlisting}

\subsection{Definiowanie}
W Gnuplocie bardzo ważne jest prawidłowe zadeklarowanie wykresu.
Przykładowo funkcję :
\newline
$ y(x)=x^2-5x+4 $
\newline
deklarujemy jako:
\newline
$ f(x)=x*x-5*x+4 $
\newline
Należy zwrócić uwagę, że kompilator zadziała prawidłowo tylko i wyłącznie,
\newline
gdy użyjemy poprawnych znaków, tj. zamiast $5x$ wstawimy $5*x$ lub zamiast $x^2$ wpiszemy $x*x$
\newline
Niewiadome deklarujemy w bardzo prosty sposób, mianowicie:
\newline
$a=5$
\newline
$b=sqrt(a)$
\newline
i tak dalej...

\subsubsection{Rozmiar czcionek}
\begin{lstlisting}
Mozemy ustawic czcionki dla osi x i y oraz legendy
set key font ",15" # rozmiar czcionki legendy
set tics font "Verdana,15" # rozmiar czcionki osi x i y
\end{lstlisting}
\newpage
\section{Przykladowe prace}
\subsection{}
Poniżej zamieściłem przykładowy skrypt wraz z efektem końcowym, wykonany przez prof. Marka Kopciuszyńskiego. Treść polecenia jest następująca: 
\newline
Korzystając z przygotowanego wcześniej pliku dane.dat przygotujemy
wykres słupkowy (histogram). W tym celu tworzymy skrypt histogram.pg

\begin{lstlisting}
reset
set terminal pdf size 5,3.5
set output "histogram.pdf"
set style data histograms
set style fill solid
set title "Srednie dobowe temperatury"
set key left top #ustawiamy polozenie legendy"
set yrange [0:10]
# podpisy osi x
set xtics ("Styczen" 0, "Luty" 1, "Marzec" 2, "Kwiecien" 3, "Maj" 4)
plot 'dane.dat' using 1 title "noc", 'dane.dat' using 2 title "dzien"
\end{lstlisting}
\begin{center}
\includegraphics[scale=0.7]{zad}
\end{center}
\newpage
\subsection{}
Gnuplot to nie tylko wykresy matematyczne i czysta nauka. Jest to także forma zabawy. Odpowiednio użyty program może wyświetlić praktycznie wszystko co chcemy, czego przykładem są poniższe prace, które ilustrują wykres z wyrazów(słownie zapisanych liczb po angielsku) oraz mapę Francji stworzoną z nazw miast:
\begin{lstlisting}
#
# Additional data columns can be used to hold text rotation or color
# for plot style "with labels"
#
$Data <<EOD
1 one -30
2 two -60
3 three -90
4 four -120
5 five -150
6 six -180
7 seven -210
8 eight -240
9 nine -270
10 ten -300
11 eleven -330
12 twelve -360
EOD

set angle degrees
unset key
set title "variable color and orientation in plotstyle 'with labels'" offset 0,-2

set xrange [0:13]
set yrange [0:13]
set xtics 1,1,12 nomirror
set ytics 1,1,12 nomirror
set border 3

plot $Data using 1:1:2:3:0 with labels rotate variable tc variable font ",20"
\end{lstlisting}
\begin{center}
\includegraphics[scale=0.7]{zad2}
\end{center}

\subsection{}
\begin{lstlisting}
#
# Demonstrates how to derive variable font size from a data file column.
# 
# If you are viewing this via the HTML canvas terminal, be sure to toggle
# the font scaling icon so that the fonts change size as you zoom in.
#
Scale(size) = 0.25*sqrt(sqrt(column(size)))
CityName(String,Size) = sprintf("{/=%d %s}", Scale(Size), stringcolumn(String))

set termoption enhanced
save_encoding = GPVAL_ENCODING
set encoding utf8
unset xtics
unset ytics
unset key
set border 0
set size square
set datafile separator "\t"
plot 'cities.dat' using 5:4:($3 < 5000 ? "-" : CityName(1,3)) with labels

\end{lstlisting}
\begin{center}
\includegraphics[scale=0.7]{zad3}
\end{center}

\section{Informacje}
Tworząc ten krótki, okrojony tutorial do Gnuplota posiłkowałem się następującymi pomocami:
\newline
Wykresami wraz z listą poleceń ze strony:
\begin{lstlisting}
http://gnuplot.sourceforge.net/demo_5.0/
\end{lstlisting}
Wykresem wraz z poleceniem opracowanym w programie nauczania Pana Profesora Marka Kopciuszyńskiego.
\newline
Informacjami zawartymi na oficjalnej stronie Wikipedii

\end{document}
