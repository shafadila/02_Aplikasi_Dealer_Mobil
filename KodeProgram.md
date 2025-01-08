# 02_Aplikasi_Dealer_Mobill

## Kelas: IF-11-05

## Anggota Kelompok 2:
- **Amanda Windhu Gustyas** (2311102121)
- **Liya Khoirunnisa** (2311102124)
- **Naya Putwi Setiasih** (2311102155)
- **Shafa Adila Santoso** (23111102158)

```go
package main

import (
	"fmt"
	"strings"
)

// Struct menyimpan data mobil
type Mobil struct {
	Nama          string  // Nama mobil
	Pabrikan      string  // Nama pabrikan mobil
	Tahun         int     // Tahun pembuatan mobil
	JumlahTerjual int     // Jumlah mobil yang terjual
	Harga         float64 // Harga mobil
	Warna         string  // Warna mobil
	Transmisi     string  // Jenis transmisi mobil (manual/otomatis)
	Kode          string  // Kode unik untuk identifikasi mobil
}

// Struct menyimpan data pabrikan
type Pabrikan struct {
	Nama  string
	Mobil [100]Mobil // Array mobil
	Kode  string     // Menambahkan atribut kode
}

var pabrikans [100]Pabrikan // Array pabrikan
var pabrikanCount int        // Menyimpan jumlah pabrikan yang ada

// Fungsi untuk mengurutkan pabrikan berdasarkan nama menggunakan Selection Sort
func sortPabrikanByName() {
	for i := 0; i < pabrikanCount-1; i++ {
		// Cari indeks elemen terkecil dari i ke pabrikanCount-1
		minIndex := i
		for j := i + 1; j < pabrikanCount; j++ {
			if pabrikans[j].Nama < pabrikans[minIndex].Nama {
				minIndex = j
			}
		}
		// Tukar elemen terkecil dengan elemen di posisi i
		if minIndex != i {
			pabrikans[i], pabrikans[minIndex] = pabrikans[minIndex], pabrikans[i]
		}
	}
}

// Fungsi untuk menambah pabrikan
func tambahPabrikan() {
	var nama string
	fmt.Println("+-------------------------------------------------+")
	fmt.Print("Masukkan nama pabrikan: ")
	fmt.Scan(&nama)

	// Validasi apakah pabrikan sudah ada
	for i := 0; i < pabrikanCount; i++ {
		if pabrikans[i].Nama == nama {
			fmt.Println("===================================================")
			fmt.Println("Pabrikan dengan nama tersebut sudah ada.")
			fmt.Println("===================================================")
			return
		}
	}

	// Membuat kode pabrikan
	kode := nama[:2] // Mengambil 2 huruf pertama dari nama pabrikan
	pabrikans[pabrikanCount] = Pabrikan{Nama: nama, Mobil: [100]Mobil{}, Kode: kode}
	pabrikanCount++
	fmt.Println("===================================================")
	fmt.Println("Pabrikan berhasil ditambahkan.")
	fmt.Println("===================================================")
	
	// Urutkan pabrikan berdasarkan nama secara ascending
	sortPabrikanByName()

}


// Fungsi untuk mencari pabrikan menggunakan Binary Search
func binarySearchPabrikan(nama string) (int, bool) {
	kiri, kanan := 0, pabrikanCount-1
	for kiri <= kanan {
		tengah := kiri + (kanan-kiri)/2 // Hitung indeks tengah
		if pabrikans[tengah].Nama == nama {
			return tengah, true // Jika ditemukan, kembalikan indeks dan status true
		} else if pabrikans[tengah].Nama < nama {
			kiri = tengah + 1 // Pencarian di bagian kanan
		} else {
			kanan = tengah - 1 // Pencarian di bagian kiri
		}
	}
	return -1, false // Jika tidak ditemukan, kembalikan -1 dan status false
}

// Fungsi untuk mengedit pabrikan
func editPabrikan() {
	var nama string
	fmt.Println("+-------------------------------------------------+")
	fmt.Print("Masukkan nama pabrikan yang ingin diedit: ")
	fmt.Scan(&nama)

	// Urutkan pabrikan sebelum melakukan pencarian
	sortPabrikanByName()

	// Cari pabrikan menggunakan Binary Search
	index, found := binarySearchPabrikan(nama)
	if found {
		fmt.Print("Masukkan nama baru pabrikan: ")
		var namaBaru string
		fmt.Scan(&namaBaru)

		// Validasi apakah nama baru sudah digunakan oleh pabrikan lain
		for i := 0; i < pabrikanCount; i++ {
			if pabrikans[i].Nama == namaBaru {
				fmt.Println("===================================================")
				fmt.Println("Pabrikan dengan nama tersebut sudah digunakan.")
				fmt.Println("===================================================")
				return
			}
		}

		// Update nama dan kode pabrikan jika valid
		pabrikans[index].Nama = namaBaru
		pabrikans[index].Kode = namaBaru[:2] // Update kode pabrikan
		fmt.Println("===================================================")
		fmt.Println("Pabrikan berhasil diedit.")
		fmt.Println("===================================================")
	} else {
		fmt.Println("===================================================")
		fmt.Println("Pabrikan tidak ditemukan.")
		fmt.Println("===================================================")
	}
}

// Fungsi untuk menghapus data pabrikan
func hapusPabrikan() {
	var nama string
	fmt.Println("+-------------------------------------------------+")
	fmt.Print("Masukkan nama pabrikan yang ingin dihapus: ")
	fmt.Scan(&nama)

	// Urutkan pabrikan sebelum melakukan pencarian
	sortPabrikanByName()

	// Cari pabrikan menggunakan Binary Search
	index, found := binarySearchPabrikan(nama)
	if found {
		var konfirmasi string
		fmt.Printf("Apakah Anda yakin ingin menghapus pabrikan '%s'? (y/t): ", nama)
		fmt.Scan(&konfirmasi)
		if konfirmasi == "y" || konfirmasi == "Y" {
			// Menggeser elemen ke kiri untuk menghapus pabrikan
			for i := index; i < pabrikanCount-1; i++ {
				pabrikans[i] = pabrikans[i+1]
			}
			pabrikanCount-- // Kurangi jumlah pabrikan
			fmt.Println("===================================================")
			fmt.Println("Pabrikan berhasil dihapus.")
			fmt.Println("===================================================")
		} else {
			fmt.Println("===================================================")
			fmt.Println("Penghapusan dibatalkan.")
			fmt.Println("===================================================")
		}
	} else {
		fmt.Println("===================================================")
		fmt.Println("Pabrikan tidak ditemukan.")
		fmt.Println("===================================================")
	}
}

// Fungsi untuk menambah data mobil
func tambahMobil() {
	var nama, pabrikan, warna, transmisi string
	var tahun, jumlahTerjual int
	var harga float64

	// Input data mobil
	fmt.Println("+-------------------------------------------------+")
	fmt.Print("Masukkan nama mobil                    : ")
	fmt.Scan(&nama)
	fmt.Print("Masukkan nama pabrikan                 : ")
	fmt.Scan(&pabrikan)
	fmt.Print("Masukkan tahun keluar                  : ")
	fmt.Scan(&tahun)
	fmt.Print("Masukkan warna mobil                   : ")
	fmt.Scan(&warna)
	fmt.Print("Masukkan jenis transmisi (Manual/Matic): ")
	fmt.Scan(&transmisi)
	fmt.Print("Masukkan harga mobil                   : ")
	fmt.Scan(&harga)
	fmt.Print("Masukkan jumlah terjual                : ")
	fmt.Scan(&jumlahTerjual)

	// Mencari pabrikan
	for i := 0; i < pabrikanCount; i++ {
		if pabrikans[i].Nama == pabrikan {
			// Membuat kode mobil dengan huruf kapital
			words := strings.Fields(nama)
			kode := strings.ToUpper(pabrikan[:2]) + strings.ToUpper(words[0][:4]) + strings.ToUpper(string(warna[0]))
			
			// Menambahkan mobil ke slot kosong di array Mobil
			for j := 0; j < 100; j++ {
				if pabrikans[i].Mobil[j].Nama == "" { // Cek slot kosong
					pabrikans[i].Mobil[j] = Mobil{
						Nama:          nama,
						Pabrikan:      pabrikan,
						Tahun:         tahun,
						Warna:         warna,
						Transmisi:     transmisi,
						Harga:         harga,
						JumlahTerjual: jumlahTerjual,
						Kode:          kode,
					}
					fmt.Println("===================================================")
					fmt.Println("Mobil berhasil ditambahkan.")
					fmt.Println("===================================================")
					return
				}
			}

			// Jika tidak ada slot kosong
			fmt.Println("===================================================")
			fmt.Println("Pabrikan sudah mencapai batas maksimum mobil.")
			fmt.Println("===================================================")
			return
		}
	}

	// Jika nama pabrikan tidak ditemukan
	fmt.Println("===================================================")
	fmt.Println("Pabrikan tidak ditemukan.")
	fmt.Println("===================================================")
}

// Fungsi untuk mencari mobil dengan sequential search
func sequentialSearchMobil(kode string) (int, int, bool) {
	// Looping pada setiap pabrikan dan mobil
	for i := 0; i < pabrikanCount; i++ {
		for j := 0; j < 100; j++ {
			// Jika kode mobil cocok
			if pabrikans[i].Mobil[j].Kode == kode {
				return i, j, true
			}
		}
	}
	// Jika tidak ditemukan
	return -1, -1, false
}

// Fungsi untuk mengedit data mobil
func editMobil() {
	var kode string
	fmt.Println("+-------------------------------------------------+")
	fmt.Print("Masukkan kode mobil yang ingin diedit: ")
	fmt.Scan(&kode)

	// Cari mobil berdasarkan kode
	i, j, found := sequentialSearchMobil(kode)
	if found {
		// Inputan data mobil baru
		fmt.Print("Masukkan nama mobil terbaru                    : ")
		var namaBaru string
		fmt.Scan(&namaBaru)
		fmt.Print("Masukkan tahun keluar terbaru                  : ")
		var tahunBaru int
		fmt.Scan(&tahunBaru)
		fmt.Print("Masukkan warna mobil terbaru                   : ")
		var warnaBaru string
		fmt.Scan(&warnaBaru)
		fmt.Print("Masukkan jenis transmisi (Manual/Matic) terbaru: ")
		var transmisiBaru string
		fmt.Scan(&transmisiBaru)
		fmt.Print("Masukkan harga terbaru mobil                   : ")
		var hargaBaru float64
		fmt.Scan(&hargaBaru)
		fmt.Print("Masukkan jumlah terjual terbaru                : ")
		var jumlahBaru int
		fmt.Scan(&jumlahBaru)

		// Update data mobil
		pabrikans[i].Mobil[j].Nama = namaBaru
		pabrikans[i].Mobil[j].Tahun = tahunBaru
		pabrikans[i].Mobil[j].Warna = warnaBaru
		pabrikans[i].Mobil[j].Transmisi = transmisiBaru
		pabrikans[i].Mobil[j].Harga = hargaBaru
		pabrikans[i].Mobil[j].JumlahTerjual = jumlahBaru

		// Update kode mobil dengan huruf kapital
		pabrikans[i].Mobil[j].Kode = strings.ToUpper(pabrikans[i].Kode + namaBaru[:2] + string(warnaBaru[0]))

		fmt.Println("===================================================")
		fmt.Println("Mobil berhasil diedit.")
		fmt.Println("===================================================")
	} else {
		fmt.Println("===================================================")
		fmt.Println("Mobil tidak ditemukan.")
		fmt.Println("===================================================")
	}
}

// Fungsi untuk menghapus data mobil
func hapusMobil() {
	var kode string
	fmt.Println("+-------------------------------------------------+")
	fmt.Print("Masukkan kode mobil yang ingin dihapus: ")
	fmt.Scan(&kode)

	// Cari mobil berdasarkan kode
	i, j, found := sequentialSearchMobil(kode)
	if found {
		// Hapus data mobil dengan mengosongkan slot
		pabrikans[i].Mobil[j] = Mobil{} // Mengosongkan slot mobil
		fmt.Println("===================================================")
		fmt.Println("Mobil berhasil dihapus.")
		fmt.Println("===================================================")
	} else {
		// Jika mobil tidak ditemukan
		fmt.Println("===================================================")
		fmt.Println("Mobil tidak ditemukan.")
		fmt.Println("===================================================")
	}
}

// Fungsi untuk mengurutkan mobil berdasarkan jumlah terjual menggunakan metode selection sort (descending)
func sortMobilByJumlahTerjualDescending(mobils [100]Mobil, count int) {
    // Iterasi melalui array mobil untuk menemukan elemen maksimum
    for i := 0; i < count-1; i++ {
        maxIdx := i // Inisialisasi indeks elemen maksimum
        for j := i + 1; j < count; j++ {
            if mobils[j].JumlahTerjual > mobils[maxIdx].JumlahTerjual { // Membandingkan jumlah terjual
                maxIdx = j // Update indeks elemen maksimum
            }
        }
        // Swap elemen jika elemen maksimum tidak berada di posisi yang benar
        if maxIdx != i {
            mobils[i], mobils[maxIdx] = mobils[maxIdx], mobils[i]
        }
    }
}

// Fungsi untuk mencari mobil berdasarkan nama pabrikan
func cariMobilBerdasarkanPabrikan() {
    var pabrikan string
    fmt.Println("+-------------------------------------------------+")
    fmt.Print("Masukkan nama pabrikan: ")
    fmt.Scan(&pabrikan) // Input nama pabrikan dari pengguna

    found := false // Menandai apakah pabrikan ditemukan atau tidak
    for i := 0; i < pabrikanCount; i++ {
        if pabrikans[i].Nama == pabrikan { // Memeriksa apakah pabrikan cocok
            // Mengurutkan mobil berdasarkan jumlah terjual (descending)
            sortMobilByJumlahTerjualDescending(pabrikans[i].Mobil, 100)

            // Menampilkan daftar mobil dari pabrikan yang ditemukan
            fmt.Println("+==============================================================================================+")
            fmt.Printf("|%58s %-35s|\n", "Daftar Mobil dari Pabrikan", pabrikan)
            fmt.Println("+==============================================================================================+")
            fmt.Printf("| %-9s | %-15s | %-5s | %-9s | %-10s | %-12s | %-14s |\n", "Kode", "Nama", "Tahun", "Warna", "Transmisi", "Harga", "Jumlah Terjual")
            fmt.Println("+----------------------------------------------------------------------------------------------+")
            for j := 0; j < 100; j++ {
                if pabrikans[i].Mobil[j].Nama != "" { // Hanya menampilkan mobil yang ada
                    m := pabrikans[i].Mobil[j]
                    // Format output untuk setiap mobil
                    fmt.Printf("| %-9s | %-15s | %-5d | %-9s | %-10s | Rp%-10.0f | %-14d |\n", m.Kode, m.Nama, m.Tahun, m.Warna, m.Transmisi, m.Harga, m.JumlahTerjual)
                }
            }
            fmt.Println("+----------------------------------------------------------------------------------------------+")
            found = true // Menandai bahwa pabrikan ditemukan
        }
    }
    if !found {
        // Pesan jika pabrikan tidak ditemukan
        fmt.Println("===================================================")
        fmt.Println("Pabrikan tidak ditemukan.")
        fmt.Println("===================================================")
    }
}

// Fungsi untuk menampilkan jumlah mobil pada suatu pabrikan
func tampilkanPabrikan() {
	// Memeriksa apakah ada pabrikan yang terdaftar
	if pabrikanCount == 0 {
		// Jika tidak ada pabrikan, tampilkan pesan
		fmt.Println("===================================================")
		fmt.Println("Pabrikan tidak ditemukan.")
		fmt.Println("===================================================")
		return
	}

	// Menampilkan header tabel
	fmt.Println("+===========================================+")
	fmt.Printf("|%30s %-12s|\n", "Daftar Pabrikan", "")
	fmt.Println("+===========================================+")
	fmt.Printf("| %-6s | %-15s | %-14s |\n", "Kode", "Pabrikan", "Jumlah Mobil")
	fmt.Println("+-------------------------------------------+")

	// Iterasi untuk setiap pabrikan yang ada
	for i := 0; i < pabrikanCount; i++ {
		p := pabrikans[i]
		count := 0

		// Menghitung jumlah mobil yang dimiliki pabrikan
		for j := 0; j < 100; j++ {
			if p.Mobil[j].Nama != "" { // Jika nama mobil tidak kosong, hitung sebagai mobil yang ada
				count++
			}
		}

		// Menampilkan informasi pabrikan dan jumlah mobil
		fmt.Printf("| %-6s | %-15s | %-14d |\n", strings.ToUpper(p.Kode), p.Nama, count)
	}

	// Menampilkan footer tabel
	fmt.Println("+===========================================+")
}

// Fungsi untuk mencari mobil berdasarkan nama mobil
func cariMobil() {
	var namaMobil string
	fmt.Println("+-------------------------------------------------+")
	fmt.Print("Masukkan nama mobil: ")
	fmt.Scan(&namaMobil) // Input nama mobil dari pengguna

	const maxMobil = 1000
	var mobilCocok [maxMobil]Mobil
	mobilCount := 0

	// Mencari mobil yang cocok dengan nama yang diberikan
	for i := 0; i < pabrikanCount; i++ {
		for j := 0; j < 100; j++ {
			// Mengecek kesamaan nama mobil tanpa memperhatikan kapitalisasi
			if strings.EqualFold(pabrikans[i].Mobil[j].Nama, namaMobil) && mobilCount < maxMobil {
				mobilCocok[mobilCount] = pabrikans[i].Mobil[j] // Menyimpan mobil yang cocok
				mobilCount++
			}
		}
	}

	// Jika tidak ada mobil yang cocok, tampilkan pesan
	if mobilCount == 0 {
		fmt.Println("===================================================")
		fmt.Println("Mobil tidak ditemukan.")
		fmt.Println("===================================================")
		return
	}

	// Mengurutkan hasil pencarian mobil berdasarkan tahun dan nama pabrikan menggunakan insertion sort
	for i := 1; i < mobilCount; i++ {
		key := mobilCocok[i]
		j := i - 1

		// Mengurutkan berdasarkan tahun dan jika sama, mengurutkan berdasarkan nama pabrikan
		for j >= 0 && (mobilCocok[j].Tahun > key.Tahun ||
			(mobilCocok[j].Tahun == key.Tahun && mobilCocok[j].Pabrikan > key.Pabrikan)) {
			mobilCocok[j+1] = mobilCocok[j] // Geser elemen jika lebih besar
			j-- // Lanjut ke elemen berikutnya
		}
		mobilCocok[j+1] = key // Menyisipkan elemen yang sedang diproses
	}

	// Menampilkan hasil pencarian dalam bentuk tabel
	fmt.Println("+==============================================================================================+")
	fmt.Printf("|%50s %-43s|\n", "Data Mobil", namaMobil)
	fmt.Println("+==============================================================================================+")
	fmt.Printf("| %-9s | %-15s | %-5s | %-9s | %-10s | %-12s | %-14s |\n", "Kode", "Nama", "Tahun", "Warna", "Transmisi", "Harga", "Jumlah Terjual")
	fmt.Println("+----------------------------------------------------------------------------------------------+")

	// Menampilkan detail mobil yang ditemukan
	for i := 0; i < mobilCount; i++ {
		m := mobilCocok[i]
		fmt.Printf("| %-9s | %-15s | %-5d | %-9s | %-10s | Rp%-10.0f | %-14d |\n",
			m.Kode, m.Nama, m.Tahun, m.Warna, m.Transmisi, m.Harga, m.JumlahTerjual)
	}

	// Menampilkan footer tabel
	fmt.Println("+==============================================================================================+")
}

// Fungsi untuk menampilkan 3 mobil terlaris
func tampilkanMobilTerlaris() {
	const maxMobil = 1000
	var mobilArray [maxMobil]Mobil
	mobilCount := 0

	// Mengumpulkan semua mobil yang ada ke dalam mobilArray
	for i := 0; i < pabrikanCount; i++ {
		for j := 0; j < 100; j++ {
			// Memastikan hanya mobil yang memiliki nama yang akan dimasukkan ke array
			if mobilCount < maxMobil && pabrikans[i].Mobil[j].Nama != "" {
				mobilArray[mobilCount] = pabrikans[i].Mobil[j]
				mobilCount++
			}
		}
	}

	// Jika tidak ada mobil yang terlaris, tampilkan pesan
	if mobilCount == 0 {
		fmt.Println("===================================================")
		fmt.Println("Tidak ada mobil yang terlaris.")
		fmt.Println("===================================================")
		return
	}

	// Melakukan Selection Sort (Descending) berdasarkan jumlah terjual untuk menemukan mobil terlaris
	for i := 0; i < mobilCount-1; i++ {
		maxIdx := i
		// Mencari mobil dengan jumlah terjual terbanyak
		for j := i + 1; j < mobilCount; j++ {
			if mobilArray[j].JumlahTerjual > mobilArray[maxIdx].JumlahTerjual {
				maxIdx = j
			}
		}
		// Menukar elemen jika ditemukan mobil dengan jumlah terjual lebih tinggi
		if maxIdx != i {
			mobilArray[i], mobilArray[maxIdx] = mobilArray[maxIdx], mobilArray[i]
		}
	}

	// Menampilkan header tabel untuk Daftar 3 Mobil Terlaris
	fmt.Println("+==========================================================================================+")
	fmt.Printf("|%57s %-32s|\n", "Daftar 3 Mobil Terlaris", "")
	fmt.Println("+==========================================================================================+")
	fmt.Printf("| %-3s | %-9s | %-15s | %-15s | %-5s | %-9s | %-14s |\n", "No", "Kode", "Nama", "Pabrikan", "Tahun", "Warna", "Jumlah Terjual")
	fmt.Println("+------------------------------------------------------------------------------------------+")

	// Menentukan batas jumlah mobil yang ditampilkan (3 mobil terlaris atau sesuai jumlah mobil yang ada)
	limit := 3
	if mobilCount < 3 {
		limit = mobilCount // Jika mobil yang tersedia kurang dari 3, tampilkan sebanyak yang ada
	}

	// Menampilkan 3 mobil terlaris dalam tabel
	for i := 0; i < limit; i++ {
		m := mobilArray[i]
		// Menampilkan informasi tentang mobil dengan format tabel
		fmt.Printf("| %-3d | %-9s | %-15s | %-15s | %-5d | %-9s | %-14d |\n", i+1, m.Kode, m.Nama, m.Pabrikan, m.Tahun, m.Warna, m.JumlahTerjual)
	}

	// Menampilkan footer tabel
	fmt.Println("+==========================================================================================+")
}


// Program utama
func main() {
	for {
		fmt.Println("\n+=================================================+")
		fmt.Println("|              Aplikasi Dealer Mobil              |")
		fmt.Println("+=================================================+")
		fmt.Println("| 1. Tambah Pabrikan                              |")
		fmt.Println("| 2. Edit Pabrikan                                |")
		fmt.Println("| 3. Hapus Pabrikan                               |")
		fmt.Println("| 4. Tambah Mobil                                 |")
		fmt.Println("| 5. Edit Mobil                                   |")
		fmt.Println("| 6. Hapus Mobil                                  |")
		fmt.Println("| 7. Cari Mobil Berdasarkan Pabrikan              |")
		fmt.Println("| 8. Tampilkan Pabrikan Mobil                     |")
		fmt.Println("| 9. Cari Nama Mobil                              |")
		fmt.Println("| 10. Tampilkan 3 Mobil Terlaris                  |")
		fmt.Println("| 11. Keluar                                      |")
		fmt.Println("+-------------------------------------------------+")
		fmt.Print("Pilih menu: ")

		var pilihan int
		fmt.Scan(&pilihan)

		switch pilihan {
		case 1:
			tambahPabrikan()
		case 2:
			editPabrikan()
		case 3:
			hapusPabrikan()
		case 4:
			tambahMobil()
		case 5:
			editMobil()
		case 6:
			hapusMobil()
		case 7:
			cariMobilBerdasarkanPabrikan()
		case 8:
			tampilkanPabrikan()
		case 9:
			cariMobil()
		case 10:
			tampilkanMobilTerlaris()
		case 11:
			fmt.Println("Terima kasih telah menggunakan aplikasi ini!")
			return
		default:
			fmt.Println("Pilihan tidak valid! Silakan coba lagi.")
		}
	}
}
```
