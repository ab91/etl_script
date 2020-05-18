![ETL Bash Script Image](https://res.cloudinary.com/ab91/image/upload/v1576821949/ETL%20Script/etl_script.png)

## Summary

This Extract, Transform, Load script takes a JSON file as input and outputs a JSON file. It is for use with an ETL software that dynamically builds JSON requests to other systems.

**Here is how it works:**

1. Provide a sample JSON request
* This request body is in the structure that the destination system is expecting its request from the ETL software
* The JSON file resulting from this script will "teach" the ETL software how to build JSON like the request body. It is ultimately loaded into the ETL software (a process separate from this script).

2. Answer whether you'd like to preserve the JSON order
* While JSON is unordered, the ETL software displays the mappings in the order they were input. It can be more readable for users of the ETL software for mappings to match the order of the JSON keys.

3. Obtain resulting JSON file
* Again, this is loaded into the ETL software via a process outside this script.

I wrote this script to automate a previously manual process that required careful review of sometimes complex JSON bodies. 

It asks for an endpoint URL at one point, at which point the user should enter the requested value.

This is a lengthy BASH script, at the 350-line mark. If I could do this over I may consider using Python instead.

## Getting Started
### System Requirements
The script relies on several command line tools for managing JSON:

* [jq](https://stedolan.github.io/jq/), for general JSON processing
* [jo](https://github.com/jpmens/jo), for generating JSON objects
* [gron](https://github.com/tomnomnom/gron), for transforming JSON into clear paths

You can install all tools at once via Homebrew with this command:
```
$ brew install jq jo gron
```

### Usage
To grant executable permission to the script, run this command:
```
$ chmod u+x generate_mappings.sh
```

Run the script:
```
$ ./generate_mappings.sh
```

## Notes
* Ensure you have placed a single .json file in the directory at the root level named `parse/`
* An error will be thrown if the input file contains invalid JSON

## Known Issues
* JSON bodies with keys that contain a `-` character will be escaped in the generated JSON, causing problems for loading into the ETL software

## FAQ

**Q:** Why doesn't my JSON delivery look perfectly identical to my example JSON request body?

**A:** JSON "an [unordered](https://www.json.org) set of name/value pairs". After reviewing jq, catj, gron, and other tools that find all paths in a JSON object, none consistently preserved the location of each key-value pair unfortunately. 

gron gets the closest, however. And so when the select menu asks you if you want to preserve the location of your key-value pairs, you can execute gron in a way that attempts to preserve the location. The method is not very high tech and so if it tries and fails after a couple seconds, `^C` out of the script and decline location preservation next time.
