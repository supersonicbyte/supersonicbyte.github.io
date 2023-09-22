---
title: "xcdatamodeld No File Inspector"
tags:
- Xcode
- Swift
- iOS
- CoreData
date: 2023-09-22T12:17:43+02:00
---

Recently I worked on a project that uses Core Data. The underlying model changed and I needed to update an entity. This involved changing an attribute from `String?` to `[String]?`. In order to make the migration I added a new version of the model. When I tried to set the current active model in the `xcdatamodeld` file, the file inspector was blank and there wasn’t a way to do it through the UI (Xcode 14.3.1). So, I tried opening the `xcdatamoded` file in Finder as a text file and modifying the `_XCCurrentVersionName` key in the XML, but that didn’t work either. The green checkmark still stayed on the old model. I ran `git diff` in my terminal and saw that the `pbxproj` file was modified. After some digging around I saw a `XCVersionGroup` in the pbxproj file, which looked like this:
```
/* Begin XCVersionGroup section */
		724C37322ABD9D030026634A /* Model.xcdatamodeld */ = {
			isa = XCVersionGroup;
			children = (
				724C37332ABD9D030026634A /* Model.xcdatamodel */,
				724C37342ABD9D030026634A /* Model 2.xcdatamodel */,
			);
			currentVersion = 724C37332ABD9D030026634A /* Model.xcdatamodel */;
			path = Cocoon.xcdatamodeld;
			sourceTree = "<group>";
			versionGroupType = wrapper.xcdatamodel;
		};
/* End XCVersionGroup section */
```
I updated the currentVersion to point to the Model 2 identifier. So it looked like this after the change:
```
/* Begin XCVersionGroup section */
		724C37322ABD9D030026634A /* Model.xcdatamodeld */ = {
			isa = XCVersionGroup;
			children = (
				724C37332ABD9D030026634A /* Model.xcdatamodel */,
				724C37342ABD9D030026634A /* Model 2.xcdatamodel */,
			);
			currentVersion = 724C37342ABD9D030026634A /* Model 2.xcdatamodel <-- THIS CHANGED> */;
			path = Cocoon.xcdatamodeld;
			sourceTree = "<group>";
			versionGroupType = wrapper.xcdatamodel;
		};
/* End XCVersionGroup section */
```

I restared Xcode and the Model 2 was selected as the active model version!