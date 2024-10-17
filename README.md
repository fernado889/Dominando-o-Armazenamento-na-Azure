# Dominando-o-Armazenamento-na-Azure
<dependency>
    <groupId>com.azure</groupId>
    <artifactId>azure-storage-blob</artifactId>
    <version>12.20.0</version> <!-- Verifique se há uma versão mais recente -->
</dependency>
import com.azure.storage.blob.BlobClientBuilder;
import com.azure.storage.blob.BlobContainerClient;
import com.azure.storage.blob.BlobContainerClientBuilder;
import com.azure.storage.blob.models.BlobItem;

import java.io.File;
import java.io.FileInputStream;
import java.io.IOException;

public class AzureStorageExample {

    // Insira sua string de conexão aqui
    private static final String CONNECTION_STRING = "sua_string_de_conexao";

    public static void main(String[] args) {
        String containerName = "meu-container"; // Nome do seu container
        String blobName = "meuBlob.txt"; // Nome do blob
        String filePath = "caminho/do/seu/arquivo.txt"; // Caminho do arquivo local

        // Criação do container
        createContainer(containerName);

        // Upload do arquivo
        uploadBlob(containerName, blobName, filePath);

        // Download do arquivo
        downloadBlob(containerName, blobName, "caminho/do/diretorio/download/");

        // Listar blobs
        listBlobs(containerName);
    }

    private static void createContainer(String containerName) {
        BlobContainerClient containerClient = new BlobContainerClientBuilder()
                .connectionString(CONNECTION_STRING)
                .containerName(containerName)
                .buildClient();

        containerClient.createIfNotExists();
        System.out.println("Container criado: " + containerName);
    }

    private static void uploadBlob(String containerName, String blobName, String filePath) {
        BlobClientBuilder blobClientBuilder = new BlobClientBuilder()
                .connectionString(CONNECTION_STRING)
                .containerName(containerName)
                .blobName(blobName);

        try {
            blobClientBuilder.buildClient().upload(new FileInputStream(new File(filePath)), true);
            System.out.println("Upload bem-sucedido: " + blobName);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void downloadBlob(String containerName, String blobName, String downloadPath) {
        BlobClientBuilder blobClientBuilder = new BlobClientBuilder()
                .connectionString(CONNECTION_STRING)
                .containerName(containerName)
                .blobName(blobName);

        try {
            blobClientBuilder.buildClient().downloadToFile(downloadPath + blobName);
            System.out.println("Download bem-sucedido: " + blobName);
        } catch (IOException e) {
            e.printStackTrace();
        }
    }

    private static void listBlobs(String containerName) {
        BlobContainerClient containerClient = new BlobContainerClientBuilder()
                .connectionString(CONNECTION_STRING)
                .containerName(containerName)
                .buildClient();

        System.out.println("Lista de Blobs no container: " + containerName);
        for (BlobItem blobItem : containerClient.listBlobs()) {
            System.out.println("Blob: " + blobItem.getName());
        }
    }
}
