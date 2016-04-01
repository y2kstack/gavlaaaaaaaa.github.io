---
layout: post
title: Android Programming: List View Adapters 
author: Lewis Gavin
comments: true
tags:
- android
- java
- programming
---

# List View Adapters

List view adapters are a common pattern used in android development when an application wishes to represent some information as a list. 

Some applications may have many lists. It is possible that these lists will all have the same general style. It is also possible that clicking an item in one list, will take you to another list.

The Java objects that are represented as items within a list may differ for each list, for example you may have a list of Car objects. When a car is selected you may wish to open a new list that contains location objects, stating where the car may be sold. 

The overall look and feel of these lists, along with a set of actions such as re-ordering or deleting will be common across them all. The differences will include the type of object contained within them and potentially the way the information is displayed for each item. 

## List View Adapter Template

To avoid code duplication, you can set up an adapter template class that contains the core functionality. This class can then be inherited so child classes can make the most of these core features but because it is Templated, it will also allow the children to have different object types contained within the list. 

During the development of my Gymify application, I used a ListViewAdapter template class as follows:

~~~java

public abstract class ListViewAdapter<T> extends ArrayAdapter<T> {

	// objects to hold the list, contect and layout inflator (used later)
	protected List<T> list;
	protected Context context;
	protected LayoutInflater inflater;

	/* SparseBooleanArray that will be used to hold whether a list item has been selected or not
	this functionality will allow the user to select multiple list items and perform an action
	such as delete on multiple list items */
	private SparseBooleanArray mSelectedItemsIds;

	//basic constructor
	public ListViewAdapter(Context context, int resourceId, List<T> list) {
        super(context, resourceId, list);
        mSelectedItemsIds = new SparseBooleanArray();
        this.list = list;
        this.context = context;
        inflater = LayoutInflater.from(context);
    }

    // abstract getView function that must be defined by implementing classes
	public abstract View getView(int position, View view, ViewGroup parent);

	// function to remove items from the list
	public void remove(T object) {
        list.remove(object);
        notifyDataSetChanged();
    }

    // toggle a list item as selected or not selected
    public void toggleSelection(int position) {
        selectView(position, !mSelectedItemsIds.get(position));

    }

    //function used to cancel selection of items and reset the SparseBooleanArray
    public void removeSelection() {
        mSelectedItemsIds = new SparseBooleanArray();
        notifyDataSetChanged();
    }

    //function to add or remove list items to the selected items array
    public void selectView(int position, boolean value) {

        if (value){
            mSelectedItemsIds.put(position, value);
        }
        else{
            mSelectedItemsIds.delete(position);
        }
        notifyDataSetChanged();
    }

    //return the number of selected items
    public int getSelectedCount() {
        return mSelectedItemsIds.size();
    }

    //return a SparseBooleanArray of selected items
    public SparseBooleanArray getSelectedIds() {
        return mSelectedItemsIds;
    }

}

~~~
//TODO: What happens if I remove the abstract function? Does it still work? Can I remove the getView function from children, does it still work? when I delete an item, it gets removed from the list, does it get removed from the DB?

What I have implemented here is the skeleton for all other ListViewAdapters to extend on. This means that all ListViews will have the multi-selection and deletion capabilites, it will be inherently built in. However how each list item is displayed, will be different for each ListView; hence the abstract getView function. This enforces the children classes to implement this themselves.




