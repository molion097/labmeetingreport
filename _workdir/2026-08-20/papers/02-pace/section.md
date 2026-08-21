## PACE: Part-Wise Slow-Fast Conditioning for Dance-to-Music Generation

Authors unverified (see note below). ICLR 2026, OpenReview submission. https://openreview.net/forum?id=1Ip4pZoHTH

I couldn't get past OpenReview's bot check for this one, every fetch method hit the same wall, there's no arXiv copy, and it doesn't appear on any ICLR 2026 accepted list yet, so it may still be under review. What follows is reconstructed from the title and an abstract snippet in search results, so treat it as provisional. The claimed idea: dance-to-music diffusion models usually treat the dancer's whole body as one motion signal, losing the fact that different body parts carry different rhythmic information and that a single trace mixes slow and fast movement together. PACE instead splits the body into five parts, computes a kinetic energy signal per part, filters each into slow and fast frequency bands, and uses that decomposition to condition a latent diffusion model. The abstract claims consistent improvement over prior methods on AIST++ and TikTok data, but no numbers are available. Someone with OpenReview access should pull the real PDF before this gets presented.

