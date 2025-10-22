n8n ile AI Destekli Google Maps Lead Toplama Otomasyonu
Bu n8n projesi, Google Haritalar'dan potansiyel müşteri (lead) bilgilerini toplamak için tasarlanmış AI destekli bir chatbot altyapısı sunar. Kullanıcıdan gelen sohbet mesajlarını anlayan, bu mesajlara göre Google Haritalar'da arama yapan ve bulunan işletme bilgilerini işleyip bir Google E-Tablosuna kaydeden iki aşamalı bir otomasyon sistemidir.

Projenin Amacı
Projenin temel amacı, "Ankara'daki restoranları bul" veya "İstanbul'daki tesisatçıları listele" gibi doğal dil komutlarını alıp, bu komutlara karşılık gelen işletmelerin iletişim bilgilerini (isim, adres, telefon, web sitesi vb.) otomatik olarak bir Google E-Tablosuna aktarmaktır.

Özellikler
Sohbet Arayüzü: When chat message received tetikleyicisi ile kullanıcılarla sohbet edebilir.

AI Destekli: OpenAI Chat Model kullanarak kullanıcının niyetini anlar ve araçları (Tools) ne zaman kullanacağına karar verir.

Hafıza: Simple Memory sayesinde önceki konuşmaları hatırlayabilir.

Harita Arama Aracı: google.serper.dev servisini kullanarak Google Haritalar'da arama yapan özel bir Map Search Tool içerir.

Otomatik Veri Kaydı: Bulunan bilgileri işlemek ve Google E-Tablolar'a kaydetmek için 2.otomasyon adlı bir alt iş akışını tetikler.

Veri Temizleme ve Standardizasyon: Gelen veriye UUID ekler, eksik alanları (örn: e-posta için 'N/A') doldurur ve alan adlarını (örn: i.name -> Name) standartlaştırır.

İş Akışı (Workflow) Mimarisi
Proje, birbiriyle konuşan iki ana iş akışından oluşur:

1. Ana İş Akışı: AI Agent
Bu akış, kullanıcıyla etkileşime giren ve orkestrasyonu sağlayan ana chatbot mantığıdır.

Tetikleyici: When chat message received

Kullanıcıdan bir sohbet mesajı geldiğinde başlar.

Ana Düğüm: AI Agent

Model: OpenAI Chat Model (Sistemin beyni).

Memory: Simple Memory (Konuşma geçmişini tutar).

Tools (Araçlar):

Map Search Tool: Google Maps API'si (google.serper.dev) ile entegre olur. AI, bir yer araması gerektiğine karar verirse bu aracı kullanır.

2.otomasyon: Map Search Tool'dan gelen verileri kaydetmek için çağrılan bir alt iş akışı aracıdır.

2. Alt İş Akışı: 2.otomasyon (Veri Kayıt Akışı)
Bu akış, AI Agent tarafından tetiklenir ve sadece veri işleme ve kaydetme görevini üstlenir.

Tetikleyici: When Executed by Another Workflow

Ana AI Agent tarafından çağrıldığında çalışır.

Adım 1: Code in JavaScript

Ana akıştan gelen ham veriyi (işletme listesi) alır.

Her bir işletme kaydı için:

v7() fonksiyonu ile benzersiz bir UUID oluşturur.

Alan adlarını standartlaştırır (örn: i.name veya i.Name her zaman Name olur).

Eksik veriler için varsayılan değerler atar (örn: Email: i.Email || i.email || 'N/A').

Adım 2: Append row in sheet

Temizlenen ve standartlaşan veriyi belirtilen Google E-Tablosuna yeni satırlar olarak ekler.

Adım 3: Code in JavaScript1

İşlemin başarıyla tamamlandığını belirtmek için ana AI Agent'a {"response": "ok"} şeklinde bir JSON yanıtı döndürür.

Kurulum ve Gereksinimler
Bu projeyi çalıştırmak için aşağıdaki servislere ve kimlik bilgilerine (credentials) ihtiyacınız olacaktır:

n8n: Projenin çalıştığı otomasyon platformu.

