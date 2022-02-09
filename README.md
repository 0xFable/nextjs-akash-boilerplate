# NextJS With Akash

This example shows how to use Docker with Next.js based on the [deployment documentation](https://nextjs.org/docs/deployment#docker-image). Additionally, it contains instructions for deploying to Akash.

## How to use

Execute [`create-next-app`](https://github.com/vercel/next.js/tree/canary/packages/create-next-app) with [npm](https://docs.npmjs.com/cli/init) or [Yarn](https://yarnpkg.com/lang/en/docs/cli/create/) to bootstrap the example:

```bash
npx create-next-app --example with-docker nextjs-docker
# or
yarn create next-app --example with-docker nextjs-docker
```

Or use this the `nextjs-akash-boilerplate` project in this repo which was started with that command

## Using Docker

1. [Install Docker](https://docs.docker.com/get-docker/) on your machine.
1. Build your container: `docker build -t <yourusername>/nextjs-akash .`.
1. Run your container: `docker run -p 3000:3000 nextjs-docker`.

You can view your images created with `docker images`.

## Running Locally

First, run the development server:

```bash
npm run dev
# or
yarn dev
```

Open [http://localhost:3000](http://localhost:3000) with your browser to see the result.

You can start editing the page by modifying `pages/index.js`. The page auto-updates as you edit the file.

[API routes](https://nextjs.org/docs/api-routes/introduction) can be accessed on [http://localhost:3000/api/hello](http://localhost:3000/api/hello). This endpoint can be edited in `pages/api/hello.js`.

The `pages/api` directory is mapped to `/api/*`. Files in this directory are treated as [API routes](https://nextjs.org/docs/api-routes/introduction) instead of React pages.


## Deploying to Akash Network

1. Install the [Akash Command Line Tool](https://docs.akash.network/guides/cli/part-1.-install-akash) so you can use `akash` on the command line.
1. Configure the environment variables to setup your account for deployment.
2. [Create a new certificate](https://docs.akash.network/guides/cli/part-6.-create-your-certificate) using the Akash CLI
3. Build your container image using Docker: `docker build -t <yourusername>/nextjs-akash .`. 
4. Push your container image to a PUBLIC repository. This will enable Akash deployments to utilize your image. Currently only public images are supported which may be fine if you are making a public service anyways. Ensure you follow best practices for container security.
5. Create a deployment file which will describe the units you will need as well as the image you will use. You can find a quickstart example in `deploy.yaml` with an already public image if your just testing.
6. Begin a deployment to Akash. Follow the Akash guide from [part 7-10](https://docs.akash.network/guides/cli/part-7.-create-your-deployment)
7. View your newly deployed NextJS app! The URI will be included in the response of `akash provider lease-status` command noted in [step 10](https://docs.akash.network/guides/cli/part-10.-send-the-manifest) of the Akash guide

### Setting up a custom domain with SSL

After doing your first successful deploy, take a moment. You have already achieved so much by getting NextJs into the Akash network. Two problems remains which are 1. Your uri is not the best to share and market around and 2. HTTPS with secured connections aren't right but we can fix both of these by getting a custom domain and putting cloudflare in front of it. 

For the custom domain I use NameCheap but you are free to choose as long as they play nice with cloudflare which will be what handles SSL and HTTPS. 

After you have your domain you need to create a cloudflare account and add your website. A free account should do and for instructions on setting up the website head [here](https://support.cloudflare.com/hc/en-us/articles/201720164-Creating-a-Cloudflare-account-and-adding-a-website)

Next, head to your newly added domain and add a new CNAME DNS record for the Akash uri :

![](/screenshots/cloudflare1.png)

While in the management panel, scroll down and ensure your SSL and TLS are set to 'FULL'

![](/screenshots/cloudflare2.png)

Under the SSL/TLS Tab, navigate to Edge Certificates. Make sure ‘always use HTTPS’ is enabled.

![](/screenshots/cloudflare3.png)

You can now visit your custom domain. When you do so it will redirect to your Akash deployment uri but this time with HTTPS available and with a proper SSL cert. 

Reference -- I got some of these images from this [great article](https://teeyeeyang.medium.com/how-to-use-a-custom-domain-with-your-akash-deployment-5916585734a2)