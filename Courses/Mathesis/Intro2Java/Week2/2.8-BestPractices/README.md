# 2.7 Καλές τεχνικές προγραμματισμού {#Java} 
© Γιάννης Κωστάρας

---

[<-](../2.7-JavaDoc/README.md) | [Δ](../../README.md)

---

Παρακάτω παρουσιάζουμε μερικές χρήσιμες συμβουλές που θα σας βοηθήσουν να γράφετε συντηρήσιμο και αποτελεσματικό κώδικα. Ο ποιοτικός κώδικας πρέπει να είναι: επεκτάσιμος, συντηρήσιμος, να μπορεί να τεστάρεται, κλιμακωτός (scalable), και αποδοτικός.

* Καλό είναι να σχολιάζετε τις κλάσεις και τις μεθόδους σας (χρησιμοποιώντας JavaDoc). Έτσι βοηθάτε και τους άλλους αλλά και τον εαυτό σας όταν ξαναεπισκέπτεστε τον κώδικα αργότερα χωρίς να χρειάζεται να διαβάζετε τον κώδικα για να καταλάβετε τι κάνει. 
* Αποφεύγετε να γράφετε "κρυπτοκώδικα", δηλ. δυσανάγνωστο κώδικα με "μαγικούς αριθμούς" χωρίς να εξηγείτε τι κάνουν. Ο κώδικας θα πρέπει να είναι ευκολοδιάβαστος έτσι ώστε, όπως λέμε, να μη χρειάζεται σχολιασμό, αν κι αυτό δεν είναι τις πιο πολλές φορές δυνατό. 
* Μια μέθοδος θα πρέπει να κάνει μόνο ένα πράγμα, π.χ. ```calculateSum()```. Μέθοδοι που κάνουν περισσότερα από ένα πράγματα π.χ. ```calculateSumAndAverage()``` καλό είναι να σπάνε σε μικρότερες μεθόδους. Οι περισσότερες μέθοδοί σας δε πρέπει να ξεπερνούν τις 10-15 γραμμές κώδικα. Αυτό δεν βοηθάει μόνο στην ευαναγνωσιμότητα του κώδικα αλλά και τον compiler να παράγει πιο αποδοτικό κώδικα.
* Προσπαθήστε ο κώδικάς σας και κυρίως οι μεθόδοι σας να είναι όσο πιο αυτόνομες γίνεται. Π.χ. να μην εξαρτιώνται από γνωρίσματα (καθολικές μεταβλητές). Ότι χρειάζεται μια μέθοδος καλό είναι να της το περνάτε ως όρισμα και να επιστρέφει την τιμή που πρέπει αντί ν' αλλάζει την τιμή ενός γνωρίσματος. Με αυτόν τον τρόπο μπορείτε αργότερα να μεταφέρετε τη μέθοδο σε άλλη κλάση χωρίς να χρειάζεται να κάνετε αλλαγές σ'αυτή καθώς δεν έχει εξαρτήσεις από την κλάση που την περιέχει.
* Χρησιμοποιείτε ```enum```s για να περιορίσετε τα πεδία τιμών των μεταβλητών όπου αυτό είναι δυνατό.

## Αμετάβλητες (Immutable) κλάσεις 
Κλάσεις που μπορούν ν' αλλάξουν τις τιμές των γνωρισμάτων τους μετά τη δημιουργία τους λέγονται μεταβαλλόμενες (mutable) κλάσεις. Αυτές συνήθως διαθέτουν μεθόδους ```setXXX()``` που επιτρέπουν ν' αλλάξουν την τιμή ενός γνωρίσματος. Μεταβάλλοντας την κατάσταση ενός αντικειμένου κατά τη διάρκεια εκτέλεσης ενός προγράμματος μερικές φορές είναι επικίνδυνο, ιδιαίτερα αν οι τιμές των γνωρισμάτων αλλάζουν από διαφορετικές διαδικασίες (processes) ή νήματα (threads) με μη ελεγχόμενα αποτελέσματα.
Οι immutable classes είναι ευκολότερες να σχεδιαστούν και να υλοποιηθούν απ' ότι οι mutable classes, πιο ασφαλής και είναι λιγότερο ευάλωτες σε λάθη.

Οι καλές τεχνικές προγραμματισμού προωθούν τη χρήση αμετάβλητων (immutable) κλάσεων. Οι τιμές των γνωρισμάτων (δηλ. η κατάσταση) ενός στιγμιοτύπου μιας immutable class δεν μπορούν (μπορεί) να αλλάξουν (αλλάξει) μετά τη δημιουργία του. Ας δούμε ένα παράδειγμα μιας αμετάβλητης κλάσης:

```java
public final class Point {
	private final int x, y;
	
	public Point() {
		x=0;
		y=0;
	}
	
	public Point(int x, int y) {
		this.x = x;
		this.y = y;
	}
	
	public int getX() {
		return x;
	}
	
	public int getY() {
		return y;
	}
}
```
Το πρώτο πράγμα που παρατηρούμε στην παραπάνω κλάση είναι ότι δε διαθέτει setters, ενώ τα γνωρίσματά του δηλώνονται ως σταθερές που σημαίνει ότι όταν λάβουν την αρχική τιμή τους, αυτή δεν μπορεί ν' αλλάξει αργότερα. Αν θέλουμε ν' αλλάξουμε τις συντεταγμένες ενός σημείου, θα πρέπει να δημιουργήσουμε ένα νέο στιγμιότυπο της κλάσης ```Point```. Επίσης η κλάση δηλώνεται ως ```final``` που σημαίνει ότι δεν μπορεί να κληρονομηθεί ώστε μια υποκλάση ν' αλλάξει αυτή τη συμπεριφορά.

