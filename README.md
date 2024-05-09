# Boris_Petkovic_SPA_9

# Zadatak projekta
-Kreirati model koji moze da odgovora na pitanja iz datog teksta ili poznatiji kao Question Answering Model.


-Model može da prima tekst i pitanja na Engleskom jeziku.


-Ukoliko primi pitanje na koje ne može pronaći odgovor, šalje se opšta poruka.

# Biblioteke korištene u projektu: 

# Pandas:
-Glavni cilj Pandas biblioteke je pružiti efikasan, fleksibilan i visokonivou apstrakciju za rad sa strukturiranim podacima, posebno tabelarnim podacima. Ova biblioteka često se koristi u analizi podataka, statistici, mašinskom učenju, finansijama i drugim oblastima.<br>


-DataFrame: Osnovna struktura podataka u Pandas-u. To je dvodimenzionalna tabela sa redovima i kolonama, slična SQL tabelama ili Excel radnim listovima.<br>


-Učitavanje i Snimanje Podataka: Pandas pruža funkcionalnosti za učitavanje podataka iz različitih izvora kao što su CSV fajlovi, Excel fajlovi, SQL baze podataka, JSON i drugi formata. Takođe, omogućava snimanje DataFrame-a u različite formate fajlova<br>

# Torch:
-Torch je biblioteka za mašinsko učenje otvorenog koda razvijena od strane Facebook AI Research (FAIR) laboratorije. Njena osnovna svrha je pružiti alate i resurse za rad sa neuronskim mrežama, a posebno za duboko učenje (deep learning). Torch pruža fleksibilan i efikasan okvir za implementaciju različitih modela mašinskog učenja.<br>


-Tensori: Osnovna struktura podataka u Torch-u su tensori, koji su slični NumPy array-ima, ali imaju dodatne funkcionalnosti prilagođene za rad sa GPU-om. Tensori omogućavaju efikasno izračunavanje na velikim skupovima podataka<br>

-output.start_logits je tenzor koji sadrži niz brojeva. Svaki broj u tom nizu predstavlja "logit" za određeni položaj u sekvenci tokena. Logit je merilo za verovatnoću, a u ovom slučaju, može se tumačiti kao koliko model smatra da je odgovor počeo na svakom pojedinačnom položaju u sekvenci.Na primer, ako je sekvenci tokena dužine 10, output.start_logits će biti tenzor dužine 10, gde svaki element predstavlja logit za odgovarajući položaj u toj sekvenci.

-Ako imamo niz [0.2, 0.5, 0.8, 0.3], gde su ovi brojevi rezultati izlaza modela, oni se tumače kao verovatnoće da odgovor počinje na određenom položaju u sekvenci. U ovom slučaju, indeks 2 (gde je vrednost 0.8) se uzima kao početak odgovora.

Onda, koristeći torch.argmax(output.start_logits), dobijate indeks koji odgovara poziciji sa najvećom vrednošću, a to tumačite kao predviđeni početak odgovora.

# Transformers:
-Transformers je biblioteka otvorenog koda koja se fokusira na pružanje alata za rad sa modelima transformatora u oblasti obrade prirodnog jezika (NLP) i drugim sekvencijalnim podacima.<br>


-Transformers pruža prethodno trenirane modele za različite zadatke, kao što su BERT, GPT, RoBERTa, i drugi. Ove arhitekture mogu se koristiti kao osnova za fine-tjuning na specifičnim zadacima ili za generisanje novog teksta<br>


-Biblioteka sadrži alate za tokenizaciju teksta, što je ključno za pripremu podataka za treniranje transformer modela. Tokenizacija je posebno važna jer transformeri rade sa tokenima umesto sa celim reči<br>


-Transformers se lako integriše sa popularnim radnim okruženjima za mašinsko učenje, kao što su PyTorch i TensorFlow. <br>

# BertForQuestionAnswering
-BertForQuestionAnswering je model razvijen na osnovu BERT (Bidirectional Encoder Representations from Transformers) arhitekture.

-BERT je bidirekcionalni model, što znači da može uzeti u obzir kontekst i značenje riječi s obje strane, prije i poslije trenutne pozicije u rečenici.


-BertForQuestionAnswering je vrsta prethodno treniranog (pretrained) modela. To znači da je model prethodno treniran na ogromnim skupovima podataka, obično na velikim količinama teksta, kako bi naučio reprezentacije riječi i konteksta

-bert-large-uncased-whole-word-masking-finetuned-squad' znači da se koristi veliki (large) BERT model bez razlikovanja između velikih i malih slova (uncased), sa posebnim tretmanom celih reči (whole word masking), a da je dodatno fine-tjuniran na SQuAD zadatku


-BERT se trenira na velikim količinama teksta koristeći zadatak samoproučavanja. Model pokušava predvidjeti izostavljene riječi u rečenicama, koristeći okolni kontekst

