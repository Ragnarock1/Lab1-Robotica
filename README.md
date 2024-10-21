# Lab1-Robotica


#Cod

#include <Arduino.h>

#define lamp1 10
#define lamp2 9
#define lamp3 8
#define lamp4 7
#define ledRosu 6
#define ledVerde 5
#define ledAlbastru 4
#define butonPornire 3
#define butonOprire 2

bool incarcareActiva = false;
unsigned long timpOprire = 0;
int ledCurent = 0;
unsigned long ultimulTimp = 0;
const unsigned long intervalIncarcare = 3000;

void setup() {
    pinMode(lamp1, OUTPUT);
    pinMode(lamp2, OUTPUT);
    pinMode(lamp3, OUTPUT);
    pinMode(lamp4, OUTPUT);
    pinMode(ledRosu, OUTPUT);
    pinMode(ledVerde, OUTPUT);
    pinMode(ledAlbastru, OUTPUT);
    pinMode(butonPornire, INPUT_PULLUP);
    pinMode(butonOprire, INPUT_PULLUP);

    digitalWrite(ledVerde, HIGH);
    digitalWrite(ledRosu, LOW);
    digitalWrite(ledAlbastru, LOW);
    digitalWrite(lamp1, LOW);
    digitalWrite(lamp2, LOW);
    digitalWrite(lamp3, LOW);
    digitalWrite(lamp4, LOW);
}

void loop() {
    unsigned long timpCurent = millis();
    int stareButonStart = digitalRead(butonPornire);
    int stareButonStop = digitalRead(butonOprire);

    if (stareButonStart == LOW && !incarcareActiva) {
        incarcareActiva = true;
        ledCurent = 1;
        digitalWrite(ledVerde, LOW);
        digitalWrite(ledAlbastru, LOW);
        digitalWrite(ledRosu, HIGH);
        ultimulTimp = millis();
    }

    if (stareButonStop == LOW && incarcareActiva) {
        timpOprire = millis();
        incarcareActiva = false;

        for (int i = 0; i < 3; i++) {
            digitalWrite(lamp1, HIGH);
            digitalWrite(lamp2, HIGH);
            digitalWrite(lamp3, HIGH);
            digitalWrite(lamp4, HIGH);
            delay(650);
            digitalWrite(lamp1, LOW);
            digitalWrite(lamp2, LOW);
            digitalWrite(lamp3, LOW);
            digitalWrite(lamp4, LOW);
            delay(650);
        }

        digitalWrite(ledVerde, HIGH);
        digitalWrite(ledAlbastru, LOW);
        digitalWrite(ledRosu, LOW);
        digitalWrite(lamp1, LOW);
        digitalWrite(lamp2, LOW);
        digitalWrite(lamp3, LOW);
        digitalWrite(lamp4, LOW);
    }

    if (incarcareActiva) {
        if (timpCurent - ultimulTimp >= intervalIncarcare) {
            ultimulTimp = timpCurent;

            if (ledCurent == 1)
                digitalWrite(lamp1, HIGH);
            if (ledCurent == 2)
                digitalWrite(lamp2, HIGH);
            if (ledCurent == 3)
                digitalWrite(lamp3, HIGH);
            if (ledCurent == 4)
                digitalWrite(lamp4, HIGH);

            ledCurent++;
            if (ledCurent > 4) {
                incarcareActiva = false;

                for (int i = 0; i < 3; i++) {
                    digitalWrite(lamp1, HIGH);
                    digitalWrite(lamp2, HIGH);
                    digitalWrite(lamp3, HIGH);
                    digitalWrite(lamp4, HIGH);
                    delay(650);
                    digitalWrite(lamp1, LOW);
                    digitalWrite(lamp2, LOW);
                    digitalWrite(lamp3, LOW);
                    digitalWrite(lamp4, LOW);
                    delay(650);
                }

                digitalWrite(ledVerde, HIGH);
                digitalWrite(ledRosu, LOW);
            }
        }
    }
}

#Cerinte
În această temă trebuie să simulați o stație de încărcare pentru un vehicul electric, folosind mai multe LED-uri și butoane. În cadrul acestui task trebuie să țineți cont de stările butonului și să folosiți debouncing, dar și să coordonați toate componentele ca într-un scenariu din viața reală.
Led-ul RGB reprezintă disponibilitatea stației. Dacă stația este liberă led-ul va fi verde, iar dacă stația este ocupată se va face roșu.

(2p) Led-urile simple reprezintă gradul de încărcare al bateriei, pe care îl vom simula printr-un loader progresiv (L1 = 25%, L2 = 50%, L3 = 75%, L4 = 100%). Loader-ul se încărca prin aprinderea succesivă a led-urilor, la un interval fix de 3s. LED-ul care semnifică procentul curent de încărcare va avea starea de clipire, LED-urile din urma lui fiind aprinse continuu, iar celelalte stinse.

(1p) Apăsarea scurtă a butonului de start va porni încărcarea. Apăsarea acestui buton în timpul încărcării nu va face nimic.

(2p) Apăsarea lungă a butonului de stop va opri încărcarea forțat și va reseta stația la starea liberă. Apăsarea acestui buton cat timp stația este liberă nu va face nimic.

#Setup
https://ibb.co/Tk0B36v

#Schema
https://ibb.co/Q9X2cCW

#Componente
4x LED-uri (pentru a simula procentul de încărcare)
1x LED RGB (pentru starea de liber sau ocupat)
2x Butoane (pentru start încărcare și stop încărcare)
8x Rezistoare (6x 220/330ohm, 2x 1K)
Breadboard
Linii de legătură

#Video 
https://youtu.be/3CWwVPkncLY
