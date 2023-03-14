
# Install RabbitMq

## MENGGUNAKAN HOMEBREW
Silahkan mengikuti langkah ini untuk setup rabbitMQ 

Dalam tutorial ini, kita akan belajar menginstal RabbitMQ di Mac menggunakan Homebrew.

RabbitMQ adalah perangkat lunak broker pesan open source. Ini ringan dan mudah digunakan. Ini mendukung AMQP (Advanced Message Queuing Protocol), STOMP (Streaming Text Oriented Messaging Protocol), MQTT (Message Queuing Telemetry Transport) dan protokol lainnya.

Baiklah, mari instal RabbitMQ di Mac menggunakan Homebrew.

### Langkah Pertama 1: Install Homebrew
Homebrew adalah "Manajer paket yang hilang untuk macOS".

Menginstal aplikasi dan paket menggunakan Homebrew di Mac sangatlah mudah. Saya akan merekomendasikan Anda untuk menggunakan Homebrew jika Anda seorang pengembang dan menggunakan Mac untuk pekerjaan dev.

Oke, buka Terminal dan ketik perintah berikut.

```bash
/usr/bin/ruby -e "$(curl -fsSL https://raw.githubusercontent.com/Homebrew/install/master/install)"
```

Setelah Anda menginstal Homebrew di Mac Anda, ketikkan perintah berikut untuk memeriksa versinya.

```bash
brew -v
```

### Langkah Kedua 2: Install RabbitMQ using Homebrew

Setelah Anda menginstal Homebrew di Mac Anda, ketikkan perintah berikut untuk memeriksa versinya.

```bash
brew install rabbitmq
```

### Langkah Ketiga 3: Add to PATH

Server RabbitMQ dan skrip CLI diinstal di bawah /usr/local/sbin. Tambahkan ini ke PATH.

```bash
export PATH=$PATH:/usr/local/sbin
```

Di dalam file .bash_profile.
```bash
#HOMEBREW RABBITMQ
export HOMEBREW_RABBITMQ=/usr/local/Cellar/rabbitmq/3.7.11/sbin/
export PATH=$PATH:$HOMEBREW_RABBITMQ
```

### Langkah Keempat 4: Start RabbitMQ server
Untuk memulai RabbitMQ jalankan perintah berikut.
```angular2html
rabbitmq-server

  ##  ##
  ##  ##      RabbitMQ 3.7.11. Copyright (C) 2007-2019 Pivotal Software, Inc.
  ##########  Licensed under the MPL.  See http://www.rabbitmq.com/
  ######  ##
  ##########  Logs: /usr/local/var/log/rabbitmq/rabbit@localhost.log
                    /usr/local/var/log/rabbitmq/rabbit@localhost_upgrade.log

              Starting broker...
 completed with 6 plugins.
```

Langkah 5: Akses dasbor
Kita dapat mengakses dashboard web RabbitMQ dengan masuk ke http://localhost:15672 jadi, buka link tersebut di browser favorit Anda.

Nama pengguna dan kata sandi default masing-masing adalah tamu dan tamu.



# Integrasi ke Laravel
## Laravel Project Setup

