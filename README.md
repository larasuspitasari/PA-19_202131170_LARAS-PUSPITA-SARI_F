
# TEORI SEGMENTASI WARNA
Teori yang mendukung segmentasi warna dalam program yang telah saya buat adalah sebagai berikut:

1. Ruang Warna HSV:
   Segmentasi warna dalam codingan menggunakan konversi gambar dari RGB ke HSV. Ruang warna HSV (Hue, Saturation, Value) adalah model warna yang memisahkan informasi warna (hue), kejenuhan (saturation), dan nilai (value) dalam tiga komponen terpisah. Konversi ke ruang warna HSV sangat berguna dalam segmentasi warna karena memungkinkan pengaturan range nilai untuk komponen hue, saturation, dan value, yang dapat mengisolasi warna tertentu dalam gambar.

2. Thresholding:
   Setelah gambar dikonversi ke ruang warna HSV, dilakukan thresholding untuk mengubah gambar menjadi citra biner. Thresholding adalah teknik yang digunakan untuk membagi piksel dalam gambar menjadi dua kelompok berdasarkan nilai ambang tertentu. Dalam codingan ini, nilai ambang ditentukan oleh range nilai warna yang diinginkan untuk setiap jenis buah. Misalnya, dalam kode tersebut, digunakan fungsi `cv2.inRange()` untuk membuat mask yang mempertahankan piksel yang berada dalam range warna yang ditentukan.

3. Masking:
   Setelah thresholding, mask digunakan untuk menerapkan operasi bitwise AND pada gambar asli dan mask. Operasi bitwise AND akan menghasilkan gambar baru yang hanya menampilkan piksel yang ada pada mask. Dalam codingan ini, menggunakan fungsi `cv2.bitwise_and()` untuk menerapkan mask pada gambar asli, sehingga hanya menampilkan warna yang masuk dalam range yang telah ditentukan.

4. Plotting dan Visualisasi:
   Setelah proses segmentasi warna, menggunakan library matplotlib untuk membuat plot dan memvisualisasikan gambar-gambar hasil segmentasi. Dalam codingan ini, menggunakan fungsi `plt.subplots()` untuk membuat grid subplot, kemudian menggunakan `axs.imshow()` untuk menampilkan gambar-gambar hasil segmentasi. Kontur juga ditambahkan pada gambar hasil segmentasi menggunakan fungsi `axs.contour()` dengan menggunakan mask sebagai parameter. Informasi ukuran gambar ditampilkan dengan menggunakan fungsi `axs.text()`.

Dengan menggunakan konsep dan teknik di atas, codingan tersebut dapat melakukan segmentasi warna pada gambar berdasarkan range nilai warna yang ditentukan, serta menampilkan gambar-gambar hasil segmentasi dengan menggunakan matplotlib. Pendekatan ini memungkinkan untuk mengidentifikasi dan memisahkan objek atau area tertentu dalam gambar berdasarkan warna yang spesifik.

# CODINGAN DAN PENJELASAN

      import cv2
      import numpy as np
      import matplotlib.pyplot as plt
      pict = cv2.imread('buah.jpg')
      pict1 = cv2.cvtColor(pict, cv2.COLOR_BGR2GRAY)
      edges = cv2.Canny(pict, 100, 150) 
      import cv2
      import numpy as np
      import matplotlib.pyplot as plt

      def separate_fruits(image_path):
         image = cv2.imread(image_path)
         image = cv2.cvtColor(image, cv2.COLOR_BGR2RGB)

         hsv_image = cv2.cvtColor(image, cv2.COLOR_RGB2HSV)

         lower_green = np.array([35, 50, 50], dtype=np.uint8)
         upper_green = np.array([90, 255, 255], dtype=np.uint8)

         lower_red = np.array([0, 70, 70], dtype=np.uint8)
         upper_red = np.array([10, 255, 255], dtype=np.uint8)

         lower_yellow = np.array([20, 100, 100], dtype=np.uint8)
         upper_yellow = np.array([30, 255, 255], dtype=np.uint8)

         mask_red = cv2.inRange(hsv_image, lower_red, upper_red)
         mask_green = cv2.inRange(hsv_image, lower_green, upper_green)
         mask_yellow = cv2.inRange(hsv_image, lower_yellow, upper_yellow)

         result_red = cv2.bitwise_and(image, image, mask=mask_red)
         result_green = cv2.bitwise_and(image, image, mask=mask_green)
         result_yellow = cv2.bitwise_and(image, image, mask=mask_yellow)

         fig, axs = plt.subplots(1, 4, figsize=(15, 5))

         axs = axs.ravel()

         axs[0].imshow(image)
         axs[0].set_title('Original Image')
         axs[0].axis('on')
         axs[0].text(10, 10, f"({image.shape[1]}, {image.shape[0]})", color='white')

         axs[1].imshow(result_red)
         axs[1].contour(mask_red)
         axs[1].set_title('Red Fruits')
         axs[1].axis('on')
         axs[1].text(10, 10, f"({result_red.shape[1]}, {result_red.shape[0]})", color='white')

         axs[2].imshow(result_green)
         axs[2].contour(mask_green)
         axs[2].set_title('Green Fruits')
         axs[2].axis('on')
         axs[2].text(10, 10, f"({result_green.shape[1]}, {result_green.shape[0]})", color='white')

         axs[3].imshow(result_yellow)
         axs[3].contour(mask_yellow)
         axs[3].set_title('Yellow Fruits')
         axs[3].axis('on')
         axs[3].text(10, 10, f"({result_yellow.shape[1]}, {result_yellow.shape[0]})", color='white')

         # Rotate the images
         for ax in axs[1:]:
            ax.set_adjustable('box')
            ax.set_aspect('equal')
            ax.get_xaxis().set_visible(True)
            ax.get_yaxis().set_visible(True)
            ax.set_frame_on(True)
            ax.yaxis.set_label_coords(0.5, -0.1)
            ax.set_xticks([])
            ax.set_yticks([])
            ax.spines['top'].set_visible(True)
            ax.spines['right'].set_visible(True)
            ax.spines['bottom'].set_visible(True)
            ax.spines['left'].set_visible(True)
            ax.yaxis.set_ticklabels([])
            ax.xaxis.set_ticklabels([])
            ax.text(10, 10, f"({ax.get_xlim()[1]}, {ax.get_ylim()[1]})", color='white')

            ax.set_aspect('auto')

         plt.tight_layout()
         plt.show()


      image_path = "LARAS.jpg"

      separate_fruits(image_path)

