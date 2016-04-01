# List View Adapters

List view adapters are a common pattern used in android development when an application wishes to represent some information as a list. 

Some applications may have many lists. It is possible that these lists will all have the same general style. It is also possible that clicking an item in one list, will take you to another list.

The Java objects that are represented as items within a list may differ for each list, for example you may have a list of Car objects. When a car is selected you may wish to open a new list that contains location objects, stating where the car may be sold. 

The overall look and feel of these lists, along with a set of actions such as re-ordering or deleting will be common across them all. The differences will include the type of object contained within them and potentially the way the information is displayed for each item. 

## List View Adapter Template

To avoid code duplication, you can set up an adapter template class containing the core functionality. This class can then be inherited to retain the features but also allow the children to have different object types contained in the list. 