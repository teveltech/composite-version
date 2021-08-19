# composite-version

usage:
```
jobs:
  version:
    name: Handle Version
    runs-on: self-hosted
    container: < a base image to run on >
    
    steps:
      - uses: teveltech/composite-version@master
        id: semantic
        with:
          token: ${{ secrets.GITHUB_TOKEN }}
          
      - name: Check new version
        shell: bash
        run: |
          echo "Branch name: ${{ steps.semantic.outputs.branch }}"
          echo "Pre-release: ${{ steps.semantic.outputs.prerelease }}"
          echo "Workflow was triggered from ${{ github.event_name }}"
          echo "Previous annotated tag: ${{ steps.semantic.outputs.previous_tag }}"
          echo "Previous lightweight tag: ${{ steps.semantic.outputs.previous_light_tag }}"
          if [ -z "${{ steps.semantic.outputs.new_tag }}" ]; then
            echo "::error::No version bump - no new tag or release will be created. Usually means the workflow was triggered manually or a new branch was created without any changes."
          else
            echo "New tag: ${{ steps.semantic.outputs.new_tag }}"
          fi
          
    outputs:
      new_tag: ${{ steps.semantic.outputs.new_tag }}
      new_version: ${{ steps.semantic.outputs.new_version }}
      changelog: ${{ steps.semantic.outputs.changelog }}
      previous_tag: ${{ steps.semantic.outputs.previous_tag }}
      prerelease: ${{ steps.semantic.outputs.prerelease }}
      previous_light_tag: ${{ steps.semantic.outputs.previous_light_tag }}
      branch: ${{ steps.semantic.outputs.branch }}
```
