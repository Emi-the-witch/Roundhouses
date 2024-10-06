**For V67**

This mod is for the game [Songs of Syx](https://store.steampowered.com/app/1162750/Songs_of_Syx/).

It uses [4rg0n's mod example](https://github.com/4rg0n/songs-of-syx-mod-example/) as the template. 

Roundhouses is made by EmiTheWitch and Mialbowy. 

Additional houses can be added by:

	Data files:
		Adding to init.room._HOME.txt for the initial housing population
			{
				COSTS: [],
				STATS: [13,],
			},
				
		Adding to text.room._HOME.txt for the text when choosing a room (including tooltip)
			{
				NAME: "Roundhouse",
				DESC: "A huge residence for those who hate squares.",
			},
		
	Code files: 
		Adding to settlement.room.house.home.ConstructorHome.java
			Population, upgraded population, 2nd upgrade population for the room. Upgrade is 40% and 2nd Upgrade is 80% originally, rounded.
					{13,18,23},
				
			Create new furnishing placements (xx = not accessible entry, ee = entry, __ = room, ss = null (for non square shapes))
			am value (5) is how many times it can be in a row. [Limited by settlement.room.main.furnisher.Furnisher.java's "allItems" array length, so big rooms and high am cause issues.]
		
				create(new FurnisherItemTile[][] {
					{ss,ss,ss,ss,xx,ss,ss,ss,ss},
					{ss,ss,ss,xx,__,xx,ss,ss,ss},
					{ss,ss,xx,__,__,__,xx,ss,ss},
					{ss,xx,__,__,__,__,__,xx,ss},
					{xx,__,__,__,__,__,__,__,xx},
					{ss,xx,__,__,__,__,__,xx,ss},
					{ss,ss,xx,__,__,__,xx,ss,ss},
					{ss,ss,ss,xx,ee,xx,ss,ss,ss},
				}, 5);
		
			Modify the data value to be equal to the number of house designs 		
				super(init, 5, 1);
			
		Adding to settlement.room.house.home.Carpet.java

			Add a layout for the carpet that matches the usable tiles, generally0x10 is ignored, 0x00 is carpet. IDK what this really changes visually though. 
			
				data[3] = make(
					new int[][] {
					{0x10,0x10,0x10,0x10,0x00,0x10,0x10,0x10,0x10},
					{0x10,0x10,0x10,0x00,0x00,0x00,0x10,0x10,0x10},
					{0x10,0x10,0x00,0x00,0x00,0x00,0x00,0x10,0x10},
					{0x10,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x10},
					{0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x00},
					{0x10,0x00,0x00,0x00,0x00,0x00,0x00,0x00,0x10},
					{0x10,0x10,0x00,0x00,0x00,0x00,0x00,0x10,0x10},
					{0x10,0x10,0x10,0x00,0x00,0x00,0x10,0x10,0x10},
					}
				);
			
			Modify the data value to be equal to the number of house designs in the first number (second number is rotations, don't change)
				private final int[][][][] data = new int[5][4][][];
			
		
		Adding to settlement.room.house.home.Sprites.java
			Add furniture displays, you can have multiple inside of the sprites[n] but only one is used here:
			null = no furniture, 
			nic1, nic2 = nicknacks, 
			bedS, bedN = beds, 
			tabl = table, 
			staU, staD, ...I don't know.
			
				sprites[3] = mirror(new SpriteConfig[] {
					new SpriteConfig(new Sprite[][] {
						{null,null,null,null,null,null,null,null,null},
						{null,null,null,bedS,nic1,bedN,null,null,null},
						{null,null,bedS,null,null,null,bedN,null,null},
						{null,bedN,null,null,tabl,null,null,bedS,null},
						{null,bedS,null,nic2,nic1,stor,null,bedN,null},
						{null,bedN,null,null,tabl,null,null,bedN,null},
						{null,null,bedS,null,null,null,nic2,null,null},
						{null,null,null,staU,_mat,staD,null,null,null},
					}),
				});
		
			Modify the data value to be equal to the number of house designs:
					final SpriteConfig[][] sprites = new SpriteConfig[5][];
		
		Adding to settlement.room.house.home.HomeHouse.java
			If you need more than 100 usable tiles per room, update this variable:
				    public final int[] valid = new int[100];
				
		
		settlement.room.house.home.LivingDataD.java
			Do not change. This is modified to use the valid array in get / set like this:
			
				int tx = data.body().x1() + data.valid[tile]%data.body().width();
				int ty = data.body().y1() + data.valid[tile]/data.body().width();
		
		