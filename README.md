COJOCARIU SEBASTIAN 321CB
								                				Tema 1 PP

Functii auxiliare:

-functii care returneaza prima,a doua,respectiv a treia componenta a unui 3-tuplu

take1st ::(Int,Int,Int)->Int
take2nd ::(Int,Int,Int)->Int
take3rd ::(Int,Int,Int)->Int


-functie care primeste 2 noduri si un graf(sub forma de lista de 3-tupluri cu proprietatile 
din enunt) si verifica daca exista muchie intre cele 2 noduri.Returneaza o pereche de forma 
(Bool,Int) cu corespondenta:
	daca exista muchie ->(True,n) ,unde n=lungimea muchiei dintre cele 2 noduri
	daca nu exista muchie ->(False,-1) 
contains :: Int->Int->[(Int,Int,Int)]->(Bool,Int) 


1)solveSimple:
	Am folosit o dinamica in 3 dimensiuni:
d[i][j][k] = lungimea minima de la nodul i la j in care orice nod intermediar
	apartine multimii {1,2,...,k}.Observam ca orice drum de lungime minima trece
	printr-un nod de cel mult o data,de aceea are loc recurenta:
d[i][j][0] = weight[i][j] ,unde daca nu exista muchie punem -1.
d[i][j][k] = min(d[i][j][k-1], d[k][j][k-1] + d[i][k][k-1]).
	unde trebuie avut grija ca termenii sa nu fie -1(daca unul din termenii sumei e 
	-1 se considera ca toata suma e -1 si vom returna d[i][j][k-1],daca niciun termen
	al sumei nu e -1 ,dar d[i][j][k-1] e -1 vom returna suma celor 2,iar daca niciun
	element din interiorul functiei min nu e -1,atunci returnam efectiv minimul) 
Odata calculat matricea d,refacerea drumul se face astfel:de pe pozitia pozitie_curenta
si k,gasim acel index pentru care exista muchie cu pozitia_curenta si pentru care suma
dintre lungimea acelei muchii si d[1][index][q] (cu q din {1,2...,k}) este egala cu
d[1][pozitie_curenta][k].Problema iese recursiv.


2)SolveCosts:
	Am folosit o dinamica in 3 dimensiuni:
dp[k][i][j] = lungimea minima a drumului minim dintre i si j in care avem la dispozitie cel mult 
			  suma k
dp[k][i][j] = min(adiacenta[i][index] + dp[k-cost[index]][index][j])),atunci cand index parcurge
			 multimea {1,2,...,n} astfel incat sa existe muchia (i,index),costul de intrare in nodul
			 index sa fie mai mic sau egal decat suma k ,iar dp!(k-cost!index,index,j) sa fie diferit 
			 de -1(adica sa se poata ajunge de la index la j)
Caz de baza: i==j =>dp[k][i][j] = 0
			 k==0 =>dp[k][i][j] = -1			  

cost_minim[k][i][j] = costul necesar pentru a obtine drumul de lungime dp[k][i][j] dintre i si j
					= acel indice minimal index pentru care dp[k][i][j] == dp[index][i][j]
Odata construita matricea,aflarea drumului se realizeaza recursiv,prin functia afisare.
(A se urmari explicatiile din cod)					 
