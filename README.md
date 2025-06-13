# Email Verifier

This project is a Go-based command-line tool to verify the email deliverability of a domain by checking its MX, SPF, and DMARC records.

## How Email Verification Works

Email verification relies on checking several DNS records associated with a domain. These records help determine if a domain is configured to send and receive emails securely and legitimately.

*   **MX (Mail Exchange) Records:** These records specify the mail servers responsible for accepting email messages on behalf of a domain. If a domain has MX records, it indicates that it's set up to receive emails.
*   **SPF (Sender Policy Framework) Records:** SPF records are DNS TXT records that list the mail servers authorized to send email on behalf of a domain. This helps prevent email spoofing by allowing receiving mail servers to verify that incoming email from a domain is coming from an authorized server.
*   **DMARC (Domain-based Message Authentication, Reporting, and Conformance) Records:** DMARC records are also DNS TXT records. They provide a policy for how receiving mail servers should handle emails that fail SPF or DKIM (DomainKeys Identified Mail) checks. DMARC also enables reporting of email authentication results, helping domain owners monitor and improve their email security.

This tool checks for the presence and content of these records for a given domain.

## Technologies Used

*   **Go:** The project is written in the Go programming language.
    *   `net` package: Used for DNS lookups (MX and TXT records).
    *   `bufio` package: Used for reading input from the standard input (stdin).
    *   `fmt` package: Used for formatted input and output.
    *   `log` package: Used for logging errors.
    *   `os` package: Used for interacting with the operating system, specifically for reading from stdin.
    *   `strings` package: Used for string manipulation, like checking prefixes of DNS records.

## How to Use

1.  **Clone the repository (if applicable) or ensure you have the `main.go` and `go.mod` files.**
2.  **Build the project:**
    ```bash
    go build
    ```
3.  **Run the executable:**
    The program reads domains from standard input, one domain per line.
    ```bash
    ./EmailVerifier
    ```
    Or, you can pipe a list of domains from a file or another command:
    ```bash
    cat domains.txt | ./EmailVerifier
    ```
    (Where `domains.txt` is a file containing one domain per line, e.g., `google.com`, `example.com`)

4.  **Input Format:**
    Enter one domain name per line. For example:
    ```
    google.com
    github.com
    ```
5.  **Output Format:**
    The program will output a CSV (Comma Separated Values) string for each domain with the following format:
    `domain, hasMX, hasSPF, spfRecord, hasDMARC, dmarcRecord`

    *   `domain`: The domain that was checked.
    *   `hasMX`: `true` if MX records were found, `false` otherwise.
    *   `hasSPF`: `true` if an SPF record was found, `false` otherwise.
    *   `spfRecord`: The content of the SPF record if found, otherwise empty.
    *   `hasDMARC`: `true` if a DMARC record was found, `false` otherwise.
    *   `dmarcRecord`: The content of the DMARC record if found, otherwise empty.

    **Example Output:**
    ```
    google.com, true, true, v=spf1 include:_spf.google.com ~all, true, v=DMARC1; p=reject; rua=mailto:mailauth-reports@google.com
    ```

## Error Handling

The program will log errors to standard error if it encounters issues during DNS lookups or while reading input. For example, if a domain does not exist or if there are network problems.
