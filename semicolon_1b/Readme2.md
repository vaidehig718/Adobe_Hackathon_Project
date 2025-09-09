docker build -t semicolon1b-pipeline -f Challenge_1b/Dockerfile .


# Collection 1
docker run --rm -v "${PWD}/Challenge_1b/Collection` 1:/data" semicolon1b-pipeline "/data/challenge1b_input.json" "/data/PDFs" "/data/pipeline_output.json" 5

# Collection 2
docker run --rm -v "${PWD}/Challenge_1b/Collection` 2:/data" semicolon1b-pipeline "/data/challenge1b_input.json" "/data/PDFs" "/data/pipeline_output.json" 5

# Collection 3
docker run --rm -v "${PWD}/Challenge_1b/Collection` 3:/data" semicolon1b-pipeline "/data/challenge1b_input.json" "/data/PDFs" "/data/pipeline_output.json" 5


run everyhting from semicolon_1b folder