Langkah-langkah penyelesaian codingan di atas adalah sebagai berikut:

1. Membaca Gambar: 
   Membaca gambar menggunakan fungsi `cv2.imread(image_path)` dan menyimpannya dalam variabel `image`.

2. Konversi Warna:
   Mengubah format warna gambar dari BGR ke RGB menggunakan fungsi `cv2.cvtColor(image, cv2.COLOR_BGR2RGB)`. Hal ini diperlukan agar warna yang ditampilkan sesuai dengan urutan RGB yang umum digunakan dalam library matplotlib.

3. Konversi ke HSV:
   Mengubah gambar dari format RGB ke HSV menggunakan fungsi `cv2.cvtColor(image, cv2.COLOR_RGB2HSV)`. Konversi ini dilakukan untuk memudahkan dalam melakukan segmentasi warna berdasarkan ruang warna HSV.

4. Segmentasi Warna:
   Menentukan range nilai warna untuk masing-masing buah yang ingin disegmentasi. Dalam codingan ini, dilakukan segmentasi untuk warna merah, hijau, dan kuning. Range warna ditentukan dengan mengatur nilai `lower_green`, `upper_green`, `lower_red`, `upper_red`, `lower_yellow`, dan `upper_yellow` menggunakan fungsi `np.array()`.

5. Masking:
   Menerapkan mask pada gambar asli menggunakan operasi bitwise AND (`cv2.bitwise_and()`) dengan menggunakan mask yang telah ditentukan untuk masing-masing warna. Hasilnya adalah gambar yang hanya menampilkan warna yang masuk ke dalam range yang telah ditentukan.

6. Menampilkan Gambar:
   Menggunakan library matplotlib untuk menampilkan gambar-gambar yang telah di-segmentasi warnanya. Digunakan fungsi `plt.subplots()` untuk membuat subplots dengan ukuran 1x4 (satu baris dan empat kolom), kemudian menggunakan `axs.imshow()` untuk menampilkan gambar-gambar hasil segmentasi dan gambar asli. Untuk gambar hasil segmentasi, juga ditambahkan kontur menggunakan fungsi `axs.contour()` dengan menggunakan mask sebagai parameter kontur. Selain itu, juga ditampilkan informasi ukuran gambar dengan menggunakan fungsi `axs.text()`.

7. Penyesuaian Tampilan:
   Melakukan penyesuaian tampilan seperti mengatur aspek rasio, label sumbu, dan menghilangkan garis tepi pada plot menggunakan berbagai fungsi dan metode matplotlib.

8. Menampilkan Gambar:
   Menampilkan semua gambar yang telah ditampilkan ke dalam jendela tampilan menggunakan fungsi `plt.show()`.

Dengan demikian, langkah-langkah di atas menjelaskan bagaimana codingan tersebut membaca gambar, melakukan segmentasi warna, dan menampilkan gambar-gambar hasil segmentasi dengan menggunakan library OpenCV dan matplotlib.

# JOURNAL TERKAIT
Ini adalah journal terkait yang berkaitan dengan project UAS saya https://github.com/larasuspitasari/PA-19_202131170_LARAS-PUSPITA-SARI_F/blob/main/PCD.pdf