Ακολουθήστε τους παρακάτω πέντε κανόντες για να δημιουργήσετε αμετάβλητες κλάσεις:

1. μην παρέχετε mutators (δηλ. μεθόδους που αλλάζουν τις τιμές των γνωρισμάτων της κλάσης)
1. μην επιτρέψετε στην κλάση να κληρονομηθεί (ορίστε τη ως ```final```)
1. όλα τα γνωρίσματα της κλάσης πρέπει να είναι σταθερές
1. όλα τα γνωρίσματα της κλάσης πρέπει να είναι ```private```
1. αν η κλάση σας διαθέτει μεταβλητά γνωρίσματα τότε αν δεν τα εκθέτετε σε άλλες κλάσεις η κλάση σας μπορεί να χαρακτηριστεί ως immutable

Η Java διαθέτει πολλά παραδείγματα αμετάβλητων κλάσεων όπως ```String```, ```BigInteger``` και ```BigDecimal```.

## ```equals(), hashCode()``` και ```toString()```
Όταν δημιουργείτε νέες κλάσεις που αναπαριστούν αφαιρέσεις της πραγματικότητας, καλό είναι να υλοποιήσετε τις μεθόδους ```equals(), hashCode()``` και ```toString()```.

Η ```equals()``` επιστρέφει ```true``` αν ένα αντικείμενο της κλάσης είναι ίσο μ' ένα άλλο αντικείμενο της κλάσης ή μιας υποκλάσης της κλάσης. Η ```hashCode()``` επιστρέφει ένα μοναδικό αριθμό ανάλογα με τις τιμές του αντικειμένου της κλάσης και χρησιμοποιείται όταν το αντικείμενο εισαχθεί σε μια συλλογή (για τις συλλογές θα μιλήσουμε την επόμενη εβδομάδα). Τέλος η ```toString()``` επιστρέφει μια αναπαράσταση του αντικειμένου σε φιλική μορφή για τον προγραμματιστή.

Τα σύγχρονα ΟΠΕ (IDEs) μπορούν να παράγουν αυτόματα τις τρεις αυτές μεθόδους.

```java
public final class Point {
//...

   	public boolean equals(Object o) {
   		return 
   	}

```

**Σημαντική σημείωση** _Ενώ για τη σύγκριση πρωτογενών τύπων (raw types) χρησιμοποιούμε τον τελεστή ```==```, κάτι τέτοιο δε συνιστάται για τη σύγκριση αντικειμένων. Ο τελεστής ```==``` δηλώνει σύγκριση ταυτότητας των αντικειμένων αντί για ισότητα των τιμών τους. Συγκρίνει δηλ. αν η ταυτότητα του αντικειμένου ```objA``` είναι ίση με την ταυτότητα του αντικειμένου ```objB```. Για να συγκρίνουμε αν οι τιμές των δυο αντικειμένων είναι ίσες, θα πρέπει να χρησιμοποιήσουμε τη μέθοδο ```equals()```, δηλ. ```objA.equals(objB)```. Έτσι, π.χ._

```java
jshell> int num1 = 5
num1 ==> 5

jshell> int num2 = 5
num2 ==> 5

jshell> num1 == num2
$1 ==> true

jshell> Integer num1 = 5
num1 ==> 5

jshell> Integer num2 = 5
num2 ==> 5

jshell> num1 == num2
$2 ==> true

jshell> num1.equals(num2)
$3 ==> true

jshell> num1 = 150
num1 ==> 150

jshell> num2 = 150
num2 ==> 150

jshell> num1 == num2
$4 ==> false

jshell> num1.equals(num2)
$5 ==> true
``` 

_Η κλάση ```Integer``` θέλει μεγάλη προσοχή καθώς χρησιμοποιεί μια λανθάνουσα μνήμη (cache) για να αποθηκεύει τις τιμές -128 έως 127 όπως βλέπουμε στο παραπάνω δείγμα κώδικα. Επίσης το παραπάνω παράδειγμα μας δείχνει γιατί πρέπει ΠΑΝΤΑ να συγκρίνουμε αντικείμενα με τη μέθοδο ```equals()```._



## Refactorings



## Πηγές:
1. ["The Java Tutorial"](https://docs.oracle.com/javase/tutorial/)
1. Bloch J. (2018), _Effective Java_, 3rd Edition, Addison-Wesley.
1. Deitel P., Deitel H. (2018), _Java How to Program_, 11th Ed., Safari.
1. Downey A. B., Mayfield C. (2016), _Think Java_, O' Reilly. 
1. Eckel B. (2006), _Thinking in Java_, 4th Ed., Prentice-Hall.
1. Hillar G.C. (2017), _Java 9 with JShell_, Packt.
1. Horstmann C. S. (2016), _Core Java, Volume 1 Fundamentals_, 10th Ed., Prentice-Hall.
1. Horstmann C. S. (2018), _Core Java SE 9 for the impatient_, 2nd Ed., Addison-Wesley. 
1. Naftalin M., Wadler P. (2006), _Java Generics and Collections_, O'Reilly. 
1. Sharan K. (2017), _Java 9 Revealed: For Early Adoption and Migration_, Apress.
1. Sierra K. & Bates B. (2005), _Head First Java_, 2nd Ed. for Java 5.0, O’Reilly.

---

[<-](../2.7-JavaDoc/README.md) | [Δ](../../README.md)

---