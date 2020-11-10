# Aircall.io - DevOps technical test

This test is a part of our hiring process at Aircall for [DevOps positions](https://aircall.io/jobs#SystemAdministrator). It should take you between 1 and 6 hours depending on your experience.

**Feel free to apply! Drop us a line with your Linkedin/Github/Twitter/AnySocialProfileWhereYouAreActive at jobs@aircall.io**

## Summary

An intern in our team has developped an application to resize images. It's working fine.

Unfortunatly, he left the company and we have no documentation or no insights at all about
what he did.

We just have the code.

With the following request to the application, the image is resized, stored and accessible from s3.

```
curl --location --request POST 'http://resize.aircall.com/image' \
--form 'file=@img.jpg' \
--form 's3Key=img.jpg'
```

The provided code is working.

It seems that our intern was using something called Lambda. Don't know what is it.

## What we want ?

Could you please take this code, and deploy it on AWS in a cool way?

Please, make Infra as Code because I heard that it's cool to do so.

You can also make some diagrams if needed.

Technically speaking, we need to get:

- Application URL
- URL of the resized images
- GitHub repo

## Good to know

- packages have to be builded from a linux platform (docker is your friend)
- input is not a JSON but a form data (multipart/form-data)

## Nice to have

- logs
- tracing
- deployment framework
- CI/CD
- auth

Good luck !

## Results

### Install Node modules

To install node_modules, go in main folder and enter the following commands:

```
docker run --rm -v "$PWD":/var/task lambci/lambda:build-nodejs12.x npm install
```

Then, because I use lambda layers, run this one:

```
docker run --rm -v "$PWD":/var/task lambci/lambda:build-nodejs12.x npm --prefix ./lib/nodejs install lib/nodejs/
```

### Deploying the app

To deploy the app, you have to run:

```
docker run --rm -v "$PWD":/var/task lambci/lambda:build-nodejs12.x npm run deploy
```

The script use the serverless framework to create resources in AWS Cloud.

### Auth

The authentication is done by using an API Key provided by API Gateway

```
curl --location --request POST 'https://GENERATED_URL/image' \
--header 'x-api-key: GENERATED_API_KEY' \
--form 'file=@img.jpg' \
--form 's3Key=img.jpg'
```

You have to replace GENERATED_URL and GENERATED_API_KEY by values provided by serverless.

### CI/CD

When a push is set on _main_ branch or a pull request is validated, then a CI task start, build the app and deploy it on AWS.

### Tracing and Logs

The tracing is done by AWS X-Ray service and logs are registered in CloudWatch for 14 days.