-Model prima ulaz u obliku parova pitanje-kontekst. Ova dva dijela se tokeniziraju, pretvarajući ih u niz tokena koji će služiti kao ulaz za BERT model.

-Dodaju se segmentni embeddingi kako bi se modelu omogućilo razlikovanje između pitanja i konteksta

-Model izlazi sa dvije distribucije vjerojatnosti (start i end logits) koje označavaju početak i kraj odgovora unutar konteksta

# BertTokenizer

-BertTokenizer je dio Transformers biblioteke koji se koristi za tokenizaciju teksta tako da ga možete koristiti s modelima zasnovanim na BERT-u.

-Koristi tokenizaciju koja uključuje dodavanje posebnih tokena, kao što su [CLS] za označavanje početka sekvence i [SEP] za označavanje kraja sekvence. Osim toga, riječi se mogu razbijati na podriječi ("subwords") kako bi se omogućila bolja reprezentacija različitih dijelova riječi.

-U BERT-u, rijetke riječi se raščlanjuju na podriječi/dijelove. Tokenizacija riječi koristi ## za razgraničenje tokena koji su podijeljeni.
![trecaSlika](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/e771753e-9a6c-4f3c-9394-ceb5323f781f)


-Ideja iza upotrebe tokenizacije riječi je smanjenje veličine rječnika što poboljšava performanse treninga. Razmotrite riječi, run, running, runner. Bez tokenizacije riječi, model mora nezavisno pohraniti i naučiti značenje sve tri riječi. Međutim, sa tokenizacijom riječi, svaka od tri riječi bi bila podijeljena na 'run' i srodni '##SUFFIX' (ako ima sufiksa - na primjer, "run", "##ning", "##ner ”). Sada će model naučiti kontekst riječi "trčati", a ostatak značenja će biti kodiran u sufiksu, koji bi se naučio iz drugih riječi sa sličnim sufiksima



# CoQA
-CoQA(Conversational Question Answering) je skup podataka za konverzacijsko odgovaranje na pitanja koji je objavio Stanford NLP 2019. To je skup podataka velikih razmjera za izgradnju sistema za odgovor na konverzacijsko pitanje. Ovaj skup podataka ima za cilj da izmeri sposobnost mašina da razumeju tekst i odgovore na niz međusobno povezanih pitanja koja se pojavljuju u razgovoru. Jedinstvena karakteristika ovog skupa podataka je da se svaki razgovor prikuplja uparivanjem dvaju radnika da razgovaraju o odlomku u obliku pitanja i odgovora i stoga su pitanja konverzacijski.U ovom projektu se koristi  "story", "question" i "answer" iz JSON skupa podataka da formiramo naš okvir podataka





![prvaSlika](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/3e4a363c-3da4-4017-bec1-c4cac36ea9e2)
![drugaSlika](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/cfe6b294-e868-448f-974d-55ca02a6afdd)


# Statistika tačnosti
-U slijedecem dijelu koda je uredjena statistika tačnosti ovoga modela. Uzeto je nasumičan broj tekstova i pitanja, maksimalno 100, odnosno pokušala su se izbjeći pitanja koja se nadovezuju na prethodno pitanje kao što je npr. "and?". Ovaj model nema sposobnost da se nadovezuje na razgovor i da odgovara na takva pitanja kao i da/ne pitanja. 


![codeStatistika](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/31d65453-6762-4cb1-b3e0-f3bed68f3140)


-Ono što je specifično korišćeno u ovom kodu jeste biblioteka "fuzzywuzzy" koja je davala procenat od 0 do 100 koliko su slicne rečenice, odnosno u mom slučaju, koliko su slični odgovori modela i odgovori navedeni u skupu podataka.Ako je procenat sličnosti bio preko 50% ili ako je jedan od tih odgovor bio sadržavao drugi odgovor, onda je broj tačnih odgovora modela se povećavao, npr.

![primjer1](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/29d71d00-b288-4db8-ab41-c260592b644e)

![primjer2](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/23c2f9c4-fb7a-45ca-a103-5775f85b563a)

![primjer3](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/9929b676-7131-4a5d-be36-1550c3774d9a)

![primjer6](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/10111075-ee8d-4a64-9702-3590034c1832)

-Dok na primjer je bilo i ovakvih slučajeva, gdje je odgovor bio da/ne oblika i samim time nas model bi dao netačan odgovor jer nema tu sposobnost:

![primjer7](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/83e88504-aa8b-474b-8958-7f39bc88b9b3)

-Procenat tačnosti kaže sledeće:
![statistika1](https://github.com/BorisPetkovic70/Boris_Petkovic_SPA_9/assets/133760465/cce66c7c-7575-4ee2-af73-2e114d732cbe)

-Uzimajući u obzir da je bilo pitanja za koje model nije uopšte namijenjen da odgovori, kao što su da/ne odgovori ili odgovori koji se ne mogu izvući iz teksta direktno već je potrebna veća inteligencija i razmišljanje da se zaključi odgovor, ovaj model je uradio vrlo dobar posao.



