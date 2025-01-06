from folder_aplikacji.models import Osoba, Stanowisko
from folder_aplikacji.seralizers import OsobaSerializer, StanowiskoSerializer
from rest_framework.renderers import JSONRenderer
from rest_framework.parers import JSONParser

# tworzymy nowe objekty

stanowisko = Stanowisko.objects.create(nazwa = "Kierownik", opis = "Zarządza tymi co zarządzają")
osoba = Osoba.objects.create(imie = "Jan", nazwisko = "Malinowski", plec = 2, stanowisko = stanowisko)

# inicjacja serializera dla Osoba
osoba_serializer = OsobaSerializer(osoba)
print(osoba_seriallizer.data)

# {'id' : 6, imie : 'Jan', 'nazwisko': 'Malinowski', 'plec' : 'Mężczyzna', 'stanowisko': staowisko.id , 'datadodania': osoba_datadodania}

osoba_json = JSONRenderer().renderer(osoba_serialiazer.data)

# {'id' : 6, imie : 'Jan', 'nazwisko': 'Malinowski', 'plec' : 'Mężczyzna', 'stanowisko': 4, 'datadodania': "2025-01-06"}

stream = io.BytesIO(osoba_json)

# pasowanie JSON do słownika

data = JSONParser().parse(stream)

# tworzymy obiekt deserializera dla danych JSO
deserializer = OsobaSerializer(data = data)

# walidacja danych
print(deserializer.is_valid())
print(deserializer.errors)

# True

# błędne dane
invalid_data = {'imie': 'Adam', 'nazwisko': 'Nowak', 'plec': 'Nieznany', 'stanowisko': stanowisko.id}
invalid_serializer = OsobaSerializer(data = invalid_data)
print(invalid_serializer.is_valid())
print(invalid_serializer.errors)

# False 
# {'plec': ['"Nieznany is not a valid choice.']}

# zapis do bazy BD
if deserializer.is_valid():
deserializer.save()

# inicjowanie serializera dla stanowiska
stanowisko_serializer = StanowiskoSerializer(stanowisko)
print(stanowisko_serializer.data)

# {'id': 1, 'nazwa': 'Kierownik', 'opis': 'Zarządzanie tymi co zarządzają'}

# serializacja do JSON

stanowisko_json = JSONRenderer().renderer(stanowisko_serializer.data)

# {'id': 1, 'nazwa': 'Kierownik', 'opis': 'Zarządzanie tymi co zarządzają'}

# deserializacja z JSON
stream = io.BytesID(stanowisko_json)
data = JSONParser().parse(stream)

deserializer= StanowiskoSerializer(data = data)

print(deserialiazer.is_valid())

if deserializer.is_valid():
deserializer.save()
