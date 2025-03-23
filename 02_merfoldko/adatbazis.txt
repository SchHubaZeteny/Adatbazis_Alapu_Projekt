DROP TABLE CEG;
DROP TABLE HR;
DROP TABLE FELKERES;
DROP TABLE ALLAS;
DROP TABLE JELENTKEZIK;
DROP TABLE ALLASKERESO;
DROP TABLE ADATLAP;
DROP TABLE AADMIN;

CREATE TABLE CEG(
nev VARCHAR2(60) NOT NULL PRIMARY KEY,
email VARCHAR2(60) NOT NULL,
jelszo VARCHAR2(60) NOT NULL,
cim VARCHAR2(60) NOT NULL
);

CREATE TABLE HR(
email VARCHAR2(60) NOT NULL PRIMARY KEY,
nev VARCHAR2(60) NOT NULL,
jelszo VARCHAR2(60) NOT NULL,
ceg_nev VARCHAR2(60) NOT NULL,
CONSTRAINT HR_FOREIGN_KEY FOREIGN KEY (ceg_nev) REFERENCES CEG (nev) ON DELETE CASCADE
);


CREATE TABLE ALLASKERESO 
   (email VARCHAR2(60) NOT NULL PRIMARY KEY, 
    nev VARCHAR2(60) NOT NULL, 
    jelszo VARCHAR2(60) NOT NULL, 
    statusz VARCHAR2(10) DEFAULT 'aktiv', 
    bejelentkezes_idopont DATE
   );
   
CREATE TABLE FELKERES(
    hr_email VARCHAR2(60),
    allaskereso_email VARCHAR2(60),
    CONSTRAINT HR_ALLAS PRIMARY KEY (hr_email, allaskereso_email),
    CONSTRAINT FK_HR FOREIGN KEY (hr_email) REFERENCES HR (email) ON DELETE CASCADE,
    CONSTRAINT FK_ALLASKER FOREIGN KEY (allaskereso_email) REFERENCES ALLASKERESO (email) ON DELETE CASCADE
);

CREATE TABLE ALLAS 
   ( id NUMBER NOT NULL PRIMARY KEY, 
    ertekeles NUMBER, 
    leiras VARCHAR2(2000), 
    fizetes NUMBER, 
    pozicio VARCHAR2(100), 
    munkakor VARCHAR2(100), 
    cim VARCHAR2(200), 
    hirdetes_datum DATE, 
    jelentkezes_datum DATE, 
    ceg_nev VARCHAR2(60), 
     CONSTRAINT FK_ALLAS_CEG FOREIGN KEY (ceg_nev)
      REFERENCES CEG (nev) 
      ON DELETE CASCADE
   );

CREATE TABLE JELENTKEZIK (
  allas_id             NUMBER,
  allaskereso_email    VARCHAR2(60),
  CONSTRAINT pk_jelentkezik PRIMARY KEY (allas_id, allaskereso_email),
  CONSTRAINT fk_jelentkezik_allas FOREIGN KEY (allas_id)
    REFERENCES ALLAS (id)
    ON DELETE CASCADE,
  CONSTRAINT fk_jelentkezik_allaskereso FOREIGN KEY (allaskereso_email)
    REFERENCES ALLASKERESO (email)
    ON DELETE CASCADE
);



CREATE TABLE AADMIN
     (email VARCHAR2(60) NOT NULL PRIMARY KEY,
      nev VARCHAR2(60) NOT NULL,
      jelszo VARCHAR2(60)NOT NULL);
      
CREATE TABLE ADATLAP 
    (id NUMBER NOT NULL PRIMARY KEY,
     cim VARCHAR2(60) NOT NULL,
     vegzettseg VARCHAR2(50),
     szuldatum DATE,
     nyelvvizsga VARCHAR2(50),
     kepzettseg VARCHAR2(50),
     oneletrajz BLOB DEFAULT NULL,
     kitoltes_datum DATE,
     admin_email VARCHAR2(60),
     allaskereso_email VARCHAR2(60),
   CONSTRAINT fk_adatlap_admin FOREIGN KEY (admin_email)
     REFERENCES AADMIN(email) ON DELETE CASCADE,
   CONSTRAINT fk_adatlap_allaskereso FOREIGN KEY (allaskereso_email)
     REFERENCES ALLASKERESO(email) ON DELETE CASCADE);
        
