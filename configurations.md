# Configurations

Reference for specific programs whose configuration information is sparse or highly customized to my setup

## `colorls`

https://github.com/athityakumar/colorls

Colors are defined in `dark_colors.yaml` in `~/.config/colorls`

`colorls` uses the `rainbow` ruby gem to colorize output. Available colors are listed [here]([https://github.com/sickill/rainbow#color-list)

To set the colors, identify which color is desired for each category. Example:

```yaml
# Main Colors
unrecognized_file: yellow
recognized_file:   white
dir:               blue

# Link
dead_link: red
link:      magenta

# Access Modes
write:     blue
read:      white
exec:      green
no_access: red

# Age
day_old:     cyan
hour_old:    green
no_modifier: white

# File Size
file_large:  red
file_medium: yellow
file_small:  white

# Random
report: white
user:   green
tree:   cyan
empty:  yellow
error:  red
normal: black

# Git
addition:     cyan
modification: yellow
deletion:     red
untracked:    blue
unchanged:    white
```



