# Effect of Room Size, Amount of Fruit, and Placement of Fruit on Fruit Fly Population over Time

## Abstract
This paper models, in a 3-step process, fruit fly population over time in a room based on room size, amount of fruit, and placement of fruit. From this, the ideal interval of time to dispose of trash can be determined. I discovered that room size does not significantly prolong the interval between trash days, but placement of fruit far away from the door has a significant impact on both decreasing fruit fly populations and lengthening the interval between trash days.

## Introduction
The goal of this project is to model the population of fruit flies in a room over time given the size of the room and the placement and amount of fruit. Many small apartments, including mine, have the issue of fruit flies, and the goal is to throw fruit away before fruit fly populations boom, but also, for convenience, to take out the trash as infrequently as possible. I hypothesized that the placement of fruit, the amount of fruit, and the size of the room can affect whether or not the population booms within the amount of time before the fruit is thrown away. In this paper I design and run a basic model, then run sensitivity on these three parameters to determine how they affect the amount of time it takes for fruit fly populations to boom, and thus the maximum interval between trash days. 

## The Model
According to research, fruit flies are attracted to the smell of fermenting fruit<sup>1</sup>, which is a direct byproduct of bacterial processes<sup>2</sup>. So my model starts with a model for bacterial growth in a piece of fruit. The most common model for single-species growth is by far the logistic differential model. 

I simply used the solution of the logistic to plot the first part of my model, modeling bacteria population. According to research<sup>3</sup> an apple starts with about 100 million bacteria but only about 1% makes it go bad so my P(0) = 1 million. I made the assumption that once an apple is rotten through, it’ll have about as much bacteria as its weight in dirt. After refining the rate so the time scale for rotting makes sense (apples become completely rotten after being left for 1-3 days<sup>4</sup>), with an hour for my timestep, I assume that amount of bacteria directly relates to amount of smell produced since it’s a bacterial byproduct. 

Every hour, I initialize smell particles starting at a set location where the fruit is in the room corresponding to how many bacteria I have. Each hour, I also walk these smell particles one step (this would be one foot in any direction), assuming a 2-dimensional room. If the smell particles hit the wall, they bounce. If a smell particle hits the door, I assume that it attracts a new fly. I record the number of new flies I get per hour for the final step. 

The final step, of course, is to model fly populations. I do this by modifying the logistic growth differential equation to account for immigration. At each time step, I add the number of flies attracted at that step for a recursive approximation: *P\[t]=P\[t-1]+r\*P\[t-1]\*(1-P\[t-1]/K)+newflies\[t]*, with t being the hour in question, *P* being the population, *K* being the original carrying capacity, and *newflies\[t]* being the number of new flies attracted at that time as recorded from my diffusion model. From research and experience fruit fly counts often grow to be in the hundreds so I set my carrying capacity at 350 and I assume I start with no flies. I also assume that once flies are added they go immediately to the fruit.

## Initial Results/Discussion
As described above, I tailored my parameters so that initial results match research and experience. Our base case is a small 10ft by 10ft apartment with an apple placed in the center of the room and the door in a corner. 

Apples rot between 1-3 days, starting from a million bacteria according to research and ending with about as much bacteria as its weight in dirt. 

At hour 0, my smell molecules start at the center of the room. At hour 10, they’ve spread a lot, but haven’t quite reached the door, the same for hour 20. At hour 30 they’ve started reaching the door (depicted on this graph in the top right corner). This makes sense according to experience, as it often takes about a day for fruit flies to discover fruit in my similarly sized room. By hour 40 the smell is already distributed. By hour 50 and 60 our smell has covered the room, as can be seen in the last graph on the right. It is important to note that all this time new smell particles are being produced in the center of the room so despite being distributed, smell particles are in no sense uniform and are heavily concentrated in the center of the room where the smell is produced. 

This leads us to our final model of fruit fly population, set up as a logistic differential with immigration at each time step corresponding to the number of new fruit flies attracted by smell diffusion. The pure logistic differential part had a carrying capacity of 350 flies, so the fact that at hour 60 our model is at 700 flies and barely slowing on growth means that our population is carried heavily by new fruit flies attracted each hour. Our population really starts growing at hour 35, so that would be the best time to take out the trash. 

## Sensitivity to Room Size/Shape
I tested four scenarios. A wider room (doubling the room along the axis that the door is on), a deeper room (doubling the room along the axis perpendicular to the door), a larger room in general (doubled in size along both axes), and a smaller room (like a pantry).

**Wider Room:** With a wider room we don’t get smell distributed throughout the room until close to the end. This, of course, also means that we attract less fruit flies and the population is, as expected, smaller. Interestingly, our room area is exactly double the original room area and our fruit flies at the end are almost exactly half the amount of fruit flies we had at the end of the original model. However, this does not mean we get to take out the trash half as much. Our population here really starts growing around hour 45, so that would be the ideal time for garbage day, just 10 hours delayed from the original model. 

