# Azure API Management End-to-end encryption

## Requirements

- There's a requirement have TLS encryption with custom domains from the caller, through API management to the APIs
- API Management deployed in internal mode

```bash
Left: http://gateway.company.com  Right: https://api.company.com
```

## Azure Services

- Azure API Management
- Azure Private DNS
- App Services

### Paid certificates or Let's Encrypt free certificates

Steps:

- Obtain your certificates or [generate](https://medium.com/@akitikkx/generate-a-free-ssl-certificate-with-lets-encrypt-and-certbot-53eb71c56788) your certificates using let's encrypt
- Deploy the certificate and update the portal and gateway domains in API Management
- Deploy the root and intermediate certificates to API Management
- Deploy the client certificates to the APIs
- Configure the APIs using ssl.
  - If you were using App Services for example, make sure the use a custom domain
- If you are working with internal services, make sure to configure DNS. You will need entries for the portal and gateway which is the same internal IP address for the API Management.

#### How it works

The client maching making the call the through API should have all the required certificates installed. Once the requests lands on API Management, it will forward the requests onto the target API over https. Encryption and decryption occers by using the root and intermediate certificates.

### Self-signed certificates

Steps:

- Generate a root CA
- Generate a client certificates for portal, gateway and APIs
- Deploy the certificate and update the portal and gateway domains
- Deploy the self-signed root certificate API Management
- Deploy the client certificates to the APIs
- Configure the APIs using ssl
  - If you were using App Services for example, make sure the use a custom domain
- If you are working with internal services, make sure to configure DNS. You will need entries for the portal and gateway which is the same internal IP address for the API Management.
