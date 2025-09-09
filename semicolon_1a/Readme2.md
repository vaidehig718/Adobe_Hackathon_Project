these commands should be ran from Challenge_1a/

docker build -t mysolution:latest .

docker run --rm `
  -v "${PWD}\sample_dataset\pdfs:/app/input" `
  -v "${PWD}\CodeOutput:/app/output" `
  mysolution:latest
