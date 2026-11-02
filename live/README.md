# Florina Social Lab · Live Classroom

## Τι προστέθηκε

- \`index.html\`: δημόσια σελίδα συμμετεχόντων, μόνο για ανάγνωση. Τα πεδία απαντήσεων μένουν στον browser και **δεν αποστέλλονται**.
- \`instructor.html\`: οθόνη εισηγητή με σενάρια, αλλαγή κειμένων, τοπική προεπισκόπηση και (όταν ρυθμιστεί) δημοσίευση στο Firebase.
- \`config.js\`: δημόσιες ρυθμίσεις Firebase web app. **Δεν περιέχει μυστικά**· έως ότου συμπληρωθεί, λειτουργούμε σε τοπική προεπισκόπηση.
- \`database.rules.EXAMPLE.json\`: πρότυπο κανόνων για δημόσια ανάγνωση ενός ΜΟΝΟ εκπαιδευτικού σεναρίου και εγγραφή αποκλειστικά από το Firebase UID του εισηγητή.

## Πριν τη σύνδεση Firebase

Η εφαρμογή δείχνει υποθετικά δεδομένα, αλλά **δεν ενημερώνει άλλες συσκευές**. Το κουμπί «Δημοσίευση στα κινητά» μένει απενεργοποιημένο. Αυτό είναι σκόπιμο για ασφάλεια.

## Σύνδεση βήμα βήμα

1. Άνοιξε https://console.firebase.google.com/ και δημιούργησε **νέο εκπαιδευτικό project**, όχι project εργασίας/πελάτη.
2. Πρόσθεσε Web App στο project. Στο Project settings → Your apps → SDK setup and configuration βρες το δημόσιο αντικείμενο \`firebaseConfig\`. Χρειαζόμαστε τα \`apiKey\`, \`authDomain\`, \`databaseURL\`, \`projectId\`, \`appId\` (και όσα άλλα δίνει). **Μην ανεβάζεις ποτέ service-account JSON/private keys.** Το \`databaseURL\` προστίθεται από το Realtime Database.
3. Build → Realtime Database → Create database. **Μην ενεργοποιήσεις Test mode για δημόσια χρήση**· χρησιμοποίησε Locked mode και μετά συγκεκριμένους κανόνες.
4. Build → Authentication → Get started → Sign-in method → ενεργοποίησε **Email/Password**. Δημιούργησε **έναν** λογαριασμό εισηγητή στην καρτέλα Users. Μην κοινοποιείς τον κωδικό στους μαθητές ή στο ChatGPT.
5. Αν χρειαστεί, Authentication → Settings → Authorized domains → πρόσθεσε \`dimitriosbatistakis-max.github.io\`. Μη βάζεις ολόκληρο URL, μόνο domain.
6. Από Authentication → Users αντέγραψε το **UID** του δικού σου λογαριασμού εισηγητή. Αυτό είναι αναγνωριστικό, **όχι κωδικός**.
7. Realtime Database → Rules: επικόλλησε τους κανόνες από το \`database.rules.EXAMPLE.json\`, **αντικαθιστώντας** το \`REPLACE_WITH_INSTRUCTOR_UID\` με το πραγματικό UID. Δημοσίευσε τους κανόνες. Δεν πρέπει να υπάρχουν άλλοι κανόνες που επιτρέπουν γενική εγγραφή σε γονικό path.
8. Μέσα στο GitHub repository, άνοιξε \`live/config.js\` και βάλε τα στοιχεία στο \`window.FLORINA_FIREBASE_CONFIG = { ... }\`; βάλε το ίδιο UID στο \`window.FLORINA_INSTRUCTOR_UID\`. Τα Firebase Web configuration keys δεν είναι private admin credentials, αλλά πρέπει πάντα να συνδυάζονται με ασφαλείς κανόνες. **Μην περιλάβεις email/password.**
9. Άνοιξε \`/live/instructor.html\`. Συνδέσου ως εισηγητής. Επιλογή σεναρίου → προεπισκόπηση → **Δημοσίευση στα κινητά**.
10. Άνοιξε \`/live/\` από δεύτερη συσκευή ή παράθυρο incognito. Έλεγξε ότι ενημερώνεται αυτόματα. Έλεγξε επίσης ότι μη συνδεδεμένος μαθητής **δεν μπορεί** να γράψει δεδομένα στο Firebase.

## Σημαντικά

- Το GitHub Pages φιλοξενεί τα στατικά HTML. Το Firebase Realtime Database μεταφέρει τις ζωντανές αλλαγές.
- Μόνο οι 7 πληροφορίες της υποθετικής επιχείρησης δημοσιεύονται στην κοινή βάση. Όχι απαντήσεις μαθητών.
- Οι φράσεις «Σήμερα» και «Ωράριο» είναι **σκόπιμα ασύμβατες** στο αρχικό σενάριο, για εκπαιδευτική χρήση.
- Η καρτέλα εισηγητή **δεν είναι μυστική**: ο έλεγχος εγγραφής γίνεται από Firebase Authentication και Realtime Database Rules, όχι επειδή ο σύνδεσμος είναι κρυφός.
- Οι κανόνες με \`REPLACE_WITH_INSTRUCTOR_UID\` δεν θα δώσουν δικαίωμα δημοσίευσης σε κανέναν μέχρι να συμπληρωθεί η πραγματική τιμή.
- Μπορείς να δείξεις το \`/live/\` στην τάξη ακόμη και αν ο συγχρονισμός δεν έχει ενεργοποιηθεί: θα είναι ξεκάθαρα χαρακτηρισμένο ως δοκιμαστικό.

Επίσημες οδηγίες:
- https://firebase.google.com/docs/web/setup
- https://firebase.google.com/docs/database/web/read-and-write
- https://firebase.google.com/docs/database/security
- https://firebase.google.com/docs/auth/web/password-auth
