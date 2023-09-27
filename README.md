# Fix Clipping for Dragon Hunter Armor

When playing DAI for Human Male with warrior class, I like wearing [Plate Mail of the Dragon Hunter](https://dragonage.fandom.com/wiki/Plate_Mail_of_the_Dragon_Hunter) armor, but it has a nasty visual bug, it's clipping through the shields. This mod fixes this issue.

![comparison.png](https://github.com/antontkv/dai-fix-clipping-dragon-hunter-armor/assets/17809291/a3e62d1b-1401-4cf8-a509-865363f1c7a7)

Values for moving weapon and shield away from the armor, should be applied based on character, gender, race, and class. Because of this right now this mod fixes these combinations:

- **Vestments of the Dragon Hunter (Light Armor)**
  - None
- **Armor of the Dragon Hunter (Medium Armor)**
  - None
- **Plate Mail of the Dragon Hunter (Heavy Armor)**
  - Human Male Warrior Inquisitor

## How to

Files in the `src` directory were made by using Frosty Editor export EBX to XML function.

Files have this structure:

![image.png](https://github.com/antontkv/dai-fix-clipping-dragon-hunter-armor/assets/17809291/e5a02de6-07d7-4016-8cc6-c9d4d4f8f687)

Where each `ItemPartPartyMemberRuntimeAppearance` defines which appearance armor would have based on character, gender, race, and class. So the same armor may have a different appearance for different party members or the main character.

![image-1.png](https://github.com/antontkv/dai-fix-clipping-dragon-hunter-armor/assets/17809291/1a4f4cee-84da-48a0-8f1b-31d3ac3d7e2c)

`AppearanceID` defines variables for which combination of character, gender, race, and class block of `ItemPartPartyMemberRuntimeAppearance` applies. It's a lot of combinations to find the exact match, so I thought that it would take me a while to figure out which is which. But thanks to the kind soul that wrote [this post](https://www.nexusmods.com/dragonageinquisition/videos/145) on nexusmods, I don't have to guess.

PartyMemberID values:

0. Inquisitor
1. Cassandra
2. Sera
3. Dorian
4. Blackwall
5. Cole
6. Vivienne
7. Solas
8. Varric
9. Ironbull

Gender values:

1. Male
2. Female

PartyMemberClassTypeID values:

1. Warrior
2. Rogue
3. Mage
4. (For any companion)

ItemRaceID values:

0. Human (or for any companion)
1. Elf
2. Dwarf
3. Qunari

So for my combination (Human Male Warrior Inquisitor), I need these values:

```xml
<PartyMemberID>0</PartyMemberID>
<Gender>1</Gender>
<PartyMemberClassTypeID>1</PartyMemberClassTypeID>
<ItemRaceID>0</ItemRaceID>
```


Under `AppearanceRuntimeRef/OnDisplayTimelines` add a new item and set it to the `BWTimeline` file from `DA3/Animation`. Since I needed to move the shield I chose `DA3/Animation/set_torso_sockets_to_armor_thickness_50`.

![image-2.png](https://github.com/antontkv/dai-fix-clipping-dragon-hunter-armor/assets/17809291/0d20e43f-2965-4354-8f34-4e83b9094cc3)

Thats all.

## Why this happens

When you change your armor, you can notice that weapons on the side or back move a little. This is very noticeable in the inventory screen when you remove and apply armor. This happens because each armor has `OnDisplayTimelines`, which is an animation that applies to armor, that moves weapons.

Because this armor doesn't have any `OnDisplayTimelines`, no animation is run when wearing it. So weapons stay in the position of the last run animation. You can actually fix this bug with this armor, by simply wearing any other armor and then applying Dragon Hunter armor.
