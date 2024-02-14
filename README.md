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









