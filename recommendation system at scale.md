---
tags:
  - interview
---
source: [Eugene](https://eugeneyan.com/writing/system-design-for-discovery/)

![[efa042cf6ee31da270b0001a1bd5a5f43149ffb3df52c2c69e03ff2e6d0dfb99.png]]

# Four Stage Recommender Examples

## Personalized Music Discovery Playlist:

- **Retrieval**: Nearest neighbor search  in item embedding space  
- **Filtering**: Remove tracks user has heard before (e.g. w/ Bloom filters)  
- **Scoring**: Re-use  embedding space distance from nearest neighbor search  
- **Ordering**: Trade off between score, similarity and BPM to  reduce jarring track transitions

## Social Media Feed:

- **Retrieval**: Random walks on social graph to find new items in user's network
- **Filtering**: Remove posts from muted or blocked users 
- **Scoring**: Predict user’s **likelihood of interacting** with posts  
- **Ordering**: “Twiddle” the list so  adjacent posts are from different authors


## eCommerce “Add to Cart” Recs:

- **Retrieval**: Look up items commonly *co-purchased* with cart contents  
- **Filtering**: Remove candidate items that are currently *out of stock* (or already in your cart)  
- **Scoring**: Predict how *likely to buy* each candidate item the user is  
- **Ordering**: Arrange the items to present possibilities at various price points to *maximize expected revenue*  

## Streaming Service Home Screen:

- **Retrieval**: *Many candidate sources* for the various rows/shelves/banners  
- **Filtering**: Remove items that *aren’t licensed* for streaming in user’s country  
- **Scoring**: Predict user’s *stream time* for each item  
- **Ordering**: Arrange a set of shelves that trade off between predicted relevance and *matching the genre distribution* of the user’s previous consumption  

