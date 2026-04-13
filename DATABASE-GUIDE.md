# Pokopia Space Optimizer — Database Documentation

## Overview

The Pokopia Space Optimizer uses a three-layer data model derived from Serebii's Pokopia database:

1. **Pokemon** → individual creatures with favorite item groups
2. **Favorite Groups** → 44 category tags items can belong to
3. **Items** → in-game decorations/objects that belong to multiple groups

## Data Structure

### Pokemon Object
```json
{
  "id": 1,
  "name": "Pikachu",
  "habitat": "Sky",
  "favoriteGroups": ["Electronics", "Round stuff", "Colorful stuff"]
}
```

- **id**: Unique Pokemon identifier
- **name**: Pokemon species name
- **habitat**: One of 7 locations (Forest, Water, Mountain, Sky, Cave, Wasteland, Urban)
- **favoriteGroups**: Array of 44 possible favorite item group categories

### Favorite Groups (44 Total)
Serebii's official item preference categories that Pokemon can like:

- **Aesthetic**: Cute stuff, Colorful stuff, Shiny stuff, Pretty flowers, Spooky stuff
- **Material**: Soft stuff, Hard stuff, Metal stuff, Stone stuff, Wooden stuff, Glass stuff, Fabric
- **Nature-based**: Lots of nature, Lots of water, Lots of fire, Lots of dirt, Nice breezes
- **Function**: Electronics, Construction, Containers, Exercise, Gatherings, Group Activities
- **Special**: Luxury, Spooky stuff, Ocean vibes, Complicated stuff, and 25+ more

### Item Object
```json
{
  "id": 1,
  "name": "Soft Cushion",
  "groups": ["Soft stuff", "Cute stuff", "Fabric"]
}
```

- **id**: Unique item identifier
- **name**: In-game item name
- **groups**: Array of 1-3 favorite categories the item belongs to

## Scoring Algorithm

When you select Pokemon, the app:

1. **For each item**, counts how many of the selected Pokemon's favorite groups it belongs to
2. **Scores**: +2 points per matched favorite group
3. **Ranks**: Items sorted descending by total score
4. **Displays**: 
   - Final score (sum of all matches)
   - Number of group matches (how many distinct favorite categories are satisfied)

### Example
- **Pikachu** likes: Electronics, Round stuff, Colorful stuff
- **Charizard** likes: Lots of fire, Hard stuff, Nice breezes
- **Electric Lamp** belongs to: Electronics, Shiny stuff, Noisy stuff

Score calculation:
- Pikachu: Electronics ✓ (+2) = 2 points
- Charizard: none = 0 points
- **Total: +2, 1 match**

## Database Source

All data comes from Serebii.net's Pokopia section:
- **Pokemon list**: https://www.serebii.net/pokemonpokopia/availablepokemon.shtml
- **Favorite groups**: https://www.serebii.net/pokemonpokopia/favorites.shtml
- **Individual category pages**: Manually mapped Pokemon to groups based on official data

## Files Included

1. **pokopia-database.json**
   - Complete, machine-readable database
   - 95 Pokemon, 30 items, 44 favorite groups
   - Use this to extend the app or integrate into other projects

2. **pokopia-optimizer-real-data.html**
   - Standalone web app with embedded database
   - No external dependencies
   - Works offline, single-file deployment

3. **pokopia-optimizer.jsx**
   - React component source (for build tools/bundlers)
   - Can be imported into larger applications

## How to Extend

### Add a New Pokemon
Edit `pokopia-database.json`:
```json
{
  "id": 96,
  "name": "NewPokemon",
  "habitat": "Forest",
  "favoriteGroups": ["Cute stuff", "Pretty flowers", "Lots of nature"]
}
```

### Add a New Item
```json
{
  "id": 31,
  "name": "New Furniture",
  "groups": ["Cute stuff", "Wooden stuff"]
}
```

Items will automatically match against all Pokemon's favorites.

### Add a New Favorite Group
First add to `favoriteGroups` array, then update Pokemon `favoriteGroups` arrays and item `groups` arrays to reference it.

## Game Mechanics Reference

In Pokopia (based on Serebii documentation):

- **Friendship**: Giving items matching a Pokemon's favorites increases friendship faster
- **Comfy Level**: Decorating habitats with matching items increases the Comfy Level faster (visible as larger sparkles)
- **Trade Value**: For Pokemon with the Trade specialty, items matching their favorites are worth 50% more in trades

This optimizer shows you which single item best satisfies the most Pokemon's preferences simultaneously — useful for shared spaces or decorating multiple habitats efficiently.