INSERT INTO CEG VALUES ('Szebb Jövőért','szebjov@gmail.com','jelszo12', 'Budapest Fasor utca 5');
INSERT INTO CEG VALUES ('Zsibbvásár','zsibbi@gmail.com','zsibbike56', 'Sopron Kalap utca 34');
INSERT INTO CEG VALUES ('IT Kuckó','kukac78@gmail.com','erosJelszo89', 'Győr Fenyő utca 9');
INSERT INTO CEG VALUES ('MarketLeader','azenboltom@gmail.com','bolt45', 'Pécs Széchenyi tér');
INSERT INTO CEG VALUES ('CreativeSoft','epit334@gmail.com','epCentr_88', 'Miskolc, Petőfi utca');

INSERT INTO HR VALUES ('gabihr@gmail.com','Kerekes Gábor','gabi777','Szebb Jövőért');
INSERT INTO HR VALUES ('csilla@gmail.com','Nagy Csilla','csillag0','Zsibbvásár');
INSERT INTO HR VALUES ('anitahr@gmail.com','Kovács Anita','anita_hr23','IT Kuckó');
INSERT INTO HR VALUES ('zoltanhr@gmail.com','Fazekas Zoltán','edeny0101','MarketLeader');
INSERT INTO HR VALUES ('tibor@gmail.com','Kiss Tibor','csoki667','CreativeSoft');

INSERT INTO ALLASKERESO VALUES ('adam@example.com', 'Adam Nagy', 'titok123', 'aktív', TO_DATE('2025-03-15','YYYY-MM-DD'));
INSERT INTO ALLASKERESO VALUES ('bea@example.com', 'Bea Kiss', 'jelszo456', 'inaktív', TO_DATE('2025-03-14','YYYY-MM-DD'));
INSERT INTO ALLASKERESO VALUES ('ciri@example.com', 'Ciri Farkas', 'abc123', 'aktív', TO_DATE('2025-03-16','YYYY-MM-DD'));
INSERT INTO ALLASKERESO VALUES ('dani@example.com', 'Dani Horváth', 'pass789', 'aktív', TO_DATE('2025-03-13','YYYY-MM-DD'));
INSERT INTO ALLASKERESO VALUES ('eva@example.com', 'Eva Szabó', 'qwerty', 'inaktív', TO_DATE('2025-03-12','YYYY-MM-DD'));

INSERT INTO FELKERES VALUES ('anitahr@gmail.com','adam@example.com');
INSERT INTO FELKERES VALUES ('anitahr@gmail.com','bea@example.com');
INSERT INTO FELKERES VALUES ('gabihr@gmail.com','ciri@example.com');
INSERT INTO FELKERES VALUES ('zoltanhr@gmail.com','dani@example.com');
INSERT INTO FELKERES VALUES ('tibor@gmail.com','eva@example.com');

