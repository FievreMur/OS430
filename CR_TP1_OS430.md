# TP01 OS430

# Authors

Hugo Hoarau, Agathe Michel

# Exercice 0

Le programme

# Exercice 1

# 1.a

**gcc -o int_overflow1 int_overflow.c**

On obtient :

```
no detected error
overflow error
```

En effet sans options de compilation on peut voir que la condition est parcourue et le code détecte un dépassement car on fait l'opération avec le int max (signé). L'addition donne donc un résultat négatif

**gcc -O2 -o int_overflow2 int_overflow.c**

On obtient :

```
no detected error
no detected error
```

Avec l'option de compilation -O2 (contenant l'option -fstrict-overflow), le compilateur regarde la première condition et réalise que si a < 0 ou b <= 0 on ne passe pas à la suite du code. a et b sont donc forcément positifs, donc leur somme aussi. Il saute donc le if suivant qui teste a+b > 0.

**gcc -fno-strict-overflow -o int_overflow3 int_overflow.c**

On obtient :

```
no detected error
overflow error
```

Avec l'option de compilation -fnostrict-overflow (à l'inverse de -fstrict-overflow) passer dans la condition vérifiant a+b > 0.

**gcc -O2 -fno-strict-overflow -o int_overflow4 int_overflow.c**

On obtient :

```
no detected error
overflow error
```

Qui est la même sortie qu'avec l'option précédente. On a donc -fno-strict-overflow qui désactiver les optimisations de l'option -O2.

**Solution de sécurisation :**

```
unsigned int c = a+b ;

if (c > INT_MAX) {
    /* Handle error */
}
```

On obtient bien:

```
no detected error
overflow error
```

# 1.b

L'option de compilation -fstrict-overflow va enlever le 2e if ce qui va générer un overflow.

**Solution de sécurisation :**

```
unsigned int somme = len + offset;

if (somme ) {
    /* Handle error */
}
```

# 1.c

Si length est très grand, il peut devenir négatif si il y a dépassement. Il sera donc < 1024 mais sera considéré comme très grand par la fonction read car length sera casté en un size_t (entier non signé).

**Solution de sécurisation :**

```
size_t length;
```

Ici on change le type de length pour qu'il concorde avec le type d'entrée de la fonction read. Il ne pourra pas être négatif car sera considéré comme non signé.

# Contents

TODO for STUDENTS : Say a bit about the code infrastructure ...

# Howto

`make test-interpret TEST_FILES='TP03/tests/provided/examples/test_print_int.c'` for a single run

`make test` to test all the files in `*/tests/*` according to `EXPECTED` results.

You can select the files you want to test by using `make test TEST_FILES='TP03/**/*bad*.c'` (`**` means
"any number of possibly nested directories").

# Test design

TODO: explain your tests. Do not repeat what test files already contain, just give the main objectives of the tests.

# Design choices

TODO: explain your choices - explain the limitations of your implementation.

# Known bugs

TODO: document any known bug and limitations. Did you do everything asked for? Did you implement an extension?
