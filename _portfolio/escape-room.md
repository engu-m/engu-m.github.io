---
title: "Escape room 🔐"
excerpt: "An agent using Reinforcement Learning tries to escape a room and gets stuck."
header:
  image: /assets/images/header_escape_room.jpg
  teaser: /assets/images/header_escape_room.jpg
video_src: /assets/videos/Slideshow_Escape_Room_Project.mp4
room_img: /assets/images/rooms.png
---

## Introduction

In this little game, the player has to escape from a room using the directional keys. To play the game by yourself, head to the <a href="https://github.com/engu-m/Escape-room">project page</a> and follow the instructions.

Seven rooms were designed, with increasing difficulty. It takes less than a minute for a human to solve all levels. But the true goal of the project is to use a computer to solve these levels programmatically. This is achieved using techniques known as Reinforcement Learning, where one trains an "agent" to solve a challenge using trial and error. The agent is free to move in all four directions, provided it stays within the bounds of the room. What the agent decides to do is called its policy.
  
In this game, the player is represented by the letter <span style="color: #66D9EF">`P`</span>. The <span style="color: #F92672">`X`</span> are obstacles that the player cannot cross. The door <span style="color: #A6E22E">`D`</span>, open by default, may require the use of a key <span style="color: #E6DB74">`K`</span>.

Below are the seven rooms for this project.

<figure>
  <img src="{{page.room_img}}" alt="View of the seven rooms as shown on a terminal" width = "100%" class="dark-border">
  <figcaption>In the first room, the player only needs to go to the left once. In the room 3, it must avoid its first obstacle. In the last two rooms, the door is locked and needs to be opened by first picking up the key.</figcaption>
</figure>

## Video showcase

Because a picture is worth a thousand words, please find a presentation and demonstration below illustrating the whole learning process:

<video src="{{page.video_src}}" width="100%" controls></video>

## Technical considerations

The learning agents proposed are using the standard "Q-learning" and "Expected SARSA" approaches.

In the current implementation, there are only three rewards possible :
- +10 if the agent solves the room, i.e. stands on the door cell, with the key in its possession if required 
- +1 if the agent takes possession of the key, required to solve the room 
- -1 otherwise

The room definition is fully modular and can be changed at will. The width `w` and the height `h` of the room can be changed. Moreover, common location indications such as `"top"`, `"right"`, `"middle"` are understood. For example, the last room is simply defined in the code as a dictionary :

{% highlight python %}
{
  "door_location": "top-left",
  "key_location": "bottom-right",
  "agent_location": "bottom-middle",
  "obstacle_locations": [(1, w // 2), "top-right", "bottom-left"],
  "need_key": True,
}
{% endhighlight %}

This allows for flexible experiment settings.

The levels are not independent in the sense that we do not reset the value map after each agent has learnt a specific room. In other words, the reason why it gets stuck in room 3 is because it has learnt previously that going up is always the best action. It needs to "unlearn" first this behaviour to solve the puzzle and learn to avoid the obstacle. 

## Conclusion

Two Reinforcement Learning agents have successfully been implemented in a modular environment. Both agents showed similar behaviours and were quick to learn the optimal policy.

The learning process could further be improved by providing the agent with more information on the environment than the sole current reward: by giving as input a screenshot of the game at every step, one could get closer to human behaviour, like DeepMind did by solving the Atari games.

To see the code and read the pdf report, head to <a href="https://github.com/engu-m/Escape-room">github</a>.
