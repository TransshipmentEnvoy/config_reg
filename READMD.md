# ConfigReg

c++

## API design

the overall design should be very similar to ```CLI11```

main class ```ConfigReg```

method ```ConfigReg.reg()```
accept name, prefix, destination var, description; also capture default value

need a concept to check if the var is supported by
+ command line
+ ```json``` (optional)
+ ```yaml``` (optional)
+ ```toml``` (optional)