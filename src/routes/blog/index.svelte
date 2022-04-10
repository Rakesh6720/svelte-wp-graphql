<script context="module">
	export async function load({ fetch }) {
		const query = `
        query getPosts {
            posts {
                nodes {
                databaseId
                uri
                title
                excerpt
                date
                featuredImage {
                    node {
                    sourceUrl
                    altText
                    mediaDetails {
                        width
                        height
                    }
                    }
                }
                id
                }
            }
            }
        `;

		const response = await fetch(import.meta.env.VITE_PUBLIC_WORDPRESS_API_URL, {
			method: 'POST',
			headers: {
				'Content-Type': 'application/json'
			},
			body: JSON.stringify({ query })
		});

		if (response.ok) {
			const responseObj = await response.json();
			const posts = responseObj.data.posts.nodes;

			return {
				props: {
					posts
				}
			};
		}

		return {
			status: response.status,
			error: new Error(`Could not load`)
		};
	}
</script>

<script>
	export let posts;
</script>

<h1>Blog</h1>
{#if posts}
	<ul>
		{#each posts as post}
			<li>{post.title}</li>
		{/each}
	</ul>
{:else}
	<p>No posts fix_and_outro_and_destroy_block.</p>
{/if}
