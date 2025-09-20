# Repository Guidelines

## Project Structure & Module Organization
- `app/` contains Rails MVC layers, Turbo streams, and background jobs; extract shared services into `lib/`.
- `config/` manages environment settings; edit secrets with `bin/rails credentials:edit` and mirror Docker overrides via `.env.erb`.
- `test/` mirrors the app; fixtures sit in `test/fixtures`, UI flows in `test/system`, and static assets under `app/assets` and `app/javascript` with uploads stored in `storage/`.

## Build, Test, and Development Commands
- `bin/setup` installs gems, migrates, and seeds baseline data; pass `--skip-server` when running inside CI or containers.
- `bin/dev` starts the Rails server for local work; pair it with `bin/rails tailwindcss:watch` while iterating on styling.
- `bin/ci` replicates the full pipeline: RuboCop, security audits, Rails tests, system tests, and seed replanting.

## Coding Style & Naming Conventions
- RuboCop Omakase enforces two-space indentation, snake_case methods, and CamelCase classes; run `bin/rubocop` before committing.
- Name controllers, channels, and jobs after the resource plus `Controller`, `Channel`, or `Job`, and keep namespaces consistent between `app/` and `test/`.
- Place JavaScript ES modules in `app/javascript` using kebab-case filenames, and SCSS in `app/assets/stylesheets`.

## Testing Guidelines
- Stick to Minitest: unit cases in `test/models`, controllers in `test/controllers`, channels in `test/channels`, and end-to-end flows in `test/system`.
- Describe behavior in test names (e.g., `test "creates room with valid attributes"`) and consolidate repeated setup inside `test/test_helpers`.
- Run `bin/rails test`, `bin/rails test:system`, and `env RAILS_ENV=test bin/rails db:seed:replant` before pushing or opening a pull request.

## Commit & Pull Request Guidelines
- Write short, imperative commit subjects that mirror history (e.g., `Add tidewave`, `Avoid extra slash in cable path`).
- Commit schema changes alongside their migrations and record any manual follow-up steps in the message body.
- Pull requests need a concise summary, linked issues, a test plan, and UI screenshots or GIFs; ensure `bin/ci` passes before requesting review.

## Security & Configuration Tips
- Rotate secrets through credentials or environment variables; keep production values out of git and treat `.env.erb` as a template only.
- After dependency updates, run `bin/bundler-audit`, `bin/importmap audit`, and `bin/brakeman --quiet --no-pager --exit-on-warn --exit-on-error` locally.
- Persist uploads by mounting `/rails/storage` in deployments and manually verify attachment flows before release.