INSERT INTO ALLAS (id, ertekeles, leiras, fizetes, pozicio, munkakor, cim, hirdetes_datum, jelentkezes_datum, ceg_nev)
VALUES (1, 4, 'Fejlesztői pozíció, C/C++', 600000, 'Junior Developer', 'Szoftverfejlesztés', 'Győr Fenyő utca 9', TO_DATE('2025-03-01', 'YYYY-MM-DD'), TO_DATE('2025-03-20', 'YYYY-MM-DD'), 'IT Kuckó');
INSERT INTO ALLAS (id, ertekeles, leiras, fizetes, pozicio, munkakor, cim, hirdetes_datum, jelentkezes_datum, ceg_nev)
VALUES (2, 5, 'Projektmenedzser, tapasztalt szakembert keresünk', 900000, 'Project Manager', 'Menedsment', 'Győr Fenyő utca 9', TO_DATE('2025-02-15', 'YYYY-MM-DD'), TO_DATE('2025-03-15', 'YYYY-MM-DD'), 'IT Kuckó');
INSERT INTO ALLAS (id, ertekeles, leiras, fizetes, pozicio, munkakor, cim, hirdetes_datum, jelentkezes_datum, ceg_nev)
VALUES (3, 3, 'Rendszergazda pozíció, 24/7 műszakban', 500000, 'Rendszergazda', 'IT Support', 'Budapest Fasor utca 5', TO_DATE('2025-03-05', 'YYYY-MM-DD'), TO_DATE('2025-03-25', 'YYYY-MM-DD'), 'Szebb Jövőért');
INSERT INTO ALLAS (id, ertekeles, leiras, fizetes, pozicio, munkakor, cim, hirdetes_datum, jelentkezes_datum, ceg_nev)
VALUES (4, 4, 'Ügyfélszolgálati munkatárs keresünk', 350000, 'Ügyfélszolgálat', 'Customer Support', 'Pécs Széchenyi tér', TO_DATE('2025-02-28', 'YYYY-MM-DD'), TO_DATE('2025-03-18', 'YYYY-MM-DD'), 'MarketLeader');
INSERT INTO ALLAS (id, ertekeles, leiras, fizetes, pozicio, munkakor, cim, hirdetes_datum, jelentkezes_datum, ceg_nev)
VALUES (5, 5, 'Innovatív termékfejlesztés', 800000, 'Product Manager', 'Termékfejlesztés', 'Miskolc, Petőfi u.', TO_DATE('2025-03-10', 'YYYY-MM-DD'), TO_DATE('2025-03-30', 'YYYY-MM-DD'), 'CreativeSoft');

INSERT INTO JELENTKEZIK (allas_id, allaskereso_email) VALUES (1, 'adam@example.com');
INSERT INTO JELENTKEZIK (allas_id, allaskereso_email) VALUES (2, 'bea@example.com');
INSERT INTO JELENTKEZIK (allas_id, allaskereso_email) VALUES (3, 'ciri@example.com');
INSERT INTO JELENTKEZIK (allas_id, allaskereso_email) VALUES (4, 'dani@example.com');
INSERT INTO JELENTKEZIK (allas_id, allaskereso_email) VALUES (5, 'eva@example.com');

INSERT INTO AADMIN VALUES('admin@admin.com', 'admin', 'admin');
INSERT INTO AADMIN VALUES('kiraly@gmail.com', 'kiraly', 'kiraly');
INSERT INTO AADMIN VALUES('fonok@gmail.com', 'fonok', 'fonok');
INSERT INTO AADMIN VALUES('masikadmin@gmail.com', 'madmin', '12345');
INSERT INTO AADMIN VALUES('harmadikadmin@gmail.com', 'hadmin', '12345');

INSERT INTO ADATLAP VALUES(1, 'Budapest Király utca 5', 'SZTE', TO_DATE('2000-02-11','YYYY-MM-DD'), 'Angol C1', 'Programtervező informatikus', NULL, TO_DATE('2025-02-12','YYYY-MM-DD'), 'admin@admin.com', 'adam@example.com');
INSERT INTO ADATLAP VALUES(2, 'Szeged Fa utca 25', 'SZTE', TO_DATE('1999-01-11','YYYY-MM-DD'), 'Angol C1', 'Mérnökinformatikus', NULL, TO_DATE('2025-02-12','YYYY-MM-DD'), 'kiraly@gmail.com', 'bea@example.com'); 
INSERT INTO ADATLAP VALUES(3, 'Kaposvár Béla utca 11', 'SZTE', TO_DATE('2002-12-11','YYYY-MM-DD'), 'Német C1', 'Programtervező informatikus', NULL, TO_DATE('2025-02-12','YYYY-MM-DD'), 'fonok@gmail.com', 'ciri@example.com'); 
INSERT INTO ADATLAP VALUES(4, 'Szeged Marostői utca 20', 'ELTE', TO_DATE('1998-05-20','YYYY-MM-DD'), 'Angol C1', 'Gazdaságinformatikus', NULL, TO_DATE('2025-02-12','YYYY-MM-DD'), 'masikadmin@gmail.com', 'dani@example.com'); 
INSERT INTO ADATLAP VALUES(5, 'Budapest Kör utca 15', 'PTE', TO_DATE('2001-10-05','YYYY-MM-DD'), 'Spanyol C1', 'Programtervező informatikus', NULL, TO_DATE('2025-02-12','YYYY-MM-DD'), 'harmadikadmin@gmail.com', 'eva@example.com');
