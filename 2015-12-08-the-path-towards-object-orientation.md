# The Path towards Object-Orientation
While I can increasingly see the benefits of generic-function style OO, I also think it may be a bit overkill for where I am right now. Below is an overview of how my coding as evolved:

1. Ad hoc scripts which only really work when run by me, in an interactive fashion.
2. Well-structured scripts - reproducible, but not particularly dynamic
3. Function-based scripts - both reproducible and dynamic

The last point, 3, largely represents the idea of abstracting away as much as possible into functions, so that the actual analysis scripts become very short. This way I can have multiple scripts which represent multiple variations on the same analysis, but all using the same underlying functions. It seems to me that the next, potential steps (i.e. 4 & 5) here would be to start learning S3 and S4. However, I think I need to transition more firmly to step 3 first - being able to do "function-based scripting" in a proper manner. I think that once I feel that this is insufficient, then it's time to move over the S3, but before I even feel the limitations of "function-based scripting" I think it's overkill to bring in object-orientation.
