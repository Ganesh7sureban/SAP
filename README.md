
# Concurrent File Downloader (Java)

A Java-based **concurrent file downloader** designed to download multiple files in parallel, split large files into smaller parts for concurrent downloading, and verify file integrity using SHA-256 checksums.

Built using the **Java 17+ standard HTTP Client API**, the project explores multithreading, concurrent I/O, HTTP communication, and reliable file handling without requiring third-party downloading libraries.

## 🚀 Features

- **Concurrent Downloads:** Download multiple files simultaneously to improve download efficiency.
- **Chunk-Based Downloading:** Split large files into smaller parts for parallel downloading.
- **Integrity Verification:** Validate downloaded files using SHA-256 checksums.
- **Modern HTTP Client:** Use Java 17+ HTTP Client APIs for HTTP communication.
- **Checksum Generation:** Generate SHA-256 hashes for files using the `FileHasher` utility.
- **Configurable Downloads:** Associate file URLs with expected SHA-256 checksums.
- **Lightweight Implementation:** Use Java's built-in networking and concurrency capabilities without third-party download libraries.

## 🛠️ Tech Stack

| Technology | Purpose |
|---|---|
| Java 17+ | Core implementation |
| Java HTTP Client | HTTP requests and file downloading |
| Java Concurrency | Parallel download execution |
| SHA-256 | File integrity verification |
| Maven / Gradle | Test and build management, if configured |

## 🏗️ How It Works

1. **Configure file URLs:** Specify the files to download and their expected SHA-256 checksums.
2. **Generate checksums:** Use `FileHasher` to calculate the SHA-256 hash of each source file before downloading.
3. **Start concurrent downloads:** Download multiple files simultaneously.
4. **Split large files:** Divide supported large files into parts for parallel downloading.
5. **Combine downloaded parts:** Assemble the parts into the completed file, where supported by the implementation.
6. **Verify integrity:** Calculate the downloaded file's SHA-256 hash and compare it with the expected checksum.
7. **Validate the result:** Treat a checksum mismatch as an integrity verification failure.

## 📋 Prerequisites

- Java Development Kit (JDK) 17 or newer
- An HTTP server hosting the files to download
- Maven or Gradle, if required by the project's test setup

## ⚙️ Getting Started

### 1. Clone the repository

```bash
git clone https://github.com/Yash0330/SAP.git
```

### 2. Navigate to the project directory

```bash
cd SAP
```

### 3. Prepare files for downloading

Host the files on an HTTP server accessible to your application. For local testing, you can use a local HTTP server.

Ensure the server supports the HTTP features required by the downloader, particularly byte-range requests if the implementation downloads different parts of a file concurrently.

### 4. Generate a SHA-256 checksum

Compile the `FileHasher` utility:

```bash
javac -d . src/io/dwnd/FileHasher.java
```

Run it against a file:

```bash
java io.dwnd.FileHasher /path/to/file.bin
```

Replace `/path/to/file.bin` with the actual path to the file. Copy the generated SHA-256 hash for use in the downloader configuration.

### 5. Configure expected checksums

In `Main.java`, configure the expected checksum for each download URL:

```java
expectedChecksums.put(
    "http://localhost/file.bin",
    "YOUR_EXPECTED_SHA256_CHECKSUM"
);
```

Replace the example URL and placeholder checksum with the actual values.

**Important:** The expected checksum must come from a trusted source. Generating the checksum from an already corrupted downloaded file will not establish the integrity of the original file.

### 6. Run the application

Follow the project's existing build configuration and entry-point instructions to compile and run the downloader.

## 🔐 File Integrity Verification

The downloader uses SHA-256 checksums to detect unexpected changes to downloaded files.

The verification process is:

- Calculate the expected SHA-256 hash of the original file.
- Download the file.
- Calculate the SHA-256 hash of the downloaded result.
- Compare both hashes.
- Report whether the integrity check succeeds.

If the hashes differ, the downloaded file does not match the expected content.

## ⚡ Concurrency and Parallel Downloads

The project explores two forms of parallelism:

**Multiple-file downloading**
- Downloads multiple files concurrently instead of waiting for each download to finish sequentially.
- Can improve overall download efficiency when network bandwidth and server capacity allow.

**Chunk-based downloading**
- Splits supported large files into smaller byte ranges.
- Downloads the parts concurrently.
- Combines the parts into the completed file.

The effectiveness of parallel downloading depends on network conditions, server support, file size, and the implementation's concurrency limits.

## 🧪 Testing

Test the downloader with files of different sizes and verify the results using their expected SHA-256 checksums.

Recommended test cases:

- Download a single small file.
- Download multiple files concurrently.
- Download a large file using parallel chunks.
- Verify a successfully downloaded file against its expected checksum.
- Modify the expected checksum and confirm that verification fails.
- Test behavior when a URL is unavailable or a download is interrupted.
- Test downloads from a server that does not support HTTP byte-range requests.

The available tests and exact commands depend on the repository's build configuration.

## 📈 Future Improvements

- Add configurable limits for concurrent downloads and chunk counts.
- Implement retry logic for transient network failures.
- Support download resumption after interruptions.
- Add download progress reporting.
- Improve error handling and cleanup of incomplete files.
- Add automated tests for chunk assembly and checksum mismatches.
- Benchmark sequential versus concurrent downloads under controlled conditions.

## 👨‍💻 Author

**Yash0330**

GitHub: [Yash0330](https://github.com/Yash0330)

## 📄 License

No license information was provided in the available repository description. Add a `LICENSE` file if you intend to specify how others may use, modify, and distribute the project.
