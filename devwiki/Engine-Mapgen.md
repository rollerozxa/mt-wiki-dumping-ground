## Overview

A map generator (hereinafter referred to as \"mapgen\") object creates new map and responds to queries about map terrain. Each derived Mapgen class must implement the two pure virtual methods in the Mapgen interface, makeChunk() and getGroundLevelAtPoint().\
Members of a Mapgen object are guaranteed to be valid data used in the latest call of makeChunk() by that object.\
Access of Mapgen objects are typically not thread-safe.

### Mapgen

##### Pure virtual methods to be implemented {#pure_virtual_methods_to_be_implemented}

``` cpp
virtual int getGroundLevelAtPoint(v2s16 p);
```

Returns the ground height at the 2d coordinates p.\

``` cpp
virtual void makeChunk(BlockMakeData *data);
```

Given a BlockMakeData structure, update the included ManualMapVoxelManipulator for the range of blocks from blockpos_min up to and also blockpos_max with MapNodes from the included INodeDefManager.\
===== Non-virtual methods =====

``` cpp
void updateLiquid(UniqueQueue<v3s16> *trans_liquid, v3s16 nmin, v3s16 nmax);
```

Add liquids that need to be updated to the transforming_liquid queue.

``` cpp
void setLighting(v3s16 nmin, v3s16 nmax, u8 light);
```

Set all nodes\' light levels in vm to the specified, uniform light level.

``` cpp
void calcLighting(v3s16 nmin, v3s16 nmax);
```

Calculate and update the lighting for nodes in vm.

``` cpp
void calcLightingOld(v3s16 nmin, v3s16 nmax);
```

Same as calcLighting() and may be slightly more accurate, but 22x slower on average.

##### Members

`u64 seed`

The map seed used for generation.

`int water_level`

The node Y position below which water occurs, and MapBlocks are marked as being underground.

`bool generating`

Signifies if the Mapgen object is currently in a call to makeChunk(). Reading this variable is not thread safe, but this is not problematic since it is intended to be used for informational purposes, such as profilers or output, rather than synchronization.

`int id`

The ID and index of that Mapgen in the EmergeManager.

`ManualMapVoxelManipulator *vm`

Must be a pointer to a valid ManualMapVoxelManipulator while makeChunk() is executed. Set this equal to what is given in BlockMakeData.

`INodeDefManager *ndef`

Must be a pointer to a valid INodeDefManager while makeChunk() is executed. Set this equal to what is given in BlockMakeData.

`v3s16 csize`

Represents the size of the chunk that the Mapgen expects to process on makeChunk().

`s16 *heightmap`

An X, Z array of ground level points in the most recently generated chunk. It is csize.X \* csize.Z elements large. This member is NULL if the mapgen does not support heightmaps.

`u8 *biomemap`

An X, Z array of biome IDs (indices of biomes in the BiomeDefManager) at each horizontal point in the most recently generated chunk. It is csize.X \* csize.Z elements large. This member is NULL if the mapgen does not support biomes.

### BlockMakeData

A BlockMakeData structure has the following members:\
ManualMapVoxelManipulator \*vmanip The ManualMapVoxelManipulator where nodes are to be written to. Already contains the current map contents in the contained coordinates. If a node has not previously been written to, it is CONTENT_IGNORE.\
u64 seed The 64-bit map seed to be used for randomized events. Not need anymore, as the base MapgenParams object passed to MapgenFactory::createMapgen() will contain this as well.\
v3s16 blockpos_min

`v3s16 blockpos_max`

Respectively, the minimum and maximum (inclusive) block (NOT NODE) coordinates to be generated.\
v3s16 blockpos_requested The block coordinate of the actual block requested for generation. Not really necessary.\
UniqueQueue`<v3s16>`{=html} transforming_liquid A UniqueQueue of 3d coordinates specifying which nodes are liquid and need a liquid transform applied after generation so they can flow.\
INodeDefManager \*nodedef A NodeDef that contains the definitions for nodes to be placed to the ManualMapVoxelManipulator.\

## Boilerplate Mapgen Code {#boilerplate_mapgen_code}

Here is the bare minimum for a (playable) map generator:

``` cpp
class MapgenTest : public Mapgen {
public:
    void makeChunk(BlockMakeData *data) {
        vm   = data->vmanip;
        ndef = data->nodedef;
        
        v3s16 node_min = data->blockpos_min * MAP_BLOCKSIZE;
        v3s16 node_max = (data->blockpos_max + v3s16(1,1,1)) * MAP_BLOCKSIZE - v3s16(1,1,1);
        
        MapNode n_air(ndef->getId("mapgen_air"));
        MapNode n_grass(ndef->getId("mapgen_dirt_with_grass"));
        
        for (int z = node_min.Z; z <= node_max.Z; z++) {
            for (int y = node_min.Y; y <= node_max.Y; y++) {
                for (int x = node_min.X; x <= node_max.X; x++) {
                    int i = vm->m_area.index(x, y, z);
                    vm->m_data[i] = (y == -4) ? n_grass : n_air;
                }
            }
        }
        
        calcLighting(node_min, node_max);
    }
    
    int getGroundLevelAtPoint(v2s16 p) {
        return 0;
    }
};

class MapgenTestParams : public MapgenParams {
public:
    bool readParams(Settings *settings) {
        return true;
    }
    
    void writeParams(Settings *settings) {  
    }
};

class MapgenTestFactory : public MapgenFactory {
public:
    Mapgen *createMapgen(int mgid, MapgenParams *params, EmergeManager *emerge) {
        return new MapgenTest();
    }
    
    MapgenParams *createMapgenParams() {
        return new MapgenTestParams();
    }
};
```

Then, simply add

``` cpp
registerMapgen("test", new MapgenTestFactory());
```

to EmergeManager::EmergeManager().

[Category:Core Engine](Category:Core_Engine "wikilink") [Category:Mapgen](Category:Mapgen "wikilink")
