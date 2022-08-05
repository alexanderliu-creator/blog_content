---
title: 美赛小分析文章(26)
date: 2021-02-21 16:05:21
tags: 美赛
categories: 2021年美赛
---



# 美赛模拟赛或正赛中的一个小模块~

---



<!--more-->
Firstly , our team successfully get the network of relationships of various generations after modeling . By using the PageRank method , the relationship networks between artists of each era is superimposed with the network of all previous eras , and the long-term influence of each artist from the active era is obtained as a measure of the long-term influence of music. Some interesting results are discovered by us just as some artists may have just a small number of direct followers , but their influence spans time and space and has a profound impact on the development of music history . The identification of these key artists seems to be having profound impact on the development of record companies.

​			 Secondly , we also analyze the relationship networks of artists in each era as independent individuals , through which short-term influence of musicians in each era - heat , and get heat list of each era. Just like what we have done above , the same method can be taken to get the daily hot list in reality , which is of great significance to music software. At the same time , we restore hot lists of artists of various ages , which also play an important role in exploring the development history of music and the key artists in the development history.

​			Thirdly , we analyze the network of music genres in each era and get the influence of each genre in each era. Through the ranking changes of the influence of each genre and the KL divergence between the genres, the emerging genres in each era and their development and evolution direction are measured . This method helps us restore the history of the rise and fall of music genres, and identify the music revolution and its changing trends. At the same time, these historical data can also be used to predict the future development trend of music genres, and to point out the development direction for record companies and other related industries.

​			 Through the principal component analysis of the music feature data, we calculate the dimensionality reduction feature distribution corresponding to each artist and genre. By calculating the JS divergence between the distributions of two artists, we can measure the similarity between artists. We find that artists of the same genre are more similar than artists of different genres. This similar measurement provides a new idea for the music platform to recommend similar singers and songs based on the singers users prefer.

​			  We calculate the JS divergence between the two genres to obtain the similarity between the genres. By calculating the similarity between each genre and all other genres, the appeal of each genre is measured. We find that music such as rock and blues is very infectious, which is why they are most popular all over the world. By calculating Spearman's correlation coefficient, we conclude that infectious music lasts longer, which provides a direction for artists to create infectious songs.

​			 Next , we wanna talk about the method we take to adjust to larger and richer data. If the data can be specific to each year and become richer , we can conduct accurate time series analysis for each music genre, so as to obtain a more intuitive and accurate influence ranking and trend. You're supposed to combine the hidden Markov model or other forecasting tools with the model we have established to make more accurate predictions for future development. This will have great reference significance for music platforms. 

​			 Last but not least , we want to explore the influence of music on culture. We find that before the 1940s, it is the era of traditional music. These traditional music are compatible with the social culture at the time. With the full outbreak of the World War in the 1940s, the social environment has undergone tremendous changes, and people are eager to release their depressed emotions. Under this kind of social situation, emerging music like R&B and Latin pay more attention to individualization, rather than being restricted by traditional social culture thus draw more and more attention from people at that era. People’s personalities break through the cage and promote people’s ideological emancipation. After the World War, the economy recover and everything reborn. Until the 1950s, a new generation of young Americans who were baptized in World War II are more equal to equal and free lives. They develop the rock genre representing rebellion and freedom. It is not only popular in the United States, but also spread across the oceans and the world. This trend of thought containing democracy and freedom spreads throughout the world. In the 1960s, with the outbreak of new European thoughts, Europeans developed independent music genres. This is Electronic, which promotes the characteristics of European music. Electronic has gradually moved from a European independent genre to the world, and promoted such as Treman(a kind of instrument of Electronic) and the development of emerging technologies such as music synthesizers and music production software.