+++
title = 'clash royale'
date = 2024-09-10T00:15:22-04:00
draft = false
+++

Old data analysis hack project on the Clash Royale mobile game from 2020. 

Card decks used by the top players in the world are scraped from the official Clash Royale API. Each card is a node, and together form deck graphs that model strategy. Deck observations can be clustered by attribute (e.g. troops, buildings, spells, etc.). The original code used node2vec as feature embedding, but I was exploring conditional probability networks and graph attention for edge prediction.

The goal was to provide utilities for time-series graph analysis of player trends. The project is deprecated, but I dream of reviving it. Here is demo code that worked back in 2020.

# code

Wrapper on `networkx` for querying the Clash Royale API. You'll need to [create an account](https://developer.clashroyale.com/#/register) to get a token. Edges are undirected and weighted by how many times two cards are used together.

```Python
from RoyaleAPI import Client
import matplotlib.pyplot as plt

with open('RoyaleAPI/key.txt', 'r') as file:
    dev_key = file.read().replace('\n', '')
proxy_url = 'https://proxy.royaleapi.dev/v1'

# Sample decks used by the top 10 players in global
client = Client(token=dev_key, url=proxy_url)
graph = client.create_empty_graph()
top10_graph = client.build_graph(graph, depth=10)

nx.draw_circular(top_graph, with_labels=True)
plt.show()
```

![](/cards.png)

Probabilities are more informative. The normalized attribute matrix has rows that sum to 1. It says that for all edges (u,v), the element A(i,j) is the P(v=types[j] | u=types[i]). Diagonal elements are the probability of the same type being used in a deck. Buildings were only used with other buildings in about 4.4% of decks sampled, a not so popular strategy.

```Python
client = Client(token=dev_key, url=proxy_url)
graph = client.create_empty_graph()
top_graph = client.build_graph(graph, depth=100)

types = ['Troop', 'Spell', 'Building']
probs = nx.attr_matrix(top_graph, node_attr="type", edge_attr='usages', normalized=True, rc_order=types)
print(probs)
```
```
[[0.45658263 0.41936775 0.12404962]
 [0.78915663 0.10692771 0.10391566]
 [0.66098081 0.29424307 0.04477612]]
```

This image shows how many times each card was used in decks from the top 500 players. There are clear usage hierarchies with an exponential looking dropoff. The game was so fun, and I really wanted to learn trends over time. I should revisit this.

![](/deg500.png)

# Disclaimer

This content is not affiliated with, endorsed, sponsored, or specifically approved by Supercell and Supercell is not responsible for it. For more information see Supercell’s Fan Content Policy: www.supercell.com/fan-content-policy.
