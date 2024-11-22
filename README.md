package primeiro_programa;

import javax.crypto.Cipher;
import javax.crypto.spec.SecretKeySpec;
import java.util.Base64;
import java.util.Scanner;

public class CriptografiaSimples {

    // Chave de 16 caracteres (16 bytes)
    private static final String CHAVE_SECRETA = "chave16byteslong";

    public static String criptografar(String mensagem) throws Exception {
        SecretKeySpec chave = new SecretKeySpec(CHAVE_SECRETA.getBytes(), "AES");
        Cipher cifra = Cipher.getInstance("AES");
        cifra.init(Cipher.ENCRYPT_MODE, chave);

        byte[] mensagemCriptografada = cifra.doFinal(mensagem.getBytes());
        return Base64.getEncoder().encodeToString(mensagemCriptografada);
    }

    public static String descriptografar(String mensagemCriptografada) throws Exception {
        SecretKeySpec chave = new SecretKeySpec(CHAVE_SECRETA.getBytes(), "AES");
        Cipher cifra = Cipher.getInstance("AES");
        cifra.init(Cipher.DECRYPT_MODE, chave);

        byte[] mensagemDescriptografada = cifra.doFinal(Base64.getDecoder().decode(mensagemCriptografada));
        return new String(mensagemDescriptografada);
    }

    public static void main(String[] args) {
        Scanner scanner = new Scanner(System.in);

        try {
            System.out.println("Digite a mensagem que você quer criptografar:");
            String mensagem = scanner.nextLine();

            String mensagemCriptografada = criptografar(mensagem);
            System.out.println("Mensagem criptografada: " + mensagemCriptografada);

            System.out.println("Agora, digite a mensagem criptografada para descriptografar:");
            String entradaCriptografada = scanner.nextLine();

            String mensagemOriginal = descriptografar(entradaCriptografada);
            System.out.println("Mensagem original: " + mensagemOriginal);

        } catch (Exception e) {
            System.out.println("Ocorreu um erro durante o processo: " + e.getMessage());
            e.printStackTrace();
        } finally {
            scanner.close();
        }
    }
}
