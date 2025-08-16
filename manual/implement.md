# เอกสารขั้นตอนการสร้างระบบจัดการสินค้า (CRUD)

เอกสารนี้สรุปขั้นตอนการพัฒนาระบบจัดการสินค้า (Product) ด้วย Laravel Framework ตั้งแต่เริ่มต้นจนสามารถใช้งานฟังก์ชันพื้นฐาน CRUD (Create, Read, Update, Delete) ได้

## 1. สร้าง Model และ Migration
เริ่มต้นด้วยการใช้ Artisan command เพื่อสร้าง Model `Product` พร้อมกับไฟล์ Migration สำหรับสร้างตารางในฐานข้อมูล

```bash
php artisan make:model Product -m
```

## 2. แก้ไขไฟล์ Migration
เพิ่มคอลัมน์ที่ต้องการ (`name` และ `price`) ในไฟล์ Migration ที่เพิ่งถูกสร้างขึ้นใน `database/migrations/`

```php
// ในไฟล์ ..._create_products_table.php
use Illuminate\Database\Migrations\Migration;
use Illuminate\Database\Schema\Blueprint;
use Illuminate\Support\Facades\Schema;

return new class extends Migration
{
    public function up(): void
    {
        Schema::create('products', function (Blueprint $table) {
            $table->id();
            $table->string('name');
            $table->decimal('price', 8, 2);
            $table->timestamps();
        });
    }

    public function down(): void
    {
        Schema::dropIfExists('products');
    }
};
```

## 3. รัน Migration
ใช้คำสั่ง `migrate` เพื่อสร้างตาราง `products` ลงในฐานข้อมูลตามโครงสร้างที่กำหนด

```bash
php artisan migrate
```

## 4. สร้าง Seeder (ข้อมูลจำลอง)
สร้าง Seeder เพื่อเพิ่มข้อมูลสินค้าตัวอย่างสำหรับใช้ในการทดสอบ

```bash
php artisan make:seeder ProductSeeder
```
จากนั้น เพิ่มโค้ดสำหรับสร้างข้อมูลลงในไฟล์ `database/seeders/ProductSeeder.php`

```php
// database/seeders/ProductSeeder.php
use Illuminate\Database\Seeder;
use App\Models\Product;

class ProductSeeder extends Seeder
{
    public function run(): void
    {
        Product::create(['name' => 'Product 1', 'price' => 10.99]);
        Product::create(['name' => 'Product 2', 'price' => 20.49]);
        Product::create(['name' => 'Product 3', 'price' => 5.99]);
    }
}
```
และสั่งรัน Seeder ด้วยคำสั่ง:
```bash
php artisan db:seed --class=ProductSeeder
```

## 5. กำหนด Mass Assignment ใน Model
ใน Model `app/Models/Product.php` กำหนด property `$fillable` เพื่ออนุญาตให้สามารถบันทึกข้อมูล `name` และ `price` ผ่าน Mass Assignment ได้

```php
// app/Models/Product.php
protected $fillable = ['name', 'price'];
```

## 6. สร้าง Controller
สร้าง `ProductController` แบบ resource เพื่อจัดการ request ที่เกี่ยวข้องกับการทำ CRUD ทั้งหมด

```bash
php artisan make:controller ProductController --resource
```

## 7. กำหนด Routes
ในไฟล์ `routes/web.php` เพิ่ม resource route เพื่อให้ Laravel สร้าง routes ที่จำเป็นสำหรับ `ProductController` โดยอัตโนมัติ

```php
// routes/web.php
use App\Http\Controllers\ProductController;

Route::resource('products', ProductController::class);
```

## 8. Implement Logic ใน Controller
ใส่ Logic การทำงานของแต่ละฟังก์ชัน (index, create, store, show, edit, update, destroy) ลงใน `app/Http/Controllers/ProductController.php` พร้อมทั้งแก้ไขข้อผิดพลาด `Undefined variable $i` ที่เกิดขึ้น

```php
// app/Http/Controllers/ProductController.php
namespace App\Http\Controllers;

use App\Models\Product;
use Illuminate\Http\Request;

class ProductController extends Controller
{
    public function index()
    {
        $products = Product::all();
        return view('products.index', compact('products'))
            ->with('i', (request()->input('page', 1) - 1) * 5);
    }

    // ... (โค้ดสำหรับ create, store, show, edit, update, destroy)
}
```

## 9. สร้าง Views (Blade Templates)
สร้างไฟล์ Views ที่จำเป็นสำหรับแต่ละหน้าในโฟลเดอร์ `resources/views/products/`:
- `index.blade.php`
- `create.blade.php`
- `edit.blade.php`
- `show.blade.php`

และสร้างไฟล์ Layout กลาง `resources/views/layouts/app.blade.php` เพื่อให้ทุกหน้ามีโครงสร้างเว็บที่เหมือนกัน

## 10. การรันโปรเจกต์
ใช้คำสั่ง Artisan เพื่อรัน development server:

```bash
php artisan serve
```
จากนั้นสามารถเข้าถึงระบบผ่านเบราว์เซอร์ได้ที่ URL: `http://127.0.0.1:8000/products`
