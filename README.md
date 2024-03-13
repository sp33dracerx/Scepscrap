# Define the URL of the SCEP server
$URL = "https://example.com"

# Invoke the web request to get the HTML content
$response = Invoke-WebRequest -Uri $URL

# Parse the HTML content to find certificate information
$certificates = $response.ParsedHtml.getElementsByTagName("a") | Where-Object { $_.href -like "*.pem" }

# Create an array to store certificate information
$certInfo = @()

# Iterate through each certificate and extract its details
foreach ($cert in $certificates) {
    $certUrl = $URL + $cert.href
    $certName = $cert.innerText
    $certInfo += [PSCustomObject]@{
        Name = $certName
        URL = $certUrl
    }
}

# Export the certificate information to a CSV file
$certInfo | Export-Csv -Path "SCEP_Certificates.csv" -NoTypeInformation

# Display a message indicating the CSV export is complete
Write-Host "SCEP certificate information exported to SCEP_Certificates.csv"
