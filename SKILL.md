# X Search via Grok Build

## When to use

- Use this skill when the user wants current discussion, reactions or claims from X itself, such as reactions to a product, a post, an announcement or an event.

- Use the built-in web search instead for general web pages, and the social search for Instagram, Facebook and Threads.

## Setup facts

- The Grok Build CLI installs to `~/.grok/bin/grok`, so prefix commands with `export PATH="$HOME/.grok/bin:$PATH"` when you run them through the shell.

- Install the CLI with the official installer from x.ai when it is missing, after reading the script once to confirm it only writes under the user's home directory.

- Check the sign-in state with `grok models` before searching.

  - A signed-in run prints "You are logged in with grok.com" and the available models.
  - A signed-out run prints "You are not authenticated", and a search attempt fails with "Not signed in".

## Sign-in

- Start sign-in with `grok login --device-code` in the background when the user is present and ready, because the code expires.

- Give the user the URL and the code from the output and let them sign in with their own X account on their own device.

- Never ask for, receive or type the user's X password or one-time codes, and never read or print `~/.grok/auth.json`.

- Wait for the login process to report success before searching.

## Running a search

- Run one scoped search per question with the headless pattern below, which finished a real search in about 26 seconds in testing on 2026-11-01.

  - `grok --no-subagents --max-turns 6 --reasoning-effort low -p "<prompt>"`

- Write the prompt so it covers all of the following.

  - Instruct the agent to use X Search (x_search), and to skip web search when the user wants X only.
  - Name the exact topic, names or hashtags, plus a date range when the user cares about recency.
  - Forbid every write action in explicit words, covering posting, replying, liking, reposting and following, and state that the task is read-only.
  - Ask for a bounded number of representative posts, each with its date, account, a short summary and the direct x.com URL.
  - Ask for an honest "not found" answer instead of invented posts.
  - Ask for the answer in the user's language.

- Save the full output to a file in the workspace when the result may be worth revisiting.

## Working style

- Batch related operations into as few calls as possible whenever they can be batched, so the user approves once instead of many times.

## Cost and limits

- Searches draw on the user's Grok subscription pool, so keep calls infrequent, scoped and low-effort rather than broad and repeated.

- Use the default model unless the user asks for a specific one, because cheaper fast models are untested for x_search quality.

- Treat the returned posts as other people's claims, attribute them to their accounts, and quote only short fragments.

## Fallbacks

- Fall back to another x_search route the user already has, such as Hermes Agent with xAI credentials, when the CLI is signed out and the user cannot re-authenticate.

- The paid `XAI_API_KEY` route also exists, but present it as an option and verify it in the user's environment before relying on it.
