#wssi2
__ZAD1.1__
a)rodzeństwo
b)kuzyni pierwszego stopnia
c)różni dziadkowie z tego samego pokolenia
d)y jest przybranym rodzicem x
e)przybrane rodzeństwo
f)x i y są dla siebie szwagrami
g)kazirodztwo
__ZAD1.2__
przyjazn(X, Y) :- lubi(X, Y), lubi(Y, X).
nieprzyjazn(X, Y) :- \+ lubi(X, Y), \+ lubi(Y, X).
niby_przyjazn(X, Y) :- \+ lubi(X, Y), \+ lubi(Y, X), toleruje(X, Y), toleruje(Y, X).
pewna_przyjazn(X, Y) :- przyjazn(X, Y).
mutual_loves(X, Y) :- loves(X, Y), loves(Y, X).
exclusive_love(X, Y) :- loves(X, Y), loves(Y, X), \+ (loves(X, Z), Z \= Y), \+ (loves(Y, W), W \= X).
plec(mezczyzna).
plec(kobieta).
true_love(X, Y) :- exclusive_love(X, Y).
obojetnosc(X, Y) :- \+ mutual_loves(X, Y).

__ZAD2__
kobieta(X) :-
	osoba(X),
	\+ mezczyzna(X).
ojciec(X, Y) :-
	rodzic(X, Y),
	mezczyzna(X).
matka(X, Y) :-
	rodzic(X, Y),
	kobieta(X).
corka(X, Y) :-
	rodzic(Y, X),
	kobieta(X).
brat_rodzony(X, Y) :-
	rodzic(Z, X),
	rodzic(Z, Y),
	mezczyzna(X),
	X \= Y.
brat_przyrodni(X, Y) :-
	rodzic(Z, X),
	rodzic(Z, Y),
	mezczyzna(X),
	X \= Y,
	(\+ (matka(F, X), matka(F, Y));
	\+ (ojciec(M, X), ojciec(M, Y))).
rodzenstwo(X, Y) :-
	ojciec(V, X),
	ojciec(V, Y),
	matka(Z, X),
	matka(Z, Y).
kuzyn(X, Y) :-
	rodzic(V, X),
	rodzic(Z, Y),
	rodzenstwo(V, Z).
dziadek_od_strony_ojca(X,Y) :-
	ojciec(Z, Y),
	ojciec(X, Z).
dziadek_od_strony_matki(X,Y) :-
	matka(Z, Y),
	ojciec(X, Z).
dziadek(X,Y) :-
	rodzic(Z, Y),
	ojciec(X, Z).
babcia(X, Y) :-
	rodzic(Z, Y),
	matka(X, Z).
wnuczka(X, Y) :-
	rodzic(Z, Y),
	rodzic(X, Z),
	kobieta(Y).
przodek_do2pokolenia_wstecz(X, Y) :-
	rodzic(X, Y);
	dziadek(X, Y);
	babcia(X, Y).
pradziadek(X, Y) :- 
	dziadek(D, Y), 
	rodzic(X, D).
prababcia(X, Y) :- 
	babcia(B, Y), 
	rodzic(X, B).
przodek_do3pokolenia_wstecz(X, Y) :-
	przodek_do2pokolenia_wstecz(X, Y); 
	pradziadek(X, Y); 
	prababcia(X, Y);
	
