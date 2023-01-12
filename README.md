# time_domain_stage_problem

For study time domain stage game puzzles. ( Building environments, AI from the last working versions )

Games input is based on the time steps limits, some warning messages are from compatibility for the game and Gym environment which currently has 5 variables output but the Super Mario games are based on 4 outputs as the versions of the game and I found they asking on the Webboard. ```gym-super-mario-bros 7.4.0```, file: ```\Python310\lib\site-packages\gym\wrappers\time_limit.py```

## Step definition ##

Simply remarked game environment name and versions. ```if ( str( self.env.spec.name ) == "SuperMarioBros" ) and str( self.env.spec.version ) == "0" :```

```
def step(self, action):
    """Steps through the environment and if the number of steps elapsed exceeds ``max_episode_steps`` then truncate.

    Args:
    action: The environment step action

    Returns:
    The environment step ``(observation, reward, terminated, truncated, info)`` with `truncated=True`
    if the number of steps elapsed >= max episode steps

    observation, reward, terminated, truncated, info = self.env.step(action)
    return observation, reward, terminated, truncated, info
    """
    info = []
	
    if ( str( self.env.spec.name ) == "SuperMarioBros" ) and str( self.env.spec.version ) == "0" :
        observation, reward, terminated, truncated = self.env.step(action)
    else :	
        observation, reward, terminated, truncated, info = self.env.step(action)
		
    self._elapsed_steps += 1

    if self._elapsed_steps >= self._max_episode_steps:
        truncated = True
    
    return observation, reward, terminated, truncated, info
```

## step_api_compatibility ##

We remarked it based on the program compatibilities and support read original ```step_api_compatibility``` file: ```\Python310\lib\site-packages\gym\utils\step_api_compatibility.py ```

```
def step_api_compatibility(
    step_returns: Union[TerminatedTruncatedStepType, DoneStepType],
    output_truncation_bool: bool = True,
    is_vector_env: bool = False,
) -> Union[TerminatedTruncatedStepType, DoneStepType]:
    """Function to transform step returns to the API specified by `output_truncation_bool` bool.

    Done (old) step API refers to step() method returning (observation, reward, done, info)
    Terminated Truncated (new) step API refers to step() method returning (observation, reward, terminated, truncated, info)
    (Refer to docs for details on the API change)

    Args:
        step_returns (tuple): Items returned by step(). Can be (obs, rew, done, info) or (obs, rew, terminated, truncated, info)
        output_truncation_bool (bool): Whether the output should return two booleans (new API) or one (old) (True by default)
        is_vector_env (bool): Whether the step_returns are from a vector environment

    Returns:
        step_returns (tuple): Depending on `output_truncation_bool` bool, it can return (obs, rew, done, info) or 
        (obs, rew, terminated, truncated, info)

    Examples:
        This function can be used to ensure compatibility in step interfaces with conflicting API. 
        Eg. if env is written in old API,
        wrapper is written in new API, and the final step output is desired to be in old API.

        >>> obs, rew, done, info = step_api_compatibility(env.step(action), output_truncation_bool=False)
        >>> obs, rew, terminated, truncated, info = step_api_compatibility(env.step(action), output_truncation_bool=True)
        >>> observations, rewards, dones, infos = step_api_compatibility(vec_env.step(action), is_vector_env=True)
    """
    if output_truncation_bool:
        return convert_to_terminated_truncated_step_api(step_returns, is_vector_env)
    else:
        return convert_to_done_step_api(step_returns, is_vector_env)
```

## Files and Directory ##

| File name | Description |
|--- |--- |
| sample.py | sample codes |
| Figure_1.png | image remak and results |
| Figure_1.png | image remak and results |
| Marios Bros.gif | result |

## Results ##

![Sample problem solving](https://github.com/jkaewprateep/time_domain_stage_problem/blob/main/Marios%20Bros.gif "Sample problem solving")

![Sample problem solving](https://github.com/jkaewprateep/time_domain_stage_problem/blob/main/Figure_1.png "Sample problem solving")

![Sample problem solving](https://github.com/jkaewprateep/time_domain_stage_problem/blob/main/Figure_2.png "Sample problem solving")

## References ##

#### gym-super-mario-bros 7.4.0 ####

1. https://pypi.org/project/gym-super-mario-bros/
2. https://github.com/Kautenja/gym-super-mario-bros