Laravel adalah framework aplikasi web dengan sintaks yang ekspresif dan elegan. Kami percaya pengembangan harus menjadi pengalaman yang menyenangkan dan kreatif agar benar-benar memuaskan. Laravel menghilangkan rasa sakit dari pengembangan dengan mengurangi tugas-tugas umum yang digunakan di banyak proyek web, seperti:
- [Simple, fast routing engine](https://laravel.com/docs/routing).
- [Powerful dependency injection container](https://laravel.com/docs/container).
- Multiple back-ends for [session](https://laravel.com/docs/session) and [cache](https://laravel.com/docs/cache) storage.
- Expressive, intuitive [database ORM](https://laravel.com/docs/eloquent).
- Database agnostic [schema migrations](https://laravel.com/docs/migrations).
- [Robust background job processing](https://laravel.com/docs/queues).
- [Real-time event broadcasting](https://laravel.com/docs/broadcasting).

Laravel dapat diakses, kuat, dan menyediakan alat yang diperlukan untuk aplikasi yang besar dan kuat.
## Belajar Laravel

Laravel memiliki yang paling luas dan menyeluruh [documentation](https://laravel.com/docs)dan pustaka tutorial video dari semua kerangka kerja aplikasi web modern, membuatnya mudah untuk memulai dengan kerangka kerja.

Anda juga dapat mencoba [Laravel Bootcamp](https://bootcamp.laravel.com), di mana Anda akan dipandu untuk membangun aplikasi Laravel modern dari awal.

Jika Anda tidak ingin membaca, [Laracasts](https://laracasts.com) bisa membantu. Laracasts berisi lebih dari 2000 video tutorial tentang berbagai topik termasuk Laravel, PHP modern, pengujian unit, dan JavaScript. Tingkatkan keterampilan Anda dengan menggali perpustakaan video lengkap kami.

## Bagaimana Caranya?
Saya menganggap Anda telah menginstal dua proyek Laravel di lokal Anda. Untuk menggunakan RabbitMQ, kami akan menggunakan paket ini https://github.com/vyuldashev/laravel-queue-rabbitmq. Jadi mari instal paket di kedua proyek kami.

```bash
composer require vladimir-yuldashev/laravel-queue-rabbitmq
```

Kemudian, buka file config/queue.php Anda, lalu tambahkan koneksi kelinci baru.
```bash
'connections' => [

        'rabbitmq' => [

            'driver' => 'rabbitmq',
            'queue' => env('RABBITMQ_QUEUE', 'default'),
            'connection' => PhpAmqpLib\Connection\AMQPLazyConnection::class,

            'hosts' => [
                [
                    'host' => env('RABBITMQ_HOST', '127.0.0.1'),
                    'port' => env('RABBITMQ_PORT', 5672),
                    'user' => env('RABBITMQ_USER', 'guest'),
                    'password' => env('RABBITMQ_PASSWORD', 'guest'),
                    'vhost' => env('RABBITMQ_VHOST', '/'),
                ],
            ],

            'options' => [
                'ssl_options' => [
                    'cafile' => env('RABBITMQ_SSL_CAFILE', null),
                    'local_cert' => env('RABBITMQ_SSL_LOCALCERT', null),
                    'local_key' => env('RABBITMQ_SSL_LOCALKEY', null),
                    'verify_peer' => env('RABBITMQ_SSL_VERIFY_PEER', true),
                    'passphrase' => env('RABBITMQ_SSL_PASSPHRASE', null),
                ],
                'queue' => [
                    'job' => VladimirYuldashev\LaravelQueueRabbitMQ\Queue\Jobs\RabbitMQJob::class,
                    // 'job' => App\Jobs\CustomHandleJob::class,
                ],
            ],

            /*
             * Set to "horizon" if you wish to use Laravel Horizon.
             */
            'worker' => env('RABBITMQ_WORKER', 'default'),

        ],
      
 // ...
]
```

Hal terakhir yang harus dilakukan, kita perlu menambahkan beberapa variabel lingkungan baru. Anda bisa mendapatkan informasi .env dari langkah #1, dan Anda bisa menggunakan port 5672 sebagai port default. Dan jangan lupa untuk mengubah QUEUE_CONNECTION default Anda menjadi rabbitmq. Tentu saja, kami dapat mengubahnya kembali secara terprogram di dalam proyek kami jika diperlukan.

```bash
QUEUE_CONNECTION=rabbitmq
RABBITMQ_HOST=
RABBITMQ_PORT=5672
RABBITMQ_USER=
RABBITMQ_PASSWORD=
RABBITMQ_VHOST=
```

Pertama, kita akan membangun pekerjaan ping pertama kita. Pekerjaan ini tidak melakukan apa-apa, kami memerlukan ini untuk menguji koneksi antara penerbit dan penerima. Mari buat PingJob di kedua proyek. Tunggu apa? Mengapa kami membuat PingJob di kedua tempat? Saya akan menjelaskannya di bawah, Anda akan lebih memahami cara kerja paket ini setelah beberapa kali uji coba.

```bash
php artisan make:job PingJob
```

Buka App\Jobs\PingJob.php di Service 1 (Publisher) dan di dalamnya, tidak ada apa-apa. Pindah.

```bash
<?php

namespace App\Jobs;

use Illuminate\Bus\Queueable;
use Illuminate\Contracts\Queue\ShouldBeUnique;
use Illuminate\Contracts\Queue\ShouldQueue;
use Illuminate\Foundation\Bus\Dispatchable;
use Illuminate\Queue\InteractsWithQueue;
use Illuminate\Queue\SerializesModels;

class PingJob implements ShouldQueue
{
    use Dispatchable, InteractsWithQueue, Queueable, SerializesModels;

    /**
     * Execute the job.
     *
     * @return void
     */
    public function handle()
    {
        echo 'Ping Event Received' . PHP_EOL;
    }
}
```

Apa yang dilakukan PingJob pada Layanan 2 hanyalah mengulanginya jika ada kelas PingJob yang dikirim/dikirim. Tentu saja, kami akan mengirimkannya dari Layanan 1.

Mari kita Uji. Buka kedua proyek Anda, dan karena Layanan 2 adalah penerima, kami dapat mengetik ini di terminal untuk membuat proyek mendengarkan pesan/acara yang masuk.

Ini untuk monitoring antrian di rabbiMq silahkan jalankan perintah ini

```bash
php artisan rabbitmq:consume
```

Setelah Layanan 2 mendengarkan, lalu dari Layanan 1 (Penerbit), Anda dapat memublikasikan pesan dengan mengirim tugas.

```bash
PingJob::dispatch();
```

Karena kami tidak dapat mengirimkan PingJob dari baris perintah/terminal, kami perlu membuat perintah khusus yang akan mengirimkan PingJob. Jika Anda sudah tahu cara menggunakan tinker, Anda bisa menggunakannya.

```bash
<?php

namespace App\Console\Commands;

use App\Jobs\PingJob;
use Illuminate\Console\Command;

class PingJobCommand extends Command
{
    /**
     * The name and signature of the console command.
     *
     * @var string
     */
    protected $signature = 'ping:job';

    /**
     * The console command description.
     *
     * @var string
     */
    protected $description = 'Command description';

    /**
     * Create a new command instance.
     *
     * @return void
     */
    public function __construct()
    {
        parent::__construct();
    }

    /**
     * Execute the console command.
     *
     * @return int
     */
    public function handle()
    {
        PingJob::dispatch();
    }
}
```

Kemudian untuk mengirimkan PingJob, kita dapat menggunakan perintah yang baru saja kita buat.

```bash
php artisan ping:job
```
