# gh-action-git-log-stats

This action will provide outputs relating to files changed, lines inserted and lines deleted based off of chosen commits and optionally a path.

It can be used to prevent deletions to a particular file or files depending on your use case.

## Inputs

| Setting        | Description                                                                                                               | Values                                      |
| -------------- | ------------------------------------------------------------------------------------------------------------------------- | ------------------------------------------- |
| `start-commit` | The commit to begin with for the log, defaults to the first commit of a Pull Request                                      | `${{ github.event.pull_request.base.sha }}` |
| `end-commit`   | The commit to end with for the log, defaults to the last commit in a Pull Request                                         | `${{ github.event.pull_request.head.sha }}` |
| `path`         | If you wish to restrict the stats output by path, you can do so. Leave this default to apply it to the current workspace. |                                             |

## Outputs

| Name             | Description             | Default value |
| ---------------- | ----------------------- | ------------- |
| `FILES_CHANGED`  | Count of files changed  | 0             |
| `LINES_INSERTED` | Count of lines inserted | 0             |
| `LINES_DELETED`  | Count of lines deleted  | 0             |

## Example usage

```
      - name: Get Git log stats for this PR
        id: git-log-stats
        uses: IMGARENA/gh-action-git-log-stats@v1
        with:
          path: 'src/super_important_stuff'

      - name: Prevent line deletions from important files
        if: ${{ steps.git-log-stats.outputs.LINES_DELETED > 0 }}
        run: exit 1
```

## Notes

The bulk of the command itself to get this data is from the following Stack Exchange thread, thanks!
https://stackoverflow.com/questions/2528111/how-can-i-calculate-the-number-of-lines-changed-between-two-commits-in-git/2528129