OpenAI: AI Agent'ın beyni için bir OpenAI API anahtarı.

Google Sheets: Verilerin kaydedileceği Google E-Tablosu ve n8n için ilgili Google kimlik bilgileri.

Google Serper: Map Search Tool'un kullandığı google.serper.dev servisi için bir API anahtarı.

Nasıl Kullanılır
Her iki iş akışının (.json dosyaları) n8n editörünüze aktarın.

Gerekli n8n kimlik bilgilerini (Credentials) OpenAI, Google Sheets ve Google Serper (HTTP Request düğümü için) oluşturun.

2.otomasyon akışındaki Append row in sheet düğümünü kendi Google E-Tablonuzla eşleştirin.

Ana AI Agent iş akışını aktif hale getirin.

When chat message received düğümünün "Open chat" özelliğini kullanarak AI Agent ile sohbet etmeye başlayın.

Örnek Komut: "Bana İstanbul, Kadıköy'deki 10 kahve dükkanını bul.

"<img width="1832" height="821" alt="Ekran görüntüsü 2025-10-22 182605" src="https://github.com/user-attachments/assets/81038c29-59c2-4ace-a1ff-1245a925f183" />
<img width="526" height="826" alt="Ekran görüntüsü 2025-10-22 182650" src="https://github.com/user-attachments/assets/93478bd4-7dc8-4815-9be1-3bd9a63886fa" />
<img width="535" height="843" alt="Ekran görüntüsü 2025-10-22 182722" src="https://github.com/user-attachments/assets/aa25e9e0-e165-42af-8016-48451d47d84e" />
<img width="1212" height="362" alt="Ekran görüntüsü 2025-10-22 182752" src="https://github.com/user-attachments/assets/24bb9823-e1e4-4129-bac3-676206c12832" />
<img width="1085" height="852" alt="Ekran görüntüsü 2025-10-22 182813" src="https://github.com/user-attachments/assets/302ce017-d3eb-4834-8d89-a8607626acd1" />
<img width="1166" height="876" alt="Ekran görüntüsü 2025-10-22 182834" src="https://github.com/user-attachments/assets/78a59eb7-7edf-4da3-938b-2cda48d0bf03" />


n8n ile Gelişmiş RAG Chatbot: Belgelerle Sohbet
Bu n8n projesi, Retrieval-Augmented Generation (RAG) mimarisini kullanarak PDF belgelerinizle sohbet etmenizi sağlayan gelişmiş bir yapay zeka sistemidir. Proje, belgeleri bir vektör veritabanına yükleyen bir "veri yükleme" akışı ve kullanıcı sorularını bu belgelerdeki bilgilere dayanarak yanıtlayan bir "yapay zeka ajanı" akışı olmak üzere iki ana bölümden oluşur.

Sistem, en doğru bilgiyi bulmak için OpenAI (Embedding ve Chat için), Supabase (Vektör Depolama için) ve Cohere (Yeniden Sıralama/Reranking için) teknolojilerini bir araya getirir.

Projenin Amacı
Bu projenin amacı, bir sohbet arayüzü aracılığıyla, daha önce sisteme yüklenmiş olan PDF belgelerinin içeriği hakkında sorular sormanıza ve bu sorulara doğrudan belgelerden alınan bilgilere dayalı, akıllı yanıtlar almanıza olanak tanımaktır.

Özellikler
Belge Yükleme (Ingestion): Google Drive'dan PDF dosyalarını otomatik olarak çeker, metinlerini çıkarır ve Supabase vektör veritabanına gömer (embedding).

AI Chatbot (Ajan): OpenAI modeli kullanarak doğal dil sorularını anlar.

Vektör Arama: Sorularla ilgili belge parçalarını bulmak için Supabase'de vektör araması yapar.

Gelişmiş Yeniden Sıralama (Reranking): Cohere Reranker kullanarak bulunan sonuçlar arasından en alakalı olanları önceliklendirir ve yanıt kalitesini artırır.

