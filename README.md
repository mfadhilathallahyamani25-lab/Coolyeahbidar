/*
 * Tugas 1 - Sistem Bilangan (Soal No. 18)
 * Implementasi algoritma konversi bilangan (Java)
 *
 * Nama : M FADHIL ATHALLAH YAMANI
 * NIM  : 261420018
 */
import java.util.Scanner;

public class KonversiBilangan {

    static final String NAMA = "M FADHIL ATHALLAH YAMANI";
    static final String NIM = "261420018";
    static final String DIGIT = "0123456789ABCDEF";
    static final int PRESISI = 10; // jumlah maksimal digit pecahan

    // Basis apa pun (2, 8, 10, 16) -> desimal
    static double keDesimal(String teks, int basis) {
        teks = teks.trim().toUpperCase().replace(',', '.');
        String[] bagian = teks.split("\\.");
        double nilai = 0;

        for (char c : bagian[0].toCharArray()) {
            int d = DIGIT.indexOf(c);
            if (d < 0 || d >= basis)
                throw new NumberFormatException("Digit '" + c + "' tidak valid untuk basis " + basis);
            nilai = nilai * basis + d;
        }
        if (bagian.length > 1) {
            double pangkat = 1.0 / basis;
            for (char c : bagian[1].toCharArray()) {
                int d = DIGIT.indexOf(c);
                if (d < 0 || d >= basis)
                    throw new NumberFormatException("Digit '" + c + "' tidak valid untuk basis " + basis);
                nilai += d * pangkat;
                pangkat /= basis;
            }
        }
        return nilai;
    }

    // Desimal -> basis 2, 8, atau 16
    static String dariDesimal(double angka, int basis) {
        long bulat = (long) angka;
        double pecah = angka - bulat;
        StringBuilder sb = new StringBuilder();

        // Bagian bulat: bagi berulang, baca sisa dari bawah ke atas
        if (bulat == 0) sb.append('0');
        while (bulat > 0) {
            sb.insert(0, DIGIT.charAt((int) (bulat % basis)));
            bulat /= basis;
        }

        // Bagian pecahan: kali berulang, ambil bagian bulat dari atas ke bawah
        if (pecah > 1e-12) {
            sb.append(',');
            int count = 0;
            while (pecah > 1e-12 && count < PRESISI) {
                pecah *= basis;
                int d = (int) pecah;
                sb.append(DIGIT.charAt(d));
                pecah -= d;
                count++;
            }
        }
        return sb.toString();
    }

    static String namaBasis(int b) {
        switch (b) {
            case 2:  return "Biner";
            case 8:  return "Oktal";
            case 10: return "Desimal";
            default: return "Heksa";
        }
    }

    static void prosesKonversi(int asal, Scanner in) {
        System.out.print("Masukkan bilangan " + namaBasis(asal) + " (pecahan boleh pakai koma/titik): ");
        String input = in.nextLine();
        try {
            double desimal = keDesimal(input, asal);
            System.out.println("\nHasil konversi " + input.trim().toUpperCase() + " (" + namaBasis(asal) + "):");
            int[] semua = {10, 2, 8, 16};
            for (int tujuan : semua) {
                if (tujuan == asal) continue;
                System.out.printf("  %-8s : %s%n", namaBasis(tujuan), dariDesimal(desimal, tujuan));
            }
        } catch (NumberFormatException e) {
            System.out.println("Input salah: " + e.getMessage());
        }
    }

    public static void main(String[] args) {
        Scanner in = new Scanner(System.in);
        boolean jalan = true;

        while (jalan) {
            System.out.println("\n==============================================");
            System.out.println("      PROGRAM KONVERSI SISTEM BILANGAN");
            System.out.println("  Nama : " + NAMA);
            System.out.println("  NIM  : " + NIM);
            System.out.println("==============================================");
            System.out.println("1. Desimal -> Biner, Oktal, Heksa");
            System.out.println("2. Biner   -> Desimal, Oktal, Heksa");
            System.out.println("3. Oktal   -> Desimal, Biner, Heksa");
            System.out.println("4. Heksa   -> Desimal, Biner, Oktal");
            System.out.println("0. Keluar");
            System.out.print("Pilih menu: ");
            String pilih = in.nextLine().trim();

            switch (pilih) {
                case "1": prosesKonversi(10, in); break;
                case "2": prosesKonversi(2, in);  break;
                case "3": prosesKonversi(8, in);  break;
                case "4": prosesKonversi(16, in); break;
                case "0": jalan = false; System.out.println("Terima kasih. - " + NAMA + " (" + NIM + ")"); break;
                default:  System.out.println("Menu tidak valid.");
            }
        }
        in.close();
    }
}
