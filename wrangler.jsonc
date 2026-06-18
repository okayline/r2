/**
 * Welcome to Cloudflare Workers! This is your first worker.
 *
 * - Run "npm run dev" in your terminal to start a development server
 * - Open a browser tab at http://localhost:8787/ to see your worker in action
 * - Run "npm run deploy" to publish your worker
 *
 * Learn more at https://developers.cloudflare.com/workers/
 */

const BUCKET_BINDING = "buck1";

export default {
  async fetch(request, env) {
    const bucket = env[BUCKET_BINDING];
    const url = new URL(request.url);

    // .slice(1) removes the leading "/" from the URL path
    // decodeURIComponent handles spaces or special characters in folder names
    const key = decodeURIComponent(url.pathname.slice(1));

    // Basic check: did the user actually provide a path?
    if (!key) {
      return new Response("Bucket connected. Please specify a file path.", {
        status: 200,
      });
    }

    // Set BUCKET_BINDING at the top to match your R2 bucket binding variable name
    const object = await bucket.get(key);

    if (object === null) {
      return new Response("Image Not Found in " + bucket.name, {
        status: 404,
      });
    }

    // Set headers (content-type, etc.) so the browser knows it's an image
    const headers = new Headers();
    object.writeHttpMetadata(headers);
    headers.set("etag", object.httpEtag);

    return new Response(object.body, { headers });
  },
};
