# restAPI 
Mój projekt to biblioteka filmów , w której możemy wyświetlić obecne tytuły, dodać nowy tytuł, usunąć , posortować.

Aby otworzyć aplikacje musimy zainstalować flask, do tego służy komenda pip install flask oraz pip install flask-restful, są to dwie biblioteki które bedziemy używać.
Nastepnie musimy stworzyć wirtualne środowisko komenda python -m venv sciecka-do-katalogu gdzie chcemy uruchomic, nastepnie cd sciezka-do-katalogu Scripts/activate.
Nastepnie w konsoli wpisujemy flask-run, projekt powininien odtworzyć sie w przeglądarce.

Aby wyświetlić wszystkie tytuły wpisujemy http://localhost:5000/videos/all
Aby wyświetlić pierwszy tyutł wpisujemy http://localhost:5000/videos/video1
Aby utworzyć nowy tyuł w zakładce np video5 wpisujemy curl http://localhost:5000/videos/video5 -d "title=My new Video" -X PUT
Aby usunąć tytuł np tytuł nr2 wpisujemy  curl http://localhost:5000/videos/video2 -X DELETE







Szczegóły techniczne:

W klasie Videos znajdujemy metody takie jak: get,put,delete.

metoda get pobiera obiekt self i video_id, oraz zwraca nam plik json z tytułami filmów,
w metodzie get mamy również obiekt video_id ktory jest odpowiedzialny za poszczególny tytuł czyli jeżeli np chce wyświetlić tylko jeden film

metoda put jest stworzona abyśmy mogli np stworzyć nowy tytuł, pobiera rownież self i video_id oraz zwraca nam utowrzony tytuł,
aby utowrzyć nowy film musimy określić tytuł i jeśli film ma wiele pól musimy określić wszystkie te pola, aby to zrobić importujemy z biblioteki flask_restful , reqparse.
dalej tworzymy zmienną taką jak parser = reqparse.RequestParser() i odwołujemy sie parser.add_argument('title', required=True), 
oznacza to że parser pyta się o argument 'title' i jeżeli go nie otrzymuje to nie bedzie to działało, analogicznie robimy z parser.add_argument('uploadData', type=int, required=False)
w funkcji put mowimy że argumenty przekazywane do tego wywołania będa argumentami uzywami przez parser.parse_args()

metoda deleate pobiera self i video_id, wyrzuca błąd jeżeli niema video, albo kasuje video jezeli jest, ten sam schemat jest z reszta w metodzie get, natomiast tam nie usuwa tylko zwraca nam video .



W klasie VideoSchedule jest metoda get oraz post.

klasa jest swtorzona aby można było stworzyć tak jakby harmonogram a nie tylko pojedynczy film,
metoda get pobiera self i zwraca tylko obiekt videos,

metoda post tworzy nowe video ale bez koniecznosci podawania identyfikatora, identyfikator jest liczony i brany jako kolejny wolny
identyfikator video jest obliczeniem wszystkich video które mamy i zwiekszamy o jeden jako kolejny



funkcja write_changes_to_file służy do dwoch zeczy, pierwsza to sortowanie videos, wykorzystywane w tym celu jest dictionary comprehension znane z pierwszych modelów kursu
nastepnie otwiera sie w niej videos.json w którym mamy zapisane tytuły


aby resource był dostepny musimy dodać go do aplikacji, w tym celu używamy api.add_resource(Video),
również ustalamy tam scieżke mapowania adresu w naszym przypadku api.add_resource(Video,'/videos/<video_id>')
to samo robimy w przypadku klasy VideoSchedule ,czyli api.add_resource(VideoSchedule, '/videos'),
aby aplikacja została odpalona musimy użyć schematu if if __name__ == '__main__': app.run(),
parametr new_video tworzy nowe video



