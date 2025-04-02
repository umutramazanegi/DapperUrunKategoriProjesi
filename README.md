# DapperNorthwind 📊

Bu proje, .NET Framework üzerinde C# ve Windows Forms kullanılarak geliştirilmiş, **Dapper** micro-ORM'u ile Microsoft'un örnek **Northwind** veritabanı üzerinde temel CRUD (Create, Read, Update, Delete) işlemleri ve bazı istatistiksel sorguların nasıl yapılabileceğini gösteren bir uygulamadır.

## 🚀 Genel Bakış

Uygulama, Dapper'ın ADO.NET üzerine getirdiği kolaylıkları ve performans avantajlarını sergilemeyi amaçlar. Kategoriler üzerinde tam CRUD işlevselliği sunar ve form yüklendiğinde çeşitli veritabanı istatistiklerini asenkron olarak hesaplayıp gösterir. Proje ayrıca DTO (Data Transfer Object) ve Repository pattern (kısmen uygulanmış) gibi yapıları içerir.

## ✨ Özellikler

*   **Dapper Kullanımı:** Veritabanı işlemleri için Dapper micro-ORM'u kullanılır (`QueryAsync`, `ExecuteAsync`, `ExecuteScalarAsync`).
*   **Asenkron İşlemler:** Veritabanı sorguları `async/await` kullanılarak asenkron olarak çalıştırılır, bu da kullanıcı arayüzünün yanıt vermeye devam etmesini sağlar.
*   **İstatistiksel Sorgular (`Form1_Load`):**
    *   Toplam Kategori Sayısı (`ExecuteScalarAsync`)
    *   Toplam Ürün Sayısı (`ExecuteScalarAsync`)
    *   Ortalama Ürün Stok Sayısı (`ExecuteScalarAsync`)
    *   Belirli Bir Kategorideki (Deniz Ürünleri) Ürünlerin Toplam Fiyatı (`ExecuteScalarAsync` ve alt sorgu)
*   **Kategori Yönetimi (`Form1`):**
    *   Kategorileri Listeleme (`QueryAsync` ve `ResultCategoryDto`).
    *   Yeni Kategori Ekleme (`ExecuteAsync` ve `DynamicParameters`).
    *   Kategori Silme (`ExecuteAsync` ve `DynamicParameters`).
    *   Kategori Güncelleme (`ExecuteAsync` ve `DynamicParameters`).
*   **DTO (Data Transfer Objects):** Veri transferi için `Dtos` klasörü altında DTO sınıfları tanımlanmıştır (`ResultCategoryDto`, `CreateCategoryDto` vb.).
*   **Repository Pattern (Yapısal):** `Repositories` klasörü altında Kategori işlemleri için bir Repository arayüzü (`ICategoryRepository`) ve somut sınıfı (`CategoryRepository`) bulunur. (*Not: `Form1.cs` içindeki mevcut kodlar doğrudan `SqlConnection` ve Dapper metotlarını kullanmaktadır, Repository tam olarak entegre edilmemiştir.*)

## 🛠️ Kullanılan Teknolojiler

*   **Programlama Dili:** C#
*   **Framework:** .NET Framework 4.7.2
*   **Arayüz:** Windows Forms (WinForms)
*   **Veri Erişimi:** Dapper micro-ORM, ADO.NET (`System.Data.SqlClient`)
*   **Veritabanı:** Microsoft SQL Server (Northwind şeması)

## 💾 Veritabanı Kurulumu

Uygulamanın çalışması için Northwind şemasına sahip bir SQL Server veritabanına ihtiyaç vardır.

1.  **Veritabanı Oluşturma:**
    *   SQL Server Management Studio (SSMS) veya başka bir araç kullanarak `DapperNorthwindDb` adında **boş bir veritabanı** oluşturun.
2.  **Northwind Şemasını Yükleme:**
    *   Proje içerisindeki `NorthwindScriptsFolder/NorthwindScripts.txt` dosyasında bulunan SQL script'ini kopyalayın.
    *   SSMS'te yeni oluşturduğunuz `DapperNorthwindDb` veritabanını seçin ve yeni bir sorgu (New Query) penceresi açın.
    *   Kopyaladığınız script'i bu pencereye yapıştırın ve çalıştırın (Execute veya F5). Bu script, gerekli tabloları (Categories, Products vb.), view'ları ve stored procedure'leri oluşturacaktır.
3.  **Bağlantı Dizesini Ayarlama:**
    *   `DapperNorthwind` projesindeki `Form1.cs` dosyasını açın.
    *   En üstteki `SqlConnection connection = ...` satırını bulun.
    *   `Server=UMUT\SQLEXPRESS` kısmını kendi SQL Server sunucu adınızla değiştirin (örn: `.` , `(localdb)\mssqllocaldb`, `YOUR_PC_NAME\SQLEXPRESS` vb.).
    *   `initial catalog=DapperNorthwindDb` kısmının, 1. adımda oluşturduğunuz veritabanı adıyla eşleştiğinden emin olun.
    *   Eğer SQL Server'ınız Windows Authentication (integrated security=true) kullanmıyorsa, bağlantı dizesini SQL Server Authentication'a göre düzenlemeniz gerekir (User ID=...;Password=...;).

    ```csharp
    // Form1.cs içindeki satırı güncelleyin:
    SqlConnection connection = new SqlConnection("Server=YOUR_SERVER_NAME;initial catalog=DapperNorthwindDb;integrated security=true");
    ```

## 🏃 Nasıl Çalıştırılır?

1.  **Gereksinimler:**
    *   Visual Studio 2019 veya üzeri (.NET Framework 4.7.2 desteği ile)
    *   Microsoft SQL Server (Express, Developer veya başka bir sürüm)
2.  **Projeyi Klonlama:**
    ```bash
    git clone https://github.com/kullanici-adiniz/DapperNorthwind.git
    ```
    *(kullanici-adiniz kısmını kendi GitHub kullanıcı adınızla değiştirin)*
3.  Yukarıdaki "Veritabanı Kurulumu" adımlarını takip ederek veritabanını hazırlayın.
4.  Gerekirse `Form1.cs` dosyasındaki bağlantı dizesini güncelleyin.
5.  `DapperNorthwind.sln` dosyasını Visual Studio ile açın.
6.  NuGet Paket Yöneticisi'nin Dapper paketini geri yüklediğinden emin olun (genellikle otomatik yapılır). Gerekirse `Update-Package -reinstall Dapper` komutunu Paket Yöneticisi Konsolu'nda çalıştırın.
7.  Projeyi derleyin (Build -> Build Solution).
8.  Uygulamayı başlatın (Debug -> Start Debugging veya F5). `Form1` açılacak ve istatistikler görüntülenecektir. Kategori işlemleri butonlarla yapılabilir.

## 🏗️ Proje Yapısı

*   **`/Dtos`**: Data Transfer Objects. Veritabanı ve uygulama katmanları arasında veri taşımak için kullanılan sınıflar.
*   **`/Repositories`**: Veritabanı işlemlerini soyutlamak için kullanılan Repository pattern arayüzleri ve sınıfları. (Mevcut formda doğrudan kullanılmasa da yapısal olarak mevcuttur).
*   **`/NorthwindScriptsFolder`**: Veritabanını oluşturmak için gerekli SQL script'ini içerir.
*   **`Form1.cs`**: Ana form. İstatistikleri gösterir ve Kategori CRUD işlemlerini gerçekleştirir.

Bu README, projenizin Dapper kullanarak temel veritabanı işlemlerini ve basit istatistik sorgularını nasıl gerçekleştirdiğini anlamanıza yardımcı olacaktır.
