# Vagrant
W tym repozytorium znajdziecie pliki Vagrantfile pozwalające na powrócenie do wybranego etapu pracy podczas kursu DevOps Core.

## Galezie 

|Galaz  | Blok*  | Opis | 
|---|---|---|
| Bare | Git 1  |Podstawowa konfiguracja maszyny z zainstalowanym jedynie Gitem. |
| Maven_Java  | Maven 1   | Dodana Java i Maven.  |
| Jenkins_Artifactory  | Artifactory 1   | Uruchomione przy pomocy docker run Jenkins oraz Artifactory. Wewnątrz job, który wymaga konfiguracji swojego repozytorium. Brak wolumenow! |
| Selenium_Grid | Projekt CI/CD/CD 2 | Dodatkowo uruchomione przy pomocy docker run kontenery Selenium. Brak wolumenow! |
| Docker_Compose | Projekt CI/CD/CD 3 |  Cały projekt CI/CD/CD uruchamiany z docker-compose.|
| Terraform_Ansible | Terraform | Instalacja Terraform, Ansible, AWS-CLI. Przed startem wymaga uzupełnienia w pliku zmiennych AWS_ACCESS_KEY_ID oraz AWS_SECRET_ACCESS_KEY.|
| Final | CI/CD 7 | Projekt końcowy. Przed startem wymaga uzupełnienia w pliku zmiennych AWS_ACCESS_KEY_ID oraz AWS_SECRET_ACCESS_KEY. |
||||
| K8S | Kubernetes | Vagrantfile używany przy zajęciach z Kubernetesa, nie związany z resztą. |

*Blok na koniec, którego dodane zostaną wszystkie potrzebne elementy.