**Deeper Room:** As expected, a deeper room has slightly less flies than a wider room because the average distance of the door from the center of the room is further, but not by a lot. However, this does not have a huge impact and the population nonetheless starts booming around hour 45, meaning with double the room size along an axis the time to take out the trash is similar, regardless of axis. 

**Larger Room:** This room is 4x larger than the original room and twice as large as the wider room and the deeper room. However, interestingly, the fruit fly population has significantly decreased, by around 70-fold from the original model. While at first this may seem inconsistent since rooms 2x larger than the original room had about half as many fruit flies, this starts to make sense when we consider that our fly population is mostly carried by the amount of new flies we attract each hour. As the diffusion plots show, we barely reach the door even at hour 60, so it makes sense that not many new flies are attracted. Nonetheless, once flies are attracted, the population grows rapidly, so it’s best to take out the trash around hour 55. This means that a bigger room size does not significantly delay intervals between trash days. 
Pantry (from left to right, diffusion of smell at hours 0, 10, and 20):
 
**Pantry:** This replicates a 4ft x 4ft pantry where the doors compose of the entire lower border of the room. As we can see, the smell quickly fills the room and fruit flies start coming in rapidly. However, by the end of the 60 hours we’re also seeing the population start to plateau at just under 2000 flies. Obviously, 2000 flies is concerning and the time to take out the trash would’ve been right before rapid growth at around hour 15. An interesting thing to note is that the population in this model looks relatively stable until it starts to plateau and the new flies attracted cause the model to start to blow up volatilely. This shows a weakness in our model which may be remedied by limiting the number of new flies that can be attracted from outside (which is more realistic as well). 

## Sensitivity to Fruit Placement
My original model has fruit placed in the center of the room. I tested two other trash can placements; the corner furthest from the door and a corner closest to the door. 

**Far from the door:** Not only has the fruit fly population decreased around fourteenfold from the original case with the same-sized room, but the population doesn’t really start to grow until hour 45, meaning that the trash day delay is as effective as doubling the size of the room. This makes sense since the distance from the far corner to the door corner is just slightly longer than the distance from the center of the room to the door corner in a room twice as wide/deep (sqrt(2) vs sqrt(5)/2, respectively). 

**Next to the door:** Like with the pantry, due to the nature of my approximations/assumptions the model blows up when the fruit is too close, except this scenario is worse in that the numbers make no sense at all. Too many smell particles are hitting the door and attracting an unrealistic number of flies at once, causing calculation issues. As with the pantry, potential future refinement to the model is limiting the rate of flies that can be attracted from outside.

## Sensitivity to Amount of Fruit
Assuming that bacteria cannot travel between fruit, doubling the amount of fruit would directly double the amount of bacteria per day. Double the fruit also means double the food for the flies, so carrying capacity would double as well. As can be expected, the population of fruit flies exactly doubles the base model. With that said, it’s interesting to note that doubling the fruit does not change when the population really starts to grow, at around hour 35. Thus, more fruit does not necessarily mean one needs to take out the trash more often to keep fruit fly populations in check. 

## Conclusion
In this model, fruit fly population growth seems to be attributed mostly to new flies attracted each hour. Increases in room size greatly decrease the population of flies but do not significantly prolong the interval between trash days. Meanwhile, placement of the fruit does significantly affect both the population of flies and the interval between trash days. More rotting fruit does not seem to indicate that the trash needs to be taken out more often to keep fruit flies in check; however, the smell will be stronger and fly populations will grow much more rapidly. 

While this model generally approximates fruit fly populations well, including the time scales, it blows up when the fruit is too close. To prevent this problem in the future one can limit the rate of flies that can be attracted from outside. Another area to consider in the future is that fruit flies consume the rotting fruit itself so they eventually should run out of food, regardless of new flies being attracted. Thus, perhaps some carrying capacity should be included with the attracted flies as well. 


## Works Cited
1. Where Do Fruit Flies Come From and What Attracts Them? (2020). Orkin.Com. https://www.orkin.com/flies/fruit-fly/where-do-fruit-flies-come-from
2. Bacteria - The Role Of Bacteria In Fermentation. (2020). JRank Science. https://science.jrank.org/pages/710/Bacteria-role-bacteria-in-fermentation.html
3. Frontiers. (2019, July 24). An apple carries about 100 million bacteria -- good luck washing them off: Most microbes are inside the apple -- but the strains depend on which bits you eat, and whether you go organic. ScienceDaily. Retrieved August 7, 2020 from www.sciencedaily.com/releases/2019/07/190724090255.htm
4. How Long Does it Take for Food to Spoil? (2016, September 23). Mental Floss. https://www.mentalfloss.com/article/86571/how-long-does-it-take-food-spoil
5. Matis, J. H., & Kiffe, T. R. (2004). On stochastic logistic population growth models with immigration and multiple births. Theoretical population biology, 65(1), 89–104. https://doi.org/10.1016/j.tpb.2003.08.003