Bağlama Dayalı Yanıt (RAG): AI Agent, sadece veritabanından alınan belge parçacıklarını ("context") kullanarak yanıt üretir, böylece halüsinasyon görmesi engellenir.

İş Akışı Mimarisi
Proje, birbirini tamamlayan iki ana iş akışından (workflow) oluşur:

1. Akış: "Dosya Yukleme" (Veri Yükleme Akışı)
Bu akış, PDF belgelerinizi "öğrenmek" ve vektör veritabanına kaydetmek için kullanılır.

Tetikleyici: When clicking 'Execute workflow' (Manuel olarak başlatılır).

Adım 1: Download file - Belirtilen PDF dosyasını Google Drive'dan indirir.

Adım 2: Extract from File - İndirilen PDF dosyasının metin içeriğini çıkarır.

Adım 3: Code in JavaScript - (Muhtemelen) Çıkarılan metni daha küçük, işlenebilir parçalara (chunks) böler.

Adım 4: Supabase Vector Store - Bu metin parçalarını Embeddings OpenAI kullanarak vektörlere dönüştürür ve Default Data Loader aracılığıyla Supabase veritabanındaki documents tablosuna kaydeder.

2. Akış: "Yapay zeka ajanı" (Soru-Cevap Akışı)
Bu akış, son kullanıcının sohbet ettiği ve sorularına yanıt aldığı ana chatbot'tur.

Tetikleyici: When chat message received - Kullanıcıdan bir sohbet mesajı geldiğinde başlar.

Ana Düğüm: AI Agent

Model: OpenAI Chat Model (Sistemin beyni, yanıtları üretir).

Tool (Araç): Supabase Vector Store1 (Sistemin bilgi kaynağı).

RAG Süreci Nasıl Çalışır:

Kullanıcı bir soru sorar (örn: "Proje bütçesi ne kadardı?").

AI Agent, bu soruyu yanıtlamak için bilgiye ihtiyacı olduğuna karar verir ve Supabase Vector Store1 aracını kullanır.

Arama Aşaması:

Embeddings OpenAI1 kullanılır ve kullanıcının sorusu bir vektöre dönüştürülür.

Supabase veritabanında (documents tablosu) bu soru vektörüne en yakın 4 adet belge parçası (chunk) bulunur.

Yeniden Sıralama Aşaması:

Bulunan bu 4 sonuç, Reranker Cohere modeline gönderilir.

Cohere, bu 4 sonucu "alakalılık" (relevance) bakımından yeniden sıralar ve en doğru olanı/olanları en üste taşır.

Yanıt Üretme Aşaması:

Yeniden sıralanmış ve en alakalı olan belge parçaları, AI Agent'a "bağlam" (context) olarak sunulur.

OpenAI Chat Model'e şu şekilde bir komut verilir: "Sadece sana verdiğim bu bağlamı kullanarak kullanıcının sorusunu yanıtla."

Model, yanıtını üretir ve kullanıcıya gönderir.

Kurulum ve Gereksinimler
Bu projeyi çalıştırmak için aşağıdaki servislere ait API anahtarlarına ve hesaplara ihtiyacınız olacaktır:

n8n

OpenAI API Anahtarı: (Hem AI Agent hem de Embeddings için)

Supabase Hesabı: (Veritabanı URL'si ve API Anahtarı)

Cohere API Anahtarı: (Reranker modeli için)

Google Drive Kimlik Bilgileri: (PDF dosyalarını indirmek için)

<img width="1542" height="778" alt="Ekran görüntüsü 2025-10-22 182913" src="https://github.com/user-attachments/assets/c7c696df-d094-4ae7-9f6b-b1aa7d9e7ee9" />
<img width="666" height="895" alt="Ekran görüntüsü 2025-10-22 182936" src="https://github.com/user-attachments/assets/db9dc3b8-bb39-4fb6-bfcf-f332e47e6ab8" />
<img width="577" height="893" alt="Ekran görüntüsü 2025-10-22 182957" src="https://github.com/user-attachments/assets/01faa82e-e87c-47f9-98a6-86cb35e4fda6" />
