---
layout: post
title: "Creating an Object Tracker"
author: "Eric"
categories: updates
tags: [updates]
image: tracker_visual.svg
---
In order to effectively target weeds while on a moving platform, we need a way to track them and identify them between consecutive frames. 

## Matching the Weeds
In order to match weeds between consecutive frames the following simple logic is used:
1. Create a matrix which the distances between each new and old object:
```c++
                             __  new_objects --->  __
             active_objects | D_1,1 . . . . . . D_1,n|
                  |         |  .       .             |
                  |         |  .           .         |
                  |         | D_m,1             D_m,n|
                  V         |__                    __|
```
1. Sort within each row so that min distance is in first column
1. Sort the rows based on the distance in the first column
1. Match each new object with the active object that is the lowest distance (euclidean), within a tolerance

In order to do this effectively, we create a matrix of distances:
```c++
/* 1. calculate distance between each current point and each new point */
int32_t m = m_active_objects.size();
int32_t n = new_objs.size();
std::vector< std::vector<distance> > dist_matrix(m, std::vector<distance>(n));

for (int i = 0; i < m; i++)
{
    for (int j = 0; j < n; j++) 
    {
        object crnt_obj = m_active_objects[m_id_list[i]];
        dist_matrix[i][j] = euclidean_distance(crnt_obj, new_objs[j]);
    }
}
```

Then we create a matrix of indices to our objects:
```c++
/* 2. Sort by distance */  
// Keep track of indices -- new objs
// | 0 1 - - - - - n |
// | 0 1 - - - - - n |
// | - - - - - - - - |
// | - - - - - - - - |
// | 0 1 - - - - - n |
std::vector<std::vector<size_t> > sorted_ids(m, std::vector<size_t>(n));
for (int i = 0; i < m; i++)
    std::iota(sorted_ids[i].begin(), sorted_ids[i].end(), 0);

// Keep track of indices -- active objs
// | 0 1 - - - - - m |
std::vector<size_t> active_obj_ids(m);
std::iota(active_obj_ids.begin(), active_obj_ids.end(), 0);


```

Then we sort intra-row as previously mentioned (we use a custom lambda comparator in order to sort the id matrix based on the distance matrix):
```c++
// Sort intra-row (find closest new object to each old object)
// | 0 1 - <---> - n |
// | 0 1 - <---> - n |
// | - - - <---> - - |
// | - - - <---> - - |
// | 0 1 - <---> - n |
for (int i = 0; i < m; i++)
{
     std::sort(sorted_ids[i].begin(), sorted_ids[i].end(), 
            [&dist_matrix, &i](const size_t& a, const size_t& b) -> bool
            {
                return dist_matrix[i][a] < dist_matrix[i][b];
            });
}
        

```

Then we sort the rows using a similar method:
```c++
// Sort the rows based on min distance in each row
// | 0 1 - <---> - m |
std::sort(active_obj_ids.begin(), active_obj_ids.end(), 
        [&dist_matrix, &sorted_ids](const size_t& a, const size_t& b) -> bool
        {
            return dist_matrix[a][sorted_ids[a][0]] < dist_matrix[b][sorted_ids[b][0]];
        });

```

Then for each active object we iterate over our sorted_ids and take the first one that matches and update our current active object.
```c++
// Loop over each active object
for (auto itr = active_obj_ids.begin(); itr != active_obj_ids.end(); itr++)
{
    // loop over each new object in the row 
    for (auto sub_itr = sorted_ids[*itr].begin(); sub_itr != sorted_ids[*itr].end(); sub_itr++)
    {
        // Update here ...
    }
}
```


## The Details
In order to avoid false positives, we only register new objects once they've been seen for a specified number of consecutive frames. Further, when we lose sight of an object we continue tracking it for a specified number of frames and discard it after that.

## Targeting
In order to make sure we get as many weeds as possible, we keep our weeds in a queue sorted by 'y' coordinate. In other words, the next weed to be going out of frame is the first one we target.
