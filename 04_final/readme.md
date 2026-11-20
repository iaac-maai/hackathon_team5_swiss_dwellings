# Final submission: Swiss Dwellings - A quiter place, a brighter horizon, and a more scenic vista.

**Team 5:** Breno Sader, Diego Alpizar, Sindhuja Davuluri and Soumyajyoti Datta.

[Live app](https://team5-swiss-dwellings.pages.dev) · [Model server](https://hackathon-team5-swiss-dwellings.onrender.com) · [Model card](https://huggingface.co/iaac-maai/hackathon-team5-swiss-dwellings) · [Final app code](app/) · [Class Presentation](/04_final/Quiet%20%26%20Bright%20Twin%20Finder%20-%20Team%205%20Hackathon.pdf)


## What the app does

Users register or log in to an individual account, or use the optional shared team-password entry without creating an account. They select a historical apartment example or enter 24 apartment-level values, and request an assessment. An autoencoder returns a reconstruction-error score and an eight-dimensional fingerprint; the app shows the score against its threshold, feature-error contributions and up to five reference apartments that meet selected improvement filters. A match is not necessarily an identical layout or an available property. “Market Ready” and “Need Renovation” are interface labels based on model flags, not verified property assessments.

The app uses Cloudflare Pages and Functions, a separate D1 account database and a D1 prediction database, plus a Gradio/ONNX server on Render. Hugging Face hosts the model. Successful results are saved in D1, but history is deliberately not accessible through the website; a reload clears the visible result. The app does not accept file uploads, so R2 is not used.

## Diagrams

- [System architecture](diagrams/system.png)
- [User flow](diagrams/user_flow.png)
- [Sequence](diagrams/sequence.png)

The diagrams show the earlier combined team-password and account login. They need updating to show the current alternative team-password-only entry.

## Three statements 

**What is clear:** We understand how apartment features are aggregated and standardized, how reconstruction error is compared with the threshold, and how the browser, Cloudflare, Render and the two databases interact.

**What is still unclear:** The noise-rule labels are proxies - based on the model, and the results do not prove real-world noise or renovation needs. Render availability can affect the live demo.

**What we would do next:** Validate similarity with users, and improve account recovery and email verification.

## Failure cases

| Situation | Current behavior |
|---|---|
| Invalid email, weak password or wrong credentials | Registration/login shows an error; protected endpoints require a session. |
| Model is asleep, unavailable or gives an invalid response | The API returns an error; the user can retry. The request timeout is 120 seconds. |
| Reload | The current visible result disappears. Results are saved privately but cannot be reopened in the current UI. |
| Two analyses at once | Each successful result gets a random ID and a separate database insert. |
| Missing or invalid measurements | Input validation rejects the request before inference. This app has no file upload. |
| No candidates meet filters | The app reports no matches; fewer than the requested number may be returned. |

## Submission status

See [PROOFS.md](PROOFS.md) for recorded checks and outstanding evidence, including the limitation that user-specific history is not implemented. Provide the team password and a test account privately in the course hand-in, never in this repository. 
