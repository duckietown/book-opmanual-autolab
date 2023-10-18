(autolab-2-autolab-overview)=
# Overview

```{needget}
* Passion for reproducible robotics
---
* Basic understanding of Autolabs
```

```{figure} ./_images/watchtower_placement/autolab_image.jpg
:width: 90%

Picture of the Zurich ETH Autolab.
```

## Definition

The goal of an Autolab is to create an automated environment for Duckiebots, in which their behaviors can be evaluated.

More specifically:

- If identical [^identicalhow] hardware and software is used on two Duckiebots, the behavior should be reproducible in the same environment.
- The environment should be highly controlled and configurable

[^identicalhow]: identical to the best knowledge

(naming-convention)=
## Entities Definition

| Entity                                                                                        | Description                                                                                          | Num Required | Configuration   |
| --------------------------------------------------------------------------------------------- | ---------------------------------------------------------------------------------------------------- | ------------ | --------------- |
| Duckies                                                                                       | Rubber ducks, as pedestrians in the city/autolab                                                     | 10           | Color: Yellow   |
| [Autobots](autolab-2-autobots) | The Duckiebots with top plate and Apriltags. Where the AIDO submissions will be installed and run. | 3            | DB21            |
| [Watchtowers](watchtower-hardware) | Cameras in the autolab that continue to recognize Apriltags (on the ground and Duckiebots).       | 6/7          | WT19B           |
| Duckietown robot                                                                              | The robot that collects the transforms published from all other robots.                             | 1            | DT21            |

## Applications

The most common applications for the autolabs are:

- research benchmarking with the Duckietown platform
- physical evaluations for challenges of AIDO

## Autolab in action

The demo video shows:

- (`00:00` ~ `00:31`) An AIDO participant submitting to an AIDO challenge
- (`00:32` ~ `03:14`) Autolab evaluating the AIDO submission
- (`03:15` ~ `end`) Evaluation experiment results and metrics presented on the AIDO website

```{vimeo} 561305335
:alt: Autolab demo (TTIC at Chicago)
```

```{trouble}
I have problems/suggestions for parts of this documentation.
---
Feel free to discuss in the #devel-autolab channel on the Duckietown Slack.
```