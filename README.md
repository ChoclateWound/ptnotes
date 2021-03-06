# PTNotes
Simple tool for taking notes in a pentest. PTNotes uses data from imported Nessus and Nmap files along with the built-in attack data to build a list of hosts, open ports, and potential attack vectors. It then allows you to add notes to each host and each attack vector. You can then view all attack notes or all host notes at one time. PTNotes allows you to create a separate project for each penetration test.

## Prerequisites
You will need to install the flask framework: `pip install flask`

## Installation
`git clone https://github.com/averagesecurityguy/ptnotes`

or

```
wget https://github.com/averagesecurityguy/ptnotes/archive/<version>.zip
gunzip <version>.zip
```

## Supported Versions
The only supported versions of PTNotes is the latest release and the dev branch. All other releases are obsolete and will be routinely removed from Github.


## Usage
From the ptnotes folder run `./server` then connect to the server on https://127.0.0.1:5000. PTNotes ships with a default TLS certificate. For security purposes, this certificate should be replaced when running the server in production. To install your certificate, replace the `config/cert.pem` and `config/key.pem` files with the appropriate files. PTNotes also supports the following command line options.

```
usage: server [-h] [-l LISTEN_ADDRESS] [-p LISTEN_PORT] [-d]

optional arguments:
  -h, --help         show this help message and exit
  -l LISTEN_ADDRESS  Address to listen on. Default is 127.0.0.1
  -p LISTEN_PORT     Port to listen on. Default is 5000.
  -d                 Enable Flask debugging. Should not be used in production.
```

## Creating New Attacks
To add new attacks to PTNotes edit the `data/attacks.json` file. Each attack uses the following structure:

```
{
    "name": "SMB Brute-force.",
    "description": "Attempt to brute-force the local administrator account on these SMB servers.",
    "keywords": ["--smb-os-discovery--", "--11011--"]
}
```

An attack needs a name and description along with a list of keywords that signify a machine may vulnerable to the attack. When data is imported to PTNotes the Nessus plugin id or the Nmap script name are extracted along with the plugin/script output. You can search for vulnerabilities using the plugin id or script name surrounded by -- as seen in the example above. You can also use any text from the plugin or script output. Multiple keywords are joined with OR to create the final query.

## To use the Docker container
Start by building it:
```
docker build . -t <your username>/ptnotes
```
Next, run it:
```
docker run -d -p 5000:5000 --name=ptnotes -v <absolute path to the repo>/data:/ptnotes/data <your username>/ptnotes
```
Destroy it when you're done (your data will persist since you used the volume mount parameter):
```
docker stop ptnotes && docker rm ptnotes
```
