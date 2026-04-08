# comment-action

This action uses [gh-comment-kit](https://github.com/srz-zumix/gh-comment-kit) to post trackable review comments to pull requests.

## Features

- **Trackable comments**: Comments are grouped by identifier, enabling update, delete, and resolve operations on previous comments.
- **Flexible comment body**: Specify the comment body directly or via a file (supports stdin).
- **Automatic splitting**: If the comment body exceeds GitHub's size limit (65,536 characters), it is automatically split into multiple comments. Use `truncate` to truncate instead.
- **Review comment support**: Attach comments to specific file paths and line numbers.

## Usage

Call this action in your workflow:

```yaml
name: Comment
on:
  pull_request:

jobs:
  comment:
    runs-on: ubuntu-latest
    permissions:
      contents: read
      pull-requests: write
    steps:
      - uses: srz-zumix/comment-action@v0
        with:
          body: 'Hello from comment-action!'
          group: my-comment-group
```

### Update existing comment

```yaml
      - uses: srz-zumix/comment-action@v0
        with:
          body: 'Updated comment body'
          group: my-comment-group
          update: true
```

### Delete previous comments before posting

```yaml
      - uses: srz-zumix/comment-action@v0
        with:
          body: 'Fresh comment'
          group: my-comment-group
          delete: true
```

## Inputs

| Name | Description | Default | Required |
| ---- | ----------- | ------- | -------- |
| `repo-token` | The GitHub token used to post comments | `''` | false |
| `pr-number` | The pull request number | `${{ github.event.number }}` | false |
| `body` | Comment body text | `''` | false |
| `body-file` | Path to a file containing the comment body (`-` for stdin) | `''` | false |
| `group` | Comment group identifier used to track related comments | `gh-comment-kit` | false |
| `path` | File path to attach the review comment to | `''` | false |
| `line` | Line number to comment on (requires `path`) | `''` | false |
| `update` | Update (edit) the last comment in the group instead of creating a new one | `false` | false |
| `delete` | Delete previous comments in the same group before posting | `false` | false |
| `resolve` | Resolve previous review comments in the same group | `false` | false |
| `truncate` | Truncate the comment body if it exceeds the size limit instead of splitting | `false` | false |
| `dryrun` | Print what would be posted without actually posting | `false` | false |
| `cache` | Whether or not to use aqua install cache | `true` | false |

## Environments

| Name | Description |
| ---- | ----------- |
| `GH_HOST` | Specify the GitHub hostname |
| `GH_TOKEN` | Takes priority over inputs.repo-token |
| `GH_ENTERPRISE_TOKEN` | Takes priority over inputs.repo-token |
| `AQUA_GITHUB_TOKEN` | Set AQUA_GITHUB_TOKEN to avoid rate limits |

## Reference

- [srz-zumix/gh-comment-kit](https://github.com/srz-zumix/gh-comment-kit)
- [aquaproj/aqua](https://github.com/aquaproj/aqua)
- [srz-zumix/aqua-installer-cache](https://github.com/srz-zumix/aqua-installer-cache)