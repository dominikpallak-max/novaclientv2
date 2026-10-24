NOVA CLIENT 2.1 - kod źródłowy (Fabric 1.21.11)
================================================

JAK WGRAĆ (tak jak wcześniej)
1. Podmień folder src/main/java/dev/nova/client/ na ten z paczki (skasuj stare pliki .java - doszły nowe: Perf, Keys, Widgets, Cosmetics, Sync, CrosshairHook).
2. Podmień src/main/resources/fabric.mod.json.
3. ./gradlew build  ->  jar w build/libs/

KLASY MINECRAFTA
Dotykają ich tylko: Mc.java, Ent.java, NovaMenu.java (nazwy class_xxx / method_xxx jak w Twoim novaclient-sources.jar)
oraz CrosshairHook.java (to API Fabrica, nie Minecrafta).
Nowe wywołania, których NIE dało się sprawdzić bez Twojego projektu (jeśli build zgłosi błąd, to tutaj):
  Mc.java: method_18800 (Entity.setVelocity x,y,z), field_1352/1351/1350 (Vec3d x,y,z), method_4490 (Window.getHandle), method_5667 (getUuid)
  Ent.java: method_5667 (getUuid), method_36454 (getYaw)
  NovaMenu.java: metoda method_25422 (shouldCloseOnEsc) - bez @Override, więc co najwyżej Esc zamknie menu podczas ustawiania klawisza
  CrosshairHook.java: HudElementRegistry / VanillaHudElements (Fabric API 0.116+). Jeśli ich nie ma, usuń wywołanie CrosshairHook.install() w NovaClient - wszystko inne zadziała, tylko vanilla celownik zostanie pod Celownikiem+.

CO NOWEGO W 2.1
- Menu w stylu macOS Ventura: zaokrąglone karty, przełączniki, boczny pasek, "światła" okna (czerwone = zamknij, zielone = rozmiar). Lewy klik = włącz / wartość +, prawy klik = wartość - / usuń bind, kółko = przewijanie.
- Przy każdym module: przełącznik, wartość(i), kolor, keybind (klik -> naciśnij klawisz; Backspace/prawy klik = usuń). Opisy modułów usunięte.
- Komenda .nova otwiera menu. Usunięte: .nova help i lista komend (zostały .nova wp ... i .nova bind <klawisz>).
- Nowe cheaty: Speed (mnożnik), Wysoki skok (moc).
- Zakładki Visual i Efekty zamienione zawartością (Visual = efekty 3D: aura, spirala, pierścienie...; Efekty = HUD, celownik, niebo...). Chcesz z powrotem: w VisualModules.java i EffectModules.java zamień Category.EFFECTS <-> Category.VISUAL.
- Celownik+: 12 stylów, regulowany rozmiar, podgląd w menu, domyślnie biały. Gdy włączony, vanilla celownik znika.
- Brak domyślnych tęcz (celownik, aura, orbity, ESP, tracery). Tęcza nadal jest w palecie kolorów, jeśli ją wybierzesz.
- Spirala celu: podwójna helisa wokół moba/gracza, na którego patrzysz (lub najbliższego).
- Custom niebo: 7 nakładek kolorystycznych (Zachód, Noc, Różowe, Zorza, Fiolet, Złote, Lód) z regulowaną siłą. UWAGA: to nakładka na cały obraz, nie podmiana skyboxa.
- Custom tekst: napis na ekranie, treść wpisujesz w Ustawieniach (kliknij pole, pisz, Enter).
- Widgety cheatów: po włączeniu cheata pojawia się małe półprzezroczyste okienko z nazwą (prawy górny róg). Otwórz menu: widgety można przeciągać, klik = wyłącz cheat. Bind wyłącza go także. Pozycje się zapisują.
- Kosmetyki: czapki (Halo, Korona, Rogi, Kapelusz maga, Czapka imprezowa, Kocie uszy, Antenki), plecy (Peleryna, skrzydła anioła/demona/motyla/smoka), kolory.
- 11 języków: PL, EN, DE, ES, FR, IT, PT, RU, CS, UK, TR (menu i ustawienia; nazwy modułów tylko PL/EN).
- FPS Boost: przepisany. Ultra = zasięg 2 chunków, symulacja 5, moby 50%, minimalne cząsteczki, bez chmur/cieni/AO/vsync, bez limitu FPS. Ustawienia są nakładane ponownie po wejściu do świata. Efekty Nova same się upraszczają (mniej segmentów/celów) przy Boost / Ultra i gdy FPS < 40.

KOSMETYKI U INNYCH GRACZY
Gra nie ma serwera Novy, więc synchronizacja wymaga małego serwera pośredniczącego: server/relay.py (sam Python, bez bibliotek).
  python3 relay.py 8765      ->   w grze: Ustawienia -> Serwer kosmetyków: http://TWOJ_ADRES:8765
Wszyscy gracze Novy muszą wpisać ten sam adres. Bez adresu widzisz kosmetyki tylko u siebie.
Kosmetyki są rysowane na ekranie (rzut 3D->2D, jak efekty), nie jako modele w grze. Najlepiej wyglądają w pierwszej osobie
i u innych graczy; własne w 3 osobie mogą być przesunięte.

UWAGI
- Nic z tego nie było uruchamiane w grze (brak Minecrafta w środowisku). Kod przeszedł kompilację na atrapach klas i test logiki, ale pierwszy build u Ciebie może wymagać poprawki nazw wymienionych wyżej.
- FPS: 300+ zależy od komputera. Boost obniża ustawienia gry i odciąża efekty Novy; nie zastąpi Sodium/Lithium.
- Cheaty używaj tylko na własnym serwerze / singleplayerze - na większości serwerów dostaniesz bana.
